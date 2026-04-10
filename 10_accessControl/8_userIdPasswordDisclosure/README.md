# WEB APPLICATION PENETRATION TEST REPORT: CREDENTIAL DISCLOSURE VIA IDOR

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 18 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report identifies a Critical vulnerability chaining an Insecure Direct Object Reference (IDOR) with sensitive credential disclosure, leading to a complete administrative account takeover.

### 2.1 Overall Risk Rating
**Rating: CRITICAL**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Administrative Account Takeover via Password Disclosure

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Critical |
| **Finding ID** | VULN-001 |
| **Affected Endpoint** | `/my-account?id=[username]` |
| **CWE** | CWE-639 / CWE-256 |
| **CVSSv3 Score** | 9.8 (Critical) |
| **OWASP Category**| A01:2021 – Broken Access Control |
| **Status** | Open |

#### 3.1 Description
The application lacks authorization checks on the `/my-account` endpoint, allowing IDOR. Concurrently, the application pre-fills the "Change password" form field with the user's plaintext password. By executing an IDOR attack against the `administrator` account, the server renders the admin page and discloses the plaintext password in the HTML source code.

#### 3.2 Proof of Concept (PoC)

**1. Identification of the Disclosure Point**
Identified that the account page pre-populates the masked password field inside the HTML `value` attribute.
<div align="center">
  <img src="./screenshots/identification.png" alt="Standard User Account" width="85%">
  <p><i><b>Figure 1:</b> Identifying the masked password field and the 'id' parameter.</i></p>
</div>

**2. Accessing the Admin Account**
Modified the URL parameter to `id=administrator`, forcing the server to render the unauthorized page.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Admin ID Traversal" width="85%">
  <p><i><b>Figure 2:</b> Navigating to the administrator's account page via IDOR.</i></p>
</div>

**3. Exfiltrating the Plaintext Password**
Inspected the HTML source code to extract the plaintext password from the `value` attribute.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Password Disclosure" width="85%">
  <p><i><b>Figure 3:</b> Viewing the administrator's password in the browser inspector.</i></p>
</div>

**4. Verification of Administrative Access**
Authenticated using the stolen credentials to completely compromise the admin account and delete the target user.
<div align="center">
  <img src="./screenshots/verification.png" alt="Deleting Carlos" width="85%">
  <p><i><b>Figure 4:</b> Successfully executing administrative actions.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
Yields immediate and complete administrative access to the platform without requiring complex exploitation or brute-forcing, leading to a catastrophic data breach.

#### 3.4 Remediation
1. Ensure strict authorization checks validate the user ID against the active session.
2. **Never** pre-fill password fields. Password changes should require the user to provide their current password manually for verification against a cryptographic hash.

#### 3.5 References
* **CWE-256:** https://cwe.mitre.org/data/definitions/256.html
* **OWASP IDOR Prevention:** https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html