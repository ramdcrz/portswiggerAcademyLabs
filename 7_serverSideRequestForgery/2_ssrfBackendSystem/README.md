# 🛡️ Lab: Basic SSRF against another back-end system

> **Category:** `Server-Side Request Forgery (SSRF)`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to leverage an SSRF vulnerability in a stock check feature to scan an internal IP range (`192.168.0.X`), discover a hidden administrative interface on port `8080`, and delete the user `carlos`.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** `stockApi` parameter.
* **Payload Type:** SSRF with Internal IP Brute-forcing.
* **Technical Logic:** The application's server-side code fetches data from a URL provided in the `stockApi` parameter. By using **Burp Intruder** to iterate through the final octet of the `192.168.0.0/24` subnet, I identified a responsive internal host. Because the request originates from the trusted local network, the back-end system allowed me to execute administrative commands without external authentication.

---

### 📑 Technical Walkthrough

#### 1. Identification
I began by intercepting the "Check stock" request. I identified that the `stockApi` parameter was fetching data from an internal URL, making it a primary target for SSRF probing.

<div align="center">
  <img src="./screenshots/identification.jpg" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Capturing the baseline request with the vulnerable stockApi parameter.</i></p>
</div>

#### 2. Internal Network Scanning
I moved the request to **Burp Intruder** and performed a horizontal scan of the internal network range. My scan revealed that the host at `192.168.0.3` was active and returned a `200 OK` status code.

<div align="center">
  <img src="./screenshots/proxy.jpg" alt="Intruder Scan Results" width="85%">
  <p><i><b>Figure 2:</b> Identifying the hidden admin interface at 192.168.0.3:8080 via Intruder.</i></p>
</div>

#### 3. Crafting the Administrative Payload
Using the discovered IP address, I moved to **Burp Repeater** to craft the final attack. I updated the `stockApi` URL to target the specific deletion endpoint for the user `carlos`.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Crafting Deletion Request" width="85%">
  <p><i><b>Figure 3:</b> Constructing the SSRF payload to trigger the deletion of 'carlos'.</i></p>
</div>

#### 4. Execution & Verification
Upon sending the request, the server responded with a `302 Found`, indicating a successful redirect to the admin panel after the deletion command was processed.

<div align="center">
  <img src="./screenshots/verification.png" alt="Server Response" width="85%">
  <p><i><b>Figure 4:</b> The server's 302 response confirming the command execution.</i></p>
</div>

#### 5. Final Confirmation
I refreshed the application to verify the solve. The "Congratulations" banner confirmed that the user `carlos` was successfully removed from the back-end system.

<div align="center">
  <img src="./screenshots/confirmation.jpg" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Successful lab completion banner.</i></p>
</div>

---

### 🧠 Key Takeaway
Internal services often lack the authentication layers of public-facing web apps. SSRF allows an attacker to bypass the network perimeter and act as a trusted internal entity.

**Remediation:** Applications should use a strict whitelist for any backend-requested URLs. Additionally, internal administrative services should require authentication regardless of the source IP address (Zero Trust).
---