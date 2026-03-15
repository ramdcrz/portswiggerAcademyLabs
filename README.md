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

## 📊 Progress Tracker (Apprentice Level)

<details>
<summary><b>SQL Injection (SQLi)</b></summary>

- [ ] SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- [ ] SQL injection vulnerability allowing login bypass
- [ ] SQL injection UNION attack, determining the number of columns returned by the query
- [ ] SQL injection UNION attack, finding a column containing text
</details>

<details>
<summary><b>Authentication</b></summary>

- [ ] Username enumeration via different responses
- [ ] 2FA broken logic
- [ ] Password reset broken logic
</details>

<details>
<summary><b>Cross-Site Scripting (XSS)</b></summary>

- [ ] Reflected XSS into HTML context with nothing encoded
- [ ] Stored XSS into HTML context with nothing encoded
- [ ] DOM XSS in `document.write` sink using source `location.search`
- [ ] DOM XSS in `innerHTML` sink using source `location.search`
- [ ] DOM XSS in jQuery anchor `href` attribute sink using source `location.search`
- [ ] DOM XSS in jQuery selector sink using source `location.hash`
- [ ] Reflected XSS into attribute with angle brackets HTML-encoded
</details>

<details>
<summary><b>Cross-Site Request Forgery (CSRF)</b></summary>

- [ ] CSRF vulnerability with no defenses
- [ ] CSRF where token validation depends on request method
</details>

<details>
<summary><b>Clickjacking</b></summary>

- [ ] Basic clickjacking with CSRF token protection
</details>

<details>
<summary><b>DOM-based vulnerabilities</b></summary>

- [ ] DOM-based open redirection
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
