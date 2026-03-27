# 🛡️ Lab: 2FA simple bypass

> **Category:** `Authentication`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to bypass a two-factor authentication (2FA) requirement by exploiting a session management flaw that allows direct access to protected pages after the first authentication factor is provided.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** Incomplete authentication state enforcement.
* **Payload Type:** Forced Browsing / Direct URL Access.
* **Technical Logic:** The application logic follows a "Password Check -> Redirect to 2FA" flow. However, the session is technically established as "authenticated" as soon as the password is correct. Because the `/my-account` page does not verify if the 2FA step was actually completed, I can skip the 2FA page entirely by manually navigating to the account URL.

---

### 📑 Technical Walkthrough

#### 1. Identification
I began by logging into my own account with the provided credentials (`wiener:peter`). This allowed me to observe the standard flow and identify the endpoint URL that should only be served after successful multi-factor authentication.

<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Establishing a baseline authentication flow by accessing the /my-account page of my own user.</i></p>
</div>

#### 2. Reaching the Proxy Gate (Factor 1)
I logged out and then attempted to log in using the victim's credentials (`carlos:montoya`). After submitting the correct password, the server validated the first factor and "proxied" me to the second-factor verification page. I am now presented with a form demanding a 4-digit security code.

<div align="center">
  <img src="./screenshots/proxy.png" alt="Proxy" width="85%">
  <p><i><b>Figure 2:</b> Reaching the 2FA wall on the /login2 endpoint for the target user, Carlos.</i></p>
</div>

#### 3. Execution of Path Traversal (Bypass)
Instead of providing a code, I decided to test if the "Fully Authenticated" session state was already set. I decided to try and "traverse" directly to the protected path. I manually modified the URL in the address bar from `/login2` to `/my-account` and submitted the request.

<div align="center">
  <img src="./screenshots/traversal.png" alt="Traversal" width="85%">
  <p><i><b>Figure 3:</b> Executing forced browsing to the account page to bypass the intended security control.</i></p>
</div>

#### 4. Verification
The server immediately processed the request without requiring the second factor, proving that the MFA control was purely cosmetic. I am now successfully authenticated as Carlos, and my full profile details are visible.

<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Gaining unauthorized access to the Carlos account. Note the solved banner already active in the header.</i></p>
</div>


#### 5. Confirmation
Final confirmation of lab completion. The "Congratulations, you solved the lab!" banner is clearly visible, marking the successful execution of the exploit.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Confirmation" width="85%">
  <p><i><b>Figure 5:</b> The final "Lab solved" confirmation banner.</i></p>
</div>

---

### 🧠 Key Takeaway
2FA is only as strong as the session logic protecting the subsequent pages. Authentication must be treated as a multi-step process where "Fully Authenticated" status is only granted after *all* factors are verified.

**Remediation:** Administrative panels and sensitive areas should be protected by server-side authorization checks that look for a "Fully Verified MFA" flag. Sessions should be held in a "partially-authenticated" state and only allowed to access the 2FA verification page until the second factor is validated.
---