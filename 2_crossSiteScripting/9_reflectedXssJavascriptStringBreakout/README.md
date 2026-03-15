# 🛡️ Lab: Reflected XSS into a JavaScript string with angle brackets HTML encoded

> **Category:** `Cross-site Scripting (XSS)`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a **Reflected XSS** vulnerability where input is reflected inside a JavaScript string variable. The challenge is to execute code despite angle brackets being HTML-encoded, necessitating a breakout from the JavaScript string context.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** JavaScript variable `searchTerms` in a `<script>` block.
* **Payload Type:** JavaScript Context Breakout.
* **Payload:** `'-alert(1)-'`
* **Technical Logic:** The server reflects input as: `var searchTerms = 'USER_INPUT';`. 
    1. The first `'` terminates the string.
    2. The `-` operator separates the string from the function.
    3. `alert(1)` is executed by the JS engine.
    4. The second `-` and `'` swallow the original closing quote, maintaining valid syntax.

---

### 📑 Technical Walkthrough

#### 1. Identification & Interception
I searched for a test string and intercepted the request using **Burp Proxy**. This allowed me to analyze the exact parameter being sent to the server.

<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Intercepting the search query for context analysis.</i></p>
</div>

#### 2. Reflection Analysis
Using **Burp Repeater**, I located the reflection point in the response. The input was placed inside a script block. I verified that angle brackets were encoded, making traditional tag injection impossible.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Reflection Analysis" width="85%">
  <p><i><b>Figure 2:</b> Observing the reflection inside a JavaScript variable in the HTTP response.</i></p>
</div>

#### 3. Exploitation (String Context Breakout)
I crafted a payload to break out of the string literal. By injecting `'-alert(1)-'`, I effectively changed the code's logic from a simple variable assignment to an executable expression.

<div align="center">
  <img src="./screenshots/payload.png" alt="XSS Payload" width="85%">
  <p><i><b>Figure 3:</b> Crafting the breakout payload in Burp Repeater.</i></p>
</div>

#### 4. Execution & Verification
I loaded the modified URL in the browser. The JavaScript engine evaluated the injected expression, resulting in the successful execution of the `alert()` function.

<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Browser executing the injected JavaScript code.</i></p>
</div>

#### 5. Final Confirmation
The execution of the script satisfied the lab requirements and triggered the completion banner.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
Encoding `<` and `>` only secures the **HTML context**. When data is reflected inside **JavaScript**, attackers can use quotes and operators to escape the data context and execute code.

**Remediation:** Data reflected in JavaScript should be JSON-encoded. Furthermore, backslashes and quotes must be escaped to prevent breakout from the string literal.

---