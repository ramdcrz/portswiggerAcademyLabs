# 🛡️ Lab: CORS vulnerability with trusted null origin

> **Category:** `Cross-Origin Resource Sharing (CORS)`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a CORS misconfiguration where the server trusts the `null` origin. By using a sandboxed iframe to generate a `null` origin request, I can steal the administrator's API key.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** `/accountDetails` endpoint.
* **Payload Type:** CORS Misconfiguration (Trusted Null Origin).
* **Technical Logic:** The server's CORS policy includes `null` in its whitelist of allowed origins. I utilized an HTML5 `<iframe>` with the `sandbox` attribute. Because `allow-same-origin` is omitted, the browser is forced to send `Origin: null` in the request header, bypassing the policy and allowing data exfiltration via an authenticated XHR request.

---

### 📑 Technical Walkthrough

#### 1. Identification & Repeater Analysis
I analyzed the `/accountDetails` request and tested it in Burp Repeater by injecting an `Origin: null` header. The server reflected this origin and confirmed credential support, identifying a clear vulnerability.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Null Origin Reflection" width="85%">
  <p><i><b>Figure 1:</b> Proving the server trusts and reflects the 'null' origin.</i></p>
</div>

#### 2. Crafting the Sandboxed Exploit
I configured the exploit server to host a sandboxed iframe. This specific configuration is necessary to trigger the browser's `null` origin behavior while still allowing the scripts to run and exfiltrate the data.

<div align="center">
  <img src="./screenshots/payload.png" alt="Iframe Sandbox Exploit" width="85%">
  <p><i><b>Figure 2:</b> The sandboxed iframe payload used to spoof the null origin.</i></p>
</div>

#### 3. Execution & Log Analysis
Upon delivery to the victim, I monitored the Access Log. The administrator's session triggered the exploit, and their API key was successfully leaked to my server logs.

<div align="center">
  <img src="./screenshots/verification.png" alt="Log Analysis" width="85%">
  <p><i><b>Figure 3:</b> Retrieving the administrator's stolen API key from the logs.</i></p>
</div>

#### 4. Final Confirmation
I submitted the stolen key to satisfy the lab requirements.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 4:</b> Lab solved confirmation.</i></p>
</div>

---

### 🧠 Key Takeaway
The `null` origin is not a security boundary. It can be easily spoofed using sandboxed iframes or certain redirects. Whitelisting `null` is just as dangerous as whitelisting `*` (wildcard) if credentials are supported.

**Remediation:** Remove `null` from the CORS whitelist. If you need to support cross-origin requests, use a strictly defined whitelist of trusted, full domain names.

---