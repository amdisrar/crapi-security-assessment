# Engagement Communication Log

## Important Info

> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

---

## Document Control

| Field | Details |
|---|---|
| Document Title | Engagement Communication Log |
| Document Type | Engagement Communication Record |
| Application | crAPI |
| Environment | UAT |
| Security Assessment Lead | Mr. Wario – Application Security Expert |
| Client Representative | Mr. Mario |
| Version | 1.0 |
| Classification | Confidential |
| Status | Active – Update Throughout Engagement |
| Last Updated | 11 September 2026 |

### Document Revision History

| Version | Date | Author | Change Description |
|---|---|---|---|
| 1.0 | 11-Sep-2026 | Mr. Wario | Created Phase 0 communication record |

---

## Purpose

This document maintains a concise record of important client communication related to the crAPI Application Security Assessment. It is intended to preserve key decisions, approvals, clarifications and engagement milestones that may affect scope, testing, evidence handling, reporting or retesting.

Routine conversations that do not materially affect the engagement do not need to be recorded here.

---

# Communication 001 – Initial Security Assessment Request

**Date:** 11 September 2026  
**From:** Mr. Mario  
**To:** Mr. Wario  
**Subject:** Security Assessment Request – crAPI UAT Application

Dear Mr. Wario,

We are currently preparing our crAPI application for further deployment and would like to perform a security assessment before moving ahead.

The application is currently in UAT and is used as a platform for vehicle owners. It is API-driven and uses a microservices architecture. I am relatively new to this environment and unfortunately we do not have complete technical or API documentation available at the moment.

We would like you to review the application from an application and API security perspective and identify any security weaknesses that should be addressed before the system progresses further.

Please let me know what information and approvals you require from our side before the assessment begins.

Regards,  
**Mr. Mario**  
Client Representative

---

# Communication 002 – Pre-Engagement Questionnaire

**Date:** 11 September 2026  
**From:** Mr. Wario  
**To:** Mr. Mario  
**Subject:** Pre-Engagement Information Required – crAPI Application Security Assessment

Dear Mr. Mario,

Thank you for requesting the security assessment of crAPI.

Before technical testing begins, I would like to establish the application scope, testing permissions, restrictions, contacts, environment details and evidence-handling expectations.

I have prepared a pre-engagement questionnaire covering the following areas:

- application and business context;
- architecture and technology stack;
- API information;
- authentication and authorization;
- infrastructure scope;
- permitted testing activities;
- prohibited activities;
- test data;
- emergency contacts;
- testing and retest timeline.

Please provide as much information as currently available. Where information is unknown, please mark it as **Not sure** or **Maybe** rather than making an assumption. Any relevant unknowns can then be validated during authorized reconnaissance.

Once the questionnaire is reviewed, I will prepare the Scope of Security Assessment, Rules of Engagement and Authorization to Test for your review.

Regards,  
**Mr. Wario**  
Application Security Expert

---

# Communication 003 – Questionnaire Response and Testing Permissions

**Date:** 11 September 2026  
**From:** Mr. Mario  
**To:** Mr. Wario  
**Subject:** RE: Pre-Engagement Information Required – crAPI Application Security Assessment

Dear Mr. Wario,

Please find the requested information provided based on what is currently known about the application.

The application is a UAT, internal-only, API-driven microservices environment using Docker. PostgreSQL and MongoDB are used as data stores and OpenResty is used as the reverse proxy. The application contains customer and vehicle information and may process PII.

Some areas remain unclear, including the exact authentication mechanism, API versions, legacy or internal APIs, third-party integrations, administrative interfaces and some Docker architecture details. These can be validated as part of the assessment.

You are permitted to perform vulnerability scanning, API fuzzing, endpoint enumeration, authentication and authorization testing, injection testing, SSRF testing, file upload testing, business-logic testing, privilege escalation, controlled exploitation, container/configuration review and limited proof-of-concept data access.

DoS/DDoS, stress/load testing, active rate-limit testing and destructive database operations are not permitted.

Please proceed with preparation of the engagement documents for review.

Regards,  
**Mr. Mario**  
Client Representative

---

# Communication 004 – Engagement Documents Submitted for Review

**Date:** 11 September 2026  
**From:** Mr. Wario  
**To:** Mr. Mario  
**Subject:** crAPI Application Security Assessment – Engagement Documents for Review

Dear Mr. Mario,

Based on the information provided during the pre-engagement discussion, I have prepared the required engagement documentation for your review.

The documents include:

1. Scope of Security Assessment;
2. Rules of Engagement;
3. Authorization to Test;
4. Evidence Handling Plan;
5. Non-Disclosure Agreement.

The Scope defines what will be assessed. The Rules of Engagement define how testing will be performed and the operational restrictions. The Authorization to Test provides formal permission to carry out the approved security activities.

The Evidence Handling Plan defines where assessment evidence will be stored, how sensitive information will be protected, how evidence may be transferred and when raw evidence will be destroyed. The NDA defines the confidentiality obligations applicable to information exchanged or obtained during the assessment.

Please review the documents and confirm any required changes before technical testing begins.

Regards,  
**Mr. Wario**  
Application Security Expert

---

# Communication 005 – Client Review and Approval

**Date:** 11 September 2026  
**From:** Mr. Mario  
**To:** Mr. Wario  
**Subject:** RE: crAPI Application Security Assessment – Engagement Documents for Review

Dear Mr. Wario,

I have reviewed the engagement documents and confirm that the scope and testing conditions are acceptable.

Please proceed with the assessment under the agreed Scope and Rules of Engagement.

For reporting, any confirmed **High or Critical severity finding should be communicated as soon as possible after validation** rather than waiting for the final report.

Assessment evidence may be maintained locally on your approved dedicated assessment workstation. Cloud storage and unapproved file-sharing services should not be used. Raw assessment evidence should be securely destroyed by **31 October 2026**, unless we agree in writing to extend the retention period.

If testing causes unexpected instability or there is a risk of data loss or impact outside the approved scope, please stop the affected activity and contact me immediately.

The Authorization to Test and NDA are approved for the engagement.

Regards,  
**Mr. Mario**  
Client Representative

---

# Communication 006 – Phase 0 Completion and Technical Assessment Start

**Date:** 11 September 2026  
**From:** Mr. Wario  
**To:** Mr. Mario  
**Subject:** Phase 0 Complete – crAPI Technical Security Assessment to Begin

Dear Mr. Mario,

Thank you for confirming the engagement documents and testing authorization.

The pre-engagement phase is now complete. The agreed documentation consists of:

- Pre-Engagement Questions and Answers;
- Scope of Security Assessment;
- Rules of Engagement;
- Authorization to Test;
- Evidence Handling Plan;
- Non-Disclosure Agreement.

I will now begin the technical phase of the crAPI Application Security Assessment.

The first activity will be to baseline normal application functionality and document the application's major workflows before beginning security testing. This will be followed by service enumeration, technology fingerprinting, API discovery and authentication/authorization mapping.

I will contact you immediately if a High or Critical finding is validated, if a material scope clarification is required, or if unexpected operational impact is observed.

Regards,  
**Mr. Wario**  
Application Security Expert

---

## Communication Log Status

**Phase 0:** Completed  
**Next Phase:** Phase 1 – Reconnaissance and Application Understanding  
**Next Planned Activity:** Baseline normal application functionality

This communication record should be updated whenever a material engagement decision, approval, escalation, scope change, High/Critical finding notification, remediation discussion or retest decision occurs.