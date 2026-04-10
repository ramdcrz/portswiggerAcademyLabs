# WEB APPLICATION PENETRATION TEST REPORT: DOM-BASED XSS (innerHTML)

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 16 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details the discovery of a DOM-based Cross-Site Scripting (XSS) vulnerability. The application insecurely handles data from the URL, writing it directly to the Document Object Model via an `innerHTML` sink, allowing for JavaScript execution.

### 2.1 Overall Risk Rating
**Rating: MEDIUM**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 1 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — DOM XSS via innerHTML Sink

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Medium |
| **Finding ID** | VULN-001 |
| **Affected Component** | `location.search` (Source) to `innerHTML` (Sink) |
| **CWE** | CWE-79: Improper Neutralization of Input During Web Page Generation |
| **CVSSv3 Score** | 6.1 (Medium) |
| **OWASP Category**| A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application uses the `innerHTML` property to render user-supplied search data from the URL. While modern browsers prevent standard `<script>` tags from executing via `innerHTML`, other HTML elements are still parsed. An attacker can inject an `<img>` tag with an invalid source and an `onerror` event handler to execute arbitrary JavaScript.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Reconnaissance**
Performed a search and inspected the resulting DOM, identifying that the search term is reflected inside a `span` element.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Inspecting the DOM to locate the search reflection point.</i></p>
</div>

**2. Exploitation (Event Handler Injection)**
Injected an event-based payload: `<img src=1 onerror=alert(1)>`. This initiates an image request that is guaranteed to fail, triggering the error handler.
<div align="center">
  <img src="./screenshots/payload.png" alt="XSS Payload" width="85%">
  <p><i><b>Figure 2:</b> Entering the img-based XSS payload.</i></p>
</div>

**3. Execution & Verification**
The browser attempted to load the invalid image. Upon failure, the `onerror` handler fired, executing the `alert(1)` function.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 3:</b> The browser executing the JavaScript alert via the onerror event.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 4:</b> Final confirmation of the solved lab.</i></p>
</div>

#### 3.3 Business Impact
DOM XSS executes entirely on the client side, bypassing many server-side security controls. It can be utilized to hijack user sessions, perform unauthorized actions, or redirect users to malicious domains.

#### 3.4 Remediation
Use `textContent` or `innerText` instead of `innerHTML`. These properties strictly treat input as literal text, preventing the browser from parsing injected HTML tags or executing associated event handlers.

#### 3.5 References
* **CWE-79:** https://cwe.mitre.org/data/definitions/79.html
* **OWASP DOM-based XSS Prevention:** https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html