# Pre-Engagement Questions and Answers
## Important Info
> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

## Engagement Background

The Application Security Expert, **Mr. Wario**, received a request from **Mr. Mario**, the client's technical contact, to perform a security assessment of an application named **crAPI** before it moves beyond its current UAT stage.

The client explained that crAPI is an API-driven web application for vehicle owners. The application was developed using a microservices architecture, but the organization currently has limited security documentation and incomplete technical knowledge of some application components. Mr. Mario is also relatively new to the environment and therefore could not confirm every architectural, API, authentication, and historical detail during the initial discussion.

Before beginning any security testing, Mr. Wario shared a pre-engagement questionnaire with Mr. Mario. The purpose was to understand the business context, application architecture, intended targets, testing permissions, restrictions, data-handling expectations, contacts, and assessment timeline. The answers below represent the information provided by the client at the start of the engagement.

Any answer marked **Not sure**, **May be**, or otherwise incomplete should not be treated as a confirmed technical fact. These gaps will instead become reconnaissance objectives during the assessment and will be validated through authorized testing.

## 1. Application and Business Context

| Question | Answer |
|---|---|
| What is the name and purpose of the application? | Name is crAPI, it is a platform for vehicle owners. |
| Is this application currently in development, staging, UAT, or production? | UAT |
| Is the application Internet-facing or internal only? | Internal only. |
| Who are the expected users? | Customers, various vehicle owners across globe. |
| Are there different user roles such as user, admin, support, developer, vendor, etc.? | Yes |
| What business functions are considered critical? | Not sure, app is important |
| What type of data does the application process? | Vehicle related info, customer info |
| Does it handle PII, financial information, credentials, tokens, or other sensitive data? | PII |
| Are there any compliance requirements such as PCI DSS, ISO 27001, GDPR, UAE PDPL, etc.? | ISO 27001 |

## 2. Architecture

| Question | Answer |
|---|---|
| What is the application architecture? | API driven, microservices baseed web app |
| Is it monolithic or microservices based? | Microservices |
| What programming languages and frameworks are used? | Java, JavaScript (Node/React), Python and Go |
| What databases are used? | PostgreSQL and MongoDB |
| Is there an API gateway? | No |
| Is there a reverse proxy or load balancer? | OpenResty |
| Are containers, Kubernetes, Docker, or cloud services used? | Docker |
| Are there third-party APIs or external integrations? | Not sure |
| Is there any authentication provider such as AD, LDAP, OAuth, OIDC, SAML, or custom authentication? | Not sure |

## 3. API Information

| Question | Answer |
|---|---|
| Does the application use REST, GraphQL, SOAP, WebSocket, gRPC, or other API technologies? | Standard REST API |
| Is API documentation available? | No docs |
| Is there Swagger/OpenAPI documentation? | Not sure |
| Are there Postman collections? | What is this? |
| Are there separate API versions? | May be, I am new in this company, not sure about history |
| Are there deprecated or legacy APIs? | May be, I am new in this company, not sure about history |
| Are there internal APIs that are not meant to be publicly accessible? | May be, I am new in this company, not sure about history |
| Are any APIs used by mobile applications? | No |

## 4. Authentication and Authorization

| Question | Answer |
|---|---|
| What authentication mechanisms are used? | Not sure |
| Are JWTs used? | Not sure |
| Are sessions or cookies used? | Not sure |
| Is MFA supported? | No |
| Are there different privilege levels? | Yes |
| Can test accounts for each role be provided? | No |
| Is there an administrator account available for testing? | No |
| Are there account lockout or rate-limiting controls? | Not sure |

## 5. Infrastructure Scope

| Item | Response |
|---|---|
| IP addresses | `http://127.0.0.1:8888` |
| Hostnames | N/A |
| Domains | N/A |
| Subdomains | N/A |
| API endpoints | Not sure |
| Ports | 8888 |
| Docker hosts | Not sure |
| Cloud resources | N/A |
| Development servers | N/A |
| Databases | Already given |
| Admin interfaces | Not sure |

## 6. Testing Permissions

| Activity | Permission |
|---|---|
| Automated vulnerability scanning | Yes |
| API fuzzing | Yes |
| Directory/endpoint enumeration | Yes |
| Authentication testing | Yes |
| Authorization testing | Yes |
| Rate-limit testing | No |
| File upload testing | Yes |
| Injection testing | Yes |
| SSRF testing | Yes |
| Business-logic testing | Yes |
| Password attacks against supplied test accounts | Yes |
| Privilege escalation | Yes |
| Exploitation of confirmed vulnerabilities | Yes |
| Data extraction as proof of concept | Yes |
| Source-code review | No |
| Container/configuration review | Yes |
| Are there any systems that must explicitly NOT be tested? | You can go for full system |

## 7. Testing Restrictions

| Question | Answer |
|---|---|
| Are DoS/DDoS tests prohibited? | Yes |
| Are stress tests prohibited? | Yes |
| Are brute-force attacks prohibited? | No |
| Are destructive database actions prohibited? | Yes |
| Can we modify data? | Yes |
| Can we create/delete users? | Yes |
| Can we upload files? | Yes |
| Can we restart services? | Yes |
| Can we access container shells? | Yes |
| Can we examine database contents? | Yes |
| Can we access source code? | Yes |

## 8. Test Data

| Question | Answer |
|---|---|
| Will the environment contain real customer data? | No |
| If yes, what data-handling restrictions apply? | No |
| Can dummy test data be created? | Yes |
| Can we create multiple test accounts? | Yes |

## 9. Contact and Incident Handling

| Question | Answer |
|---|---|
| Who is the primary technical contact? | Mr. Mario |
| Who should we contact if testing causes instability? | Mr. Mario |
| What is the emergency stop procedure? | Mr. Mario |
| Who can authorize scope changes? | Mr. Mario |
| Who receives critical findings? | Mr. Mario |
| Should critical vulnerabilities be reported immediately or only in the final report? | Final report |

## 10. Timing

| Item | Response |
|---|---|
| Testing start date | ASAP |
| Testing end date | 1 week |
| Allowed testing hours | 24 hrs |
| Maintenance windows | N/A |
| Blackout periods | N/A |
| Reporting deadline | 1 week |
| Retest period | 2 weeks |
