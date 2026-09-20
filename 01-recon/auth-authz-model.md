# Authentication and Authorization Model

## Important Info
> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

## Document Control

| Field | Details |
|---|---|
| Document Title | crAPI Application Security Assessment - Authentication and Authorization Model |
| Document Type | Phase 1 Reconnaissance |
| Engagement | crAPI Application Security Assessment |
| Client Representative | Mr. Mario |
| Assessment Lead | Mr. Wario |
| Environment | UAT |
| Related GitHub Issues | Issue #7 - Map authentication, sessions and token handling; Issue #8 - Identify roles and authorization boundaries |
| Document Owner | Mr. Wario |
| Version | 0.9.1 |
| Status | Draft - Issue #7 documented / Issue #8 pending |
| Date Created | 20 September 2026 |
| Classification | Engagement Confidential / Training Simulation |
| Repository Location | `01-recon/auth-authz-model.md` |
| Evidence Location | `01-recon/raw/` |
| Screenshot Location | `01-recon/screenshots/` |

## Document Revision History

| Version | Date | Author | Description |
|---|---|---|---|
| 0.1 | 20 September 2026 | Mr. Wario | Initial authentication and session workpaper created |
| 0.8 | 20 September 2026 | Mr. Wario | Added JWT, token storage, logout, password recovery, OTP and token-revocation observations |
| 0.9 | 20 September 2026 | Mr. Wario | Aligned document with the Phase 1 reconnaissance workpaper format and prepared shared authorization sections for Issue #8 |
| 0.9.1 | 20 September 2026 | Mr. Wario | Added consolidated findings-to-date section for authentication/session observations requiring later validation or reporting |

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#1-scope)
- [3. Assessment Methodology](#2-assessment-methodology)
- [4. Authentication Flow](#3-authentication-flow)
- [5. JWT Structure and Lifetime](#4-jwt-structure-and-lifetime)
- [6. JWT Validation Behavior](#5-jwt-validation-behavior)
- [7. Client-Side Token Storage](#6-client-side-token-storage)
- [8. Logout and Token Revocation](#7-logout-and-token-revocation)
- [9. Password Reset and Account Recovery](#8-password-reset-and-account-recovery)
- [10. OTP Security Controls](#9-otp-security-controls)
- [11. Password Reset and Existing JWTs](#10-password-reset-and-existing-jwts)
- [12. Security-Relevant Reconnaissance Observations](#11-security-relevant-reconnaissance-observations)
- [13. Findings Identified to Date](#13-findings-identified-to-date)
- [14. Commands Used](#14-commands-used)
- [15. Evidence Index](#15-evidence-index)
- [16. Limitations](#16-limitations)
- [17. Follow-Up Actions](#17-follow-up-actions)
- [18. Authentication Reconnaissance Summary](#18-authentication-reconnaissance-summary)
- [19. Authorization Model - Issue #8](#19-authorization-model---issue-8)
- [20. Role and Permission Matrix](#20-role-and-permission-matrix)
- [21. Object Ownership Model](#21-object-ownership-model)
- [22. Candidate Authorization Test Cases](#22-candidate-authorization-test-cases)

## 1. Purpose

Document how crAPI authenticates users, maintains authenticated state, handles JWTs and account recovery, and subsequently map the application's authorization boundaries within the same workpaper.

Issue #7 establishes the authentication and session model. Issue #8 will extend the same document with roles, permissions, object ownership and candidate authorization test cases.

Security-relevant observations are reconnaissance items unless explicitly identified as candidate findings. Exploitability and impact will be validated during later assessment phases.

## 2. Scope

Authentication and session reconnaissance was limited to the approved local crAPI environment.

The Issue #7 assessment covered:

- login and authenticated API behavior;
- JWT structure and lifetime;
- token storage and transmission;
- behavior when authentication is removed or altered;
- logout handling;
- password recovery and OTP behavior;
- OTP reuse and failed-attempt handling;
- behavior of existing JWTs after password reset.

Issue #8 authorization mapping will be added to the same document.

## 3. Assessment Methodology

Issue #7 was assessed primarily through manual black-box testing and controlled runtime inspection.

Activities included:

- normal application use through Burp Browser;
- request and response inspection in Burp Proxy HTTP history;
- controlled replay and modification using Burp Repeater;
- JWT Base64URL decoding to inspect token header and payload;
- Unix timestamp conversion and lifetime calculation;
- browser Local Storage and Session Storage inspection using Developer Tools;
- manual password-recovery and OTP workflow validation;
- limited manual OTP attempt testing to understand the observed attempt-control behavior.

No conclusion in this document is based only on endpoint naming. Runtime behavior was verified where noted.

## 4. Authentication Flow

### 3.1 Login Endpoint

Observed endpoint:

```http
POST /identity/api/auth/login
```

Observed request body:

```json
{
  "email": "test.user2@email.com",
  "password": "<password>"
}
```

A successful login returned HTTP 200 with a token, token type `Bearer`, `Login successful`, and `mfaRequired: false`.

### 3.2 Authentication Mechanism

Authenticated API requests use a JWT Bearer token:

```http
Authorization: Bearer <JWT>
```

The following request was used as a controlled test:

```http
GET /identity/api/v2/vehicle/vehicles
```

Observed behavior:

- valid JWT -> HTTP 200;
- Authorization header removed -> HTTP 401 `Invalid Token`;
- the existing `chat_session_id` cookie alone did not authenticate the endpoint.

**Observed conclusion:** the JWT Bearer token is the effective authentication credential for this tested API request.

## 5. JWT Structure and Lifetime

A JWT contains three dot-separated sections:

```text
HEADER.PAYLOAD.SIGNATURE
```

The header and payload were Base64URL-decoded for inspection. Base64/Base64URL is encoding, not encryption.

### 4.1 JWT Header

Decoded header:

```json
{
  "alg": "RS256"
}
```

Observed signing algorithm: **RS256**.

### 4.2 JWT Payload

Decoded payload:

```json
{
  "sub": "test.user2@email.com",
  "iat": 1789892180,
  "exp": 1790496980,
  "role": "user"
}
```

| Claim | Observed Meaning |
|---|---|
| `sub` | User identity / subject, represented by email |
| `iat` | Token issue time |
| `exp` | Token expiry time |
| `role` | User role |

### 4.3 Token Lifetime

Observed:

```text
exp - iat = 604800 seconds
```

`604800 seconds = 7 days`.

**Observed JWT lifetime: approximately 7 days.**

## 6. JWT Validation Behavior

### 5.1 Missing Token

Removing the complete Authorization header produced:

```text
HTTP 401
CRAPIResponse(message=Invalid Token, status=401)
```

### 5.2 Modified Token

Changing one character in the JWT caused the request to be rejected with HTTP 401 `Invalid Token`.

**Observed behavior:** altered JWTs are rejected, consistent with token integrity/signature validation being enforced.

## 7. Client-Side Token Storage

Browser Developer Tools showed the JWT stored in **Local Storage**.

Observed authenticated state:

```text
isLoggedIn = true
accessToken = <JWT>
```

Session Storage was empty during the test.

After normal logout:

```text
isLoggedIn = false
accessToken = ""
```

**Observed behavior:** logout clears the authentication token from browser Local Storage.

## 8. Logout and Token Revocation

No dedicated backend logout request was observed during the tested logout action.

The endpoint:

```http
POST /identity/api/auth/verify
```

was observed, but its tested behavior was token verification rather than logout.

After logout:

1. Local Storage no longer contained the access token;
2. the JWT captured before logout was retained in Burp Repeater;
3. the old JWT still received HTTP 200 from the authenticated vehicle endpoint.

**Observed behavior:** normal logout clears the client-side token but does not invalidate the previously issued JWT on the server.

Because the observed JWT lifetime is approximately 7 days, a copied token may remain usable after logout until expiry based on the tested behavior.

## 9. Password Reset and Account Recovery

### 8.1 Password Reset Request

Observed endpoint:

```http
POST /identity/api/auth/forget-password
```

Decoded request body:

```json
{
  "email": "test.user2@email.com"
}
```

Observed response:

```json
{
  "message": "OTP Sent on the provided email, test.user2@email.com",
  "status": 200
}
```

The OTP was delivered through the lab email service and was not returned in the API response.

### 8.2 OTP Verification and Password Change

Observed endpoint:

```http
POST /identity/api/auth/v3/check-otp
```

Decoded request body:

```json
{
  "email": "test.user2@email.com",
  "otp": "0728",
  "password": "Pass@123"
}
```

Observed response:

```json
{
  "message": "OTP verified",
  "status": 200
}
```

The new password authenticated successfully afterward and the previous password no longer worked.

**Observed conclusion:** `/identity/api/auth/v3/check-otp` performs the password change as part of successful OTP verification.

## 10. OTP Security Controls

### 9.1 OTP Length

The observed OTP was four numeric digits.

```text
0000-9999 = 10,000 possible values
```

Attempt limiting is therefore an important compensating control.

### 9.2 OTP Reuse

After successful use, replaying the same OTP produced:

```json
{
  "message": "Invalid OTP! Please try again..",
  "status": 500
}
```

**Observed behavior:** successfully used OTPs are single-use.

### 9.3 Failed-Attempt Limit

After approximately 10 incorrect OTP submissions in a fresh reset flow, the API returned HTTP 503 with:

```json
{
  "message": "You've exceeded the number of attempts.",
  "status": 503
}
```

**Observed behavior:** an OTP attempt limit exists at approximately 10 failed attempts.

### 9.4 Counter Reset

Requesting a new OTP for the same user reset the failed-attempt counter. The newly issued OTP was accepted successfully.

**Observed behavior:** the attempt counter is associated with the active OTP/reset cycle rather than a persistent account lockout.

### 9.5 HTTP Status-Code Behavior

Observed validation/error conditions used server/service error codes:

- invalid/reused OTP -> HTTP 500;
- exceeded OTP attempts -> HTTP 503.

These are recorded as API/error-handling observations. More typical client-side validation responses would generally use 4xx status codes.

## 11. Password Reset and Existing JWTs

A JWT issued before the password reset was retained in Burp Repeater.

After:

1. completing the password reset;
2. confirming the old password no longer worked;
3. confirming the new password worked;

the pre-reset JWT was replayed against:

```http
GET /identity/api/v2/vehicle/vehicles
```

The server still returned HTTP 200.

**Observed behavior:** password reset does not invalidate JWTs that were issued before the reset.

### Candidate Security Finding

**Previously issued JWTs remain valid after password reset.**

Security relevance:

A password reset may be used when an account compromise is suspected. If a valid JWT has already been copied, the password reset does not remove that token's access based on the observed behavior. The token may remain usable until its normal expiry.

## 12. Security-Relevant Reconnaissance Observations

| Observation | Current Interpretation |
|---|---|
| JWT Bearer authentication is used | Confirmed runtime behavior |
| JWT uses RS256 | Confirmed by decoded header |
| JWT lifetime is approximately 7 days | Confirmed from `iat` and `exp` |
| JWT stored in Local Storage | Confirmed in browser Developer Tools |
| Missing JWT rejected | Positive authentication control |
| Modified JWT rejected | Positive token-integrity control |
| Logout clears browser token only | Old JWT remained valid after logout |
| Password reset uses 4-digit OTP | Confirmed runtime behavior |
| Used OTP cannot be replayed | Positive OTP control |
| OTP failed-attempt control exists | Approximately 10 incorrect attempts |
| New OTP resets attempt counter | Confirmed runtime behavior |
| Pre-reset JWT survives password reset | Candidate security finding |
| OTP errors use HTTP 500/503 | API/error-handling observation |

## 13. Findings Identified to Date

The following items have been identified from the Issue #7 reconnaissance and should be carried forward for formal validation and reporting where applicable.

| Finding / Observation | Evidence to Date | Current Status |
|---|---|---|
| Previously issued JWT remains valid after password reset | A JWT issued before password recovery continued to receive HTTP 200 after the password was reset and the old password stopped working | Candidate security finding |
| Previously issued JWT remains valid after logout | A JWT captured before normal logout continued to receive HTTP 200 after the browser cleared its stored token | Security-relevant session-management observation |
| JWT has an approximately 7-day lifetime | Decoded `iat` and `exp` claims differ by 604800 seconds | Confirmed reconnaissance observation |
| JWT is stored in browser Local Storage | Developer Tools showed the access token in Local Storage while authenticated | Security-relevant client-side storage observation |
| OTP attempt counter resets when a new OTP is issued | Approximately 10 failed attempts triggered the attempt limit; requesting a fresh OTP restarted the counter | Recovery-control observation requiring later risk assessment |
| Invalid/reused OTP and attempt-limit conditions use HTTP 500/503 | Reused OTP returned HTTP 500 and exceeded attempts returned HTTP 503 | API/error-handling observation |

### Highest-Priority Candidate Finding

**Previously issued JWTs remain valid after password reset.**

The tested password-recovery process successfully changed the account password but did not revoke a JWT issued before the reset. Because the observed token lifetime is approximately seven days, a copied pre-reset token may continue to provide authenticated access until its normal expiry.

This item should be validated and documented formally during Phase 2, including impact, severity, reproducibility and remediation guidance.

## 14. Commands Used

The following commands were used during manual JWT analysis.

### 12.1 Decode JWT Header

```bash
echo "$TOKEN" | cut -d '.' -f1 | base64 -d
```

Purpose:

- `cut -d '.' -f1` selects the first dot-separated JWT section;
- `base64 -d` decodes the encoded header into readable JSON.

### 12.2 Decode JWT Payload

```bash
echo "$TOKEN" | cut -d '.' -f2 | base64 -d
```

Purpose:

- selects the second JWT section;
- decodes the payload into readable JSON.

JWT uses Base64URL encoding. Direct `base64 -d` worked for the observed token values; padding or URL-safe character handling may be required for other JWTs.

### 12.3 Convert Unix Timestamps

```bash
date -d @1789892180
date -d @1790496980
```

Purpose: convert JWT `iat` and `exp` Unix timestamps into readable date/time values.

### 12.4 Calculate Token Lifetime

```bash
echo $((1790496980 - 1789892180))
```

Purpose: subtract `iat` from `exp` using Bash arithmetic. The observed result was 604800 seconds, or 7 days.

## 15. Evidence Index

| Evidence | Source / Location | Relevance |
|---|---|---|
| Login request/response | Burp Proxy HTTP history | JWT issuance and Bearer token type |
| Authenticated vehicle request | Burp Repeater | Valid-token baseline |
| Request without Authorization header | Burp Repeater | Missing-token rejection |
| Modified JWT request | Burp Repeater | Token-integrity validation |
| Local Storage before/after logout | Browser Developer Tools | Client-side token storage and clearing |
| Old JWT replay after logout | Burp Repeater | Logout token-revocation behavior |
| Forgot-password request | Burp Proxy HTTP history | Password-recovery initiation |
| OTP email | MailHog | OTP delivery |
| `/auth/v3/check-otp` request | Burp Proxy / Repeater | OTP verification and password change |
| Reused OTP test | Burp Repeater | OTP single-use behavior |
| Failed OTP attempt sequence | Burp Repeater | Attempt-limit behavior |
| New OTP after attempt limit | Burp Repeater | Attempt-counter reset behavior |
| Pre-reset JWT replay after password reset | Burp Repeater | Confirms a JWT issued before password reset remains valid after the reset and can continue to be accepted for up to its original ~7-day lifetime, until normal token expiry |

## 16. Limitations

- Issue #7 focused on observed runtime authentication behavior and did not perform a full source-code review of token-generation or revocation internals.
- No exhaustive JWT algorithm-confusion or cryptographic attack testing was performed during this reconnaissance step.
- OTP testing was limited to controlled manual attempts in the local lab.
- Refresh-token behavior was not observed during the tested workflows.
- Password policy has not yet been fully assessed.
- Authorization mapping is intentionally deferred to Issue #8 and will be added to this same document.

## 17. Follow-Up Actions

- Continue Issue #8 in this document and build the role/permission matrix.
- Identify ownership relationships between users, vehicles, orders, posts and other objects.
- Validate candidate BOLA, BOPLA and BFLA cases during authorization testing.
- Assess password policy and account-enumeration behavior.
- Determine whether any refresh-token mechanism exists.
- Compare normal password-change behavior with password-reset behavior for existing JWTs.
- Carry the pre-reset JWT behavior into Phase 2 for formal finding validation and impact assessment.

## 18. Authentication Reconnaissance Summary

| Area | Observed Behavior |
|---|---|
| Authentication | JWT Bearer token |
| JWT algorithm | RS256 |
| Identity claim | Email in `sub` |
| Role claim | `role: user` |
| Token lifetime | Approximately 7 days |
| Client storage | Local Storage |
| Missing JWT | HTTP 401 |
| Modified JWT | HTTP 401 |
| Logout | Clears Local Storage token |
| Server-side logout revocation | Not observed |
| Password reset | Email + OTP + new password |
| OTP length | 4 numeric digits |
| OTP reuse | Rejected |
| OTP attempt limit | Approximately 10 failed attempts |
| New OTP | Resets attempt counter |
| Password reset revokes old JWTs | No; pre-reset JWT remained valid |
| Error handling | Invalid OTP / attempt limit use HTTP 500/503 |

Issue #7 authentication/session reconnaissance is substantially documented. The document remains **Draft** because Issue #8 authorization mapping is still pending.

## 19. Authorization Model - Issue #8

Issue #8 will continue within this same workpaper.

Planned coverage:

- observable user roles;
- permissions available to each role;
- authenticated versus privileged operations;
- ownership relationships between users and application objects;
- access-control boundaries between two standard users;
- privileged/admin-only operations;
- candidate BOLA, BOPLA and BFLA test cases.

## 20. Role and Permission Matrix

_To be completed during Issue #8._

## 21. Object Ownership Model

_To be completed during Issue #8._

## 22. Candidate Authorization Test Cases

_To be completed during Issue #8._
