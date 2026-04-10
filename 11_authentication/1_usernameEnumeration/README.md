# WEB APPLICATION PENETRATION TEST REPORT: USERNAME ENUMERATION & BRUTE-FORCE

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 19 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details a High-severity authentication vulnerability. The application leaks the validity of usernames through observable response discrepancies, enabling an attacker to enumerate valid accounts and subsequently brute-force the password to achieve unauthorized access.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Account Enumeration via Observable Response Discrepancy

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected Endpoint** | `/login` |
| **CWE** | CWE-204: Observable Response Discrepancy |
| **CVSSv3 Score** | 7.4 (High) |
| **OWASP Category**| A07:2021 – Identification and Authentication Failures |
| **Status** | Open |

#### 3.1 Description
The application returns distinct error messages depending on whether a submitted username exists in the database ("Invalid username" vs. "Incorrect password"). This discrepancy allows attackers to rapidly compile a list of valid accounts using automated tools. Once a valid username is confirmed, the attacker can focus a brute-force attack entirely on the password field, exponentially increasing the likelihood of a successful account compromise.

#### 3.2 Proof of Concept (PoC)

**1. Identification**
Captured a failed login attempt in Burp Suite and transferred the request to Burp Intruder to set up probing parameters.
<div align="center">
  <img src="./screenshots/identification.png" alt="Login Interception" width="85%">
  <p><i><b>Figure 1:</b> Setting up the username enumeration positions in Burp Intruder.</i></p>
</div>

**2. Username Enumeration**
Executed a Sniper attack using a common username wordlist. Analyzed response lengths to isolate the username that triggered the "Incorrect password" message.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Intruder Results" width="85%">
  <p><i><b>Figure 2:</b> Discovering the valid username via response length differentiation.</i></p>
</div>

**3. Password Brute-forcing**
Targeted the discovered username and executed a second Sniper attack against the password parameter using a password dictionary.
<div align="center">
  <img src="./screenshots/traversal.png" alt="302 Redirect Found" width="85%">
  <p><i><b>Figure 3:</b> Identifying the correct password through a 302 Found redirect status.</i></p>
</div>

**4. Credential Verification**
Successfully authenticated using the discovered credentials, granting full access to the target user's account page.
<div align="center">
  <img src="./screenshots/verification.png" alt="Account Access" width="85%">
  <p><i><b>Figure 4:</b> Verifying access to the compromised user's profile.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
Allows attackers to easily identify valid user accounts, paving the way for targeted credential stuffing or brute-force attacks that result in account takeover and unauthorized access to user data.

#### 3.4 Remediation
Implement generic error messaging for all authentication failures (e.g., "Invalid username or password") to prevent enumeration. Enforce strict account lockout policies or progressive rate-limiting (throttling) after a small number of consecutive failed login attempts.

#### 3.5 References
* **CWE-204:** https://cwe.mitre.org/data/definitions/204.html
* **OWASP Authentication Cheat Sheet:** https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html