# WEB APPLICATION PENETRATION TEST REPORT: STORED XSS (ANCHOR HREF)

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 16 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This assessment identified a High-severity Stored Cross-Site Scripting (XSS) vulnerability. The application allows attackers to persistently embed malicious JavaScript pseudo-protocols within author profile links, bypassing standard HTML encoding defenses.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Stored XSS via Pseudo-protocol Injection

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected Component** | Website input field (stored in `<a>` tags) |
| **CWE** | CWE-79: Improper Neutralization of Input During Web Page Generation |
| **CVSSv3 Score** | 7.3 (High) |
| **OWASP Category**| A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application attempts to prevent attribute breakout by encoding quotes within the user's provided website URL. However, it fails to validate the URI scheme itself. By injecting the `javascript:` pseudo-protocol directly into the field, the malicious payload is stored in the database and executed by the browser whenever a victim clicks the generated author link.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Interception**
Intercepted the comment submission and identified the `website` parameter as the primary reflection vector.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Intercepting the comment submission POST request.</i></p>
</div>

**2. Reflection Analysis**
Confirmed via Repeater that the input from the website field is reflected unencoded within the `href` attribute.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Reflection Analysis" width="85%">
  <p><i><b>Figure 2:</b> Observing the reflected input in the HTML response via Repeater.</i></p>
</div>

**3. Exploitation (Repeater Injection)**
Modified the POST request to inject the payload `javascript:alert(1)` into the website parameter.
<div align="center">
  <img src="./screenshots/payload.png" alt="XSS Payload" width="85%">
  <p><i><b>Figure 3:</b> Injecting the JavaScript pseudo-protocol payload using Burp Repeater.</i></p>
</div>

**4. Execution & Verification**
Clicked the author's name on the live page, which executed the command and triggered the `alert(1)` function.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Triggering the alert by interacting with the stored malicious link.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of the successful Stored XSS exploit.</i></p>
</div>

#### 3.3 Business Impact
Stored XSS requires zero social engineering; the payload is served directly by the application's database to unsuspecting users. This can lead to widespread session hijacking and compromise of administrative accounts.

#### 3.4 Remediation
Implement a strict whitelist for protocols. Ensure that URLs provided in the website field must start with standard, safe schemes (`http://` or `https://`) and explicitly reject any input containing `javascript:` or `data:`.

#### 3.5 References
* **CWE-79:** https://cwe.mitre.org/data/definitions/79.html
* **OWASP XSS Prevention Cheat Sheet:** https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html