# 🛡️ Lab: User ID controlled by request parameter, with unpredictable user IDs

> **Category:** `Access Control`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a horizontal privilege escalation vulnerability by discovering a user's unpredictable GUID through public information and using it to access their private account data.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** `id` query parameter in the `/my-account` page.
* **Payload Type:** IDOR with GUID disclosure.
* **Technical Logic:** While the application uses non-sequential GUIDs to identify users (making them "unpredictable"), it leaks these identifiers on public pages (blog posts). Since the backend does not check if the requesting user's session matches the ID in the URL, the "randomness" of the ID provides no actual security.

---

### 📑 Technical Walkthrough

#### 1. Information Disclosure Discovery
I navigated to a blog post written by the target user. By clicking on the author's profile, I discovered that the application leaks the user's "unpredictable" GUID in the public URL.

<div align="center">
  <img src="./screenshots/identification.png" alt="GUID Discovery" width="85%">
  <p><i><b>Figure 1:</b> Discovering Carlos's internal GUID via a public-facing profile page.</i></p>
</div>

#### 2. Establishing a Session Baseline
I logged in with standard credentials to observe how the application handles my own account. My account was also identified by a GUID in the URL.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Standard Account Access" width="85%">
  <p><i><b>Figure 2:</b> Observing the standard session behavior and GUID format.</i></p>
</div>

#### 3. Executing Horizontal Escalation
By replacing my own GUID with the one discovered from Carlos's profile, I successfully bypassed the intended account boundaries.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Unauthorized Access" width="85%">
  <p><i><b>Figure 4:</b> Gaining unauthorized access to Carlos's private API key.</i></p>
</div>

#### 4. Data Exfiltration & Verification
I exfiltrated the sensitive API key from the target account to satisfy the lab's requirements.

<div align="center">
  <img src="./screenshots/verification.png" alt="Submission" width="85%">
  <p><i><b>Figure 4:</b> Preparing the exfiltrated data for the final submission.</i></p>
</div>

#### 5. Final Confirmation
The lab was solved once the exfiltrated key was validated by the server.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
Security by obscurity (using long, random IDs) is not a replacement for proper authorization. If an identifier is used to fetch sensitive data, it must be protected by a server-side check that validates the user's permission to see that specific resource.

**Remediation:** Implement strict authorization checks. The server should ensure that the user ID requested in the parameter matches the ID stored in the user's authenticated session cookie.
---