# API Endpoint Inventory

## Important Info
> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

## Document Control

| Field | Details |
|---|---|
| Document Title | API Endpoint Inventory |
| Document Type | Phase 1 Reconnaissance |
| Engagement | crAPI Application Security Assessment |
| Client Representative | Mr. Mario |
| Assessment Lead | Mr. Wario |
| Environment | UAT |
| Related GitHub Issue | Issue #6 - Discover and inventory REST API endpoints |
| Document Owner | Mr. Wario |
| Version | 1.0 |
| Status | Reviewed - Complete |
| Date Created | 19 September 2026 |
| Classification | Engagement Confidential / Training Simulation |
| Repository Location | `01-recon/api-endpoints.md` |
| Evidence Location | `01-recon/raw/` |
| Screenshot Location | `01-recon/screenshots/api-endpoints/` |

## Document Revision History

| Version | Date | Author | Description |
|---|---|---|---|
| 0.1 | 19 September 2026 | Mr. Wario | Initial API endpoint inventory structure created |
| 0.9 | 19 September 2026 | Mr. Wario | Added source-discovered endpoints, access classification, user-controlled inputs, object identifiers, Burp runtime validation, commands used and security-relevant reconnaissance observations |
| 1.0 | 19 September 2026 | Mr. Wario | Added runtime Swagger/OpenAPI/API-documentation validation result and completed workpaper review |

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Methodology](#3-methodology)
- [4. Runtime Validation with Burp Suite](#4-runtime-validation-with-burp-suite)
- [5. Identity Service API Inventory](#5-identity-service-api-inventory)
- [6. Community Service API Inventory](#6-community-service-api-inventory)
- [7. Workshop Service API Inventory](#7-workshop-service-api-inventory)
- [8. Chatbot Service API Inventory](#8-chatbot-service-api-inventory)
- [9. Gateway Service API Inventory](#9-gateway-service-api-inventory)
- [10. Object Identifiers and User-Controlled Inputs](#10-object-identifiers-and-user-controlled-inputs)
- [11. Security-Relevant Reconnaissance Observations](#11-security-relevant-reconnaissance-observations)
- [12. Commands Used](#12-commands-used)
- [13. Evidence and Runtime Correlation](#13-evidence-and-runtime-correlation)
- [14. Limitations](#14-limitations)
- [15. Follow-Up Actions](#15-follow-up-actions)
- [16. Reconnaissance Summary](#16-reconnaissance-summary)

## 1. Purpose

Discover, inventory and classify the REST API endpoints exposed by the approved crAPI deployment.

The client did not provide Swagger/OpenAPI documentation, a Postman collection, developer endpoint documentation or a complete endpoint list. The inventory was therefore built using white-box source review and runtime observation of normal application workflows.

This workpaper records:

- HTTP methods and paths
- authentication and role requirements where identifiable
- path, query and body inputs
- object identifiers controlled by the client
- source-only, internal and conditional endpoints
- runtime-observed endpoints captured through Burp Suite
- security-relevant implementation characteristics for later testing

Security-relevant observations in this document are reconnaissance items only unless otherwise stated. Exploitability and impact are to be validated during Phase 2.

## 2. Scope

Endpoint discovery was limited to the approved crAPI application and associated backend services:

- Identity service
- Community service
- Workshop service
- Chatbot service
- Gateway service
- browser-facing reverse-proxy paths used by the application

General services on the Kali host and unrelated infrastructure were outside the scope of this activity.

## 3. Methodology

The endpoint inventory was developed through a combination of:

- source-code route discovery
- authentication and authorization rule review
- controller/view inspection
- review of request-body handling
- identification of path and query parameters
- identification of object identifiers
- review of backend-to-backend service calls
- normal application browsing through Burp Suite Community Edition
- correlation of source-defined routes with runtime HTTP history

Endpoint access classifications used in this document are:

| Classification | Meaning |
|---|---|
| Public | No authentication requirement observed at the reviewed application layer |
| Authenticated | JWT, session state or other authenticated context required |
| Privileged | Explicit role restriction identified |
| Basic Auth | HTTP Basic authentication required |
| Conditional | Route is only registered or available under a runtime condition |
| Source-only / Not runtime-validated | Discovered in source but not exercised during the normal-user Burp pass |

Where source comments disagreed with implementation, the implemented function or handler was treated as authoritative and runtime behavior was used where available.

## 4. Runtime Validation with Burp Suite

Burp Suite Community Edition was used as a passive runtime validation tool.

The Burp built-in browser was used to access the crAPI application and exercise normal user workflows. Proxy interception was not required for the majority of this activity; Burp Proxy HTTP history was used to review requests and responses without interrupting application flow.

The runtime pass included normal use of:

- authentication
- password-recovery initiation and OTP verification
- vehicle onboarding and vehicle-location functionality
- community posts and comments
- mechanic/service-request workflows
- shop products, orders and return functionality
- chatbot state
- profile video upload, retrieval and update

For each useful request, the following were reviewed:

- HTTP method
- request path
- query parameters
- request body or multipart data
- response status
- response content type
- JWT or other authentication context
- user-controlled identifiers

A final video workflow was captured and confirmed:

- `POST /identity/api/v2/user/videos` using JWT authentication and multipart form-data with file field `file`
- `GET /identity/api/v2/user/videos/{video_id}`
- `PUT /identity/api/v2/user/videos/{video_id}` with a JSON body containing `videoName`

The Burp runtime pass was intended to validate normal application behavior and correlate source-defined routes. Deliberate ID manipulation, authorization bypass attempts and active attack testing were deferred to Phase 2.

Common Swagger, OpenAPI and API-documentation paths were also tested against the main crAPI application and relevant service prefixes. All tested documentation paths returned HTTP 404. No exposed Swagger, OpenAPI or other API-documentation endpoint was identified during runtime validation.

## 5. Identity Service API Inventory

### 5.1 Authentication and Account Endpoints

| Method | Path | Access | Input / Identifier | Runtime | Notes |
|---|---|---|---|---|---|
| POST | `/identity/api/auth/login` | Public | email, password | Observed | Returns JWT on successful authentication |
| POST | `/identity/api/auth/signup` | Public | signup fields | Source-discovered | User registration |
| POST | `/identity/api/auth/verify` | Public | JWT token form | Source-discovered | Token verification |
| GET | `/identity/api/auth/jwks.json` | Public | None | Source-discovered | JWKS also exposed through reverse proxy |
| POST | `/identity/api/auth/forget-password` | Public | account/email input | Observed | Password-reset workflow; OTP delivered through simulated email environment |
| POST | `/identity/api/auth/v2/check-otp` | Public | OTP form | Source-discovered | Source comments identify unlimited invalid OTP attempts |
| POST | `/identity/api/auth/v3/check-otp` | Public | OTP form | Observed | Source comments identify invalid-attempt limit |
| POST | `/identity/api/auth/v4.0/user/login-with-token` | Public | email-token form | Source-discovered | Versioned token-login endpoint |
| POST | `/identity/api/auth/v2.7/user/login-with-token` | Public | email-token form | Source-discovered | Versioned token-login endpoint returning JWT |
| POST | `/identity/api/auth/reset-test-users` | Public at filter layer | None | Source-only | Resets seeded test-user passwords |
| POST | `/identity/api/auth/unlock` | Public at filter layer | unlock form | Source-only | Internal service logic also validates token |

### 5.2 User, Vehicle and Profile Endpoints

| Method | Path | Access | Input / Identifier | Runtime | Notes |
|---|---|---|---|---|---|
| GET | `/identity/api/v2/user/dashboard` | Public at filter layer; internal token handling present | request token/context | Observed during normal use | Source and internal behavior should be reconciled during auth testing |
| POST | `/identity/api/v2/user/reset-password` | Authenticated | password/login form | Source-discovered | Uses request token |
| POST | `/identity/api/v2/vehicle/add_vehicle` | Authenticated | VIN, PIN | Observed | Vehicle enrollment data obtained through simulated email workflow |
| POST | `/identity/api/v2/vehicle/resend_email` | Authenticated | request context | Source-discovered | Resends vehicle email |
| GET | `/identity/api/v2/vehicle/vehicles` | Authenticated | request context | Observed | Returns vehicles and previous owners |
| GET | `/identity/api/v2/vehicle/{carId}/location` | Authenticated | `carId` | Observed | Direct client-controlled vehicle identifier |
| GET | `/identity/api/v2/user/videos/{video_id}` | Authenticated | `video_id` | Observed | Retrieves video object |
| POST | `/identity/api/v2/user/pictures` | Authenticated | multipart file | Source-discovered | Profile image upload |
| POST | `/identity/api/v2/user/videos` | Authenticated | multipart file field `file` | Observed | Video upload confirmed with video/mp4 |
| PUT | `/identity/api/v2/user/videos/{video_id}` | Authenticated | `video_id`, `videoName` | Observed | Video update/rename |
| DELETE | `/identity/api/v2/user/videos/{video_id}` | Authenticated | `video_id` | Source-only | Source comments reference BFLA behavior |
| DELETE | `/identity/api/v2/admin/videos/{video_id}` | Authenticated by generic filter | `video_id` | Source-only | Route is not under the reviewed `/management/admin/**` role matcher |
| GET | `/identity/api/v2/user/videos/convert_video` | Authenticated | query `video_id` | Source-only | Source comment references shell-injection behavior |
| POST | `/identity/api/v2/user/change-email` | Authenticated | change-email form | Source-only | Email-change flow |
| POST | `/identity/api/v2/user/verify-email-token` | Authenticated | change-email/token form | Source-only | Email verification |
| POST | `/identity/api/v2/user/change-phone-number` | Authenticated | phone-change form | Source-only | Phone-change flow |
| POST | `/identity/api/v2/user/verify-phone-otp` | Authenticated | phone/OTP form | Source-only | Phone OTP verification |

### 5.3 Management and Health Endpoints

| Method | Path | Access | Input | Runtime | Notes |
|---|---|---|---|---|---|
| POST | `/identity/management/admin/lockUser` | Privileged - ADMIN | account-lock form | Source-only | Explicit `ADMIN` role matcher applies |
| POST | `/identity/management/user/apikey` | Public at Spring filter layer | login form/request | Source-only | Internal authorization and key privilege require validation |
| GET | `/identity/health_check` | Public | None | Source-discovered | Health endpoint |

## 6. Community Service API Inventory

The Community router applies `AccessControlMiddleware` globally and wraps the principal community/coupon routes with authentication middleware.

| Method | Path | Access | Input / Identifier | Runtime | Notes |
|---|---|---|---|---|---|
| GET | `/community/api/v2/community/posts/recent` | Authenticated | query `limit`, `offset` | Observed | Default limit 30; maximum 50 |
| GET | `/community/api/v2/community/posts/{postID}` | Authenticated | `postID` | Observed | Direct post identifier |
| POST | `/community/api/v2/community/posts` | Authenticated | JSON `Post` body | Observed | Creates post |
| POST | `/community/api/v2/community/posts/{postID}/comment` | Authenticated | `postID`, JSON comment | Observed | Adds comment |
| POST | `/community/api/v2/coupon/new-coupon` | Authenticated | JSON `Coupon` body | Source-only | Creates coupon |
| POST | `/community/api/v2/coupon/validate-coupon` | Authenticated | arbitrary JSON decoded to BSON map | Source-only | Generic BSON-map input |
| GET | `/community/home` | Public at route layer | None | Source-only | Home-style endpoint |
| Any under prefix | `/debug/pprof/` | Conditional | profiling paths | Source-only | Registered only when `DEBUG=1` |

## 7. Workshop Service API Inventory

### 7.1 Mechanic Endpoints

| Method | Path | Access | Input / Identifier | Runtime | Notes |
|---|---|---|---|---|---|
| POST | `/workshop/api/mechanic/signup` | Public | name, email, number, password, mechanic_code | Source-only | Creates mechanic |
| GET | `/workshop/api/mechanic/` | Authenticated | pagination | Observed | Mechanic list |
| GET | `/workshop/api/mechanic/receive_report` | Public at view layer | mechanic_code, problem_details, vin | Runtime used indirectly / source-discovered | Source docstring says POST but implementation is GET |
| GET | `/workshop/api/mechanic/mechanic_report` | Authenticated | query `report_id` | Source-only | Generates report data/PDF |
| GET | `/workshop/api/mechanic/service_requests` | Authenticated | pagination | Observed | Mechanic-scoped requests |
| GET | `/workshop/api/mechanic/service_request` | Authenticated | pagination | Source-discovered | Same view class as service_requests |
| POST | `/workshop/api/mechanic/service_request/{service_request_id}/comment` | Authenticated + mechanic role check | `service_request_id`, comment | Source-only | Explicit mechanic-role check |
| GET | `/workshop/api/mechanic/service_request/{service_request_id}/comment` | Authenticated | `service_request_id` | Source-only | Reads comments |
| PUT | `/workshop/api/mechanic/service_request/{service_request_id}` | Authenticated | `service_request_id`, status | Source-only | Updates status |
| GET | `/workshop/api/mechanic/service_request/{service_request_id}` | No JWT decorator observed | `service_request_id` | Source-only | Access requires runtime validation |
| GET | `/workshop/api/mechanic/download_report` | No JWT decorator observed | query `filename` | Source-only | Filename validation occurs before URL decoding |

### 7.2 Merchant Endpoints

| Method | Path | Access | Input / Identifier | Runtime | Notes |
|---|---|---|---|---|---|
| POST | `/workshop/api/merchant/contact_mechanic` | Authenticated | mechanic_api, optional repeat controls | Observed | Server performs GET to supplied `mechanic_api` |
| GET | `/workshop/api/merchant/service_requests/{vin}` | No JWT decorator observed | `vin`, pagination | Observed | Vehicle VIN is client-controlled identifier |

### 7.3 Management Endpoint

| Method | Path | Access | Input | Runtime | Notes |
|---|---|---|---|---|---|
| GET | `/workshop/api/management/users/all` | Authenticated | pagination | Source-only | No explicit admin-role check observed in reviewed view |

### 7.4 Shop Endpoints

| Method | Path | Access | Input / Identifier | Runtime | Notes |
|---|---|---|---|---|---|
| GET | `/workshop/api/shop/products` | Authenticated | pagination | Observed | Returns products and available credit |
| POST | `/workshop/api/shop/products` | Authenticated | name, price, image_url | Source-only | Creates product |
| POST | `/workshop/api/shop/orders` | Authenticated | product_id, quantity | Observed | Creates order |
| GET | `/workshop/api/shop/orders/{order_id}` | No JWT decorator observed | `order_id` | Observed | Retrieves order and backend payment information |
| PUT | `/workshop/api/shop/orders/{order_id}` | Authenticated | `order_id`, quantity/status | Source-only | Ownership check present |
| GET | `/workshop/api/shop/orders/all` | Authenticated | pagination | Observed | Returns current user's orders |
| POST | `/workshop/api/shop/orders/return_order` | Authenticated | query `order_id` | Observed | Ownership check present |
| GET | `/workshop/api/shop/return_qr_code` | Public at view layer | None | Observed during return workflow | Returns static QR image |
| POST | `/workshop/api/shop/apply_coupon` | Authenticated | coupon_code, amount | Source-discovered | Coupon code is used in a dynamically concatenated SQL query; credit increment uses request-supplied amount |

## 8. Chatbot Service API Inventory

The chatbot application mounts `chat_bp` below `/chatbot/genai`.

| Method | Path | Access / State | Input | Runtime | Notes |
|---|---|---|---|---|---|
| GET | `/chatbot/` | Public | None | Source-only | Root message |
| POST | `/chatbot/genai/init` | Session-based | openai_api_key | Source-only | Stores API key for session |
| POST | `/chatbot/genai/model` | Session-based | optional model_name | Source-only | Sets model |
| POST | `/chatbot/genai/ask` | Session API key required | message, optional id | Source-only | Also retrieves user JWT |
| GET | `/chatbot/genai/state` | Session-based | None | Observed | Returns initialization state and up to 20 messages |
| GET | `/chatbot/genai/history` | Session-based | None | Source-only | Returns up to 20 messages |
| POST | `/chatbot/genai/reset` | Session-based | None | Source-only | Deletes chat history |
| GET | `/chatbot/genai/health` | Public at route layer | None | Source-only | Returns OK |

## 9. Gateway Service API Inventory

The Go gateway uses `http.HandleFunc`, which does not itself restrict HTTP methods. The method shown below reflects intended use based on handler behavior and application calls.

| Expected Method | Path | Access | Input | Runtime | Notes |
|---|---|---|---|---|---|
| Any technically accepted | `/` | Public | None | Source-only | Returns gateway banner |
| GET-style | `/v1/vin/ownership` | HTTP Basic Auth | query `vin` | Backend/internal | Returns generated owner data including identity/contact information |
| POST-style | `/v1/payment` | HTTP Basic Auth | JSON order, user, amount | Backend/internal | Returns generated payment metadata |

The gateway credential values are hard-coded in source:

```text
vendorcrapi
Pa$$4Vendor_1
```

Their security impact is deferred to Phase 2.

## 10. Object Identifiers and User-Controlled Inputs

Important object references and user-controlled values identified during enumeration include:

| Service | Input / Identifier | Example Use |
|---|---|---|
| Identity | `carId` | Vehicle location lookup |
| Identity | `video_id` | Video read/update/delete/convert |
| Community | `postID` | Post retrieval and comments |
| Community | `limit`, `offset` | Pagination |
| Workshop | `service_request_id` | Service request retrieval, status and comments |
| Workshop | `vin` | Vehicle service-request lookup |
| Workshop | `report_id` | Mechanic report retrieval |
| Workshop | `filename` | Report download |
| Workshop | `mechanic_api` | Server-side mechanic contact request |
| Workshop | `order_id` | Order retrieval/update/return |
| Workshop | `product_id`, `quantity` | Order creation |
| Workshop | `coupon_code`, `amount` | Coupon application |
| Gateway | `vin` | Ownership lookup |
| Chatbot | `message`, `model_name` | LLM interaction |

These values should form the basis of targeted Phase 2 authorization, input validation and business-logic testing.

## 11. Security-Relevant Reconnaissance Observations

The following observations were identified during endpoint discovery. They are not classified as confirmed vulnerabilities in this Phase 1 workpaper.

### 11.1 Authorization Candidates

Several source-defined routes deserve explicit runtime authorization validation:

- `GET /workshop/api/shop/orders/{order_id}` has no JWT decorator in the reviewed view.
- `GET /workshop/api/mechanic/service_request/{service_request_id}` has no JWT decorator in the reviewed view.
- `GET /workshop/api/merchant/service_requests/{vin}` has no JWT decorator in the reviewed view.
- `GET /workshop/api/management/users/all` requires JWT but no explicit administrator-role check was observed.
- `DELETE /identity/api/v2/admin/videos/{video_id}` is outside the reviewed `/identity/management/admin/**` Spring role matcher.
- `POST /identity/management/user/apikey` is permitted by the Spring filter rule for `/management/user/**`.

### 11.2 Direct Object References

Multiple endpoints accept direct client-controlled object identifiers including `carId`, `video_id`, `postID`, `service_request_id`, `vin` and `order_id`.

Ownership and authorization controls must be validated per object type during Phase 2.

### 11.3 Server-Side URL Fetching

`POST /workshop/api/merchant/contact_mechanic` accepts a user-supplied `mechanic_api` URL and performs a server-side HTTP GET request to that URL.

This behavior is a Phase 2 server-side request validation candidate.

### 11.4 Coupon Input Handling

`POST /community/api/v2/coupon/validate-coupon` decodes arbitrary JSON into a generic BSON map.

`POST /workshop/api/shop/apply_coupon`:

- includes `coupon_code` in a dynamically concatenated SQL query
- separately retrieves the coupon from MongoDB
- increases the user's available credit using the request-supplied `amount`

These behaviors require input-validation and business-logic testing.

### 11.5 File and Video Operations

Profile and report functionality exposes several user-controlled file-related inputs:

- multipart image upload
- multipart video upload
- video identifiers
- video conversion
- report filename

The report-download implementation validates the encoded filename before applying URL decoding. File handling should be assessed separately during Phase 2.

### 11.6 Conditional Debug Interface

The Community service can register Go `pprof` handlers under `/debug/pprof/` when `DEBUG=1`.

Runtime exposure should be checked before any security conclusion is made.

### 11.7 Gateway Authentication and Sensitive Data

The Gateway service relies on HTTP Basic authentication with hard-coded credentials in source.

The ownership handler can return name, phone, email, SSN, address and registration information. The payment handler returns payment metadata including masked card number and expiry information.

Gateway reachability and credential exposure should be evaluated within the approved scope during Phase 2.

### 11.8 MailHog Workflow Dependency

MailHog acts as the simulated email system for application workflows.

During runtime exploration it was determined that:

- password-reset OTPs are delivered through MailHog
- vehicle VIN/PIN information used for vehicle onboarding is delivered through MailHog

A separate follow-up issue was created to update the application overview with these workflows. Unauthenticated MailHog access remains a security-relevant observation for later validation rather than a confirmed finding in this workpaper.

## 12. Commands Used

Repeated commands and equivalent variations are intentionally omitted. Each command family is listed once with its purpose.

### 12.1 Identity Route Discovery

```bash
grep -RInE '@(RequestMapping|GetMapping|PostMapping|PutMapping|DeleteMapping|PatchMapping)' services/identity/src
```

Used to locate Spring controller route annotations and build the Identity endpoint list.

### 12.2 Identity Security Rule Review

```bash
grep -RInE 'SecurityFilterChain|authorizeHttpRequests|requestMatchers|permitAll|hasRole|authenticated' services/identity/src/main/java/com/crapi
```

Used to identify Spring Security path matchers and classify public, authenticated and administrator-restricted routes.

### 12.3 Community Route Discovery

```bash
grep -RInE 'HandleFunc|Methods\(|PathPrefix|Router|NewRouter' services/community --exclude-dir=vendor
```

Used to locate Gorilla Mux route definitions and middleware registration.

```bash
sed -n '1,140p' services/community/api/router/routes.go
```

Used to review route registration and authentication middleware.

```bash
find services/community/api/controllers -maxdepth 1 -type f -print
```

Used to identify Community controller files.

```bash
grep -RInE 'func \(.*\) (GetPost|GetPostByID|AddNewPost|Comment|AddNewCoupon|ValidateCoupon|Home)' services/community/api/controllers
```

Used to locate the relevant handler implementations.

```bash
sed -n '1,170p' services/community/api/controllers/post_controller.go
sed -n '1,130p' services/community/api/controllers/coupon_controller.go
```

Used to identify request parameters, pagination behavior and body handling.

### 12.4 Workshop Route Discovery

```bash
find services/workshop -name "urls.py" -o -name "views.py"
```

Used to locate Django route and view files.

```bash
sed -n '1,220p' services/workshop/crapi/urls.py
sed -n '1,220p' services/workshop/crapi/mechanic/urls.py
sed -n '1,220p' services/workshop/crapi/merchant/urls.py
sed -n '1,220p' services/workshop/crapi/user/urls.py
sed -n '1,260p' services/workshop/crapi/shop/urls.py
```

Used to reconstruct the Workshop URL hierarchy.

```bash
grep -RInE '^class (SignUpView|ReceiveReportView|GetReportView|ServiceCommentView|ServiceRequestView|MechanicServiceRequestsView|DownloadReportView|MechanicView|ContactMechanicView|UserServiceRequestsView|AdminUserView|ProductView|OrderDetailsView|ReturnOrder|OrderControlView|ApplyCouponView|ReturnQRCodeView)' services/workshop/crapi
```

Used to locate the endpoint view implementations.

```bash
sed -n '40,430p' services/workshop/crapi/mechanic/views.py
sed -n '30,220p' services/workshop/crapi/merchant/views.py
sed -n '30,390p' services/workshop/crapi/shop/views.py
sed -n '30,100p' services/workshop/crapi/user/views.py
sed -n '350,430p' services/workshop/crapi/shop/views.py
```

Used to review HTTP methods, decorators, request fields, object references and authorization checks.

### 12.5 Chatbot Route Discovery

```bash
grep -RInE '@(app|bp)\.(route|get|post|put|delete|patch)|add_url_rule|Quart\(' services/chatbot
```

Used as the initial Quart route search.

```bash
sed -n '1,260p' services/chatbot/src/chatbot/app.py
sed -n '1,320p' services/chatbot/src/chatbot/chat_api.py
```

Used to review blueprint mounting, chatbot routes, session state, API-key handling and JWT retrieval.

### 12.6 Gateway Route Discovery

```bash
grep -RInE 'HandleFunc|Methods\(|PathPrefix|http\.HandleFunc|NewServeMux' services/gateway-service
```

Used to locate Go gateway HTTP handlers.

```bash
sed -n '1,240p' services/gateway-service/main.go
```

Used to review gateway authentication, request structures, sensitive response fields and handler behavior.

## 13. Evidence and Runtime Correlation

The API inventory uses two primary evidence classes:

| Evidence Type | Use |
|---|---|
| Source review | Complete route discovery, method identification, middleware/decorator review, request field identification and internal/backend routes |
| Burp HTTP history | Runtime confirmation of normal-user paths, methods, JWT usage, request bodies, object identifiers and response behavior |

Burp was intentionally used in a mostly passive mode during this task. Normal workflows were exercised using the built-in browser and the resulting HTTP history was inspected.

Runtime-observed traffic included representative requests across Identity, Community, Workshop, Shop and Chatbot functionality.

The Gateway service primarily participates in backend-to-backend communication and therefore is not expected to appear as a normal browser-originated API sequence in the same way as the reverse-proxied application routes.

Sensitive authorization tokens from Burp traffic should not be committed to the repository unless sanitized.

## 14. Limitations

The following limitations apply:

- The runtime pass focused on normal user functionality rather than deliberate attack traffic.
- Not every source-defined endpoint was manually invoked.
- Some endpoints are internal, role-specific, conditional or not directly exposed by the normal frontend workflow.
- Source-level access classification does not replace runtime authorization testing.
- No authorization bypass, ID manipulation, injection or business-logic exploit attempts were performed as part of this endpoint-inventory task.
- Swagger/OpenAPI/API-documentation endpoints were not provided by the client. Common documentation paths were tested at runtime against the main application and relevant service prefixes; all tested paths returned HTTP 404 and no exposed API-documentation endpoint was identified.
- Burp-captured JWT values and other sensitive request data are not reproduced in this document.

## 15. Follow-Up Actions

The following activities should be carried into later tasks:

- validate authentication boundaries
- validate role-based authorization
- test direct object references for BOLA/IDOR behavior
- assess server-side URL fetching
- assess coupon and business-logic handling
- assess file upload, download and conversion behavior
- validate conditional debug/profiling interfaces
- review Gateway reachability, credentials and trust boundaries
- update the application overview with MailHog-dependent password-reset and vehicle-onboarding workflows under Issue #33

## 16. Reconnaissance Summary

The crAPI application exposes a broad multi-service API surface across Identity, Community, Workshop, Chatbot and Gateway components.

White-box source review provided the most complete route inventory, while Burp Suite Community Edition was used to validate representative normal-user runtime behavior and confirm authentication context, methods, request bodies and client-controlled object identifiers.

The endpoint inventory identifies several high-value Phase 2 test areas, especially authorization boundaries, direct object references, server-side URL requests, coupon handling, file operations, versioned authentication endpoints and backend gateway trust.

No security-relevant item in this workpaper should be treated as a confirmed vulnerability solely on the basis of source discovery. Dedicated exploitability and impact validation remains part of the subsequent testing phase.
