# WEB APPLICATION PENETRATION TEST REPORT: SQL INJECTION LOGIN BYPASS

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 15 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report presents the findings of a web application penetration test performed against the PortSwigger Academy laboratory environment. The assessment identified a critical authentication bypass vulnerability allowing unauthorized administrative access.

### 2.1 Overall Risk Rating
**Rating: CRITICAL**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Authentication Bypass via SQL Injection

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Critical |
| **Finding ID** | VULN-001 |
| **Affected URL** | `/login` (`username` parameter) |
| **CWE** | CWE-89: Improper Neutralization of Special Elements in SQL |
| **CVSSv3 Score** | 9.8 (Critical) |
| **OWASP Category** | A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application's authentication mechanism is vulnerable to SQL injection. The backend query likely follows the structure: `SELECT * FROM users WHERE username = '[USER]' AND password = '[PASS]'`. By injecting SQL comment syntax into the username field, an attacker can truncate the query and bypass the password verification entirely.

#### 3.2 Proof of Concept (PoC)
**1. Identification & Reconnaissance**
Located the login interface and identified the `POST /login` endpoint.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Locating the login interface and target username.</i></p>
</div>

**2. Proxy Interception**
Intercepted the login attempt to manipulate the `username` parameter in the request body.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Burp Proxy" width="85%">
  <p><i><b>Figure 2:</b> Intercepted POST login request.</i></p>
</div>

**3. Exploitation (Logic Bypassing)**
Modified the username parameter to `administrator'--`. The `'` closes the string literal, and `--` comments out the remainder of the query.
<div align="center">
  <img src="./screenshots/request.png" alt="Burp Request" width="85%">
  <p><i><b>Figure 3:</b> Crafting the login bypass payload.</i></p>
</div>

**4. Server Response & Verification**
The server responded with a `302 Found` redirect to the administrator dashboard, issuing a valid session cookie.
<div align="center">
  <img src="./screenshots/response.png" alt="Server Response" width="85%">
  <p><i><b>Figure 4:</b> Successful redirect response.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Dashboard view confirming administrative access.</i></p>
</div>

#### 3.3 Business Impact
Complete authentication bypass allows an attacker to gain administrative privileges without credentials, leading to total account takeover, data exfiltration, and system compromise.

#### 3.4 Remediation
Implement **Prepared Statements with Parameterized Queries** for all authentication logic to ensure input is treated strictly as a string literal.

#### 3.5 References
* **CWE-89:** https://cwe.mitre.org/data/definitions/89.html
* **OWASP Authentication Cheat Sheet:** https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html