# 🛡️ Lab: Basic SSRF against the local server

> **Category:** `Server-Side Request Forgery (SSRF)`  
> **Difficulty:** `Apprentice`  
> > **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit an SSRF vulnerability in a stock check feature to access a restricted internal admin interface and delete the user `carlos`.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** `stockApi` parameter in the stock check feature.
* **Payload Type:** Basic SSRF (Localhost).
* **Technical Logic:** The application takes a URL from the user and fetches data from it without proper validation. By providing `http://localhost/admin`, I forced the server to make a request to itself. Since the request originated from the server's own IP, the admin panel's access controls were bypassed, allowing me to execute administrative actions like deleting users.

---

### 📑 Technical Walkthrough

#### 1. Identification
I intercepted the stock check request and noticed the `stockApi` parameter, which contained a URL to an internal stock-checking service.

<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Intercepting the baseline request containing the vulnerable stockApi parameter.</i></p>
</div>

#### 2. Probing the Admin Interface
By changing the `stockApi` parameter to `http://localhost/admin`, I successfully forced the server to fetch its own administrative dashboard.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Initial SSRF" width="85%">
  <p><i><b>Figure 2:</b> Bypassing access controls to view the internal admin panel.</i></p>
</div>

#### 3. Analyzing Administrative Actions
I inspected the source code of the returned admin panel to identify the specific endpoint and parameters required to delete a user.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Finding the Delete URL" width="85%">
  <p><i><b>Figure 3:</b> Discovering the user deletion endpoint for 'carlos'.</i></p>
</div>

#### 4. Execution of the SSRF Attack
I updated the `stockApi` parameter to target the deletion endpoint. The server processed the request as a legitimate internal administrative action.

<div align="center">
  <img src="./screenshots/verification.png" alt="Executing Deletion" width="85%">
  <p><i><b>Figure 4:</b> Triggering the deletion of the 'carlos' user via SSRF.</i></p>
</div>

#### 5. Final Confirmation
Refreshing the application confirmed that the user was successfully deleted.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
Internal services often lack the same level of authentication and authorization as public-facing ones, assuming the network perimeter is secure. SSRF proves that the "call is coming from inside the house."

**Remediation:** Implement a strict **whitelist** of allowed hostnames/IPs for any outbound requests. Never allow users to provide full URLs or control the destination of server-side requests.

---