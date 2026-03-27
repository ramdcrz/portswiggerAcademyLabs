# 🛡️ Lab: User role controlled by request parameter

> **Category:** `Access Control`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a broken access control vulnerability where the user's privilege level is determined by a forgeable cookie, allowing for unauthorized access to administrative functions.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** `Admin` cookie value.
* **Payload Type:** Cookie Manipulation / Privilege Escalation.
* **Technical Logic:** The application relies on a client-side cookie (`Admin=false`) to determine if a user has administrative rights. Because the server does not verify this role against a backend database or use a cryptographically signed token (like a JWT), an attacker can simply modify the cookie value to `true` to gain full administrative access.

---

### 📑 Technical Walkthrough

#### 1. Identification of the Role Marker
After logging in with standard credentials, I intercepted the server's response and identified a clear Boolean flag in the cookies used to define the user's role.

<div align="center">
  <img src="./screenshots/identification.png" alt="Cookie Identification" width="85%">
  <p><i><b>Figure 1:</b> The server setting the 'Admin=false' cookie upon login.</i></p>
</div>

#### 2. Forging Administrative Privileges
I used Burp Suite to intercept the login response and manually changed the `Admin` cookie value from `false` to `true` before it reached the browser.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Cookie Manipulation" width="85%">
  <p><i><b>Figure 2:</b> Modifying the cookie to escalate privileges to Administrator.</i></p>
</div>

#### 3. Bypassing Access Controls
With the forged cookie stored in the browser, I successfully accessed the restricted `/admin` endpoint, which previously returned a 401 or 403 error.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Admin Panel Access" width="85%">
  <p><i><b>Figure 3:</b> Accessing the administrative interface with the forged identity.</i></p>
</div>

#### 4. Unauthorized Action Execution
I utilized the administrative delete function to remove the user `carlos`. The server processed the request as if it originated from a legitimate administrator.

<div align="center">
  <img src="./screenshots/verification.png" alt="User Deleted" width="85%">
  <p><i><b>Figure 4:</b> Deleting the target user via the escalated session.</i></p>
</div>

#### 5. Final Confirmation
The lab was marked as solved, confirming that the unauthorized administrative action was successfully executed.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
Never trust user-supplied data for authorization decisions. If a user can see a value, they can change it.

**Remediation:** Store user roles in a secure, server-side session or use cryptographically signed tokens (like HMAC or JWT) that cannot be tampered with by the client.
---