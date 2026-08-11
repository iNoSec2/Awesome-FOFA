# Cyberspace Mapping Platform Benchmark V1 Evaluation Standard

---

**Document ID:** BenchMark-NCM-V1

**Version:** V1

**Release Date:** August 2026

**Evaluation Target:** Cyberspace Mapping Platforms

---

## Preface

Cyberspace mapping is the process of identifying, collecting, and continuously tracking global network assets and the status of their services. As cyberspace assets continue to grow, differences in platform capabilities directly affect the coverage and timeliness of security awareness, threat hunting, and attack-surface management.

This standard establishes a unified evaluation framework and scoring methodology for cyberspace mapping platforms. Any eligible cyberspace mapping platform may be evaluated under this standard.

## 1 Scope

This standard applies to capability evaluations of cyberspace mapping platforms. The standard evaluates the following four capability categories:

1. Data collection capability;
2. Data freshness;
3. Search and retrieval performance;
4. DNS discovery capability.

## 2 Terms and Definitions

### 2.1 Unique Network Service Asset

The key for a unique network service asset is:

```text
IP + Port + Application Protocol
```

### 2.2 Application Protocol

An application protocol is a service protocol actually identified by a platform, such as HTTP, HTTPS, SSH, RDP, or MySQL.

### 2.3 Application-Protocol Normalization

The application-protocol normalization table must be frozen before scoring begins.

Protocol names from all participating platforms are first mapped to the same normalization table, then deduplicated and scored. Synonymous names, abbreviations and full names, version names of the same protocol, and name differences caused solely by TCP/UDP detection methods are merged into a single canonical protocol.

Different protocol families, different management interfaces, and protocols with independent service semantics are retained separately. Vendor-specific or extension protocols identified by a platform are not excluded merely because they are not listed in a public protocol library, as long as they point to a clearly identifiable, verifiable service protocol.

Protocol names with semantic ambiguity that cannot be confirmed must not enter formal scoring before the normalization table is frozen. The table is frozen before testing begins and published in the evaluation report.

### 2.4 FQDN

FQDN (Fully Qualified Domain Name). In this standard, FQDN normalization includes conversion to lowercase and removal of any trailing dot.

### 2.5 Wildcard DNS

Wildcard DNS is a phenomenon in which DNS wildcard records cause a large number of nonexistent subdomains to resolve to the same IP address. This standard identifies and consolidates wildcard-DNS records under a unified set of rules. The cleanup rules and the records folded under those rules are published in the evaluation report.

### 2.6 Valid Subdomain

A valid subdomain must satisfy all of the following conditions:

1. It is unique after FQDN normalization and deduplication;
2. It resolves to a public IP address at the time of testing;
3. It passes the unified wildcard-detection and folding rules.

### 2.7 Standard Field List

The standard field list is based on public protocol specifications and verifiable field meanings. It does not directly adopt any platform's proprietary field names. Fields with equivalent meanings across platforms are mapped to the same standard field. The field-mapping table and the standard field list are frozen together before testing begins.

### 2.8 Evaluator

The evaluator is the organization or individual that conducts testing, collects evidence, and issues the evaluation report in accordance with this standard.

## 3 Evaluation Framework and Scoring Method

### 3.1 Capability Categories

This standard defines four capability categories to organize test tasks and result tables. Each result table is scored independently.

| Capability Category | Sub-Categories |
| --- | --- |
| Data Collection Capability | 30-Day Data Scale, Protocol Coverage Breadth, Valid Unique Protocols, Deep Protocol Recognition (Sampled), Sample Coverage and Recognition |
| Data Freshness | Data Freshness |
| Search and Retrieval Performance | Page Query Performance, Single API Query Performance, Continuous API Pagination Performance |
| DNS Discovery Capability | DNS Discovery Capability |

### 3.2 Scoring Rules

All independent scoring results are converted to 0–100 according to their corresponding sections. Different result tables may use different targets, denominators, units, meanings of a full score, and failure conditions. Scores are used only for cross-platform comparison within each table.

Each result table must disclose its scoring type, raw value, denominator or band, time window, and sample scope. The meaning of a 100 under each scoring type is as follows:

| Scoring Type | Applicable Results | Meaning of 100 |
| --- | --- | --- |
| Fixed benchmark | 30-Day Data Scale | Reaching or exceeding 1 billion unique network service assets |
| Reference coverage | Protocol Coverage Breadth, Valid Unique Protocols, DNS Discovery Capability | Covering the full reference denominator defined by that formula |
| Fixed bands | Data Freshness, Search and Retrieval Performance | Every scored sample or task falls in the highest band |
| Sample-weighted | Sample Coverage and Recognition | Asset, port, and application-protocol sample coverage all receive full scores |
| Field-weighted | Deep Protocol Recognition (Sampled) | Acquiring all predefined high-value field points on valid common targets |

Detailed formulas are defined in their corresponding sections. Thresholds, denominators, and weights must not be adjusted according to the platforms' rankings in the current evaluation round.

- If a platform lacks a capability required by an item, the item receives 0 and is marked "Unsupported."
- If execution by the evaluator fails or evidence is missing, the item is retested.
- If a platform cannot complete an item because of product functionality, account entitlements, pagination limits, or export restrictions, the result receives 0 or is marked "Unsupported" as specified in the corresponding section.
- If evaluation cannot be completed due to environment failure or undelivered evidence, the result is marked "Pending Retest"; it must not be replaced with a zero, and the ranking for that table must not be published.

### 3.3 Result Report Requirements

The evaluation report must publish the following independent scores:

1. 30-Day Data Scale Score
2. Protocol Coverage Breadth Score
3. Valid Unique Protocol Score
4. Deep Protocol Recognition (Sampled) Score
5. Sample Coverage and Recognition Score
6. Data Freshness Score
7. Page Query Performance Score
8. Single API Query Performance Score
9. Continuous API Pagination Performance Score
10. DNS Discovery Capability Score

## 4 Test Conditions

### 4.1 Account Requirements

The highest-tier individual account directly purchasable from each platform's official website on the evaluation date is used. This standard evaluates product capabilities that are actually available and verifiable through publicly purchasable accounts. It does not evaluate internal data or enterprise-only capabilities that are unavailable to the account used in testing.

### 4.2 Unique Asset Definition

A unique network service asset uses `IP + Port + Application Protocol` as its key (defined in 2.1). Protocol names from all participating platforms are first mapped to the same application-protocol normalization table (defined in 2.3), then deduplicated and scored. Synonymous names are consolidated. The table is frozen before testing begins and published in the evaluation report.

### 4.3 Test Environment

All participating platforms are tested in the same round. The test location, network, timeout settings, and retry counts remain identical. The execution order is randomized.

## 5 Data Collection Capability Evaluation

Data Collection Capability comprises five independent results.

### 5.1 Scoring Items

| Independent Result | Max Score | Question Answered |
| --- | ---: | --- |
| 30-Day Data Scale Score | 100 | How many unique network service assets are available within the past 30 days? |
| Protocol Coverage Breadth Score | 100 | What is the platform's coverage quality across the canonical application-protocol set? |
| Valid Unique Protocol Score | 100 | How many verified protocols are detected only by this platform in the current comparison? |
| Deep Protocol Recognition (Sampled) Score | 100 | After recognizing a protocol, how much valuable field information can the platform acquire? |
| Sample Coverage and Recognition Score | 100 | How does the platform cover assets, ports, and protocols in the unified mixed sample? |

### 5.2 30-Day Hard Metrics

The underlying detection data used by Unique Asset Count, Protocol Coverage Breadth, and Valid Unique Protocols is measured over the continuous 30-day period immediately preceding the evaluation reference date. Each participating platform must disclose the actual query conditions, start and end times, and time zone. A platform that cannot set a time range must disclose the actual temporal meaning of its returned data and the resulting comparability limitation.

All three hard metrics use the same 30-day window. Calculation rules:

- Unique Asset Count: deduplicate by `IP+Port+Application Protocol`.
- Application Protocol Coverage Breadth: after consolidation through the normalization table, each canonical protocol's detection quality is computed using the diminishing-returns function `ln(1+n)`. The platform's total quality across all protocols is divided by the sum of per-protocol maximum quality across all platforms.
- Valid Unique Protocol: after consolidation through the normalization table, a canonical protocol detected with valid assets by only one participating platform. No minimum asset threshold; even a single valid asset qualifies.

#### 5.2.1 30-Day Unique Asset Count

Unique Asset Count uses **1 billion** unique network service assets as its benchmark denominator, set with reference to the relevant order of magnitude.

```text
30-Day Unique Asset Count Score
= min(Platform 30-Day Unique Asset Count ÷ 1,000,000,000 × 100, 100)
```

#### 5.2.2 Protocol Coverage Breadth

Let:

- P be the set of all participating platforms;
- C be the set of canonical application protocols after normalization;
- n(p,i) be the number of valid unique assets detected by platform p for canonical protocol i;
- Unique assets are deduplicated by `IP+Port+Canonical Application Protocol`;
- Per-protocol detection quality uses a diminishing-returns function:

```text
q(p,i) = ln(1 + n(p,i))
```

This function sets no minimum asset threshold: even a protocol with only 1 valid asset receives a small weight, and marginal returns diminish as asset count increases.

For each canonical protocol, take the highest detection quality across all participating platforms:

```text
m(i) = max(q(p,i)), for all p in P
```

Platform protocol coverage quality:

```text
B(p) = Sum of q(p,i), for all i in C
```

Joint protocol coverage quality:

```text
B_ref = Sum of m(i), for all i in C
```

Platform protocol coverage breadth score:

```text
Protocol Coverage Breadth Score = 100 × B(p) ÷ B_ref
```

When B_ref = 0, all platforms receive 0 for this result.

#### 5.2.3 Valid Unique Protocols

If canonical protocol i is detected with valid assets by only platform p, then that protocol is an effective unique detection protocol of platform p.

Define the unique-detection indicator:

```text
I(p,i) = 1, when n(p,i) > 0 and for all r ≠ p, n(r,i) = 0
I(p,i) = 0, otherwise
```

Platform unique-detection quality:

```text
U(p) = Sum of I(p,i) × q(p,i), for all i in C
```

Denominator for unique-detection quality across all participating platforms:

```text
U_ref = Sum of U(p), for all p in P
```

Platform effective unique detection protocol score:

```text
Valid Unique Protocol Score = 100 × U(p) ÷ U_ref
```

When U_ref = 0, all platforms receive 0 for this result. If all valid unique protocols belong to a single platform, that platform receives 100 for this result.

Even if another platform detects only 1 valid asset, the protocol is no longer a unique detection protocol.

### 5.3 Deep Protocol Recognition (Sampled)

This item evaluates the ability of participating platforms to acquire protocol field information after identifying the same application protocol. Asset discovery and protocol identification are scored by the other results in this chapter and are not scored again here.

This item is called "sampled" evaluation because it uses representative samples rather than a full census at three levels: at the protocol level, 10 typical non-Web protocols are selected and do not represent all application protocols; at the target level, common targets shared by all participating platforms (the intersection of targets actually detected by each platform) are used and do not represent all assets of that protocol; at the field level, a predefined set of high-value fields is used and does not represent all fields of the protocol. The sampling scope is frozen before testing begins.

#### 5.3.1 Samples

This item evaluates 10 application protocols across three categories of representative non-Web services:

| Category | Protocols |
| --- | --- |
| Remote Access | RDP, SSH, Telnet |
| Databases and Middleware | MySQL, MongoDB, Redis |
| Email, Network Management, and IoT | FTP, SMTP, SNMP, MQTT |

Each protocol is scored using common targets shared by all participating platforms. A common target is matched by `Protocol+IP+Port`; every platform must return a reviewable response, and the actual service must match the protocol under test.

Sampling process:

1. Generate candidate targets for each of the 10 protocols and retain target IP, port, transport, protocol label, and observation time.
2. Independently validate that the service is live and that each platform's response belongs to the same target protocol.
3. Retain only common targets for which every participating platform has a reviewable response. If any platform's response is unavailable, that target is excluded from the protocol average for every platform.
4. If multiple records exist for the same target, select records with the closest observation times.
5. All 10 protocols are equally weighted.

#### 5.3.2 Field Weights and Acquisition Coefficients

Each protocol is independently allocated 100 points distributed across its fields according to each field's contribution to asset mapping, vulnerability discovery, and attack-surface management. The complete field catalogue and weights are published in the evaluation report.

##### Acquisition Coefficient

| Coefficient | Condition |
| ---: | --- |
| 1.0 | Structured or readable protocol text |
| 0.5 | Decodable but unparsed binary, or a deterministic derived field reproducible using a fixed algorithm |
| 0 | No valid value, or a subjective inference that cannot be reproduced |

Direct, formatted, and derived representations of the same semantic information are counted once and are not accumulated.

##### High-Value Fields

High-value fields are protocol fields that directly contribute to security asset mapping, categorized by purpose:

- Asset identity and unique identification (e.g., host-key fingerprint, device model)
- Product and version information (e.g., server software version, firmware version)
- Security configuration and exposure status (e.g., authentication status, anonymous access, TLS parameters)
- Device role and functional capabilities (e.g., node role, supported methods, function codes)

Field scores are concentrated by the contribution criteria above; a small number of key fields may carry high scores. Scores are not set based on any participating platform's return rate or collection difficulty.

##### Scoring Formula

```text
Per-Target Score = Sum of (Field Score × Field Acquisition Coefficient)
```

Each protocol's field scores total 100; the per-target maximum remains 100. Missing-field scores are not redistributed to other fields.

```text
Deep Recognition Score for a Protocol
= Sum of Per-Target Scores for All Valid Common Targets
÷ Number of Valid Common Targets for the Protocol
```

If a platform returns only the protocol name, port, or generic asset labels without any valid protocol content required by any field, that target receives 0.

#### 5.3.3 Aggregate Scoring

```text
Deep Protocol Recognition (Sampled) Score
= Sum of Deep Recognition Scores for All Scorable Protocols
÷ Number of Scorable Protocols
```

When the number of scorable protocols is less than 10, only protocols with valid common targets are equally averaged. Protocols without valid common targets are marked N/A and do not participate in the average.

### 5.4 Mixed Sample

The mixed sample uses `IP+Port+Application Protocol` as its unique key. The evaluator draws 50 candidate assets from the default-scope results of each participating platform. Samples must cover different geographic regions and different application protocols, with priority given to covering different ports and network segments.

Before testing begins, every candidate asset must be independently verified as a real, live service, and its actual application protocol must be confirmed.

The formal sample is the union of candidate results from all participating platforms. The target formal sample size is the number of participating platforms × 50. If fewer unique records remain after consolidation and deduplication, candidates are added equally from each source until the target is reached; if more remain, the sample is not truncated. A platform receives one hit for each matched record and 0 for each missed record.

### 5.5 Mixed-Sample Scoring

**Asset Coverage Rate**

```text
Mixed-Sample Asset Coverage Rate Score
= Correctly Matched Unique Assets
÷ Formal Sample Size
× 100
```

The IP, port, and application protocol must all match.

**Port Coverage Rate**

First calculate the asset hit rate for each port in the sample, then average the rates across all sample ports. Every port has equal weight so common ports such as 80 and 443 do not dominate the result.

```text
Hit Rate for a Port
= Samples on That Port Matched by the Platform
÷ Total Formal Samples on That Port
× 100
```

```text
Mixed-Sample Port Coverage Rate Score
= Sum of Hit Rates for All Sample Ports
÷ Number of Unique Ports in the Formal Sample
```

**Application Protocol Recognition Rate**

First calculate the correct recognition rate for each application protocol in the sample, then average the rates across all sample protocols. Every application protocol has equal weight.

```text
Recognition Rate for an Application Protocol
= Samples of That Protocol Correctly Identified by the Platform
÷ Total Formal Samples of That Protocol
× 100
```

```text
Mixed-Sample Application Protocol Recognition Rate Score
= Sum of Recognition Rates for All Sample Protocols
÷ Number of Application Protocols in the Formal Sample
```

**Sample Coverage and Recognition Score**

The three metrics are weighted at 40%:30%:30%:

```text
Sample Coverage and Recognition Score
= Mixed-Sample Asset Coverage Rate Score × 40%
+ Mixed-Sample Port Coverage Rate Score × 30%
+ Mixed-Sample Application Protocol Recognition Rate Score × 30%
```

### 5.6 Results Display

This chapter publishes five independent scores.

## 6 Data Freshness Evaluation

This chapter uses the union of samples from all participating platforms to compare asset coverage and last-update time. If a platform does not contain a sample asset, that sample receives 0.

### 6.1 Samples

This chapter uses the same formal sample union as Chapter 5 (see 5.4 for sample composition and sampling rules). The formal sample as a whole must cover different network segments, ports, and application protocols, and must not be concentrated in a small number of network segments, common ports, or a single protocol.

### 6.2 Scoring Method

Each sample uses the last-update time returned externally by each platform. If the platform misses the asset, omits the time field, or provides a time field whose meaning cannot be confirmed, that sample receives 0.

Each sample is scored according to the number of days between its last-update time and the test time:

| Last Update Time | Per-Sample Score |
| --- | ---: |
| Within the past 7 days | 100 |
| 8–30 days | 80 |
| 31–90 days | 50 |
| 91–365 days | 20 |
| More than 365 days | 0 |
| Time field missing or meaning cannot be confirmed | 0 |

```text
Data Freshness Score
= Sum of Scores for All Formal Samples
÷ Formal Sample Size
```

## 7 Search and Retrieval Performance Evaluation

This chapter evaluates the response performance of each participating platform in page-search and API-call scenarios.

### 7.1 Scoring Items

| Independent Result | Max Score | Test Method |
| --- | ---: | --- |
| Page Query Performance | 100 | Contains four sub-scores, each at 25%, see table below |
| Single API Query Performance | 100 | 50 fixed API requests |
| Continuous API Pagination Performance | 100 | Retrieve 50 consecutive pages at one page per second and calculate per-page P95 response time |

Page Query Performance consists of the following four sub-scores, each weighted at 25%:

| Sub-Item | Weight | Test Method |
| --- | ---: | --- |
| Full-Text Search | 25% | 40 fixed page tasks |
| Simple Query | 25% | 40 single-field exact-match or filter page tasks |
| Complex Query | 25% | 40 page tasks using AND, OR, NOT, and parenthesized combinations |
| Extreme Query | 25% | 10 fuzzy, regular-expression, or deeply nested page tasks |

Execution rules:

- Web query time is measured from query submission until the complete result is returned. API time is measured from request submission until the complete response is received.
- A page or single-API task receives 0 if it times out, returns an error, cannot be expressed in the platform's syntax, or executes a non-equivalent query.
- Continuous pagination requires successful completion of 50 pages. If it is interrupted or reaches a platform limit, the task receives 0.
- Each task is scored independently before the average for its task category is calculated. The four sub-scores of Page Query Performance are combined at equal 25% weight.
- A missing item caused by evaluator non-collection or undelivered evidence must not be treated as platform failure. The ranking for that result table is not published until the missing item is retested.

### 7.2 Response-Time Bands

Each band includes its lower bound and excludes its upper bound; a value exactly on a boundary belongs to the band whose lower bound it equals.

Full-text, simple, complex, and extreme queries all use the following page-search response-time bands:

| Page Search Response Time | Score |
| --- | ---: |
| No more than 100 ms | 100 |
| 101–300 ms | 95 |
| 301–500 ms | 90 |
| 501–800 ms | 85 |
| 801 ms–1.2 s | 80 |
| 1.2–1.8 s | 70 |
| 1.8–2.5 s | 60 |
| 2.5–3.5 s | 45 |
| 3.5–5 s | 30 |
| 5–8 s | 15 |
| More than 8 s | 5 |
| Timeout, failure, or non-equivalent query | 0 |

Single API response-time bands:

| Single API Response Time | Score |
| --- | ---: |
| No more than 5 ms | 100 |
| 6–15 ms | 95 |
| 16–30 ms | 90 |
| 31–50 ms | 85 |
| 51–100 ms | 75 |
| 101–200 ms | 65 |
| 201–500 ms | 50 |
| 501 ms–1 s | 35 |
| 1–2 s | 20 |
| 2–5 s | 10 |
| More than 5 s | 5 |
| Failure or non-equivalent request | 0 |

Continuous API pagination P95 response-time bands:

| Continuous API Pagination P95 Response Time | Score |
| --- | ---: |
| No more than 20 ms | 100 |
| 21–50 ms | 95 |
| 51–100 ms | 90 |
| 101–200 ms | 85 |
| 201–500 ms | 80 |
| 501 ms–1 s | 75 |
| 1–2 s | 70 |
| 2–3 s | 60 |
| 3–5 s | 40 |
| More than 5 s | 20 |
| Fewer than 50 pages completed | 0 |

### 7.3 Scoring Method

Page-search and single-API tasks receive the score for their measured duration band:

```text
Page or Single-API Task Score
= Fixed Band Score Corresponding to the Task's Measured Response Time
```

The task average is then calculated for each metric category:

```text
Performance Score for a Task Category
= Sum of Scores for All Tasks in the Category
÷ Total Number of Tasks in the Category
```

Page Query Performance is combined from four sub-scores at equal 25% weight:

```text
Page Query Performance Score
= Full-Text Search Score × 25%
+ Simple Query Score × 25%
+ Complex Query Score × 25%
+ Extreme Query Score × 25%
```

API continuous pagination retrieves 50 consecutive pages at one page per second. After completion, the P95 per-page response time is taken and scored using the fixed bands. An interrupted task, a platform-limit stop, or failure to complete all 50 pages receives 0.

```text
Continuous API Pagination P95 Response Time Score
= Fixed Band Score Corresponding to the Task's P95 Response Time
```

### 7.4 Results Display

This chapter publishes three independent scores.

## 8 DNS Discovery Capability Evaluation

The evaluator independently provides 150 fixed base domains as test entry points. Base domains are selected from publicly known organizations, including listed companies and large internet services.

Each participating platform queries the same 150 base domains. Each platform must export the complete subdomain results for all 150 base domains. The results are consolidated into that platform's candidate subdomain set and then scored under the unified rules.

A valid subdomain must satisfy all of the following conditions:

1. It is unique after FQDN normalization and deduplication;
2. It resolves to a public IP address at the time of testing;
3. It passes the unified wildcard-detection and folding rules.

If the same subdomain has multiple IP addresses, ports, or services, it is counted only once in Valid Subdomain Count.

Domain-Port Coverage Count and Associated Unique IP Count are calculated independently and do not depend on the valid-subdomain set: as long as a record appears in the platform's complete result and passes normalization and deduplication, it enters its corresponding metric.

### 8.1 Scoring Items

| Metric | Weight | Calculation Method |
| --- | ---: | --- |
| Valid Subdomain Count | 40% | Unique FQDNs retained after normalization, deduplication, validity verification, and wildcard cleanup of the complete candidate result |
| Domain-Port Coverage Count | 25% | Unique normalized and deduplicated `FQDN+Port` combinations in the platform's complete result |
| Associated Unique IP Count | 15% | Unique public IP addresses after normalization and deduplication of the platform's complete result |
| Unique Valid Subdomain Count | 20% | Valid subdomains found only by that platform and by none of the other platforms that completed the item |

For the fixed base-domain sample, Candidate Subdomain Count is calculated in this order:

1. Obtain the platform's complete results for the 150 base domains;
2. Convert FQDNs to lowercase, remove trailing dots, and filter out records that do not belong to the fixed base domains;
3. Deduplicate by FQDN to form the candidate subdomain set;
4. Perform public-resolution verification and unified wildcard identification and folding to form the valid-subdomain set.

Domain-Port Coverage Count uses the complete result after Step 2 and deduplicates by `FQDN+Port`. Associated Unique IP Count uses public IP addresses in the same complete result, removes private, reserved, and malformed addresses, and then deduplicates them. Both metrics use the platform's complete result as their scope and are not filtered by the valid-subdomain set first.

Query syntax, result-object meaning, validity rules, wildcard rules, and normalization methods for domains, ports, and IP addresses are all frozen before testing begins.

The denominator for each of the four metrics is the deduplicated union total for that metric across all participating platforms (for Unique Valid Subdomain Count, the simple sum across platforms). A platform therefore does not receive a full score unless it independently covers the complete union of subdomains, ports, or IP addresses. The score reflects each platform's coverage of the asset space represented by the sampled domains.

### 8.2 Scoring Method

The four metrics are calculated separately:

```text
Valid Subdomain Count Score
= Platform Valid Subdomain Count
÷ Deduplicated Union of Valid Subdomains Across All Participating Platforms
× 100
```

```text
Domain-Port Coverage Count Score
= Platform Unique FQDN+Port Count
÷ Deduplicated Union of Unique FQDN+Port Combinations Across All Participating Platforms
× 100
```

```text
Associated Unique IP Count Score
= Platform Associated Unique Public IP Count
÷ Deduplicated Union of Associated Unique Public IP Addresses Across All Participating Platforms
× 100
```

```text
Unique Valid Subdomain Count Score
= Platform Unique Valid Subdomain Count
÷ Sum of Unique Valid Subdomain Counts Across All Participating Platforms
× 100
```

```text
DNS Discovery Capability Score
= Valid Subdomain Count Score × 40%
+ Domain-Port Coverage Count Score × 25%
+ Associated Unique IP Count Score × 15%
+ Unique Valid Subdomain Count Score × 20%
```

### 8.3 Results Display

This chapter publishes the DNS Discovery Capability Score.

---

## Disclaimer

The participating platforms referenced by this evaluation standard are third-party cyberspace mapping platforms. Their data scope, query syntax, account permissions, and platform limitations may change over time. This standard reflects platform capabilities only at the time of evaluation and does not constitute a judgment about any platform's long-term capabilities.

Evaluation results are affected by account tier, test time, network environment, and other conditions. Repeating the evaluation at a different time or under different conditions may produce different results. The scoring method and weighting framework in this standard reflect one interpretation of the core capabilities of cyberspace mapping platforms and are not the only reasonable evaluation approach.

This standard is provided for reference only. No responsibility is assumed for the data accuracy or service availability of any evaluated platform, or for the consequences of business decisions based on the evaluation. Any business decision using results produced under this standard should be supported by independent validation against the decision maker's actual requirements.
