# Evidence Handling Plan

## Important Info

> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

---

## Document Control

| Field | Details |
|---|---|
| Document Title | Evidence Handling Plan |
| Document Type | Operational Control Document |
| Engagement Type | White-Box Application and API Security Assessment |
| Application | crAPI |
| Environment | UAT |
| Security Assessment Lead | Mr. Wario – Application Security Expert |
| Client Representative | Mr. Mario |
| Document Owner | Application Security Team |
| Version | 1.0 Draft |
| Classification | Confidential |
| Status | Draft for Client Review |
| Formal Signature Required | No |
| Evidence Destruction Date | 31 October 2026 |
| Last Updated | 11 September 2026 |

### Document Revision History

| Version | Date | Author | Change Description |
|---|---|---|---|
| 0.1 | 11-Sep-2026 | Mr. Wario | Initial Evidence Handling Plan draft |
| 1.0 | TBD | Mr. Wario | Finalized following client review and agreement |

---

## Table of Contents

1. [Purpose](#1-purpose)  
2. [Scope of Evidence](#2-scope-of-evidence)  
3. [Testing Workstation and Evidence Storage](#3-testing-workstation-and-evidence-storage)  
4. [Sensitive Information Handling](#4-sensitive-information-handling)  
5. [Evidence Collection and Minimization](#5-evidence-collection-and-minimization)  
6. [Evidence Sharing and Transfer](#6-evidence-sharing-and-transfer)  
7. [Unexpected Real Customer Data](#7-unexpected-real-customer-data)  
8. [Evidence Retention and Destruction](#8-evidence-retention-and-destruction)  
9. [Access Control and Responsibility](#9-access-control-and-responsibility)  
10. [Review and Agreement](#10-review-and-agreement)  

---

# 1. Purpose

This document defines how evidence collected during the **crAPI Application Security Assessment** will be handled, stored, protected, shared, retained and destroyed.

The objective is to ensure that assessment evidence is:

- collected only where necessary;
- stored only on approved systems;
- protected against unauthorized access;
- shared only with authorized parties;
- retained only for the agreed period;
- securely destroyed when no longer required.

This document should be read together with the NDA, Scope of Security Assessment, Rules of Engagement and Authorization to Test.

---

# 2. Scope of Evidence

Assessment evidence may include:

- screenshots;
- HTTP requests and responses;
- API request and response samples;
- application logs;
- container output;
- command output;
- vulnerability scanner results;
- proof-of-concept scripts;
- endpoint inventories;
- database output;
- test account details;
- authentication information;
- session cookies;
- access tokens;
- application error messages;
- Burp Suite project files;
- packet captures;
- configuration excerpts;
- source-code excerpts used to validate specific findings;
- notes produced during reconnaissance and testing.

Only evidence necessary to support testing, findings, reporting or retesting should be retained.

---

# 3. Testing Workstation and Evidence Storage

Assessment activities must be performed only from a **client-approved testing workstation or system**.

The workstation requirement should be agreed before technical testing begins.

For this engagement, the approved arrangement is:

| Control | Engagement Requirement |
|---|---|
| Testing Workstation | Assessor-provided dedicated assessment workstation |
| Primary User | Mr. Wario |
| Evidence Storage | Local storage on the approved assessment workstation |
| Cloud Storage | Prohibited unless specifically approved by Mr. Mario |
| External Storage | Prohibited unless specifically approved |
| Personal Email / File Sharing | Prohibited |
| Evidence Export | Only where required and authorized |
| Evidence Destruction Date | 31 October 2026 |

Where the client requires the use of a **client-provided or client-managed workstation**, that requirement takes precedence.

In such cases:

- Mr. Wario must use the client-approved workstation;
- personal laptops or other personal systems must not be used for assessment activity;
- evidence must remain within the client-controlled environment;
- evidence must not be copied to personal devices;
- unapproved USB or removable media must not be used;
- personal cloud storage, personal email and unapproved file-transfer services must not be used;
- assessment tools may be installed only with client approval;
- evidence export outside the client environment requires explicit approval from Mr. Mario.

For this engagement, evidence stored on Mr. Wario's workstation must remain local and under his control.

Where practical, sensitive evidence should be stored on encrypted local storage.

Temporary files created during testing should be removed when no longer required.

---

# 4. Sensitive Information Handling

Sensitive information may include:

- passwords;
- API keys;
- private keys;
- access tokens;
- session cookies;
- JWTs;
- database credentials;
- PII;
- customer records;
- application secrets;
- sensitive environment variables.

Sensitive information must be handled according to the principle of **minimum necessary exposure**.

Where full values are not required to demonstrate a finding, they should be redacted.

For example:

```text
eyJhbGciOi...<redacted>...7F9x
```

Credentials and authentication material should not be copied into unnecessary notes or documentation.

Any active credentials or session material retained for testing should be deleted or invalidated when no longer required.

---

# 5. Evidence Collection and Minimization

Mr. Wario should collect only the evidence required to:

- reproduce a vulnerability;
- demonstrate security impact;
- support remediation;
- support retesting;
- prepare the final report.

Where practical, evidence should include:

- date and time;
- affected endpoint or component;
- account or role used;
- test performed;
- observed result;
- relevant finding or test-case reference.

Evidence collection should avoid unnecessary bulk extraction.

For example, where a single record is sufficient to prove unauthorized access, an entire dataset should not be extracted.

Raw evidence should be retained only where it adds value to validation or reporting.

---

# 6. Evidence Sharing and Transfer

Assessment evidence may be shared with **Mr. Mario** where required for:

- vulnerability validation;
- remediation;
- incident response;
- reporting;
- retesting.

Evidence must not be shared with unrelated third parties without client approval.

Where evidence is transferred to the client:

- only necessary evidence should be provided;
- sensitive values should be redacted where possible;
- an approved transfer method should be used;
- evidence should not be sent through personal email or unapproved file-sharing services.

If the client provides an approved secure transfer mechanism, that mechanism should be used.

No onward distribution of assessment evidence should occur without authorization.

---

# 7. Unexpected Real Customer Data

The client has stated that the UAT environment should not contain real customer data.

If real customer data is unexpectedly discovered:

1. unnecessary access to the data must stop;
2. additional extraction must be avoided unless required to confirm the issue;
3. only the minimum evidence necessary should be retained;
4. Mr. Mario must be informed as soon as reasonably possible;
5. unnecessary local copies must be securely deleted;
6. the discovery should be documented appropriately.

If the presence of real customer data introduces additional security or compliance concerns, those concerns may be raised separately as part of the assessment.

---

# 8. Evidence Retention and Destruction

Raw assessment evidence will be retained only for the period required to complete:

- testing;
- reporting;
- client clarification;
- remediation support;
- agreed retesting.

The agreed **Evidence Destruction Date is 31 October 2026**.

On or before this date, Mr. Wario will securely remove raw assessment evidence that is no longer required, including:

- screenshots containing sensitive information;
- HTTP captures;
- raw scanner output;
- temporary proof-of-concept files;
- authentication tokens;
- credentials;
- Burp Suite project files;
- packet captures;
- database extracts;
- temporary logs;
- other sensitive assessment artifacts.

Deletion should be performed from the approved assessment workstation and any authorized temporary storage locations.

The following may be retained as formal engagement records:

- final approved report;
- finalized Scope of Security Assessment;
- finalized Rules of Engagement;
- Authorization to Test;
- NDA;
- Evidence Handling Plan;
- retest report;
- sanitized finding records where required.

Any extension to the retention period must be agreed with Mr. Mario and documented.

---

# 9. Access Control and Responsibility

Mr. Wario is responsible for maintaining custody of assessment evidence while it remains under assessor control.

Access to raw evidence should be limited to personnel who require it for the engagement.

| Role | Responsibility |
|---|---|
| Mr. Wario | Evidence collection, protection, local storage, transfer and destruction |
| Mr. Mario | Receives evidence required for remediation, escalation, reporting or client records |

Mr. Wario must ensure that evidence remains protected until it is transferred to the client or securely destroyed.

Where the client provides the testing workstation or storage platform, custody of evidence remains subject to the client's access-control and retention requirements.

---

# 10. Review and Agreement

This Evidence Handling Plan should be reviewed with Mr. Mario before significant assessment evidence is collected.

Formal signature is not required unless requested by the client.

Review confirms agreement on:

- the approved testing workstation;
- evidence storage location;
- restrictions on personal systems and cloud storage;
- permitted evidence transfer methods;
- handling of sensitive information;
- retention requirements;
- the evidence destruction date.

| Role | Name | Review Status | Date |
|---|---|---|---|
| Client Representative | Mr. Mario | Reviewed / Agreed | __________ |
| Application Security Expert | Mr. Wario | Prepared / Acknowledged | __________ |
