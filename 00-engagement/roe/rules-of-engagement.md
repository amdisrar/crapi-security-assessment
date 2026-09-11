# crAPI Application Security Assessment - Rules of Engagement

## Important Info

> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

## Document Control

| Field | Details |
|---|---|
| Document Title | crAPI Application Security Assessment – Rules of Engagement |
| Document Type | Rules of Engagement |
| Engagement Type | White-Box Application and API Security Assessment |
| Application | crAPI |
| Environment | UAT |
| Assessment Lead | Mr. Wario – Application Security Expert |
| Client Representative | Mr. Mario |
| Document Owner | Application Security Team |
| Version | 1.0 |
| Classification | Training / Public |
| Status | Approved for Engagement Use |
| Assessment Start | ASAP |
| Planned Testing Duration | 1 Week |
| Allowed Testing Hours | 24 Hours |
| Reporting Period | 1 Week |
| Retest Period | 2 Weeks |
| Last Updated | 11 September 2026 |

### Document Revision History

| Version | Date | Author | Change Description |
|---|---|---|---|
| 0.1 | 11-Sep-2026 | Mr. Wario | Initial Rules of Engagement draft |
| 1.0 | 11-Sep-2026 | Mr. Wario | Finalized Rules of Engagement for assessment use |

## Table of Contents

1. [Purpose and Relationship to Other Documents](#1-purpose-and-relationship-to-other-documents)
2. [Roles, Responsibilities and Testing Authority](#2-roles-responsibilities-and-testing-authority)
3. [Approved Environment and Testing Window](#3-approved-environment-and-testing-window)
4. [Permitted and Prohibited Activities](#4-permitted-and-prohibited-activities)
5. [Indicative Toolset](#5-indicative-toolset)
6. [Authentication, Authorization and API Testing Rules](#6-authentication-authorization-and-api-testing-rules)
7. [Exploitation and Proof-of-Concept Rules](#7-exploitation-and-proof-of-concept-rules)
8. [Special Testing Areas](#8-special-testing-areas)
9. [Service Changes and Operational Impact](#9-service-changes-and-operational-impact)
10. [Third-Party and External Systems](#10-third-party-and-external-systems)
11. [Evidence, Logging and Documentation](#11-evidence-logging-and-documentation)
12. [Finding Escalation and Communication](#12-finding-escalation-and-communication)
13. [Emergency Stop and Scope Change Management](#13-emergency-stop-and-scope-change-management)
14. [Retest Rules](#14-retest-rules)
15. [Rules of Engagement Approval](#15-rules-of-engagement-approval)

# 1. Purpose and Relationship to Other Documents

This document defines the operational rules under which the **crAPI Application Security Assessment** will be performed.

The Scope document defines **what may be tested**. These Rules of Engagement define **how the approved testing may be performed**.

The objectives of this document are to:

- protect the stability of the UAT environment;
- ensure testing remains within client-approved boundaries;
- define permitted and prohibited testing activities;
- establish rules for exploitation and proof-of-concept activities;
- define how security findings are communicated;
- establish escalation and emergency-stop procedures;
- protect assessment evidence and test data;
- provide clear operational authority for Mr. Wario.

This document should be read together with the following engagement documents:

- Pre-Engagement Questions and Answers;
- Scope of Security Assessment;
- Authorization to Test;
- Non-Disclosure Agreement;
- Evidence Handling Plan;
- final assessment and retest documentation.

Where a conflict exists between engagement documents, the affected activity should be clarified with Mr. Mario before testing proceeds.

# 2. Roles, Responsibilities and Testing Authority

## 2.1 Client Representative

**Mr. Mario** is the client representative and primary contact for the engagement.

Mr. Mario is responsible for:

- reviewing and approving the Scope and Rules of Engagement;
- approving material scope changes;
- acting as the primary technical and emergency contact;
- receiving High and Critical findings;
- coordinating client-side support where required;
- providing or approving access required for assessment activities.

## 2.2 Security Assessment Lead

**Mr. Wario – Application Security Expert** is responsible for conducting the assessment professionally and in accordance with the approved engagement documents.

Mr. Wario is responsible for:

- remaining within the approved scope;
- minimizing unnecessary operational impact;
- validating security findings before reporting them;
- documenting meaningful assessment activity;
- protecting assessment evidence;
- escalating significant findings;
- stopping or modifying testing where continued activity presents operational risk.

## 2.3 Testing Authority

Mr. Wario is authorized to perform security testing against systems and components that form part of the approved **crAPI UAT environment**.

Because client documentation is incomplete, reasonable technical discovery is also authorized to identify application-owned components such as undocumented endpoints, legacy APIs, internal services, authentication mechanisms, Docker services and administrative interfaces.

Client uncertainty does not automatically exclude a component from assessment if the component is clearly part of the approved crAPI UAT environment.

Testing must not intentionally extend to unrelated corporate infrastructure, production systems or unrelated third-party systems without additional authorization.

# 3. Approved Environment and Testing Window

The approved assessment environment is:

| Item | Approved Value |
|---|---|
| Application | crAPI |
| Environment | UAT |
| Primary URL | `http://127.0.0.1:8888` |
| Primary Port | TCP/8888 |
| Architecture | API-driven microservices |
| Deployment | Docker |
| Reverse Proxy | OpenResty |
| API Type | REST |
| Databases | PostgreSQL and MongoDB |

Additional services may be assessed where they are clearly part of the same crAPI UAT environment and are discovered through authorized reconnaissance.

### Testing Window

| Requirement | Value |
|---|---|
| Start | ASAP |
| Testing Duration | 1 Week |
| Allowed Testing Hours | 24 Hours |
| Maintenance Window Restrictions | None specified |
| Blackout Periods | None specified |
| Reporting Period | 1 Week |
| Retest Period | 2 Weeks |

Testing is permitted at any time during the approved assessment period. However, potentially disruptive actions should still be minimized and performed only where justified.

# 4. Permitted and Prohibited Activities

The following activities are approved within the crAPI UAT environment.

| Activity | Status |
|---|---|
| Application reconnaissance | Permitted |
| Port and service enumeration | Permitted |
| Technology fingerprinting | Permitted |
| API endpoint and directory enumeration | Permitted |
| API fuzzing | Permitted |
| Automated vulnerability scanning | Permitted |
| Authentication testing | Permitted |
| Authorization testing | Permitted |
| BOLA / BOPLA / BFLA testing | Permitted |
| Password attacks against authorized test accounts | Permitted |
| Injection testing | Permitted |
| SSRF testing | Permitted |
| File upload testing | Permitted |
| Business-logic testing | Permitted |
| Privilege escalation | Permitted |
| Controlled exploitation | Permitted |
| Limited proof-of-concept data extraction | Permitted |
| Test account creation/deletion | Permitted |
| Modification of test data | Permitted |
| Database inspection | Permitted |
| Container shell access | Permitted |
| Docker/configuration review | Permitted |
| Controlled service restart | Permitted |
| Source-code access to support testing | Permitted |
| Active rate-limit exhaustion testing | Prohibited |
| Stress testing | Prohibited |
| Load testing | Prohibited |
| DoS / DDoS testing | Prohibited |
| Destructive database actions | Prohibited |
| Uncontrolled persistence | Prohibited |
| Malware deployment | Prohibited |
| Formal full source-code audit | Prohibited |
| Testing unrelated systems | Prohibited |

A permitted activity does not override the operational safety requirements defined elsewhere in this document.

# 5. Indicative Toolset

The following tool categories may be used during the assessment.

The list is **illustrative and non-exhaustive**. Equivalent tools may be used where the underlying testing activity is already authorized. A change of tool does not itself constitute a change of scope.

| Category | Example Tools |
|---|---|
| Web/API interception | Burp Suite, OWASP ZAP, browser developer tools |
| API clients | curl, Postman, Insomnia |
| Network/service discovery | Nmap, netcat, OpenSSL client tools |
| Endpoint discovery/fuzzing | ffuf, Gobuster, dirsearch, Burp Intruder |
| Vulnerability scanning | Nuclei, Burp Scanner where available |
| Injection validation | Burp Suite, sqlmap where appropriate and safely configured |
| Container/configuration review | Docker CLI, Docker Compose, Linux utilities |
| Scripting/automation | Python, Bash, JavaScript |

Automated tool output must be manually validated before being treated as a confirmed vulnerability.

Tool configuration must remain consistent with the approved testing restrictions. For example, fuzzing or scanning must not be configured in a way that becomes stress, load or denial-of-service testing.

# 6. Authentication, Authorization and API Testing Rules

Authentication testing may include:

- user registration;
- login and logout;
- password reset and account recovery;
- credential handling;
- session and cookie handling;
- JWT or other token analysis;
- token lifetime, validation and invalidation;
- account enumeration;
- password-policy assessment.

Password attacks are permitted only against accounts created for assessment purposes, accounts supplied specifically for testing, or other accounts explicitly approved by Mr. Mario.

Brute-force activity must remain controlled and must not intentionally cause account or service instability.

Authorization testing may include:

- changing object identifiers;
- cross-user object access;
- cross-user modification where safely applicable;
- restricted-property manipulation;
- lower-role access to privileged functions;
- BOLA, BOPLA and BFLA testing.

API testing may include:

- endpoint discovery;
- HTTP method testing;
- parameter and JSON-body manipulation;
- header manipulation;
- API version discovery;
- legacy/internal API discovery;
- documentation endpoint discovery;
- controlled fuzzing.

If fuzzing causes significant performance degradation or instability, Mr. Wario must reduce the request rate or stop the activity.

# 7. Exploitation and Proof-of-Concept Rules

Confirmed or suspected vulnerabilities may be exploited where exploitation is necessary to establish security impact.

The governing principle is:

> **Use the minimum impact necessary to prove the vulnerability.**

Examples include:

- demonstrating access to one unauthorized record rather than extracting all records;
- demonstrating one privilege-boundary bypass rather than performing unnecessary administrative activity;
- proving injection using limited output rather than dumping an entire database;
- proving file-upload risk with a benign test file where practical.

Mr. Wario should stop exploitation once sufficient evidence exists to demonstrate the vulnerability and its impact.

Proof-of-concept data extraction is permitted, but only the minimum amount of data necessary should be collected. Test or synthetic data should be preferred wherever possible.

Database inspection is permitted for validation purposes. Destructive database actions such as dropping databases, dropping tables, mass deletion or uncontrolled corruption are prohibited.

Unexpected discovery of real customer data should result in minimizing further access to that dataset and informing Mr. Mario where appropriate.

# 8. Special Testing Areas

## File Upload Testing

File upload testing may evaluate:

- extension validation;
- MIME/content-type validation;
- filename handling;
- storage behavior;
- access controls;
- rendering or execution behavior.

Benign proof files should be used wherever practical.

## SSRF and Server-Side Requests

SSRF testing may evaluate access to loopback, approved internal services and other in-scope lab services.

SSRF must not be used to intentionally interact with unrelated third-party systems.

## Docker and Configuration Review

Mr. Wario may inspect:

- running containers and images;
- Docker networks;
- environment variables;
- mounted files;
- exposed ports;
- runtime privileges;
- service relationships;
- relevant application and reverse-proxy configuration.

## Source-Code Access

Source-code access is permitted where needed to understand application behavior, validate a specific finding or locate relevant configuration.

A systematic line-by-line source-code security audit is not part of the engagement.

# 9. Service Changes and Operational Impact

Testing must be performed in a way that minimizes unnecessary impact on the UAT environment.

Mr. Wario should avoid unnecessary:

- service disruption;
- resource exhaustion;
- large-scale extraction;
- destructive modification;
- uncontrolled persistence.

Service restarts are permitted, but they should not be treated as routine test activity.

A restart may be performed where reasonably required to:

- recover the assessment environment;
- validate configuration;
- restore a failed component;
- support an approved test.

Where practical, Mr. Mario should be informed before a restart that could affect other users.

If an automated or manual test begins causing unexpected instability, the affected activity must be reduced, modified or stopped.

# 10. Third-Party and External Systems

Direct security testing of unrelated third-party infrastructure is not authorized unless separate approval is obtained.

Where crAPI communicates with an external service, Mr. Wario may assess the security of crAPI's interaction with that service, including:

- data sent by crAPI;
- how crAPI handles external responses;
- redirects and callback behavior;
- user-controlled URLs;
- trust assumptions and input validation.

Testing should remain focused on the security behavior of crAPI rather than the external provider itself.

Newly discovered external hosts or services must not automatically be tested merely because crAPI references them.

# 11. Evidence, Logging and Documentation

Assessment evidence may include:

- HTTP requests and responses;
- screenshots;
- logs;
- application output;
- token samples;
- configuration excerpts;
- database results;
- Docker/container output;
- proof-of-concept scripts.

Evidence should be:

- relevant;
- reproducible;
- minimal;
- clearly labeled;
- linked to the relevant test case or finding.

Sensitive evidence should be sanitized before being published in this public training repository where necessary.

Important assessment activities should be documented sufficiently to maintain traceability between:

**reconnaissance → testing → evidence → finding → report → retest**

Records should include, where useful:

- date/time;
- target endpoint or component;
- test performed;
- tool used;
- relevant account;
- result;
- evidence reference;
- related issue or test-case ID.

Automated scanner output alone is not sufficient to classify a vulnerability as confirmed. Findings should be manually validated where practical.

# 12. Finding Escalation and Communication

Mr. Mario is the primary client contact for the engagement.

Any confirmed **High or Critical severity finding must be communicated to Mr. Mario as soon as reasonably possible after validation**, without waiting for the final assessment report.

The initial notification should include, where possible:

- finding title;
- severity;
- affected component;
- concise description;
- likely impact;
- immediate mitigation recommendation where appropriate.

Medium, Low and Informational findings may normally be consolidated into the regular reporting process unless circumstances justify earlier communication.

Mr. Wario should also communicate promptly where there is:

- unexpected application instability;
- risk of data loss;
- evidence that testing is affecting unrelated systems;
- scope ambiguity that prevents safe continuation;
- a need for significant operational intervention.

# 13. Emergency Stop and Scope Change Management

Mr. Wario has authority to pause, reduce or modify testing if continued activity may create unacceptable operational risk.

Testing should be paused where:

- significant instability is observed;
- data-loss risk becomes apparent;
- testing begins affecting systems outside the approved environment;
- a test causes unexpected operational impact;
- continued exploitation is no longer necessary to prove the issue.

If an emergency condition occurs, Mr. Wario should:

1. stop the affected activity;
2. preserve relevant evidence;
3. document the action that preceded the issue;
4. notify Mr. Mario;
5. wait for guidance where continued testing may increase impact.

Mr. Mario is the authorized approver for material scope changes.

Additional approval is required before testing:

- production environments;
- unrelated corporate systems;
- unrelated third-party systems;
- newly discovered external infrastructure not clearly part of crAPI UAT.

Where a newly discovered component is clearly part of the approved crAPI UAT environment, reasonable assessment may continue where consistent with the approved Scope.

Material scope changes should be documented.

# 14. Retest Rules

Retesting will be performed during the approved two-week retest period.

Retesting should focus on vulnerabilities the client states have been remediated.

Each finding may be classified as:

- Fixed;
- Partially Fixed;
- Not Fixed;
- Unable to Retest.

Mr. Wario may also perform reasonable checks for obvious bypasses related to the implemented remediation.

Retesting does not automatically constitute a complete new assessment of the application.

# 15. Rules of Engagement Approval

Approval of this document confirms that Mr. Mario understands and accepts the operational rules defined for the **crAPI Application Security Assessment**.

Approval confirms that Mr. Wario may perform the activities defined in this document within the boundaries established by the approved Scope and Authorization to Test.

| Role | Name | Approval | Date |
|---|---|---|---|
| Client Representative | Mr. Mario | __________________ | __________ |
| Application Security Expert | Mr. Wario | __________________ | __________ |
