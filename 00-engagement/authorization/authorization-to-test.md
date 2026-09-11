# Authorization to Test

## Important Info

> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

---

## Document Control

| Field | Details |
|---|---|
| Document Title | Authorization to Test |
| Document Type | Formal Testing Authorization |
| Engagement Type | White-Box Application and API Security Assessment |
| Application | crAPI |
| Environment | UAT |
| Security Assessment Lead | Mr. Wario – Application Security Expert |
| Client Representative | Mr. Mario |
| Document Owner | Application Security Team |
| Version | 1.0 Draft |
| Classification | Training / Public |
| Status | Draft for Client Review |
| Assessment Start | ASAP |
| Planned Testing Duration | 1 Week |
| Retest Period | 2 Weeks |
| Last Updated | 11 September 2026 |

### Document Revision History

| Version | Date | Author | Change Description |
|---|---|---|---|
| 0.1 | 11-Sep-2026 | Mr. Wario | Initial Authorization to Test draft |
| 1.0 | TBD | Mr. Wario | Finalized following client review and approval |

---

## Table of Contents

1. [Purpose](#1-purpose)  
2. [Authorization Statement](#2-authorization-statement)  
3. [Authorized Environment](#3-authorized-environment)  
4. [Authorized Testing Categories](#4-authorized-testing-categories)  
5. [Conditions and Restrictions](#5-conditions-and-restrictions)  
6. [Reference to Scope and Rules of Engagement](#6-reference-to-scope-and-rules-of-engagement)  
7. [Validity and Change Control](#7-validity-and-change-control)  
8. [Approval and Authorization](#8-approval-and-authorization)  

---

# 1. Purpose

This document provides formal authorization for **Mr. Wario**, acting as the Application Security Expert, to perform the approved security assessment against the **crAPI UAT environment**.

The purpose of this document is to establish clear written permission for the testing activities agreed between the client and the assessment team.

This authorization does not independently define or expand the assessment scope.

The detailed technical scope, permitted testing activities, restrictions, operational controls and escalation requirements are defined in the approved:

- Scope of Security Assessment;
- Rules of Engagement;
- Pre-Engagement Questionnaire.

# 2. Authorization Statement

Mr. Mario, acting as the client representative, formally authorizes Mr. Wario to perform the **crAPI Application Security Assessment** against the approved UAT environment.

This authorization permits Mr. Wario to carry out the security assessment activities necessary to identify, validate and document security weaknesses, provided those activities remain within the approved Scope and Rules of Engagement.

The client acknowledges that authorized security testing may involve actions that would normally be considered intrusive if performed without permission.

Such actions may include:

- reconnaissance and service discovery;
- vulnerability scanning;
- API enumeration and fuzzing;
- authentication and authorization testing;
- controlled exploitation;
- privilege escalation testing;
- injection and SSRF testing;
- file upload testing;
- business-logic testing;
- container and configuration inspection;
- limited database inspection;
- controlled proof-of-concept validation.

# 3. Authorized Environment

The approved environment is:

| Item | Authorized Value |
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

Supporting components that are clearly part of the same crAPI UAT environment may be assessed where permitted by the approved Scope and Rules of Engagement.

# 4. Authorized Testing Categories

The following high-level categories are authorized:

| Testing Category | Authorization |
|---|---|
| Application and API reconnaissance | Authorized |
| Network and service discovery | Authorized |
| Automated vulnerability scanning | Authorized |
| API endpoint discovery and fuzzing | Authorized |
| Authentication testing | Authorized |
| Authorization testing | Authorized |
| BOLA, BOPLA and BFLA testing | Authorized |
| Injection testing | Authorized |
| SSRF testing | Authorized |
| File upload testing | Authorized |
| Business-logic testing | Authorized |
| Privilege escalation testing | Authorized |
| Security misconfiguration testing | Authorized |
| Docker and configuration review | Authorized |
| Controlled exploitation | Authorized |
| Limited proof-of-concept data access | Authorized |

Detailed execution rules and limitations for these activities are defined in the Rules of Engagement.

# 5. Conditions and Restrictions

This authorization is subject to the approved Rules of Engagement.

Activities specifically prohibited include:

- Denial-of-Service testing;
- Distributed Denial-of-Service testing;
- stress or load testing;
- active rate-limit exhaustion testing;
- destructive database operations;
- intentional permanent destruction of application data;
- uncontrolled persistence;
- malware deployment;
- testing unrelated corporate systems;
- testing unrelated third-party infrastructure;
- testing production systems unless separately authorized;
- formal full source-code review.

Where any activity may create significant operational risk, Mr. Wario must follow the escalation and emergency-stop procedures defined in the Rules of Engagement.

# 6. Reference to Scope and Rules of Engagement

This Authorization to Test must be read together with the approved:

- Scope of Security Assessment;
- Rules of Engagement.

The **Scope** defines what systems, services and application components are included in the assessment.

The **Rules of Engagement** define how the testing may be performed, including:

- permitted and prohibited techniques;
- tool categories;
- exploitation limits;
- evidence handling;
- service restart rules;
- operational safety requirements;
- finding escalation;
- emergency-stop procedures;
- third-party restrictions;
- communication requirements.

Where additional detail is required, the Scope and Rules of Engagement take precedence over this document.

If an ambiguity or conflict is identified, clarification must be obtained from Mr. Mario before the affected testing activity proceeds.

# 7. Validity and Change Control

This authorization is valid for the approved assessment period and applies only to the approved crAPI UAT environment.

Any material expansion beyond the approved scope requires additional authorization from Mr. Mario.

Examples include:

- additional environments;
- production systems;
- external hosts;
- unrelated corporate systems;
- third-party infrastructure.

Where newly discovered components are clearly part of the approved crAPI UAT environment, they may be assessed where this remains consistent with the approved Scope and Rules of Engagement.

High or Critical findings must be communicated to Mr. Mario as soon as reasonably possible after validation, in accordance with the Rules of Engagement.

# 8. Approval and Authorization

By approving this document, Mr. Mario confirms that:

- he authorizes Mr. Wario to perform the crAPI Application Security Assessment;
- the approved testing categories may be performed against the authorized UAT environment;
- the Scope of Security Assessment defines what is in scope;
- the Rules of Engagement define how testing will be performed;
- prohibited activities remain unauthorized;
- material scope changes require additional approval.

This approval represents formal authorization for Mr. Wario to perform the agreed security testing during the approved assessment period.

| Role | Name | Approval / Signature | Date |
|---|---|---|---|
| Client Representative | Mr. Mario | __________________________ | __________ |
| Application Security Expert | Mr. Wario | __________________________ | __________ |
