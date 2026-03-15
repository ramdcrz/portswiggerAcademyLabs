# 🛡️ Lab: Reflected XSS into HTML context with nothing encoded

> **Category:** `Cross-site Scripting (XSS)`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a **Reflected Cross-site Scripting (XSS)** vulnerability in the search functionality to execute arbitrary JavaScript in the victim's browser.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** Search query parameter (`search`).
* **Payload Type:** Reflected XSS.
* **Technical Logic:** The application takes input from the `search` GET parameter and reflects it directly into the HTML body. Due to a lack of output encoding, the browser interprets `<script>` tags as executable code rather than literal text.

---

### 📑 Technical Walkthrough

#### 1. Identification & Reconnaissance
I began by testing the search feature with a standard text string (`test`) to observe how the application handles input. The search term was reflected directly on the results page without any apparent sanitization.

<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Search results page showing the test string reflected in the HTML.</i></p>
</div>

#### 2. Interception and Analysis
Using **Burp Suite Proxy**, I intercepted the request containing the malicious XSS payload. This allowed me to verify that the raw script was being sent to the server via the `search` parameter.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Burp Proxy" width="85%">
  <p><i><b>Figure 2:</b> Burp Proxy showing the raw XSS payload in the GET request.</i></p>
</div>

#### 3. Exploitation (Payload Delivery)
I entered the basic XSS payload `<script>alert(1)</script>` into the search input field. This payload is designed to trigger a JavaScript alert box, proving that the browser is executing injected scripts.

<div align="center">
  <img src="./screenshots/payload.png" alt="Payload Delivery" width="85%">
  <p><i><b>Figure 3:</b> Entering the XSS payload into the application's search bar.</i></p>
</div>

#### 4. Execution & Verification
Upon submitting the search, the browser interpreted the unencoded tags and executed the JavaScript. A popup alert displaying `1` appeared, confirming the vulnerability.

<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> The browser executing the script and displaying the alert(1) popup.</i></p>
</div>

#### 5. Final Confirmation
The successful execution of the script satisfied the lab requirements. The PortSwigger banner confirmed the lab was officially solved.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation showing the "Lab Solved" notification.</i></p>
</div>

---

### 🧠 Key Takeaway
This lab demonstrates that reflecting user input directly into the HTML context without **Context-Aware Output Encoding** leads to Reflected XSS.

**Remediation:** All user-supplied data must be HTML-encoded before being rendered. For example, `<` should become `&lt;` and `>` should become `&gt;`, ensuring the browser treats the input as data rather than code.

---