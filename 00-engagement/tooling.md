# Application Security Assessment Tooling

## Important Info

> This document is part of a simulated white-box security assessment of OWASP crAPI for training and community reference. The client, contacts, scope details and engagement artifacts are fictitious.
>
> All security testing is performed only against the user's own controlled local lab environment using OWASP crAPI and other intentionally vulnerable training components. No production systems, third-party systems, public targets or unauthorized assets are involved. The project is strictly for study, education and hands-on learning.

## Document Control

| Field | Details |
|---|---|
| Document Title | Application Security Assessment Tooling |
| Document Type | Engagement Planning / Assessment Methodology |
| Engagement | crAPI Application Security Assessment |
| Client Representative | Mr. Mario |
| Assessment Lead | Mr. Wario |
| Environment | UAT / Local Lab |
| Document Owner | Mr. Wario |
| Version | 0.1 |
| Status | Draft |
| Date Created | 22 September 2026 |
| Classification | Engagement Confidential / Training Simulation |
| Recommended Repository Location | `00-engagement/tooling.md` |

## Document Revision History

| Version | Date | Author | Description |
|---|---|---|---|
| 0.1 | 22 September 2026 | Mr. Wario | Initial assessment tooling inventory created |

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Tooling Principles](#3-tooling-principles)
- [4. Core Assessment Tools](#4-core-assessment-tools)
- [5. Supporting and Discovery Tools](#5-supporting-and-discovery-tools)
- [6. White-Box and Source Review Tools](#6-white-box-and-source-review-tools)
- [7. Optional / Conditional Tools](#7-optional--conditional-tools)
- [8. Tool Selection by Assessment Activity](#8-tool-selection-by-assessment-activity)
- [9. Evidence and Output Handling](#9-evidence-and-output-handling)
- [10. Limitations and Usage Notes](#10-limitations-and-usage-notes)
- [11. Tooling Summary](#11-tooling-summary)

## 1. Purpose

Define the tools that may be used during the OWASP crAPI Application Security and API Security assessment, explain the purpose of each tool, and establish when each tool is appropriate.

The toolset is intentionally **manual-first**. Automated tools support the assessment, but they do not replace understanding normal application behavior, establishing a valid baseline, and manually validating security-relevant behavior.

This document is expected to evolve as the assessment moves from reconnaissance into authentication, authorization, input validation, business-logic testing, findings validation and retesting.

## 2. Scope

The tooling inventory applies to the approved local OWASP crAPI lab and its supporting services.

The assessment may include:

- web application testing;
- REST API testing;
- authentication and session testing;
- authorization testing;
- parameter and endpoint discovery;
- technology fingerprinting;
- source-code review;
- request/response analysis;
- evidence collection;
- controlled proof-of-concept validation;
- remediation verification and retesting.

No tool should be used against systems outside the approved local lab.

## 3. Tooling Principles

The following principles guide tool usage:

- understand normal application behavior before testing abnormal behavior;
- prefer manual validation before automated scanning;
- use the minimum number of requests necessary to demonstrate behavior;
- avoid destructive, denial-of-service or unnecessary high-volume testing;
- retain raw evidence where useful for reproducibility;
- distinguish tool output from confirmed findings;
- use multiple evidence sources when practical;
- use source review to explain or confirm runtime behavior rather than replacing runtime validation;
- introduce specialist tools only when they add value to a specific test.

## 4. Core Assessment Tools

| Tool | Brief Overview | Primary Use |
|---|---|---|
| **Burp Suite Community Edition** | Primary web and API interception platform used to observe, modify and replay HTTP requests and responses. | Main manual testing platform |
| **Burp Proxy / HTTP History** | Captures browser-to-application traffic without interrupting normal application use. | Request/response discovery and evidence |
| **Burp Repeater** | Allows individual HTTP requests to be modified and resent repeatedly in a controlled way. | Authentication, authorization, input and business-logic testing |
| **Burp Browser** | Browser preconfigured to route traffic through Burp. | Normal workflow observation and request capture |
| **Postman** | API client for organizing reusable API requests, environments, variables and collections. | Repeatable API workflows, User A/User B sessions and regression tests |
| **curl** | Command-line HTTP client suitable for small, reproducible API requests. | Quick validation and documented command-line reproduction |
| **Browser Developer Tools** | Browser-native inspection of network traffic, Local Storage, Session Storage, cookies, JavaScript and frontend behavior. | Client-side state and application behavior analysis |
| **Git / GitHub** | Version control and project-management platform used for workpapers, evidence references, issue tracking and assessment history. | Documentation, evidence tracking and project management |

## 5. Supporting and Discovery Tools

| Tool | Brief Overview | Primary Use |
|---|---|---|
| **Arjun** | Discovers hidden HTTP parameters by testing candidate query and body parameter names. | Parameter discovery after baseline endpoint mapping |
| **ffuf** | Fast web fuzzer commonly used for focused content, endpoint or parameter discovery. | Targeted discovery when known inventory has gaps |
| **httpx** | HTTP probing utility that can quickly validate hosts, services, response codes, titles and common HTTP characteristics. | Reconnaissance and HTTP validation |
| **Nmap** | Network and service discovery tool used to identify listening ports, protocols and service versions. | Service enumeration; already used in reconnaissance |
| **WhatWeb** | Web technology fingerprinting utility. | Technology and framework identification |
| **jq** | Command-line JSON parser and filter. | Reading large API responses and extracting fields, IDs and values |
| **OpenSSL** | General cryptographic and TLS command-line toolkit. | Certificate and TLS inspection |
| **ripgrep / grep** | Fast text-search tools for source files and captured output. | Route, configuration, security-rule and source review |

## 6. White-Box and Source Review Tools

| Tool | Brief Overview | Primary Use |
|---|---|---|
| **Source-code review** | Manual inspection of application source, configuration and dependency files. | Confirming routes, access-control logic and implementation behavior |
| **ripgrep (`rg`)** | Fast recursive search across source trees. | Finding endpoints, role checks, object IDs, token handling and sensitive functions |
| **grep** | Standard Unix text-search utility. | Small targeted searches |
| **sed / awk** | Text-processing utilities used for extracting and reviewing selected portions of files or output. | Controlled parsing and evidence preparation |
| **Python** | Used for small custom helpers, response comparison and controlled data processing when manual tools become inefficient. | Lightweight custom analysis |

Non-obvious parsing or transformation commands should be explained before use so that the assessment remains learning-focused.

## 7. Optional / Conditional Tools

These tools are not required for every test and should be introduced only when the target or test case justifies them.

| Tool | Brief Overview | When to Consider It |
|---|---|---|
| **Nuclei** | Template-driven vulnerability scanner. | Selective validation after manual mapping, not as the primary testing method |
| **OWASP ZAP** | Web proxy/scanner with interception and automated testing capabilities. | Secondary validation or comparison with Burp |
| **mitmproxy** | Scriptable intercepting proxy. | When programmatic traffic manipulation is useful |
| **Schemathesis** | Property-based API testing tool driven by OpenAPI schemas. | If a usable OpenAPI specification becomes available |
| **Kiterunner** | API endpoint/content discovery tool designed for API routes and wordlists. | If endpoint inventory remains incomplete after source and runtime mapping |
| **JWT inspection tools** | Utilities for decoding and reviewing JWT headers and claims. | Token analysis; manual Base64URL inspection remains preferred for learning |
| **GraphQL-specific tools** | Tools designed for schema discovery and GraphQL security testing. | Only if GraphQL is discovered |
| **SQLMap** | Automated SQL injection testing tool. | Only after a manually identified SQL injection candidate requires controlled validation |

Optional tools should not be introduced merely because they exist or appear in a course. They should solve a specific testing need.

## 8. Tool Selection by Assessment Activity

| Assessment Activity | Preferred Tools |
|---|---|
| Normal application baseline | Burp Browser, Burp Proxy, Browser DevTools |
| API endpoint inventory | Burp, source review, curl, Postman |
| Service enumeration | Nmap, curl, OpenSSL |
| Technology fingerprinting | WhatWeb, HTTP headers, source review |
| Authentication/session testing | Burp Repeater, Browser DevTools, curl |
| JWT inspection | Manual Base64URL decoding, Burp, CLI tools |
| Authorization/BOLA testing | Burp Repeater, Postman, two-user test accounts |
| BFLA testing | Burp Repeater, source review |
| BOPLA/property testing | Burp Repeater, Postman, source review |
| Hidden parameter discovery | Arjun, Burp |
| Endpoint discovery | Existing inventory first; ffuf/Kiterunner if required |
| JSON response analysis | jq |
| Source review | ripgrep, grep, sed, IDE/editor |
| Controlled automation | Python |
| Secondary vulnerability validation | Nuclei or ZAP where justified |
| Retesting | Burp Repeater, Postman, curl |

## 9. Evidence and Output Handling

Raw outputs that materially support assessment conclusions should be saved under:

```text
01-recon/raw/
```

during reconnaissance, or the corresponding later-phase evidence location when formal testing begins.

Examples include:

- Nmap output;
- HTTP header captures;
- technology-fingerprinting output;
- source-derived route lists;
- Burp XML exports;
- selected command output;
- API documentation discovery results.

Screenshots should remain under the appropriate `screenshots/` or later evidence location and should support, rather than replace, raw request/response evidence.

The assessment workpaper should reference the evidence rather than reproduce excessive raw output.

## 10. Limitations and Usage Notes

- Tool output alone does not establish a vulnerability.
- Automated scanners may produce false positives or miss business-logic and authorization weaknesses.
- Burp Suite Community Edition remains the primary manual testing platform for the current engagement.
- Postman is intended to complement Burp by maintaining structured API workflows and separate test-user environments.
- Discovery tools such as Arjun, ffuf and Kiterunner should be used only after the existing endpoint inventory has been reviewed.
- High-volume, destructive or denial-of-service testing is outside the intended methodology.
- Tool usage may be expanded as later assessment phases introduce specific requirements.

## 11. Tooling Summary

The core working toolset for the crAPI assessment is:

```text
Burp Suite
Postman
curl
Browser Developer Tools
jq
Arjun
ffuf
Nmap
WhatWeb
OpenSSL
ripgrep / grep
Git / GitHub
Python
```

The assessment remains **manual-first and evidence-driven**. Specialist or automated tools will be introduced only when they provide clear value to a defined test case.
