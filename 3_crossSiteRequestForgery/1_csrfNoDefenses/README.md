# WEB APPLICATION PENETRATION TEST REPORT: CROSS-SITE REQUEST FORGERY

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 16 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report presents the findings of a web application penetration test targeting the account management functionality. A Cross-Site Request Forgery (CSRF) vulnerability was identified, allowing an attacker to force state-changing requests on behalf of an authenticated user.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — CSRF Vulnerability on Email Change Endpoint

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected URL** | `POST /my-account/change-email` |
| **CWE** | CWE-352: Cross-Site Request Forgery (CSRF) |
| **CVSSv3 Score** | 8.1 (High) |
| **OWASP Category**| A01:2021 – Broken Access Control |
| **Status** | Open |

#### 3.1 Description
The application performs a sensitive action (email change) via a POST request but fails to implement CSRF protections, such as anti-CSRF tokens or `SameSite` cookie attributes. Because browsers automatically append session cookies to cross-origin requests by default, an attacker-controlled site can silently submit this form, changing the victim's email address without their consent.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Interception**
Logged into the application and performed a legitimate email change, intercepting the request to analyze the data structure.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Locating the email change form in the user dashboard.</i></p>
</div>

**2. Request Analysis**
Inspected the POST request and confirmed the absence of unpredictable security tokens.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Request Analysis" width="85%">
  <p><i><b>Figure 2:</b> Burp Suite showing a request lacking CSRF protection.</i></p>
</div>

**3. Crafting the Malicious Exploit**
Drafted a hidden HTML form on the Exploit Server containing a script to auto-submit the POST request upon page load.
<div align="center">
  <img src="./screenshots/payload.png" alt="CSRF Exploit" width="85%">
  <p><i><b>Figure 3:</b> Crafting the auto-submitting CSRF payload on the exploit server.</i></p>
</div>

**4. Execution & Verification**
Verified the exploit locally to ensure the script triggered the email change successfully.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Confirming the email change after executing the CSRF exploit.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
Allows an attacker to change a victim's email address, paving the way for account takeover via a subsequent password reset request directed to the attacker's newly established email.

#### 3.4 Remediation
Implement cryptographically strong, unpredictable **CSRF Tokens** for all state-changing requests. Additionally, configure session cookies with the `SameSite=Lax` or `SameSite=Strict` attribute to prevent the browser from sending cookies along with cross-site POST requests.

#### 3.5 References
* **CWE-352:** https://cwe.mitre.org/data/definitions/352.html
* **OWASP CSRF Prevention Cheat Sheet:** https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html