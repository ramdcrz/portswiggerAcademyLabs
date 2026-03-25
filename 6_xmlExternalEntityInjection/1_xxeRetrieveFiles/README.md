# 🛡️ Lab: Exploiting XXE using external entities to retrieve files

> **Category:** `XML External Entity (XXE) Injection`  
> **Difficulty:** `Apprentice`  
> **Status:** `Completed` ✅

---

### 🎯 Objective
The objective of this lab is to exploit a vulnerability in an XML parser to retrieve the contents of the `/etc/passwd` file from the server.

### 🛠️ Exploit Strategy
* **Vulnerability Point:** "Check stock" feature (XML input).
* **Payload Type:** XXE (External Entity Injection).
* **Technical Logic:** The application's XML parser is configured to resolve external entities. By defining a custom entity (`&xxe;`) with a `SYSTEM` identifier pointing to a local file, I can force the parser to read that file and include its content in the application's output (in this case, an error message).

---

### 📑 Technical Walkthrough

#### 1. Identification & Interception
I intercepted the stock check request and identified that the data is sent in XML format. This suggested that if the parser is not securely configured, it might process external entities.

<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Intercepting the standard XML stock check request.</i></p>
</div>

#### 2. Crafting the XXE Payload
In **Burp Repeater**, I modified the XML. I added a `DOCTYPE` definition that created an entity named `xxe` which points to the server's `/etc/passwd` file. I then referenced this entity within the `<productId>` tag.

<div align="center">
  <img src="./screenshots/proxy.png" alt="XXE Injection" width="85%">
  <p><i><b>Figure 2:</b> Injecting the DTD and referencing the external entity.</i></p>
</div>

#### 3. Execution & File Retrieval
Upon sending the request, the XML parser resolved the entity by reading the specified file. Since the resulting "ID" was invalid, the application reflected the file's content back to me in the error response.

<div align="center">
  <img src="./screenshots/verification.png" alt="File Retrieval" width="85%">
  <p><i><b>Figure 3:</b> Successfully retrieving the contents of /etc/passwd.</i></p>
</div>

#### 4. Final Confirmation
The successful retrieval of the system file confirmed the vulnerability and satisfied the lab requirements.

<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 4:</b> Lab solve confirmation.</i></p>
</div>

---

### 🧠 Key Takeaway
XML parsers are often "insecure by default." If they are allowed to resolve external entities, an attacker can read sensitive files, access internal APIs, or perform SSRF.

**Remediation:** Disable the resolution of external entities and support for `DOCTYPE` in all XML parsers used by the application. This is the most effective way to prevent XXE.

---