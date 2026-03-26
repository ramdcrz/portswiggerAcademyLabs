# 🛡️ Lab: File path traversal, simple case

> **Category:** `Path Traversal`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a path traversal vulnerability in an image-loading feature to retrieve the contents of the sensitive `/etc/passwd` file from the server.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** `filename` parameter in the `/image` endpoint.
* **Payload Type:** Relative Path Traversal (`../`).
* **Technical Logic:** The application takes a filename and appends it to a base directory path (e.g., `/var/www/images/`). By using `../../../`, I am instructing the operating system to move up three levels in the directory tree to the root directory, and then navigate into `/etc/passwd`.

---

### 📑 Technical Walkthrough

#### 1. Identification
I identified that product images are loaded via a dynamic parameter. This often indicates that the server-side code is reading files directly from the disk based on user input.

<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Identifying the image-loading request in Burp Suite.</i></p>
</div>

#### 2. Injecting the Traversal Payload
In **Burp Repeater**, I replaced the legitimate image filename with a traversal sequence designed to reach the root of the filesystem.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Payload Injection" width="85%">
  <p><i><b>Figure 2:</b> Injecting the dot-dot-slash sequence.</i></p>
</div>

#### 3. Data Exfiltration
Upon sending the request, the server resolved the path and returned the contents of `/etc/passwd` instead of an image file.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Password File Retrieval" width="85%">
  <p><i><b>Figure 3:</b> Successfully reading sensitive system files via the web interface.</i></p>
</div>

#### 4. Verification
The response confirms that the application lacks proper input validation or path normalization, allowing unrestricted access to the filesystem.

<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Detailed view of the exfiltrated user data.</i></p>
</div>

#### 5. Final Confirmation
The retrieval of the file satisfied the lab requirements and triggered the completion state.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
Path traversal is often a gateway to full system compromise. If an attacker can read configuration files, they can find database credentials or SSH keys.

**Remediation:** Avoid passing user-supplied input directly to filesystem APIs. Use a whitelist of allowed filenames or store files in a database/cloud storage instead of the local server filesystem.
---