# Authentication & Authorization Model

## Purpose

This document records the authentication and authorization behavior observed during the crAPI assessment, with emphasis on the Phase #8 object-level authorization testing.

The goal is to separate two different security questions:

- **Authentication:** Who is the requester?
- **Authorization:** Is that authenticated requester allowed to access the requested object?

---

## Authentication Model Observed

crAPI uses a Bearer token for authenticated API requests.

Observed behavior:

| Test | Result | Interpretation |
|---|---|---|
| Request without an Authorization token | HTTP 401 | Authentication is enforced |
| Request with a valid token | Request is processed | The API recognizes the authenticated user |

A request without the token returned an authentication failure, confirming that the tested vehicle endpoint is not anonymously accessible.

---

## Authorization Model Tested

### Endpoint

```http
GET /identity/api/v2/vehicle/{vehicleId}/location
```

### Test Method

Two separate authenticated users were used for controlled cross-user testing:

- **User X** — requester
- **User Y** — owner of the target vehicle object

The test kept User X's valid Bearer token unchanged and replaced only the vehicle object identifier with User Y's known vehicle identifier.

This is a controlled BOLA/IDOR validation technique because only the object reference changes while the authenticated identity remains User X.

---

## Confirmed Phase #8 Finding

### Cross-user request

**Authorization context:** User X token  
**Requested object:** User Y vehicle

### Actual result

The server returned:

```text
HTTP 200 OK
```

and disclosed User Y's vehicle/location information.

Observed data included:

- vehicle/car identifier
- latitude
- longitude
- full name
- email address

### Expected result

The backend should verify that the requested vehicle belongs to the authenticated user before returning the object.

A request from User X for User Y's vehicle should therefore be denied, commonly with:

```text
HTTP 403 Forbidden
```

or, where object-existence hiding is intentionally implemented:

```text
HTTP 404 Not Found
```

### Assessment

This confirms a **Broken Object Level Authorization (BOLA)** weakness, also commonly described as an **IDOR-style authorization issue**.

OWASP API Security classification:

**API1: Broken Object Level Authorization**

---

## Control Tests

### 1. Request without Authorization token

Result:

```text
HTTP 401
```

This confirms that authentication itself is being enforced.

The vulnerability is therefore not caused by anonymous access. The problem occurs **after successful authentication**, when object ownership should be checked.

---

### 2. Authenticated request using a nonexistent VIN / vehicle reference

Result:

```text
HTTP 500 Internal Server Error
```

This is important to record separately.

A nonexistent object should normally result in controlled application behavior such as:

```text
HTTP 404 Not Found
```

The HTTP 500 response indicates poor error handling or an unhandled backend condition when the supplied vehicle reference cannot be resolved.

This does **not** weaken the confirmed BOLA finding. It is a separate robustness/error-handling observation.

---

## Current Behavior Matrix

| Scenario | Observed Result | Security Meaning |
|---|---:|---|
| No token | 401 | Authentication enforced |
| User X token + User X object | Expected normal access | Authorized object access |
| User X token + User Y valid object | **200 + User Y data** | **BOLA confirmed** |
| User X token + nonexistent VIN/object | 500 | Unhandled error / poor error handling |

The most important comparison is:

> User X is authenticated correctly, but the API still returns User Y's protected object when User Y's valid object identifier is supplied.

---

## Why the Object Identifier Is Not an Authorization Control

The vehicle identifier/VIN/UUID may be difficult to guess, but secrecy or unpredictability of an identifier must not be relied on for access control.

A secure backend must perform an ownership or permission check similar to:

```text
authenticated user
        ↓
requested vehicle
        ↓
Does this vehicle belong to this user?
        ↓
YES → return the object
NO  → deny access
```

The tested endpoint appears to resolve the supplied object and return it without sufficiently verifying that it belongs to the authenticated requester.

---

## Impact

A logged-in user who obtains another valid vehicle identifier may be able to access information belonging to another user.

The tested response exposed sensitive information including:

- precise location coordinates
- full name
- email address
- vehicle identifier

The demonstrated issue is limited to the controlled test performed during this assessment. No bulk enumeration is required to establish the vulnerability.

---

## Recommended Remediation

The backend should:

1. Derive the authenticated user identity from the validated token.
2. Resolve the requested vehicle/object.
3. Verify ownership or explicit authorization between that user and object.
4. Return the object only when the authorization check succeeds.
5. Return a controlled 403 or intentionally designed 404 when authorization fails.
6. Handle nonexistent object references safely without generating HTTP 500 errors.
7. Apply the same authorization pattern consistently to all read, update, delete and action endpoints that operate on user-owned objects.

Authorization checks should be implemented server-side and must not rely on the client hiding object identifiers.

---

## Phase #8 Status

### Completed

- Confirmed authentication is required.
- Performed controlled cross-user object access test.
- Confirmed BOLA on the vehicle location endpoint.
- Confirmed disclosure of another user's protected vehicle/location information.
- Tested a nonexistent VIN/object reference.
- Recorded the resulting HTTP 500 behavior separately from the BOLA finding.

### Remaining

Before closing Phase #8, test one or two closely related vehicle/object endpoints using the same controlled User X → User Y method to determine whether the weakness is:

- isolated to the location endpoint, or
- part of a broader object-authorization pattern.

Avoid unnecessary enumeration. One controlled request per relevant endpoint is sufficient for learning and validation.

---

## Learning Summary

The key lesson from this phase is that **authentication and authorization are independent controls**.

The application correctly answers:

> "Who are you?"

but fails, for the confirmed endpoint, to correctly answer:

> "Are you allowed to access this specific vehicle?"

That distinction is the core of Broken Object Level Authorization testing.
