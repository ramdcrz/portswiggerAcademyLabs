# 🛡️ Lab: Unprotected admin functionality

> **Category:** `Access Control`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to discover a hidden, unprotected administrative interface and use it to delete the user `carlos`.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** Sensitive directory disclosure via `robots.txt`.
* **Payload Type:** Broken Access Control / Security through Obscurity.
* **Technical Logic:** The application attempts to "hide" its administrative panel by not linking to it on the main UI. However, it lists the directory in `robots.txt` to prevent search engines from indexing it. Since the panel lacks an authentication check, any user who knows the URL can access full administrative privileges.

---

### 📑 Technical Walkthrough

#### 1. Information Gathering (Robots.txt)
I began by checking the `/robots.txt` file, a common place where developers inadvertently disclose sensitive paths that they want search crawlers to ignore.

<div align="center">
  <img src="./screenshots/identification.png" alt="Robots.txt Disclosure" width="85%">
  <p><i><b>Figure 1:</b> Discovering the hidden '/administrator-panel' path in robots.txt.</i></p>
</div>

#### 2. Bypassing Obscurity
By manually navigating to the disclosed path, I bypassed the "obscurity" layer and gained direct access to the administrative dashboard without being prompted for a login.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Admin Panel Access" width="85%">
  <p><i><b>Figure 2:</b> Accessing the unprotected administrative interface.</i></p>
</div>

#### 3. Identifying the Target
Within the admin panel, I identified the user management section and the specific action required to remove the target user, `carlos`.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Locating Carlos" width="85%">
  <p><i><b>Figure 3:</b> Identifying the user deletion functionality for 'carlos'.</i></p>
</div>

#### 4. Executing the Deletion
I triggered the delete action. Because there was no server-side check to verify if my session had administrative rights, the command was processed successfully.

<div align="center">
  <img src="./screenshots/verification.png" alt="Verification of Delete" width="85%">
  <p><i><b>Figure 4:</b> Successfully executing the administrative command to delete the user.</i></p>
</div>

#### 5. Final Confirmation
The lab was marked as solved once the user was successfully removed from the system.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
"Hidden" is not the same as "Secure." Administrative functions must always be protected by robust authentication and authorization checks, regardless of how "secret" the URL is.

**Remediation:** Implement proper access control lists (ACLs) and require a valid administrator session for any requests to the `/administrator-panel` path.
---