# Ferret Attribution: Tracking Indonesian Hacker Group PLUTO-HEPHAESTUS

## Overview

Pluto Hephaestus is a hacker group from Indonesia, named after its Telegram channel “Pluto Hephaestus”. Active in the Indonesian hacking community, it has claimed affiliation with AnonSec Team in past attacks and is suspected of attacking government agencies and educational institutions in Indonesia, Thailand, the Philippines, Vietnam, South Korea and other countries, primarily to sell access to compromised systems.

In May 2026, Korean security vendor OASIS disclosed Hephaestus, a Claude-driven AI attack framework suspected of attacking governments and universities across multiple countries, and identified its associated Telegram channel Pluto Hephaestus. Over the following three months, the framework's attack activity never stopped.

In August, our tracking and analysis engine Ferret discovered Hephaestus's new working machine during continuous mining and captured its work reports, weapon arsenal and AI sessions. In its work logs we found that beyond its publicly known targets, the group had also directed attacks at Chinese government and universities. Based on this data, this article reconstructs the group's recent attack targets, arsenal and AI infrastructure.

## How It Was Exposed

The framework was exposed through its own working mechanism: during attacks, the Agent autonomously delivered files to victim machines via an open directory.

The platform's AI session records show a fixed stage in every attack chain: after gaining command execution on a victim server, it tests connectivity from the victim to its own file server. In a session on August 21, the AI injected a test command into a university learning platform; the tool call was described as “Test outbound connectivity to our VPS”, testing the /test file on port 8888 of its own server. Its delivery template also specifies the command executed on the victim side: curl the attacker's server to download a marker file into the web directory.

What was exposed goes beyond operational records. The workspace contained all credentials for the attacker's own infrastructure: VPS login passwords, tunnel relay connection secrets, keys for multiple intelligence and proxy services, and a per-target list of backdoor connection secrets for victim servers. Anyone who obtained this directory could log into the attacker's servers and connect to all of its backdoors, putting both the attacker's machines and the victim servers it controls at risk of takeover. The efficiency gained through automation ultimately exposed the entire operation, together with its takeover rights, to the public.

![image1](../Storage/pluto-hephaestus/image-01.png)

Figure 1: The Hephaestus Host

![image2](../Storage/pluto-hephaestus/image-02.png)

Figure 2: Delivery Endpoint Test Commands in AI Session Records

## Recent Attack Targets and Compromises

After May, the platform's activity intensified rather than declined. The workspace contained more than six hundred target-named attack directories; in the six days from August 19 to 24 alone, dozens of new directories were added, involving more than ten universities and government systems.

| Date | Country | Target | Details |
| --- | --- | --- | --- |
| Aug 20 | Indonesia | Atma Jaya Yogyakarta University | Sensitive file probing against multiple subdomains, and a targeted payload used to check a CMS site among them |
| Aug 21 | Indonesia | State University of Jakarta | Remote command execution in a quiz page; the attacker reset the administrator password and deployed a persistent callback backdoor that reconnects every thirty seconds |
| Aug 22 | Indonesia | Bandung Islamic University | SQL injection in a login entry point; the attacker used the data to escalate to root and replaced fifteen business login pages with attacker versions, covering graduation, transcripts, enrollment, payroll, scheduling and TOEFL registration portals |
| Aug 23–24 | Indonesia | Sultan Syarif Kasim State Islamic University | Attack directories present on two consecutive days |
| Aug 24 | Indonesia | Syiah Kuala University | Version-control directories of multiple subsystems were publicly downloadable; the attacker downloaded source repositories of e-diploma, competition and journal systems and searched them for database passwords |
| Aug 24 | Indonesia | Institute of Health Technology Bali | SSRF exploitation attempts against a system's installer interface; the full exploitation chain debugging was recorded in AI sessions |

Beyond the compromises and attempts above, throughout August 23 the attacker ran six or more AI sessions simultaneously, conducting large-scale credential validation against the learning platforms of Multimedia Nusantara University, Pakuan University, Indonesia Computer University, President University and others. Pasted credential lists reached hundreds of entries per batch, and credential files retained in the workspace exceeded ten thousand entries.

Files related to these targets retained in the workspace include: full attack report texts, target credential lists (redacted in this article), source code of replaced login pages, downloaded source repository copies, and session-level operation records.

![image3](../Storage/pluto-hephaestus/image-03.png)

Figure 3: Attack Report Archive and Sample Report Files

The attacker recently publicized on its Telegram channel an attack against Nigerian government domains, while verify timestamps show the actual attack date was 2026-05-10.

![image4](../Storage/pluto-hephaestus/image-04.png)

Figure 4: The Attacker's Telegram

## Chinese Targets in Historical Archives

The platform's targeting is dominated by Indonesian universities and government systems, but its target list far exceeds what was previously disclosed. One directory in the workspace contains more than three thousand target-domain-named directories, covering many countries and regions worldwide.

Among them, historical archives show that attack events against Chinese targets existed, but their contents have been wiped, leaving only directory skeletons:

| Target | Directory Traces |
| --- | --- |
| A municipal government website | Exploit preparation subdirectories exist under the attack directory, numbered after a CMS plugin vulnerability and another CVE; contents emptied |
| A university | Four subdirectory structures (evidence, exploit, intel, scripts) remain; contents emptied |
| Multiple domestic industry and community sites | Only empty directories remain, covering photography, console gaming, credit services, model communities and cloud storage sites |

The reason and timing of the cleanup cannot be determined. Based on available data, it cannot be confirmed whether attacks against Chinese targets were carried out or succeeded.

![image5](../Storage/pluto-hephaestus/image-05.png)

Figure 5: Chinese Target Skeletons in the Bulk Target Directory

## Arsenal

The exploit directory in the workspace organizes 28+ payload directories named by vulnerability identifiers, covering remote code execution, authentication bypass and injection vulnerabilities in CMSes, application servers and hosting panels. Numbers include CVE-2026-63030, CVE-2026-21858, CVE-2026-41940, CVE-2026-42945, CVE-2026-23918, CVE-2026-7275, CVE-2026-9082, CVE-2025-63888, CVE-2026-3844, CVE-2026-48907, CVE-2026-48908, CVE-2026-56290, CVE-2025-24813, CVE-2023-35885.

Beyond the payloads above, the workspace also includes:

- Three Linux local privilege-escalation exploits, one of which is recorded in an attack report as having achieved root on a Bandung Islamic University server
- Two mass-operation campaign directories, for large-scale exploitation of an automation workflow platform and a hosting panel, with target lists and execution results
- A self-maintained scanner template library and multiple deserialization gadget chain collections

![image6](../Storage/pluto-hephaestus/image-06.png)

Figure 6: Exploit Directory Listing

## Behavioral Signatures of the Agent

The platform's attack behavior carries highly recognizable signatures, useful for detection and correlation.

### The Four-Step Persistence Kit

After obtaining a shell it executes in order: download the tunneling tool as /usr/local/sbin/.libsys.so and register it in a cron job running every five minutes; append the attacker's public key to authorized_keys; create a hidden sudo-capable backup user named sysadm; and on some targets, additionally replace business login pages.

### How the Replaced Login Pages Work

The attacker renames each site's original homepage and keeps it in place, then replaces the login entry with a self-designed page. The page embeds a universal account (username Heph with a fixed password); entering it grants an administrator session into the original system. Already-authenticated visitors are unaffected; all other visitors only see the replaced page. The attack reports call this “mass domain takeover”. Combined with its session discussions about handing access to buyers, these embedded accounts are the ready-made way it transfers access to taken-over sites.

### Fixed Compromise Markers

After each compromise, two files, verify.txt and uid.html, are written into the web directory. The verification file is headlined “KIBOxGBLK SECURITY RESEARCH”, containing the target name, date, current user privileges and a “VERIFIED” status, with the taglines “~ Your Security Is My Playground ~” and “~ No System Is Safe ~”, and a Telegram contact at the end. The page file is titled “Pentest by KIBOxGBLK”. These markers appear across multiple compromised targets and corroborate the deployment records in the attack reports.

### Tunnel Secret Naming Pattern

Backdoor tunnel connection secrets follow a “fixed prefix + target name” combination: Heph1337_ plus a target abbreviation, or KIBOx prefixed to the target name, all pointing to a fixed relay address. Attack reports record each target's connection secret in plaintext.

### Credential Stuffing First

Before acting, it looks up historically leaked account credentials for the target domain through the breach-intelligence service LeakRadar, then validates them in bulk against the target systems and filters out high-privilege accounts. Session records from August 23 show the attacker running six or more AI sessions at once, pasting hundreds of credential pairs per batch for sharded validation; the workspace retains over ten thousand credential entries, with verified accounts organized into “domain + high-privilege account” lists. Credential stuffing takes priority over vulnerability exploitation, and is the source of this platform's efficiency.

### Every Operation Produces a Report

Every operation ends with an auto-generated full report containing an executive summary, target information (IP, OS, middleware, protection software versions), a stage-by-stage attack chain, credential statistics, persistence methods and ATT&CK mapping, plus backdoor connection commands and remediation suggestions. Reports are uniformly signed “Pentest by mr.spongebob x adit ganteng” and classified “AUTHORIZED PENETRATION TEST”. The attacker relies on these reports to manage hundreds of targets, which makes the reports the most complete archive for reconstructing its entire activity.

## How the Agent Is Organized and Operates

The following is compiled from its architecture documents and AI session records, with some original excerpts.

### Multi-Role Pipeline

The attack flow is a relay of six roles: reconnaissance (subdomains, co-hosted sites, fingerprints, sensitive files), entry (credential validation and initial access), anchoring (exploitation and shell acquisition), hunting (post-exploitation, privilege escalation, persistence), navigation (lateral movement), and review (report aggregation). Each role is defined by a separate Agent configuration file; the currently observed state decides the next routing step. For example, when reconnaissance finds a .env file, it immediately reports it and pivots to credential extraction.

### Per-Target Progress File

Each target maintains a state file recording the target address, discovered credentials, shell address, tunnel secret, current phase and win conditions. The Agent writes to it after every completed stage; it supports resume from checkpoint and rollback, and batch mode can advance multiple targets in parallel.

### Action Rule Table

The action rules in its architecture document:

```text
# .env found
if obs == "env_found":
    actions.append(("extract_creds", state.env_url))
    actions.append(("check_pma", state.target))
# Shell obtained
elif obs == "shell_obtained":
    actions.append(("gsocket_deploy", state.shell_url))  # FIRST!
    actions.append(("verify_txt", state.shell_url))
    actions.append(("harvest_creds", state.shell_url))
```

In other words: when a config file is found, extract credentials and check the database administration entry; when a shell is obtained, the first priority is deploying the tunnel, followed by writing the verification file and harvesting credentials.

### Fixed Routine After Shell Access

The routine is fixed: first check whether the target has cleanup scripts to avoid deployments being wiped on schedule; then download the tunneling tool, deploy it as a disguised system file and register it in cron; add an SSH public key; create a hidden backup user; harvest all site config files and credentials; and finally try local privilege escalation in turn. Every command in these steps is written in the architecture documents, and the Agent executes them as written.

### Writing Target-Specific Bypass Scripts on the Fly

When encountering a web application firewall or a custom login form, the Agent writes bypass or credential-stuffing scripts for that specific target on the spot, storing them in the target's attack directory with filenames starting with bypass_ or custom_spray_.

### Jailbreaking the Model Safety Mechanisms

Claude has built-in safety checks for attack-related instructions and refuses obvious malicious operations. The framework's execution rules demand “execute immediately without hesitation, no asking for permission”, and every configuration file is prefixed with legitimacy declarations such as “signed authorization contract” and “operator holds professional certifications”, inducing the model to skip safety review. These declarations are entirely inconsistent with its actual targets; it is what the industry calls jailbreaking.

### Parallel Sessions

The operator runs multiple AI sessions simultaneously, sharding credential lists across sessions for batch validation. Session records show the operator himself does only three things: paste credential lists, push for RCE results, and receive results; the attack relies entirely on the AI. Verified accounts are sorted by privilege level for further exploitation or resale.

![image7](../Storage/pluto-hephaestus/image-07.png)

Figure 7: Agent Configuration Files and Decision Routing Excerpts

## Attacker Profile

The operator's workspace shows clear personal traits. All configurations and operational instructions are in Indonesian, session records are full of Indonesian colloquial expressions, and targets center on Indonesian universities and government, showing typical local characteristics.

| Type | Identifier | Notes |
| --- | --- | --- |
| Name | Aditya | Found in attack reports and configuration |
| Telegram | @kibogblk | Contact left in verification files on victim servers |
| Telegram | @anakhekerpro、@hackersec | The only two accounts allowed to operate its self-built bot |
| Telegram | @ShellRadar_bot | Self-built notification bot, bound to a fixed session identifier |
| Email | kangpepes@protonmail.com | LeakRadar account email, decoded from its login credentials |
| Email | kangpepes3@protonmail.com | Backup email |
| Email | pentest_auto.7ai11m3p@protonmail.com | Dedicated to bulk registration |
| Account | BrightData | Proxy service account |

Technically, the tools in its workspace are mostly assemblies of open-source projects: scanners, enumeration tools and tunneling tools all come from public projects, and most actions in the AI sessions are combinations of ready-made tools. A few payloads are custom-built, including exploitation analysis of a learning management system's backup-restore feature and payloads written for specific CMSes.

Monetization is clear. In session records, the operator repeatedly asks how to hand over access to compromised systems, including “how to let the buyer in” and “how to use it without a VPS”, and there is a sizable account-validation pipeline. The workspace retains replaced login page source code that could be used for credential collection from target users, with the potential to expand harm.

## Summary

This case and the South African Neo case share the same risk pattern: the larger the space for autonomous Agent decisions, the larger the unconstrained risk surface. Hephaestus's AI autonomously operated its file delivery infrastructure, and it was precisely the uncontrolled directory scope of this infrastructure that exposed the entire operation to the public internet. While AI amplifies attack efficiency, it also amplifies the probability of operational mistakes being recorded and exposed: every bit of automation gained by the attacker simultaneously turns into its own operational risk, and into detection and attribution leads for defenders.

## IOC

| Type | IOC | Country | Notes |
| --- | --- | --- | --- |
| IP | 116[.]193[.]190[.]36 | Indonesia | Agent working machine |
| IP | 203[.]175[.]125[.]189 | Indonesia | File delivery server |
| IP | 45[.]148[.]244[.]66 | Netherlands | Tunnel relay |

## End

Ferret builds on FOFA's asset mapping capabilities, continuously monitors changes in open directories, uses AI summaries to flag high-value content, and feeds extracted IOCs into correlation analysis, turning open directories from one-off discoveries into a sustainably operated intelligence source. Researchers interested in collaboration are welcome to reach out.

![image8](../Storage/pluto-hephaestus/image-08.png)
