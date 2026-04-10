# portswiggerAcademyLabs

![Security](https://img.shields.io/badge/Security-Information%20Assurance%20%26%20Security-red)
![Burp Suite](https://img.shields.io/badge/Tools-Burp%20Suite-orange)
![Status](https://img.shields.io/badge/Level-Apprentice-green)

## 📌 Project Overview
This repository documents my progress through the **PortSwigger Web Security Academy** for my **Information Assurance and Security** course at **New Era University**.

The goal is to master web application security fundamentals, focusing on vulnerability identification, manual exploitation, and mitigation using **Burp Suite**. All completed labs have been formally documented following standard **Penetration Test Report** frameworks.

---

## 👨‍💻 Student Information
* **Name:** Ramil V. Deocariza Jr.
* **Program:** BS in Computer Science (3rd Year)
* **Institution:** New Era University

---

## 📈 Progress Report
### Executive Summary
As of March 2026, I have successfully identified, exploited, and documented **36 vulnerabilities** across 12 distinct security categories. This repository has transitioned from basic walkthroughs to professional-grade vulnerability reports, detailing exploit strategies, proof-of-concept execution, business impact, and remediation advice mapping to OWASP and MITRE CWE standards.

### 🧠 Core Skills Acquired
* **Manual Exploitation:** Utilizing Burp Suite (Proxy, Repeater, Intruder) to manipulate raw HTTP requests, intercept WebSockets, and observe server-side logic flaws.
* **Access Control & IDOR:** Bypassing horizontal and vertical access boundaries, executing mass assignments, and uncovering Execution After Redirect (EAR) flaws.
* **Advanced Injection & SSRF:** Pivoting through XML parsers (XXE) and exploiting internal networks via Server-Side Request Forgery and OS Command Injection.
* **UI Redressing:** Bypassing CSRF tokens and client-side frame busters using HTML5 sandboxing and Clickjacking overlays.

---

## 📊 Progress Tracker (Apprentice Level)

<details open>
<summary><b>1. SQL Injection (SQLi)</b></summary>

- [x] 1_retrievingHiddenData
- [x] 2_loginBypass
</details>

<details open>
<summary><b>2. Cross-Site Scripting (XSS)</b></summary>

- [x] 1_reflectedXssHtmlContext
- [x] 2_storedXssHtmlContext
- [x] 3_domXssDocumentWriteSink
- [x] 4_domXssInnerHtmlSink
- [x] 5_domXssJqueryAnchorHrefSink
- [x] 6_domXssJquerySelectorSinkHashChange
- [x] 7_reflectedXssAttributeEncoded
- [x] 8_storedXssAnchorHrefJavascript
- [x] 9_reflectedXssJavascriptStringBreakout
</details>

<details open>
<summary><b>3. Cross-Site Request Forgery (CSRF)</b></summary>

- [x] CSRF vulnerability with no defenses
</details>

<details open>
<summary><b>4. Clickjacking</b></summary>

- [x] Basic clickjacking with CSRF token protection
- [x] Clickjacking with form input data prefilled from a URL parameter
- [x] Clickjacking with a frame buster script
</details>

<details open>
<summary><b>5. Cross-origin resource sharing (CORS)</b></summary>

- [x] CORS vulnerability with basic origin reflection
- [x] CORS vulnerability with trusted null origin
</details>

<details open>
<summary><b>6. XML external entity (XXE) injection</b></summary>

- [x] Exploiting XXE using external entities to retrieve files
- [x] Exploiting XXE to perform SSRF attacks
</details>

<details open>
<summary><b>7. Server-side request forgery (SSRF)</b></summary>

- [x] Basic SSRF against the local server
- [x] Basic SSRF against another back-end system
</details>

<details open>
<summary><b>8. OS command injection</b></summary>

- [x] OS command injection, simple case
</details>

<details open>
<summary><b>9. Path traversal</b></summary>

- [x] File path traversal, simple case
</details>

<details open>
<summary><b>10. Access control vulnerabilities</b></summary>

- [x] Unprotected admin functionality
- [x] Unprotected admin functionality with unpredictable URL
- [x] User role controlled by request parameter
- [x] User role can be modified in user profile
- [x] User ID controlled by request parameter 
- [x] User ID controlled by request parameter, with unpredictable user IDs
- [x] User ID controlled by request parameter with data leakage in redirect 
- [x] User ID controlled by request parameter with password disclosure
- [x] Insecure direct object references
</details>

<details open>
<summary><b>11. Authentication</b></summary>

- [x] Username enumeration via different responses
- [x] 2FA simple bypass
- [x] Password reset broken logic
</details>

<details open>
<summary><b>12. WebSockets</b></summary>

- [x] Manipulating WebSocket messages to exploit vulnerabilities
</details>

<details>
<summary><b>13. Insecure deserialization</b></summary>

- [ ] Modifying serialized objects
</details>

<details>
<summary><b>14. Information disclosure</b></summary>

- [ ] Information disclosure in error messages
- [ ] Information disclosure on debug page
- [ ] Source code disclosure via backup files
- [ ] Authentication bypass via information disclosure
</details>

---

## 📂 Repository Structure
* **`[categoryName]/`**
    * **`[labName]/`**
        * `README.md` (Penetration Test Report & PoC)
        * `screenshots/` (Visual Evidence)

---

## 📜 Disclaimer
Educational purposes only. Labs performed in the PortSwigger Academy authorized sandbox environment.