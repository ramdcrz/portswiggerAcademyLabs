# WEB APPLICATION PENETRATION TEST REPORT: BROKEN ACCESS CONTROL (JS LEAK)

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 18 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report highlights a High-severity Access Control vulnerability. The application leaks an unpredictable administrative URL within client-side JavaScript, which lacks backend authorization controls, allowing unauthorized privilege escalation.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Admin Path Disclosure via JavaScript

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected Component** | Landing page `<script>` block |
| **CWE** | CWE-200: Exposure of Sensitive Information to an Unauthorized Actor |
| **CVSSv3 Score** | 7.5 (High) |
| **OWASP Category**| A01:2021 – Broken Access Control |
| **Status** | Open |

#### 3.1 Description
The developer attempted to secure the administrative interface by assigning it a complex, unpredictable URL. However, client-side JavaScript on the public home page references this URL. Because the endpoint does not perform Role-Based Access Control (RBAC) checks, unmasking the URL grants immediate administrative access.

#### 3.2 Proof of Concept (PoC)

**1. Source Code Analysis**
Inspected the HTML source and discovered the administrative URL defined in a JavaScript variable.
<div align="center">
  <img src="./screenshots/identification.png" alt="JavaScript Leak" width="85%">
  <p><i><b>Figure 1:</b> Discovering the unpredictable admin URL hidden in the client-side script.</i></p>
</div>

**2. Accessing the Hidden Interface**
Navigated directly to the URL, confirming the absence of privileged login requirements.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Admin Panel Access" width="85%">
  <p><i><b>Figure 2:</b> Bypassing "Security through Obscurity" to access the dashboard.</i></p>
</div>

**3. Execution & Verification**
Located the user management section and successfully deleted the target user `carlos`.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Identifying Carlos" width="85%">
  <p><i><b>Figure 3:</b> Locating the delete functionality.</i></p>
</div>
<div align="center">
  <img src="./screenshots/verification.png" alt="User Deleted" width="85%">
  <p><i><b>Figure 4:</b> Successfully removing 'carlos' via the unprotected endpoint.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
Exposing administrative routes via source code neutralizes unpredictable URL defenses, leading to unauthorized account management and system takeover.

#### 3.4 Remediation
Enforce server-side authorization checks on all administrative routes. Ensure that sensitive paths and administrative logic are completely stripped from client-side bundles delivered to unauthenticated users.

#### 3.5 References
* **CWE-200:** https://cwe.mitre.org/data/definitions/200.html
* **OWASP Access Control:** https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html