# crAPI Application Security Assessment
## Important Info

> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

## Document Control

| Field | Details |
|---|---|
| Document Title | crAPI Application Security Assessment – Scope |
| Document Type | Security Assessment Scope |
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
| Reporting Period | 1 Week |
| Retest Period | 2 Weeks |
| Last Updated | 11 September 2026 |

### Document Revision History

| Version | Date | Author | Change Description |
|---|---|---|---|
| 0.1 | 11-Sep-2026 | Mr. Wario | Initial scope draft based on pre-engagement questionnaire |
| 1.0 | 11-Sep-2026 | Mr. Wario | Finalized scope for engagement use |

## Table of Contents

1. [Purpose](#1-purpose)
2. [Engagement Background](#2-engagement-background)
3. [Assessment Objectives](#3-assessment-objectives)
4. [Scope Basis](#4-scope-basis)
5. [Assessment Approach](#5-assessment-approach)
6. [In-Scope Environment](#6-in-scope-environment)
7. [In-Scope Application Components](#7-in-scope-application-components)
8. [Discovery of Undocumented Components](#8-discovery-of-undocumented-components)
9. [In-Scope Security Testing Areas](#9-in-scope-security-testing-areas)
10. [Authentication and Authorization Assessment](#10-authentication-and-authorization-assessment)
11. [API Security Assessment](#11-api-security-assessment)
12. [Infrastructure and Configuration Assessment](#12-infrastructure-and-configuration-assessment)
13. [Data Security Assessment](#13-data-security-assessment)
14. [Third-Party and External Dependencies](#14-third-party-and-external-dependencies)
15. [Source Code and Configuration Access](#15-source-code-and-configuration-access)
16. [Out-of-Scope Activities](#16-out-of-scope-activities)
17. [Testing Constraints](#17-testing-constraints)
18. [Scope Handling for Incomplete Client Information](#18-scope-handling-for-incomplete-client-information)
19. [Scope Validation During Reconnaissance](#19-scope-validation-during-reconnaissance)
20. [Assessment Phases](#20-assessment-phases)
21. [Evidence and Finding Validation](#21-evidence-and-finding-validation)
22. [Deliverables](#22-deliverables)
23. [Assessment Limitations](#23-assessment-limitations)
24. [Scope Change Management](#24-scope-change-management)
25. [Contacts and Escalation](#25-contacts-and-escalation)
26. [Completion Criteria](#26-completion-criteria)
27. [Scope Approval](#27-scope-approval)

# 1. Purpose

This document defines the scope of the **crAPI Application Security Assessment** to be performed against the approved UAT environment.

The purpose of this document is to establish a clear understanding between the client and the Application Security Team regarding:

- systems and application components included in the assessment;
- security testing activities to be performed;
- testing limitations and prohibited activities;
- treatment of undocumented or unknown application components;
- evidence and reporting expectations;
- responsibilities of the client and assessment team.

This document should be read together with the associated **Rules of Engagement**, **Authorization to Test**, **Non-Disclosure Agreement**, and **Pre-Engagement Questionnaire**.

# 2. Engagement Background

Mr. Mario requested Mr. Wario, the Application Security Expert, to perform a security assessment of an application named **crAPI**.

The application is currently deployed in a **User Acceptance Testing (UAT)** environment and is intended to provide services to vehicle owners.

According to the information provided during the pre-engagement process, crAPI is an API-driven, microservices-based web application that processes vehicle-related and customer information, including Personally Identifiable Information (PII).

The client requested a comprehensive assessment before further deployment or production use.

During initial engagement discussions, it became clear that complete technical documentation was not available. Mr. Mario is relatively new to the environment and could not provide definitive answers for several areas, including API versions, legacy APIs, internal APIs, authentication mechanisms, token and session handling, administrative interfaces, third-party integrations, Docker host architecture, and account lockout or rate-limiting controls.

The absence of complete documentation is therefore treated as an **assessment condition**, not as a reason to exclude these areas from testing. Where appropriate, Mr. Wario will identify and validate relevant components through authorized reconnaissance and application analysis.

# 3. Assessment Objectives

The primary objective of the crAPI Application Security Assessment is to identify security weaknesses that could affect the confidentiality, integrity, or availability of the application and its associated data.

The assessment will evaluate whether an attacker or unauthorized user may be able to:

- gain unauthorized access to user accounts;
- access data belonging to another user;
- modify data or properties outside authorized permissions;
- invoke privileged or administrative functions;
- abuse application workflows;
- exploit insecure API endpoints;
- manipulate application inputs;
- interact with backend systems in unintended ways;
- exploit insecure server-side request functionality;
- upload or access unsafe files;
- obtain sensitive information through application responses;
- exploit insecure application or container configuration;
- abuse undocumented, legacy, or internal APIs;
- exploit insecure trust between internal or external services.

The assessment will also provide remediation guidance and sufficient technical evidence to allow development and infrastructure teams to reproduce and resolve confirmed findings.

# 4. Scope Basis

The scope is based on:

1. information provided by Mr. Mario through the pre-engagement questionnaire;
2. known crAPI deployment information;
3. the assessment tasks defined in the project plan and GitHub issues;
4. the expected UAT architecture;
5. security-relevant components discovered during authorized reconnaissance.

Client-provided information is treated as an important input but is not assumed to be technically complete or fully accurate.

Where the client is uncertain about implementation details, Mr. Wario is authorized to perform reasonable discovery activities within the approved UAT environment to establish the actual application architecture and attack surface.

# 5. Assessment Approach

The engagement will be performed as a **white-box application and API security assessment**.

The assessment may combine:

- application reconnaissance;
- API discovery;
- manual security testing;
- automated vulnerability scanning;
- controlled API fuzzing;
- authentication and authorization testing;
- application workflow analysis;
- configuration review;
- Docker and supporting-service review;
- controlled exploitation of confirmed vulnerabilities;
- manual validation of findings.

Although this is a white-box engagement, complete application documentation is not available. Discovery activities normally associated with grey-box testing may therefore be used to identify undocumented functionality and verify client-provided information.

# 6. In-Scope Environment

| Scope Item | Approved Value |
|---|---|
| Environment | UAT |
| Primary Application | crAPI |
| Primary URL | `http://127.0.0.1:8888` |
| Primary Port | TCP/8888 |
| Application Exposure | Internal |
| Architecture | API-driven microservices |
| Deployment Technology | Docker |
| Reverse Proxy | OpenResty |
| API Type | REST |
| Databases | PostgreSQL and MongoDB |

The application and supporting components operating within the authorized crAPI UAT environment are considered part of the assessment unless specifically excluded elsewhere in this document.

# 7. In-Scope Application Components

The following are in scope where they support crAPI:

### Web Application

The web frontend used by customers to interact with crAPI.

### REST APIs

All REST API endpoints associated with the application, including documented, undocumented, authenticated, unauthenticated, privileged, versioned, legacy, and internal application APIs that are exposed or reachable through the approved environment.

### Microservices

Application microservices supporting crAPI functionality. Client-provided technologies include Java, JavaScript, Node.js, React, Python, and Go. Mr. Wario will verify actual technologies where possible.

### Reverse Proxy and Request Routing

OpenResty and any associated routing, reverse-proxy, or gateway-like functionality supporting the application.

### Databases

PostgreSQL and MongoDB components supporting crAPI. Database assessment will focus on application security impact, access controls, exposure, configuration, sensitive information, and trust assumptions between application services and data stores. Destructive database operations are prohibited.

### Docker Components

Docker containers, Docker networks, runtime settings, and supporting configuration associated with crAPI.

### Supporting Services

Supporting services discovered during the assessment that are clearly part of the crAPI UAT environment may be reviewed where necessary to understand application security. Examples may include email services, health endpoints, metrics services, internal storage, chatbot services, routing components, and other service dependencies.

# 8. Discovery of Undocumented Components

Because complete technical and API documentation is unavailable, the assessment includes discovery of previously undocumented components that form part of the approved crAPI UAT environment.

This may include:

- API endpoints;
- API versions;
- deprecated or legacy endpoints;
- administrative endpoints;
- internal APIs;
- hidden frontend routes;
- debug endpoints;
- health or monitoring endpoints;
- service-to-service interfaces;
- additional listening ports;
- container services;
- third-party integration points.

Discovery alone does not constitute a vulnerability. Discovered components will first be documented as reconnaissance observations and then evaluated for security impact where applicable.

# 9. In-Scope Security Testing Areas

| Testing Area | Status |
|---|---|
| Automated vulnerability scanning | In Scope |
| API endpoint enumeration | In Scope |
| Directory enumeration | In Scope |
| API fuzzing | In Scope |
| Authentication testing | In Scope |
| Authorization testing | In Scope |
| BOLA testing | In Scope |
| BOPLA testing | In Scope |
| BFLA testing | In Scope |
| Injection testing | In Scope |
| SSRF testing | In Scope |
| File upload testing | In Scope |
| Business-logic testing | In Scope |
| Privilege escalation testing | In Scope |
| Security misconfiguration testing | In Scope |
| Docker/configuration review | In Scope |
| Password testing against authorized test accounts | In Scope |
| Controlled exploitation | In Scope |
| Limited data extraction for proof of concept | In Scope |
| Creation of test accounts | In Scope |
| Modification of test data | In Scope |
| Container shell access | In Scope |
| Database inspection | In Scope |
| Controlled service restart where necessary | In Scope |
| Rate-limit testing | Out of Scope |
| Stress/load testing | Out of Scope |
| DoS/DDoS testing | Out of Scope |
| Destructive database testing | Out of Scope |
| Formal source-code review | Out of Scope |

# 10. Authentication and Authorization Assessment

Authentication mechanisms are currently unknown. Mr. Wario will therefore identify and document the authentication implementation during reconnaissance.

Testing may include registration, login, logout, password reset, account recovery, credential handling, session handling, cookies, JWTs or other token mechanisms, token lifetime, validation, invalidation, and authorization enforcement.

The client confirmed that multiple privilege levels exist. The assessment will identify observable authorization boundaries and test access controls between users and roles.

Authorization testing will include:

- **BOLA** – testing whether a user can access or manipulate objects belonging to another user;
- **BOPLA** – testing whether users can read or modify object properties that should be restricted;
- **BFLA** – testing whether lower-privileged users can invoke functions intended for higher-privileged or administrative roles.

Multiple test accounts may be created where necessary to validate authorization boundaries.

# 11. API Security Assessment

Because crAPI is API-driven, API security represents a major part of the engagement.

The assessment will include discovery and analysis of API routes, HTTP methods, parameters, JSON request bodies, headers, object identifiers, authentication requirements, authorization requirements, API versions, undocumented APIs, legacy APIs, internal APIs, administrative APIs, error responses, and API documentation interfaces such as Swagger/OpenAPI where present.

The methodology will consider relevant risks from the **OWASP API Security Top 10**, including:

- Broken Object Level Authorization;
- Broken Authentication;
- Broken Object Property Level Authorization;
- Unrestricted Resource Consumption;
- Broken Function Level Authorization;
- Unrestricted Access to Sensitive Business Flows;
- Server-Side Request Forgery;
- Security Misconfiguration;
- Improper Inventory Management;
- Unsafe Consumption of APIs.

Testing related to resource consumption will remain non-disruptive because active rate-limit, stress, load, and denial-of-service testing are prohibited.

# 12. Infrastructure and Configuration Assessment

The engagement includes review of infrastructure and configuration components directly supporting crAPI, including:

- Docker container configuration;
- exposed ports;
- container privileges;
- Docker networking;
- mounted files;
- environment variables;
- secrets exposure;
- service configuration;
- OpenResty routing;
- unnecessary services;
- administrative interfaces;
- debug interfaces;
- health and metrics endpoints;
- database exposure;
- HTTP security headers;
- CORS configuration;
- verbose errors;
- stack traces;
- unnecessary HTTP methods.

# 13. Data Security Assessment

The client has stated that the UAT environment does not contain real customer data.

Dummy or synthetic data may be used and multiple test accounts may be created.

The assessment may inspect or extract limited data where required to demonstrate a vulnerability. Any proof-of-concept extraction will follow the principle of **minimum necessary evidence**.

Where credentials, tokens, PII, secrets, or other sensitive information are encountered, they will be handled according to the evidence-handling procedure.

# 14. Third-Party and External Dependencies

The client is unsure whether crAPI uses third-party APIs or external integrations. Identifying such dependencies is therefore included within reconnaissance.

Mr. Wario may identify external APIs, email services, webhooks, callbacks, remote resources, authentication providers, chatbot services, and other integrations.

Discovery and analysis of how crAPI interacts with such systems is in scope. Direct security testing of unrelated third-party infrastructure is not automatically authorized and requires explicit approval.

# 15. Source Code and Configuration Access

The client has stated that source-code access is permitted, while formal source-code review is not permitted.

This will be interpreted as follows:

Mr. Wario may access source code where necessary to support authorized testing, understand application behavior, identify configuration references, or validate a specific security observation.

A systematic source-code security review, secure-code audit, or line-by-line static analysis is not part of this engagement.

Configuration review of the application and Docker environment is explicitly in scope.

# 16. Out-of-Scope Activities

The following activities are explicitly excluded:

1. Denial-of-Service attacks.
2. Distributed Denial-of-Service attacks.
3. Stress testing.
4. Load testing.
5. Active rate-limit exhaustion testing.
6. Destructive database operations.
7. Intentional permanent destruction of application data.
8. Direct security testing against unrelated third-party systems.
9. Formal source-code security review.
10. Testing systems unrelated to the approved crAPI UAT environment.

The exclusion of active rate-limit testing does not prevent Mr. Wario from identifying weak or missing rate-limiting controls through safe observation or configuration review.

# 17. Testing Constraints

Testing must minimize unnecessary impact on the UAT environment.

Where exploitation is required to validate a vulnerability, Mr. Wario will use the minimum level of exploitation necessary to demonstrate impact.

Testing should avoid unnecessary service disruption, resource exhaustion, large-scale data extraction, destruction of data, uncontrolled persistence, or interaction with unrelated systems.

The client has authorized service restarts where required; however, such actions should only be performed where necessary and documented.

# 18. Scope Handling for Incomplete Client Information

Responses such as **“Not sure”** and **“May be”** will not be interpreted as exclusions from scope.

Where the subject relates directly to crAPI and the approved UAT environment, uncertainty will be treated as a reconnaissance objective.

| Client Response | Assessment Treatment |
|---|---|
| Authentication mechanism: Not sure | Identify authentication mechanism during reconnaissance |
| JWT usage: Not sure | Determine whether JWT or other tokens are used |
| Sessions/cookies: Not sure | Analyze session handling |
| API versions: Maybe | Enumerate API versions |
| Legacy APIs: Maybe | Search for legacy/deprecated endpoints |
| Internal APIs: Maybe | Identify internal/hidden API routes |
| Third-party APIs: Not sure | Identify external service dependencies |
| Docker hosts: Not sure | Map relevant Docker architecture |
| Admin interfaces: Not sure | Enumerate administrative interfaces |
| Lockout/rate limiting: Not sure | Observe and review controls without prohibited active rate-limit testing |

This approach is necessary because relying solely on incomplete documentation could leave significant parts of the application attack surface unassessed.

# 19. Scope Validation During Reconnaissance

The initial scope represents the best available understanding of the application before technical assessment begins.

During reconnaissance, Mr. Wario will validate:

- application architecture;
- exposed ports;
- technology stack;
- API endpoints;
- authentication mechanisms;
- authorization model;
- API versions;
- internal services;
- databases;
- Docker architecture;
- third-party integrations;
- administrative interfaces.

Any discrepancies between client-provided information and observed technical behavior will be documented.

For example, if assessment evidence indicates gateway or gateway-like routing behavior despite the client initially stating that no API gateway exists, that component may be assessed where it forms part of the approved crAPI environment.

The assessment will distinguish between **client-provided information** and **assessment-validated information**.

# 20. Assessment Phases

### Phase 0 — Engagement Setup

Includes questionnaire, scope, Rules of Engagement, Authorization to Test, NDA, and evidence-handling requirements.

### Phase 1 — Reconnaissance and Application Understanding

Includes baseline application functionality, service enumeration, technology fingerprinting, API inventory, authentication discovery, authorization mapping, API version and legacy endpoint discovery, Docker architecture mapping, integration discovery, data-flow mapping, and attack-surface definition.

### Phase 2 — Security Testing

Includes authentication testing, BOLA, BOPLA, BFLA, injection testing, SSRF, file upload testing, business-logic testing, security misconfiguration, Docker/configuration review, resource-consumption control review, unsafe API consumption, and controlled vulnerability validation.

### Phase 3 — Reporting

Includes evidence consolidation, risk rating, technical findings, remediation guidance, executive summary, final report, and client findings review.

### Phase 4 — Retesting and Closure

Includes remediation validation, vulnerability retesting, residual-risk documentation, retest reporting, and engagement closure.

# 21. Evidence and Finding Validation

Reconnaissance observations will not automatically be considered vulnerabilities.

A security issue will generally be recorded as a confirmed finding only after Mr. Wario has reproduced the behavior, identified the affected component, established reasonable security impact, captured supporting evidence, and eliminated likely false positives.

Evidence may include HTTP requests and responses, screenshots, application behavior, relevant logs, configuration excerpts, sanitized token information, database evidence, container configuration, and proof-of-concept output.

Evidence should be sufficient to reproduce the issue while avoiding unnecessary exposure of sensitive information.

# 22. Deliverables

Expected deliverables include:

### Pre-Engagement Documentation

- Pre-Engagement Questionnaire
- Scope of Assessment
- Rules of Engagement
- Authorization to Test
- NDA
- Evidence Handling Plan

### Reconnaissance Documentation

- Application overview
- Service inventory
- Technology fingerprint
- API endpoint inventory
- Authentication model
- Authorization model
- API version and hidden-endpoint inventory
- Docker service map
- External integration inventory
- Architecture and data-flow diagram
- Attack-surface inventory
- Security test-case inventory

### Security Testing Documentation

- Test notes
- Supporting evidence
- Confirmed vulnerability records

### Final Reporting

- Executive summary
- Risk-rated technical findings
- Reproduction steps
- Evidence references
- Business/security impact
- Remediation recommendations
- Final crAPI Application Security Assessment report

### Retest Documentation

- Retest status for remediated findings
- Retest evidence
- Residual risks
- Retest and closure report

# 23. Assessment Limitations

The assessment may be affected by incomplete application documentation, limited historical knowledge from the client contact, lack of administrator accounts, lack of predefined test accounts for each role, unknown API inventory, unknown third-party dependencies, unknown authentication architecture, and restrictions on stress, DoS, and active rate-limit testing.

Where limitations prevent full validation of a security control, they will be documented in the final report.

# 24. Scope Change Management

Any material expansion beyond the approved crAPI UAT environment requires authorization from Mr. Mario.

Examples include newly discovered external hosts, third-party infrastructure, unrelated corporate systems, additional environments, or production systems.

Any material scope change should be documented before testing proceeds against the newly added target.

# 25. Contacts and Escalation

| Role | Contact |
|---|---|
| Client Representative | Mr. Mario |
| Client Technical Contact | Mr. Mario |
| Scope Change Authority | Mr. Mario |
| Testing Instability Contact | Mr. Mario |
| Emergency Stop Contact | Mr. Mario |
| Security Assessment Lead | Mr. Wario |
| High/Critical Finding Recipient | Mr. Mario |

Any **High or Critical severity finding must be communicated to Mr. Mario as soon as reasonably possible after validation**, without waiting for the final report.

Mr. Wario may also immediately contact Mr. Mario and pause or modify testing where continued testing presents an immediate operational risk, creates unexpected instability, risks data loss, or indicates that continuing under the existing conditions may materially increase impact.

Medium, Low, and Informational findings may be consolidated into the normal reporting process unless circumstances justify earlier communication.

# 26. Completion Criteria

The assessment will be considered technically complete when:

- the known application attack surface has been assessed;
- significant undocumented components discovered during reconnaissance have been evaluated;
- planned security test cases have been executed where feasible;
- suspected vulnerabilities have been validated;
- evidence has been captured;
- assessment limitations have been documented;
- findings have been risk-rated;
- remediation recommendations have been prepared;
- the final report has been delivered.

Retesting will be performed separately during the agreed two-week retest period.

# 27. Scope Approval

This document defines the agreed scope of the **crAPI Application Security Assessment**.

Approval confirms that the client acknowledges the systems, components, assessment areas, and limitations described above.

Formal permission to perform the testing activities defined within this scope will be established separately through the **Authorization to Test** and **Rules of Engagement**.

| Role | Name | Approval | Date |
|---|---|---|---|
| Client Representative | Mr. Mario | __________________ | __________ |
| Application Security Expert | Mr. Wario | __________________ | __________ |

### Important

This engagement is a simulated security assessment performed against **OWASP crAPI** for security learning, professional practice, and community reference.

The client representatives, business context, and engagement scenario are fictitious. The project is intended to demonstrate a structured application and API security assessment lifecycle from pre-engagement through retesting and closure.
