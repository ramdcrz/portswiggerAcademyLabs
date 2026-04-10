# WEB APPLICATION PENETRATION TEST REPORT: EXECUTION AFTER REDIRECT (EAR)

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 18 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report highlights a High-severity logic flaw known as Execution After Redirect (EAR). The application correctly identifies unauthorized access and issues a redirect, but fails to halt server-side processing, thereby leaking the requested sensitive data.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Sensitive Data Leakage via Execution After Redirect

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected Endpoint** | `/my-account` |
| **CWE** | CWE-698: Execution After Redirect (EAR) |
| **CVSSv3 Score** | 7.5 (High) |
| **OWASP Category**| A01:2021 – Broken Access Control |
| **Status** | Open |

#### 3.1 Description
When a user requests an unauthorized account ID, the server issues a `302 Found` redirect to deny access. However, the backend script continues to execute and renders the full requested account page in the HTTP response body. While standard browsers follow the redirect invisibly, an attacker utilizing an interception proxy can capture and read the leaked data.

#### 3.2 Proof of Concept (PoC)

**1. Identification**
Targeted the unauthorized account ID in Burp Repeater.
<div align="center">
  <img src="./screenshots/identification.png" alt="Repeater Setup" width="85%">
  <p><i><b>Figure 1:</b> Targeting the Carlos account ID in Burp Repeater.</i></p>
</div>

**2. Analyzing the Redirect**
Observed the server returning a 302 Found redirect header.
<div align="center">
  <img src="./screenshots/proxy.png" alt="302 Redirect" width="85%">
  <p><i><b>Figure 2:</b> Receiving the 302 Found status and Location header.</i></p>
</div>

**3. Discovering the Leaked Data & Verification**
Inspected the raw response body in Repeater to extract the fully rendered HTML and private API key.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Body Leakage" width="85%">
  <p><i><b>Figure 3:</b> Exfiltrating Carlos's API key from the 302 response body.</i></p>
</div>
<div align="center">
  <img src="./screenshots/verification.png" alt="Submission" width="85%">
  <p><i><b>Figure 4:</b> Validating the exfiltrated key.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
EAR flaws effectively nullify authorization logic, resulting in data breaches that are completely invisible to standard application users.

#### 3.4 Remediation
Ensure that application execution is strictly terminated immediately after an unauthorized redirect header is issued (e.g., explicitly utilizing `die()`, `exit()`, or `return` statements in the code).

#### 3.5 References
* **CWE-698:** https://cwe.mitre.org/data/definitions/698.html