# 🎓 PortSwigger Academy: Mid-Term Progress Report
**Student:** Ramil V. Deocariza  Jr.
**Course:** Information Assurance and Security  
**Date:** March 15, 2026

---

## 📈 Executive Summary
This report summarizes the completion of the foundational modules in the PortSwigger Web Security Academy. I have successfully identified, exploited, and documented **11 vulnerabilities** across the SQL Injection and Cross-site Scripting categories.

---

## 🏗️ Completed Modules

### 1️⃣ SQL Injection (SQLi)
*Exploiting the database layer to bypass authentication and retrieve hidden data.*

| Lab ID | Lab Name | Key Concept Learned |
| :--- | :--- | :--- |
| **SQLi-01** | Retrieving hidden data | Manipulating `WHERE` clauses via `' OR 1=1--`. |
| **SQLi-02** | Login bypass | Bypassing authentication using comment operators. |

### 2️⃣ Cross-site Scripting (XSS)
*Injecting malicious scripts into web pages to target end-users.*

| Lab ID | Lab Name | Key Concept Learned |
| :--- | :--- | :--- |
| **XSS-01** | Reflected XSS (HTML) | Injecting `<script>` tags into unsanitized search filters. |
| **XSS-02** | Stored XSS (HTML) | Persisting payloads in databases via comment sections. |
| **XSS-03** | DOM XSS (`document.write`) | Exploiting client-side sinks using `location.search`. |
| **XSS-04** | DOM XSS (`innerHTML`) | Bypassing script blocks using `<img>` event handlers. |
| **XSS-05** | DOM XSS (jQuery Href) | Using `javascript:` pseudo-protocols in anchor tags. |
| **XSS-06** | DOM XSS (jQuery Selector) | Leveraging `location.hash` and `hashchange` events. |
| **XSS-07** | Reflected XSS (Attribute) | Escaping attributes (`"`) when tags are encoded. |
| **XSS-08** | Stored XSS (Href Javascript) | Injecting pseudo-protocols when quotes are blocked. |
| **XSS-09** | Reflected XSS (JS String) | Breaking out of JavaScript variables using `'` and `-`. |

---

## 🛠️ Tooling & Methodology
Throughout these labs, I utilized the following industry-standard tools:
* **Burp Suite Professional:** Intercepting requests, utilizing **Repeater** for manual payload testing, and **Proxy** for traffic analysis.
* **Developer Tools:** Inspecting the DOM to identify sources and sinks for XSS.
* **Git/GitHub:** Maintaining a version-controlled repository of technical walkthroughs and evidence.

## 🧠 Core Reflections
The most significant takeaway from this phase is that **Encoding is not a Silver Bullet**. I learned that while a developer might encode angle brackets to stop HTML injection, a site can still be vulnerable via:
1. **Attribute Injection:** Escaping quotes to add event handlers.
2. **JavaScript Injection:** Breaking out of existing script variables.
3. **Protocol Injection:** Using `javascript:` in URI-based attributes.

---

**Status:** `Apprentice Level Complete` ✅ | **Next Objective:** `Server-Side Request Forgery (SSRF)` 🚀