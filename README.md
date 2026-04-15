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
As of April 2026, I have successfully identified, exploited, and documented **50+ vulnerabilities** across **25 distinct security categories**. This repository has transitioned from basic walkthroughs to professional-grade vulnerability reports, detailing exploit strategies, proof-of-concept execution, business impact, and remediation advice mapping to OWASP and MITRE CWE standards.

### 🧠 Core Skills Acquired
* **Manual Exploitation:** Utilizing Burp Suite (Proxy, Repeater, Intruder) to manipulate raw HTTP requests, intercept WebSockets, and observe server-side logic flaws.
* **Access Control & IDOR:** Bypassing horizontal and vertical access boundaries, executing mass assignments, and uncovering Execution After Redirect (EAR) flaws.
* **Advanced Injection & SSRF:** Pivoting through XML parsers (XXE), exploiting internal networks via Server-Side Request Forgery, and injecting NoSQL operators.
* **Modern API & Logic Flaws:** Exploiting JWT misconfigurations, Race Conditions (TOCTOU), GraphQL Introspection, LLM Excessive Agency, and Web Cache Deception.

---

## 📊 Progress Tracker (Apprentice Level)

<details open>
<summary><b>1. SQL Injection (SQLi)</b></summary>

- [x] [1_retrievingHiddenData](./1_sqlInjection/1_retrievingHiddenData)
- [x] [2_loginBypass](./1_sqlInjection/2_loginBypass)
</details>

<details open>
<summary><b>2. Cross-Site Scripting (XSS)</b></summary>

- [x] [1_reflectedXssHtmlContext](./2_crossSiteScripting/1_reflectedXssHtmlContext)
- [x] [2_storedXssHtmlContext](./2_crossSiteScripting/2_storedXssHtmlContext)
- [x] [3_domXssDocumentWriteSink](./2_crossSiteScripting/3_domXssDocumentWriteSink)
- [x] [4_domXssInnerHtmlSink](./2_crossSiteScripting/4_domXssInnerHtmlSink)
- [x] [5_domXssJqueryAnchorHrefSink](./2_crossSiteScripting/5_domXssJqueryAnchorHrefSink)
- [x] [6_domXssJquerySelectorSinkHashChange](./2_crossSiteScripting/6_domXssJquerySelectorSinkHashChange)
- [x] [7_reflectedXssAttributeEncoded](./2_crossSiteScripting/7_reflectedXssAttributeEncoded)
- [x] [8_storedXssAnchorHrefJavascript](./2_crossSiteScripting/8_storedXssAnchorHrefJavascript)
- [x] [9_reflectedXssJavascriptStringBreakout](./2_crossSiteScripting/9_reflectedXssJavascriptStringBreakout)
</details>

<details open>
<summary><b>3. Cross-Site Request Forgery (CSRF)</b></summary>

- [x] [CSRF vulnerability with no defenses](./3_crossSiteRequestForgery/1_csrfNoDefenses)
</details>

<details open>
<summary><b>4. Clickjacking</b></summary>

- [x] [Basic clickjacking with CSRF token protection](./4_clickjacking/1_basicClickjacking)
- [x] [Clickjacking with form input data prefilled from a URL parameter](./4_clickjacking/2_prefilledFormData)
- [x] [Clickjacking with a frame buster script](./4_clickjacking/3_frameBuster)
</details>

<details open>
<summary><b>5. Cross-origin resource sharing (CORS)</b></summary>

- [x] [CORS vulnerability with basic origin reflection](./5_crossOriginResourceSharing/1_corsBasicOriginReflection)
- [x] [CORS vulnerability with trusted null origin](./5_crossOriginResourceSharing/2_corsTrustedNullOrigin)
</details>

<details open>
<summary><b>6. XML external entity (XXE) injection</b></summary>

- [x] [Exploiting XXE using external entities to retrieve files](./6_xmlExternalEntityInjection/1_xxeRetrieveFiles)
- [x] [Exploiting XXE to perform SSRF attacks](./6_xmlExternalEntityInjection/2_xxeToSsrf)
</details>

<details open>
<summary><b>7. Server-side request forgery (SSRF)</b></summary>

- [x] [Basic SSRF against the local server](./7_serverSideRequestForgery/1_ssrfLocalServer)
- [x] [Basic SSRF against another back-end system](./7_serverSideRequestForgery/2_ssrfBackendSystem)
</details>

<details open>
<summary><b>8. OS command injection</b></summary>

- [x] [OS command injection, simple case](./8_osCommandInjection/1_simpleCase)
</details>

<details open>
<summary><b>9. Path traversal</b></summary>

- [x] [File path traversal, simple case](./9_pathTraversal/1_simpleCase)
</details>

<details open>
<summary><b>10. Access control vulnerabilities</b></summary>

- [x] [Unprotected admin functionality](./10_accessControl/1_unprotectedAdmin)
- [x] [Unprotected admin functionality with unpredictable URL](./10_accessControl/2_unpredictableAdmin)
- [x] [User role controlled by request parameter](./10_accessControl/3_cookieRoleControl)
- [x] [User role can be modified in user profile](./10_accessControl/4_requestParameterRoleControl)
- [x] [User ID controlled by request parameter](./10_accessControl/5_userIdRequestParameter)
- [x] [User ID controlled by request parameter, with unpredictable user IDs](./10_accessControl/6_unpredictableUserId)
- [x] [User ID controlled by request parameter with data leakage in redirect](./10_accessControl/7_dataLeakageRedirect)
- [x] [User ID controlled by request parameter with password disclosure](./10_accessControl/8_userIdPasswordDisclosure)
- [x] [Insecure direct object references](./10_accessControl/9_insecureDirectObjectReferences)
</details>

<details open>
<summary><b>11. Authentication</b></summary>

- [x] [Username enumeration via different responses](./11_authentication/1_usernameEnumeration)
- [x] [2FA simple bypass](./11_authentication/2_2FA_bypass)
- [x] [Password reset broken logic](./11_authentication/3_passwordResetLogic)
</details>

<details open>
<summary><b>12. WebSockets</b></summary>

- [x] [Manipulating WebSocket messages to exploit vulnerabilities](./12_webSockets/1_manipulatingMessages)
</details>

<details open>
<summary><b>13. Insecure deserialization</b></summary>

- [x] [Modifying serialized objects](./13_insecureDeserialization/1_modifyingSerializedObjects)
</details>

<details open>
<summary><b>14. Information disclosure</b></summary>

- [x] [Information disclosure in error messages](./14_informationDisclosure/1_errorMessages)
- [x] [Information disclosure on debug page](./14_informationDisclosure/2_debugPage)
- [x] [Source code disclosure via backup files](./14_informationDisclosure/3_backupFiles)
- [x] [Authentication bypass via information disclosure](./14_informationDisclosure/4_authBypass)
</details>

<details open>
<summary><b>18. File Upload Vulnerabilities</b></summary>

- [x] [Unrestricted image upload with no extension blacklisting](./18_fileUpload/1_webShellUpload)
- [x] [Web shell upload via Content-Type restriction bypass](./18_fileUpload/2_contentTypeBypass)
</details>

<details open>
<summary><b>19. JWT Attacks</b></summary>

- [x] [JWT authentication bypass via unverified signature](./19_jwt/1_unverifiedSignature)
- [x] [JWT authentication bypass via flawed signature verification](./19_jwt/2_flawedSignatureVerification)
</details>

<details open>
<summary><b>20. GraphQL API Vulnerabilities</b></summary>

- [x] [Accessing private GraphQL posts](./20_graphql/1_accessingPrivatePosts)
</details>

<details open>
<summary><b>21. Race Conditions</b></summary>

- [x] [Limit overrun race conditions](./21_raceConditions/1_limitOverrun)
</details>

<details open>
<summary><b>22. NoSQL Injection</b></summary>

- [x] [Detecting NoSQL injection](./22_noSqlInjection/1_detectingNoSqlInjection)
- [x] [Exploiting NoSQL operator injection to bypass authentication](./22_noSqlInjection/2_operatorAuthBypass)
</details>

<details open>
<summary><b>23. API Testing</b></summary>

- [x] [Exploiting an API endpoint using documentation](./23_apiTesting/1_exploitingApiDocumentation)
</details>

<details open>
<summary><b>24. Web LLM Attacks</b></summary>

- [x] [Exploiting LLM APIs with excessive agency](./24_llmAttacks/1_excessiveAgency)
</details>

<details open>
<summary><b>25. Web Cache Deception</b></summary>

- [x] [Exploiting path mapping for web cache deception](./25_webCacheDeception/1_pathMappingWcd)
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