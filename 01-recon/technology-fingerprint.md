# Technical Fingerprint

## Important Info
> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.

## Document Control

| Field | Details |
|---|---|
| Document Title | Technical Fingerprint |
| Document Type | Phase 1 Reconnaissance |
| Engagement | crAPI Application Security Assessment |
| Client Representative | Mr. Mario |
| Assessment Lead | Mr. Wario |
| Environment | UAT |
| Related GitHub Issue | Issue #5 - Fingerprint technologies and application components |
| Document Owner | Mr. Wario |
| Version | 0.9 |
| Status | Draft - Ready for Review |
| Date Created | 18 September 2026 |
| Classification | Engagement Confidential / Training Simulation |
| Repository Location | `01-recon/technology-fingerprint.md` |
| Evidence Location | `01-recon/raw/` |
| Screenshot Location | `01-recon/screenshots/technology-fingerprint/` |

## Document Revision History

| Version | Date | Author | Description |
|---|---|---|---|
| 0.1 | 18 September 2026 | Mr. Wario | Initial technical fingerprint structure created |
| 0.9 | 18 September 2026 | Mr. Wario | Added verified application technologies, frameworks, component versions, reverse-proxy configuration, client-side clues, commands used and security-relevant observations |

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Fingerprinting Methodology](#3-fingerprinting-methodology)
- [4. Technology Stack Summary](#4-technology-stack-summary)
- [5. Frontend Technology](#5-frontend-technology)
- [6. Web Server and Reverse Proxy](#6-web-server-and-reverse-proxy)
- [7. Identity Service](#7-identity-service)
- [8. Community Service](#8-community-service)
- [9. Workshop Service](#9-workshop-service)
- [10. Chatbot Service](#10-chatbot-service)
- [11. Gateway Service](#11-gateway-service)
- [12. Database and Supporting Technologies](#12-database-and-supporting-technologies)
- [13. Client-Side Application Clues](#13-client-side-application-clues)
- [14. Security-Relevant Reconnaissance Observations](#14-security-relevant-reconnaissance-observations)
- [15. Commands Used](#15-commands-used)
- [16. Evidence Index](#16-evidence-index)
- [17. Limitations](#17-limitations)
- [18. Follow-Up Actions](#18-follow-up-actions)
- [19. Reconnaissance Summary](#19-reconnaissance-summary)

## 1. Purpose

Identify and document the technologies, frameworks, application components and observable versions used by the approved crAPI deployment in order to establish the application's technical attack surface and support subsequent application and API security testing.

This workpaper records technology information obtained through live-service fingerprinting and white-box review of application source files, dependency manifests, Docker configuration and reverse-proxy configuration.

Security-relevant characteristics identified during this activity are recorded as reconnaissance observations only. Their exploitability and impact will be validated during subsequent assessment phases.

## 2. Scope

Technical fingerprinting was limited to the approved crAPI application environment and its associated Docker-hosted application components.

The assessment focused on:

- crAPI web frontend
- OpenResty reverse-proxy layer
- Identity service
- Community service
- Workshop service
- Chatbot service
- API gateway service
- Application databases and supporting services
- Application source and configuration available through the white-box assessment

General Kali Linux host services unrelated to the crAPI deployment are outside the scope of this workpaper.

## 3. Fingerprinting Methodology

Technical fingerprinting used a combination of live-service inspection and white-box source review.

Activities included:

- HTTP and HTTPS response-header inspection
- WhatWeb fingerprinting
- Correlation with previously collected service-enumeration data
- Docker container and image inspection
- Review of application dependency manifests
- Review of Dockerfiles
- Review of OpenResty/nginx reverse-proxy configuration
- Review of identity-service runtime configuration
- Inspection of frontend JavaScript references and bundled application code
- Correlation of source-derived technology information with observed runtime behavior

Where possible, runtime observations and source evidence were compared before a technology or version was recorded as confirmed.

## 4. Technology Stack Summary

| Component | Technology / Framework | Version / Detail | Evidence Basis |
|---|---|---|---|
| Web frontend | React | 18.3.x | `package.json` |
| Frontend build | Node.js | 20 build image | Web Dockerfile |
| Frontend language/tooling | TypeScript | 4.9.5 | `package.json` |
| UI framework | Ant Design | 5.20.3 | `package.json` |
| State management | Redux / Redux Toolkit | 5.0.1 / 2.2.7 | `package.json` |
| Client routing | React Router | 6.26.1 | `package.json` |
| Web server / reverse proxy | OpenResty | 1.27.1.2 observed | Runtime headers / WhatWeb |
| Identity service | Java / Spring Boot | Java 17 / Spring Boot 3.2.2 | Gradle configuration |
| Identity security | Spring Security | Spring Boot security stack | Gradle configuration |
| Community service | Go | 1.21 | `go.mod` / Dockerfile |
| Community router | Gorilla Mux | 1.7.4 | `go.mod` |
| Workshop service | Python / Django | Django 4.1.13 | `requirements.txt` |
| Workshop API | Django REST Framework | 3.14.0 | `requirements.txt` |
| Chatbot service | Python / Quart | Quart 0.20.0 | `requirements.txt` |
| AI orchestration | LangChain / LangGraph | 0.3.x / 0.5.1 | `requirements.txt` |
| Gateway service | Go | 1.21 build image | Dockerfile |
| Relational database | PostgreSQL | PostgreSQL 14 container | Docker inventory |
| Document database | MongoDB | MongoDB 4.4 container | Docker inventory |
| Vector database | ChromaDB | Containerized service | Docker inventory |
| Mail testing service | MailHog | crAPI packaged image | Docker inventory |

## 5. Frontend Technology

The crAPI frontend is implemented as a React application.

The application manifest identifies:

- React `^18.3.0`
- React DOM `^18.3.0`
- React Router DOM `^6.26.1`
- Redux `^5.0.1`
- Redux Toolkit `^2.2.7`
- Redux Saga `^1.3.0`
- Ant Design `^5.20.3`
- TypeScript `^4.9.5`
- React Scripts `5.0.1`
- SuperAgent `^8.1.2`

The application is built using a Node.js 20 container and the generated static content is copied into an OpenResty container for serving.

The live frontend references the production JavaScript bundle:

```text
/static/js/main.8c78208c.js
```

The browser-facing application was observed with the page title:

```text
crAPI
```

## 6. Web Server and Reverse Proxy

The crAPI frontend and reverse-proxy layer is implemented using OpenResty.

Live HTTP and HTTPS responses returned:

```text
Server: openresty/1.27.1.2
```

WhatWeb independently identified:

```text
OpenResty[1.27.1.2]
HTTPServer[openresty/1.27.1.2]
```

The OpenResty configuration routes requests to backend application services based on URI path.

| External Path | Backend Component |
|---|---|
| `/community/` | Community service |
| `/identity/` | Identity service |
| `/workshop/` | Workshop service |
| `/chatbot/` | Chatbot service |
| `/mailhog/` | MailHog |
| `/.well-known/jwks.json` | Identity-service JWKS endpoint |

The reverse proxy also performs request and response rewriting so that internal service references can be translated into externally accessible application URLs.

## 7. Identity Service

The identity service is implemented in Java using Spring Boot.

Source configuration confirms:

- Java 17
- Gradle-based build process
- Spring Boot `3.2.2`
- Spring Boot Web
- Spring Security
- Spring Data JPA
- Spring Mail
- Spring Validation
- PostgreSQL JDBC integration
- JJWT `0.12.5`
- Nimbus JOSE JWT `9.37.3`

The application is packaged as:

```text
identity-service-1.0-SNAPSHOT.jar
```

The identity service uses PostgreSQL as its relational datastore.

Authentication-related configuration includes:

```text
app.jwksJson=${JWKS}
app.jwtExpiration=${JWT_EXPIRATION:604800000}
```

The default JWT expiration value corresponds to seven days.

The reverse proxy exposes the identity service JSON Web Key Set through:

```text
/.well-known/jwks.json
```

## 8. Community Service

The community service is implemented in Go.

The Docker build uses Go `1.21`, and the application module also identifies Go `1.21`.

Major dependencies include:

- Gorilla Mux `1.7.4`
- GORM `1.9.14`
- MongoDB Go Driver `1.3.5`
- `jwt-go` `3.2.0`
- PostgreSQL driver support

The container configuration also explicitly exposes TCP port `6060` as a profiling port.

No conclusion regarding external accessibility of the profiling interface is made in this workpaper. Exposure and security impact should be validated separately if applicable.

## 9. Workshop Service

The workshop service is implemented in Python using Django.

Identified technologies include:

- Django `4.1.13`
- Django REST Framework `3.14.0`
- django-cors-headers `4.0.0`
- PyJWT `2.7.0`
- psycopg2 `2.9.9`
- PyMongo `3.13.0`
- Gunicorn `21.2.0`
- Werkzeug `2.0.3`

The service includes libraries for both PostgreSQL and MongoDB interaction.

## 10. Chatbot Service

The chatbot service is implemented in Python and uses the Quart asynchronous web framework.

Identified components include:

- Quart `0.20.0`
- Quart-CORS `0.8.0`
- LangChain `0.3.26`
- LangGraph `0.5.1`
- OpenAI Python client `1.77.0`
- PyMongo `4.12.1`
- Motor `3.7.1`
- ChromaDB
- FAISS
- PostgreSQL client support
- FastMCP `2.10.2`

The chatbot therefore introduces additional AI/LLM-related application functionality and dependencies that may require dedicated security testing during later assessment activities.

## 11. Gateway Service

The API gateway service is implemented in Go.

The service is built using a Go `1.21` container and runs within a Debian Bookworm-based runtime image.

The service exposes HTTPS on port `443`.

During image creation, a certificate is generated for names including:

```text
127.0.0.1
gateway-service
api.mypremiumdealership.com
mypremiumdealership.com
```

The identity service contains configuration for communication with this gateway.

## 12. Database and Supporting Technologies

The Docker deployment contains the following supporting technologies:

| Technology | Role |
|---|---|
| PostgreSQL 14 | Relational application database |
| MongoDB 4.4 | Document-oriented database |
| ChromaDB | Vector database / AI-supporting datastore |
| MailHog | Development/testing email service |

Database ports were observed as Docker-internal services during service enumeration and were not identified as directly published to the host.

## 13. Client-Side Application Clues

Review of the production JavaScript bundle identified application route patterns including:

```text
/community/posts
/community/posts/<postId>
/community/posts/<postId>/comment
/community/posts/recent
```

These references confirm that the frontend exposes API route information associated with the community service.

Full API endpoint discovery and inventory is intentionally deferred to the dedicated API-enumeration activity in Phase 1.

## 14. Security-Relevant Reconnaissance Observations

The following items were identified during fingerprinting and should be carried forward for validation.

These observations are not classified as confirmed vulnerabilities at this stage.

### 14.1 HTTP Security Headers

The sampled HTTP and HTTPS frontend responses exposed the OpenResty `Server` header.

The following security headers were not observed in the sampled responses:

- `Strict-Transport-Security`
- `Content-Security-Policy`
- `X-Frame-Options`
- `X-Content-Type-Options`
- `Referrer-Policy`
- `Permissions-Policy`

Header behavior should be tested across relevant application routes before determining security impact.

### 14.2 TLS Protocol Configuration

The OpenResty configuration includes:

```text
ssl_protocols TLSv1.3 TLSv1.2 TLSv1.1;
```

This indicates configuration-level support for TLS 1.1 at the reverse-proxy layer.

Actual protocol negotiation and security impact should be validated during Phase 2.

The identity service configuration separately specifies:

```text
server.ssl.enabled-protocols=TLSv1.2,TLSv1.3
```

### 14.3 Upstream TLS Certificate Verification

The OpenResty SSL configuration includes:

```text
proxy_ssl_verify off;
```

for multiple upstream application services.

This disables certificate verification for affected reverse-proxy-to-backend TLS connections.

The actual threat model and practical impact should be assessed before treating this as a formal finding.

### 14.4 Gateway Credentials in Application Configuration

The identity application configuration contains:

```text
api.gateway.username=vendorcrapi
api.gateway.password=Pa$$4Vendor_1
```

These values represent application credentials stored directly in source-controlled configuration.

Runtime usage, privilege level, exposure and impact should be validated during Phase 2 before classification as a finding.

### 14.5 Shell-Injection Feature Flag

The identity service contains the configuration property:

```text
app.enable_shell_injection=${ENABLE_SHELL_INJECTION}
```

This suggests application behavior associated with shell-injection functionality is controlled through an environment variable.

The implementation and runtime state should be reviewed during Phase 2.

### 14.6 Request and Upload Size Configuration

OpenResty is configured with:

```text
client_max_body_size 50M;
```

The identity service additionally defines:

```text
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=50MB
```

These limits should be considered during later file-upload and resource-consumption testing.

### 14.7 Client-Side Proxy Configuration

The frontend package configuration contains:

```text
"proxy": "https://crapi.allvapps.com"
```

This is recorded as a development/build configuration clue.

No conclusion is made regarding runtime use of this address without further validation.

## 15. Commands Used

The following commands were used during technical fingerprinting. Repeated commands and equivalent variations are omitted for brevity.

### 15.1 HTTP and HTTPS Inspection

```bash
curl -sSI http://127.0.0.1:8888
```

```bash
curl -skSI https://127.0.0.1:8443
```

```bash
curl -sS http://127.0.0.1:8888 -o 01-recon/raw/tech-index.html
```

### 15.2 Technology Fingerprinting

```bash
whatweb -a 3 http://127.0.0.1:8888
```

### 15.3 Frontend JavaScript Identification

```bash
grep -oE '<script[^>]+src="[^"]+"' 01-recon/raw/tech-index.html
```

```bash
curl -sS "http://127.0.0.1:8888/static/js/main.8c78208c.js" \
  -o 01-recon/raw/tech-main-js-bundle.js
```

```bash
grep -Eo '/(identity|community|workshop|chatbot|mailhog)/[^"'"'"' ]*' \
  01-recon/raw/tech-main-js-bundle.js | sort -u
```

### 15.4 Source Manifest Discovery

```bash
find . -type f \( \
-name "package.json" \
-o -name "pom.xml" \
-o -name "build.gradle" \
-o -name "requirements.txt" \
-o -name "pyproject.toml" \
-o -name "go.mod" \
-o -name "Dockerfile" \
\) | sort
```

### 15.5 Dependency and Framework Review

```bash
cat services/web/package.json
cat services/community/go.mod
cat services/gateway-service/go.mod
cat services/chatbot/requirements.txt
cat services/workshop/requirements.txt
cat services/identity/build.gradle.kts
cat services/identity/gradle.properties
```

### 15.6 Dockerfile Review

```bash
cat services/identity/Dockerfile
cat services/web/Dockerfile
cat services/community/Dockerfile
cat services/gateway-service/Dockerfile
```

### 15.7 Identity Configuration Review

```bash
cat services/identity/src/main/resources/application.properties
```

### 15.8 Reverse-Proxy Configuration Review

```bash
sed -n '1,240p' services/web/nginx.conf.template
sed -n '1,260p' services/web/nginx.ssl.conf.template
```

### 15.9 Security Header Validation

```bash
for url in \
  http://127.0.0.1:8888 \
  https://127.0.0.1:8443
do
  echo "===== $url ====="
  curl -skI "$url" | grep -Ei \
  'server:|strict-transport-security|content-security-policy|x-frame-options|x-content-type-options|referrer-policy|permissions-policy|access-control|set-cookie'
  echo
done
```

### 15.10 Evidence Preservation

Relevant dependency manifests and configuration files were copied into `01-recon/raw/` using standard `cp` commands for preservation as assessment evidence.

## 16. Evidence Index

| Evidence ID | Description | Repository Path |
|---|---|---|
| TECH-EV-001 | HTTP response headers | `01-recon/raw/tech-http-headers.txt` |
| TECH-EV-002 | HTTPS response headers | `01-recon/raw/tech-https-headers.txt` |
| TECH-EV-003 | WhatWeb fingerprint | `01-recon/raw/tech-whatweb.txt` |
| TECH-EV-004 | Frontend HTML | `01-recon/raw/tech-index.html` |
| TECH-EV-005 | Frontend JavaScript references | `01-recon/raw/tech-js-references.txt` |
| TECH-EV-006 | Frontend application manifest | `01-recon/raw/tech-web-package.json` |
| TECH-EV-007 | Identity Gradle configuration | `01-recon/raw/tech-identity-build.gradle.kts` |
| TECH-EV-008 | Community Go module | `01-recon/raw/tech-community-go.mod` |
| TECH-EV-009 | Gateway Go module | `01-recon/raw/tech-gateway-go.mod` |
| TECH-EV-010 | Chatbot dependency manifest | `01-recon/raw/tech-chatbot-requirements.txt` |
| TECH-EV-011 | Workshop dependency manifest | `01-recon/raw/tech-workshop-requirements.txt` |
| TECH-EV-012 | OpenResty HTTP configuration | `01-recon/raw/tech-nginx.conf.template` |
| TECH-EV-013 | OpenResty SSL configuration | `01-recon/raw/tech-nginx.ssl.conf.template` |
| TECH-EV-014 | Security-header validation | `01-recon/raw/tech-security-headers.txt` |
| TECH-EV-015 | Identity runtime configuration | `01-recon/raw/tech-identity-application.properties` |
| TECH-EV-016 | Production frontend JavaScript bundle | `01-recon/raw/tech-main-js-bundle.js` |
| TECH-EV-017 | Client-side API route clues | `01-recon/raw/tech-js-endpoint-clues.txt` |

## 17. Limitations

Technology fingerprinting represents the observed state of the approved assessment environment at the time of testing.

The following limitations apply:

- Not every transitive application dependency was reviewed.
- Source-defined versions may differ from runtime versions where dependency resolution permits version variation.
- Client-side JavaScript inspection during this activity was limited to technology and route clues rather than complete API enumeration.
- Security configuration observations have not yet undergone dedicated exploitability or impact validation.
- Docker-internal components were identified primarily through deployment and source information rather than direct external probing.

## 18. Follow-Up Actions

The following activities should be carried forward into subsequent assessment tasks:

- Build the complete API endpoint inventory.
- Map authentication and JWT behavior.
- Map authorization boundaries.
- Enumerate hidden and versioned endpoints.
- Document Docker service architecture and trust boundaries.
- Validate security-header behavior across application routes.
- Test negotiated TLS protocols and cipher configuration.
- Review the security impact of disabled upstream certificate verification.
- Evaluate gateway credential handling and privilege.
- Review the shell-injection feature implementation and runtime state.
- Evaluate profiling/debug interfaces where relevant.
- Assess AI/chatbot-specific attack surfaces during applicable Phase 2 testing.

## 19. Reconnaissance Summary

The crAPI deployment uses a multi-service architecture implemented across several technology stacks.

The browser-facing application consists of a React frontend served by OpenResty, which also performs reverse-proxy routing to Java, Go, Python, Django, Quart and supporting services.

The identity service uses Spring Boot and JWT/JWKS-based authentication components. The community and gateway services are implemented in Go, while the workshop and chatbot services are Python-based. PostgreSQL, MongoDB, ChromaDB and MailHog provide supporting storage and messaging functionality.

Several security-relevant configuration characteristics were identified during fingerprinting, including TLS configuration, upstream certificate-verification behavior, absent sampled security headers, embedded gateway credentials and configurable shell-injection functionality.

These observations provide important context for later testing but require dedicated Phase 2 validation before they are treated as confirmed security findings.
