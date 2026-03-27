# 🛡️ Lab: User role controlled by request parameter

> **Category:** `Access Control`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a Mass Assignment vulnerability in a JSON-based profile update feature to escalate privileges and delete the user `carlos`.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** JSON body in the email update request.
* **Payload Type:** Mass Assignment / JSON Injection.
* **Technical Logic:** The application uses a JSON object to transfer user data between the client and server. By observing the server's response, I identified a hidden `roleid` parameter. Since the server does not filter which fields it allows the client to update, I was able to inject `"roleid": 2` into my request, effectively promoting my own account to Administrator status.

---

### 📑 Technical Walkthrough

#### 1. Discovering Hidden Parameters
After performing a standard email update, I analyzed the server's response. I discovered that the application returns the full user object, disclosing the internal `roleid` used for access control.

<div align="center">
  <img src="./screenshots/identification.png" alt="JSON Disclosure" width="85%">
  <p><i><b>Figure 1:</b> Identifying the 'roleid' parameter in the JSON response.</i></p>
</div>

#### 2. Executing Mass Assignment
I moved the update request to **Burp Repeater** and appended the administrative `roleid` to the JSON payload. This targets the backend logic that maps incoming JSON directly to the user's database record.

<div align="center">
  <img src="./screenshots/proxy.png" alt="JSON Injection" width="85%">
  <p><i><b>Figure 2:</b> Injecting the 'roleid': 2 parameter into the profile update request.</i></p>
</div>

#### 3. Privilege Escalation Verification
The server's response confirmed that the `roleid` was successfully updated to `2`. This confirmed that I had bypassed the application's intended authorization logic.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Role Confirmed" width="85%">
  <p><i><b>Figure 3:</b> Server response confirming the unauthorized role change.</i></p>
</div>

#### 4. Administrative Action
With administrative rights active, I accessed the restricted `/admin` panel and successfully executed the command to delete `carlos`.

<div align="center">
  <img src="./screenshots/verification.png" alt="Admin Action" width="85%">
  <p><i><b>Figure 4:</b> Using the escalated privileges to remove the target user.</i></p>
</div>

#### 5. Final Confirmation
The lab was marked as solved upon the successful deletion of the user account.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
Mass Assignment occurs when an application takes user input and uses it to update an object without a "whitelist" of allowed fields. You should never allow clients to modify internal-only fields like roles, permissions, or balances.

**Remediation:** Use Data Transfer Objects (DTOs) to strictly define which fields can be updated by a user, or implement a strict whitelist on the backend to ignore sensitive parameters in incoming requests.
---