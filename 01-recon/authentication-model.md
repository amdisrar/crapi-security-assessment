# Authentication, Session and Authorization Model

> Shared reconnaissance document for:
> - Issue #7 — Map authentication, sessions and token handling
> - Issue #8 — Identify roles and authorization boundaries
>
> Issue #7 findings are documented below. Authorization mapping for Issue #8 will be added to the same document.

## 1. Scope

This document records observed authentication, session/token, password-recovery and authorization behavior in the local OWASP crAPI lab.

The approach used for Issue #7 was primarily black-box/manual:
- normal application use through Burp Browser;
- request/response inspection in Burp Proxy;
- controlled request replay and modification in Burp Repeater;
- browser storage inspection through Developer Tools.

No conclusion below is based only on an endpoint name; behavior was verified where noted.

---

## 2. Authentication Flow

### 2.1 Login endpoint

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

A successful login returned HTTP 200 and a JSON response containing:

- a token;
- token type `Bearer`;
- `Login successful`;
- `mfaRequired: false`.

### 2.2 Authentication mechanism

crAPI uses a JWT Bearer token for authenticated API requests.

Example:

```http
Authorization: Bearer <JWT>
```

A request to:

```http
GET /identity/api/v2/vehicle/vehicles
```

returned:

- HTTP 200 with a valid JWT;
- HTTP 401 `Invalid Token` when the Authorization header was removed.

The existing `chat_session_id` cookie alone did not authenticate this tested endpoint.

**Conclusion:** the JWT Bearer token is the effective authentication credential for this API request.

---

## 3. JWT Structure

The JWT contains three dot-separated sections:

```text
HEADER.PAYLOAD.SIGNATURE
```

The header and payload were Base64URL-decoded for inspection. Base64/Base64URL is encoding, not encryption; decoding only converts the stored representation back into readable data.

### 3.1 Header

Decoded header:

```json
{
  "alg": "RS256"
}
```

Observed signing algorithm: **RS256**.

### 3.2 Payload

Decoded payload:

```json
{
  "sub": "test.user2@email.com",
  "iat": 1789892180,
  "exp": 1790496980,
  "role": "user"
}
```

Observed claims:

| Claim | Observed meaning |
|---|---|
| `sub` | User identity / subject, represented by email |
| `iat` | Token issue time |
| `exp` | Token expiry time |
| `role` | User role |

### 3.3 Token lifetime

Observed:

```text
exp - iat = 604800 seconds
```

`604800 seconds = 7 days`.

**Observed JWT lifetime: approximately 7 days.**

---

## 4. JWT Validation Behavior

### 4.1 Missing token

Removing the complete `Authorization: Bearer ...` header from an authenticated request resulted in:

```text
HTTP 401
CRAPIResponse(message=Invalid Token, status=401)
```

### 4.2 Modified token

Changing one character in the JWT caused the request to be rejected with HTTP 401 `Invalid Token`.

**Observed behavior:** altered JWTs are rejected. This is consistent with token integrity/signature validation being enforced.

---

## 5. Client-Side Token Storage

Browser Developer Tools showed the JWT stored in **Local Storage**, under application state containing:

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

Other user-related client state was also cleared.

**Observed behavior:** logout clears the authentication token from browser Local Storage.

---

## 6. Logout and Token Revocation

No dedicated backend logout request was observed in Burp traffic during the tested logout action.

The endpoint:

```http
POST /identity/api/auth/verify
```

was observed, but it verifies whether a JWT is valid; it is not a logout endpoint.

After logout:

1. browser Local Storage no longer contained the access token;
2. the JWT captured before logout was replayed in Burp Repeater;
3. the old JWT still received HTTP 200 from an authenticated API endpoint.

**Observed behavior:** normal logout clears the client-side token but does not invalidate the previously issued JWT on the server.

### Security relevance

Because the observed JWT lifetime is approximately 7 days, a previously copied token can remain usable after logout until it expires, based on the tested behavior.

---

## 7. Password Reset / Account Recovery

### 7.1 Password reset request

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

The OTP itself was delivered through the lab email service and was not returned by this API response.

### 7.2 OTP verification and password change

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

The new password successfully authenticated afterward and the previous password no longer worked.

**Conclusion:** `/identity/api/auth/v3/check-otp` performs the password change as part of successful OTP verification.

---

## 8. OTP Security Controls

### 8.1 OTP length

Observed OTP was four numeric digits.

A four-digit numeric OTP has:

```text
0000–9999 = 10,000 possible values
```

This makes attempt limiting an important compensating control.

### 8.2 OTP reuse

After an OTP had been successfully used to reset the password, replaying the same OTP produced:

```json
{
  "message": "Invalid OTP! Please try again..",
  "status": 500
}
```

**Observed behavior:** a successfully used OTP is single-use.

### 8.3 Failed-attempt limit

After approximately 10 incorrect OTP submissions in a fresh reset flow, the API returned:

```http
HTTP/1.1 503
```

with:

```json
{
  "message": "You've exceeded the number of attempts.",
  "status": 503
}
```

**Observed behavior:** an OTP attempt limit exists at approximately 10 failed attempts.

### 8.4 Counter reset

Requesting a new OTP for the same test user reset the failed-attempt counter. The new OTP was successfully accepted and the attempt count started again from the beginning.

**Observed behavior:** the failed-attempt counter is associated with the active OTP/reset cycle rather than acting as a persistent account lockout.

### 8.5 HTTP status-code observations

Some expected client-side validation failures were represented using server/service error codes:

- invalid/reused OTP: HTTP 500;
- exceeded OTP attempts: HTTP 503.

These are recorded as API/error-handling observations. More typical status codes for such conditions would normally be in the 4xx range.

---

## 9. Password Reset and Existing JWTs

A JWT that had been issued **before** the password reset was preserved in Burp Repeater.

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

### Candidate security finding

**Previously issued JWTs remain valid after password reset.**

Security impact to validate/report in the testing/findings phase:

A password reset may be used to recover an account after suspected compromise. If an attacker already possesses a valid JWT, changing/resetting the password does not remove that token's access based on the observed behavior. The token can remain usable until its normal expiry.

This is more security-relevant than client-side logout behavior because password recovery is expected to help regain control of a potentially compromised account.

---

## 10. Issue #7 Summary

| Area | Observed behavior |
|---|---|
| Authentication | JWT Bearer token |
| JWT algorithm | RS256 |
| Identity claim | Email in `sub` |
| Role claim | `role: user` |
| Token lifetime | ~7 days |
| Client storage | Local Storage |
| Missing JWT | Rejected with HTTP 401 |
| Modified JWT | Rejected with HTTP 401 |
| Logout | Clears Local Storage token |
| Server-side logout revocation | Not observed; old token remained valid |
| Password reset | Email + OTP + new password |
| OTP length | 4 numeric digits |
| OTP reuse | Rejected |
| OTP attempt limit | ~10 failed attempts |
| New OTP | Resets attempt counter |
| Password reset revokes old JWTs | No; pre-reset JWT remained valid |
| Error handling | Invalid OTP/attempt limit use 500/503 responses |

---

## 11. Candidate Follow-Up Tests

The following items should be carried into later testing rather than assumed to be vulnerabilities from reconnaissance alone:

- determine whether token revocation is expected by application design;
- test whether password-change flows behave the same as password-reset flows for existing JWTs;
- determine whether any refresh-token mechanism exists;
- assess password policy separately;
- validate account-enumeration behavior in recovery/login responses;
- test authorization boundaries using multiple users and owned objects.

---

# Issue #8 — Authorization Model

## 12. Objective

Issue #8 will extend this same document with the application's authorization model rather than creating a separate file.

Planned areas:

- observable roles;
- permissions per role;
- object ownership relationships;
- access to another user's objects;
- privileged/admin-only functions;
- candidate BOLA, BOPLA and BFLA test cases.

## 13. Role / Permission Matrix

_To be completed during Issue #8._

## 14. Object Ownership Model

_To be completed during Issue #8._

## 15. Candidate Authorization Test Cases

_To be completed during Issue #8._
