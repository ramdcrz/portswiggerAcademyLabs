# portswiggerAcademyLabs

![Security](https://img.shields.io/badge/Security-Information%20Assurance%20%26%20Security-red)
![Burp Suite](https://img.shields.io/badge/Tools-Burp%20Suite-orange)
![Status](https://img.shields.io/badge/Level-Apprentice-green)

## 📌 Project Overview
This repository documents my progress through the **PortSwigger Web Security Academy** for my **Information Assurance and Security** course at **New Era University**.

The goal is to master web application security fundamentals, focusing on vulnerability identification, exploitation, and mitigation using **Burp Suite**.

---

## 👨‍💻 Student Information
* **Name:** Ramil V. Deocariza Jr.
* **Program:** BS in Computer Science (3rd Year)
* **Institution:** New Era University

---

## 📈 Mid-Term Progress Report
### Executive Summary
As of March 2026, I have successfully identified, exploited, and documented **11 vulnerabilities** across the SQL Injection and Cross-site Scripting categories. This journey has focused on moving beyond automated tools to understand the manual logic required to bypass modern security filters.

### 🧠 Core Skills Acquired
* **Manual Exploitation:** Utilizing Burp Suite (Proxy & Repeater) to manipulate raw HTTP requests and observe server-side behavior.
* **Contextual Analysis:** Determining if a vulnerability exists in HTML, Attribute, or JavaScript contexts.
* **Filter Evasion:** Mastering techniques like attribute escaping, pseudo-protocol injection (`javascript:`), and JavaScript string breakout to bypass HTML encoding.

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

<details>
<summary><b>3. Cross-Site Request Forgery (CSRF)</b></summary>

- [ ] CSRF vulnerability with no defenses
</details>

<details>
<summary><b>4. Clickjacking</b></summary>

- [ ] Basic clickjacking with CSRF token protection
- [ ] Clickjacking with form input data prefilled from a URL parameter
- [ ] Clickjacking with a frame buster script
</details>

<details>
<summary><b>5. Cross-origin resource sharing (CORS)</b></summary>

- [ ] CORS vulnerability with basic origin reflection
- [ ] CORS vulnerability with trusted null origin
</details>

<details>
<summary><b>6. XML external entity (XXE) injection</b></summary>

- [ ] Exploiting XXE using external entities to retrieve files
- [ ] Exploiting XXE to perform SSRF attacks
</details>

<details>
<summary><b>7. Server-side request forgery (SSRF)</b></summary>

- [ ] Basic SSRF against the local server
- [ ] Basic SSRF against another back-end system
</details>

<details>
<summary><b>8. OS command injection</b></summary>

- [ ] OS command injection, simple case
</details>

<details>
<summary><b>9. Path traversal</b></summary>

- [ ] File path traversal, simple case
</details>

<details>
<summary><b>10. Access control vulnerabilities</b></summary>

- [ ] Unprotected admin functionality
- [ ] Unprotected admin functionality with unpredictable URL
- [ ] User role controlled by request parameter
- [ ] User role can be modified in user profile
- [ ] User ID controlled by request parameter 
- [ ] User ID controlled by request parameter, with unpredictable user IDs
- [ ] User ID controlled by request parameter with data leakage in redirect 
- [ ] User ID controlled by request parameter with password disclosure
- [ ] Insecure direct object references
</details>

<details>
<summary><b>11. Authentication</b></summary>

- [ ] Username enumeration via different responses
- [ ] 2FA simple bypass
- [ ] Password reset broken logic
</details>

<details>
<summary><b>12. WebSockets</b></summary>

- [ ] Manipulating WebSocket messages to exploit vulnerabilities
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
        * `README.md` (Technical Walkthrough)
        * `screenshots/` (Evidence)

---

## 📜 Disclaimer
Educational purposes only. Labs performed in the PortSwigger sandbox environment.