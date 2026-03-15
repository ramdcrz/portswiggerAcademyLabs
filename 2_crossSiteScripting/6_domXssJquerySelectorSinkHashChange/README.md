# 🛡️ Lab: DOM XSS in jQuery selector sink using a hashchange event

> **Category:** `Cross-site Scripting (XSS)`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a **DOM-based XSS** vulnerability within a jQuery selector. The goal is to deliver an exploit that triggers the `print()` function in the victim's browser using a `hashchange` event.

### 🛠️ Exploit Strategy
* **Source:** `location.hash`
* **Sink:** jQuery's `$()` selector function.
* **Vulnerability Point:** A `hashchange` event listener on the home page.
* **Payload:** `<iframe src="[LAB_URL]/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>`
* **Technical Logic:** The vulnerable script uses `$(location.hash)` to find an element and scroll to it. By delivering a payload via an iframe, I can append an `<img>` tag to the hash. When the iframe loads, jQuery processes the hash as a selector, creates the image element, fails to find the source `x`, and executes the `onerror` script.

---

### 📑 Technical Walkthrough

#### 1. Identification & Reconnaissance
I examined the home page source code and identified a JavaScript block using the `hashchange` event. The script passes `location.hash` directly into the jQuery selector `$()`, which is a known dangerous sink if the input is not sanitized.

<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Discovering the vulnerable jQuery hashchange logic in the source code.</i></p>
</div>

#### 2. Crafting the Multi-Stage Exploit
To automate the attack for a victim, I used the provided Exploit Server. I crafted an `<iframe>` that loads the lab and then appends the malicious image payload to the URL hash once the initial load is complete.

<div align="center">
  <img src="./screenshots/payload.png" alt="Exploit Payload" width="85%">
  <p><i><b>Figure 2:</b> Configuring the malicious iframe on the exploit server.</i></p>
</div>

#### 3. Execution & Verification
By viewing the exploit, I confirmed that the iframe successfully triggered the hash change. The jQuery selector processed the injected `<img>` tag, and the `onerror` event fired the `print()` function.

<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 3:</b> The print dialog triggered via the DOM XSS payload.</i></p>
</div>

#### 4. Final Confirmation
I delivered the exploit to the victim via the exploit server. The server successfully simulated the victim's interaction, and the lab was marked as solved.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 4:</b> Final confirmation of lab completion.</i></p>
</div>

---

### 🧠 Key Takeaway
**DOM-based XSS** can be triggered by URL fragments (`#`). Since fragments are often not sent to the server, server-side filters cannot detect this attack. The vulnerability lies entirely in how the client-side JavaScript handles the `location.hash`.

**Remediation:** Avoid passing user-controlled data directly into the `$()` selector. Use more specific selection methods or sanitize the hash input to ensure it only contains expected alphanumeric characters before processing.

---