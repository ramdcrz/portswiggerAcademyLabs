# WEB APPLICATION PENETRATION TEST REPORT: STORED CROSS-SITE SCRIPTING

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 15 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This assessment identified a high-severity Stored Cross-Site Scripting (XSS) vulnerability in the blog comment functionality, allowing persistent JavaScript execution against any user viewing the affected page.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Stored XSS via Blog Comments

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected URL** | `/post/comment` |
| **CWE** | CWE-79: Improper Neutralization of Input During Web Page Generation |
| **CVSSv3 Score** | 7.3 (High) |
| **OWASP Category**| A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application fails to sanitize input submitted via the blog comment form. The malicious payload is stored in the backend database and rendered verbatim whenever a user visits the associated blog post, resulting in Stored XSS.

#### 3.2 Proof of Concept (PoC)
**1. Identification & Interception**
Identified the comment form and intercepted the POST request containing the parameters.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> The blog comment form.</i></p>
</div>
<div align="center">
  <img src="./screenshots/proxy.png" alt="Burp Proxy" width="85%">
  <p><i><b>Figure 2:</b> Intercepted POST request.</i></p>
</div>

**2. Exploitation & Verification**
Submitted the payload `<script>alert(1)</script>`. Upon navigating back to the post, the stored script executed automatically.
<div align="center">
  <img src="./screenshots/payload.png" alt="XSS Payload" width="85%">
  <p><i><b>Figure 3:</b> Submitting the malicious script.</i></p>
</div>
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Execution of the stored script.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation.</i></p>
</div>

#### 3.3 Business Impact
Stored XSS requires no social engineering. Any user, including administrators, who simply visits the compromised page will have the malicious script executed in their session.

#### 3.4 Remediation
Apply strict **Context-Aware Output Encoding** when retrieving and displaying comments. Implement a **Content Security Policy (CSP)** to prevent inline script execution.

#### 3.5 References
* **CWE-79:** https://cwe.mitre.org/data/definitions/79.html
* **OWASP XSS Prevention Cheat Sheet:** https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html