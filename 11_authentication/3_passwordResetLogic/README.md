# WEB APPLICATION PENETRATION TEST REPORT: PASSWORD RESET BROKEN LOGIC

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 19 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details a Critical vulnerability within the password reset functionality. Flawed backend logic allows an attacker to reset the password of any user without possessing a valid security token, leading to immediate account compromise.

### 2.1 Overall Risk Rating
**Rating: CRITICAL**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Unauthorized Password Reset via Logic Bypass

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Critical |
| **Finding ID** | VULN-001 |
| **Affected Endpoint** | `POST /forgot-password` |
| **CWE** | CWE-640: Weak Password Recovery Mechanism for Forgotten Password |
| **CVSSv3 Score** | 9.8 (Critical) |
| **OWASP Category**| A07:2021 – Identification and Authentication Failures |
| **Status** | Open |

#### 3.1 Description
The application fails to strictly enforce the presence and validity of the `temp-forgot-password-token`. If an attacker submits a password reset POST request with an empty or removed token parameter, the server bypasses the token check entirely. It then updates the password for whichever account is specified in the client-supplied `username` parameter, enabling arbitrary account takeover.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Interception**
Initiated a legitimate password reset for a controlled account and captured the final submission request in Burp Suite.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Accessing the password reset interface.</i></p>
</div>
<div align="center">
  <img src="./screenshots/proxy.png" alt="Proxy" width="85%">
  <p><i><b>Figure 2:</b> Intercepting the password reset POST request.</i></p>
</div>

**2. Execution of Traversal (Exploitation)**
In Burp Repeater, removed the token value and altered the `username` parameter to target the victim account `carlos`.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Traversal" width="85%">
  <p><i><b>Figure 3:</b> Manipulating the request to target Carlos's account.</i></p>
</div>

**3. Verification & Confirmation**
The server accepted the request. Successfully authenticated as `carlos` using the newly set password, confirming full account compromise.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Gaining unauthorized access to the victim's account page.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Confirmation" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
Complete failure of the password recovery mechanism allows trivial, unauthenticated takeover of any user account, including administrative accounts, resulting in total system compromise and data breach.

#### 3.4 Remediation
Ensure that security controls "fail closed." The backend must cryptographically tie the reset token to the specific user requesting it. The server should use the identity explicitly linked to a *valid, present* token to perform the database update, entirely ignoring client-supplied `username` parameters during the final submission phase.

#### 3.5 References
* **CWE-640:** https://cwe.mitre.org/data/definitions/640.html
* **OWASP Forgot Password Cheat Sheet:** https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html