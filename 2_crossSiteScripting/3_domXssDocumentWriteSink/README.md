# WEB APPLICATION PENETRATION TEST REPORT: DOM-BASED XSS

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 15 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details a DOM-based Cross-Site Scripting (XSS) vulnerability caused by insecure client-side JavaScript processing data from the URL and writing it to the Document Object Model.

### 2.1 Overall Risk Rating
**Rating: MEDIUM**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 1 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — DOM XSS in document.write Sink

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Medium |
| **Finding ID** | VULN-001 |
| **Affected Component**| `location.search` (Source) to `document.write` (Sink) |
| **CWE** | CWE-79: Improper Neutralization of Input During Web Page Generation |
| **CVSSv3 Score** | 6.1 (Medium) |
| **OWASP Category**| A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application utilizes a `document.write` sink to output data obtained from `location.search` without proper sanitization. The script writes an `<img>` tag for tracking: `document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');`. An attacker can manipulate the query string to escape the image attribute and inject executable HTML.

#### 3.2 Proof of Concept (PoC)
**1. Identification**
Inspected the DOM to observe the input reflected inside the `src` attribute.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Inspecting the DOM for the sink.</i></p>
</div>

**2. Exploitation & Verification**
Crafted the payload `"><svg onload=alert(1)>` to break out of the `<img>` tag and initialize a new SVG element with an auto-executing event handler.
<div align="center">
  <img src="./screenshots/payload.png" alt="XSS Payload" width="85%">
  <p><i><b>Figure 2:</b> Entering the breakout payload.</i></p>
</div>
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 3:</b> Execution of the injected SVG script.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 4:</b> Lab completion confirmation.</i></p>
</div>

#### 3.3 Business Impact
DOM XSS executes entirely on the client side. Web Application Firewalls (WAFs) monitoring server traffic may not detect the attack. It leads to the same impact as Reflected XSS (session hijacking, unauthorized actions).

#### 3.4 Remediation
Avoid using dangerous sinks like `document.write()`. Use safer alternatives such as `textContent` or `innerText`, which treat input strictly as plain text.

#### 3.5 References
* **CWE-79:** https://cwe.mitre.org/data/definitions/79.html
* **OWASP DOM-based XSS Prevention Cheat Sheet:** https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html