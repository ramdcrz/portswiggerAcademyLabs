# WEB APPLICATION PENETRATION TEST REPORT: DOM-BASED XSS (jQuery href)

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 16 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report outlines a DOM-based Cross-Site Scripting (XSS) vulnerability caused by the insecure use of jQuery to dynamically update an anchor tag's `href` attribute based on user-supplied URL parameters.

### 2.1 Overall Risk Rating
**Rating: MEDIUM**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 1 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — DOM XSS via jQuery Attribute Manipulation

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Medium |
| **Finding ID** | VULN-001 |
| **Affected Component** | `location.search` (Source) to jQuery `attr('href')` (Sink) |
| **CWE** | CWE-79: Improper Neutralization of Input During Web Page Generation |
| **CVSSv3 Score** | 6.1 (Medium) |
| **OWASP Category**| A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application uses jQuery to read the `returnPath` parameter from the URL and dynamically set the destination of a "Back" link. Because the input is not validated against a list of safe protocols, an attacker can supply a JavaScript pseudo-protocol payload that executes when the link is clicked.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Reconnaissance**
Navigated to the feedback page and modified the `returnPath` parameter, observing changes to the destination of the "Back" link.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Locating the returnPath parameter and the associated back link.</i></p>
</div>

**2. Source and Sink Analysis**
Inspected the DOM to confirm that the input was placed directly into the `href` attribute of the anchor tag via jQuery manipulation.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Source/Sink Analysis" width="85%">
  <p><i><b>Figure 2:</b> Inspecting the href attribute to confirm the reflection of the returnPath.</i></p>
</div>

**3. Exploitation (Pseudo-Protocol Injection)**
Replaced the `returnPath` value with `javascript:alert(document.cookie)`.
<div align="center">
  <img src="./screenshots/payload.png" alt="XSS Payload" width="85%">
  <p><i><b>Figure 3:</b> Crafting the javascript: alert payload in the URL.</i></p>
</div>

**4. Execution & Verification**
Upon clicking the "Back" link, the browser executed the JavaScript context of the `href` attribute, exposing the session cookies.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> The alert box displaying document.cookie upon clicking the link.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of the solved lab.</i></p>
</div>

#### 3.3 Business Impact
Exploitation allows for session hijacking via cookie theft. Because the payload is delivered via a URL parameter, it can be distributed through phishing campaigns.

#### 3.4 Remediation
Validate that the `returnPath` starts with an expected safe character (like a single `/` for relative paths) and strictly reject any input containing pseudo-protocols such as `javascript:` or `data:`.

#### 3.5 References
* **CWE-79:** https://cwe.mitre.org/data/definitions/79.html
* **OWASP DOM-based XSS Prevention:** https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html