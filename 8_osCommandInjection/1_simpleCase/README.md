# WEB APPLICATION PENETRATION TEST REPORT: OS COMMAND INJECTION

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 17 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report documents a Critical vulnerability involving OS Command Injection. The application unsafely passes user input to an underlying system shell, resulting in unauthorized Remote Code Execution (RCE) on the host server.

### 2.1 Overall Risk Rating
**Rating: CRITICAL**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Remote Code Execution via OS Command Injection

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Critical |
| **Finding ID** | VULN-001 |
| **Affected Component** | `/product/stock` (`storeId` parameter) |
| **CWE** | CWE-78: Improper Neutralization of Special Elements used in an OS Command |
| **CVSSv3 Score** | 9.8 (Critical) |
| **OWASP Category**| A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application passes user-supplied input from the `storeId` parameter directly into a system shell command without sanitization. By appending shell metacharacters such as the pipe operator (`|`), an attacker can terminate the intended command and chain arbitrary OS commands (e.g., `whoami`). The standard output of the injected command is reflected in the HTTP response.

#### 3.2 Proof of Concept (PoC)

**1. Identification**
Intercepted the stock check request to identify the parameters passed to the backend system.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Intercepting the baseline stock check request.</i></p>
</div>

**2. Injecting the Command**
Appended the pipe operator followed by the `whoami` command (`| whoami`) to the `storeId` parameter in Burp Repeater.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Payload Injection" width="85%">
  <p><i><b>Figure 2:</b> Injecting the command separator and the 'whoami' command.</i></p>
</div>

**3. Command Execution Results**
The server executed the command and returned the operating system username in the raw HTTP response.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Command Output" width="85%">
  <p><i><b>Figure 3:</b> Observing the successfully executed command output in the response.</i></p>
</div>

**4. Verification & Confirmation**
Analyzed the in-band reflection to confirm complete execution context.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Analyzing the reflected output from the server.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
OS Command Injection yields full Remote Code Execution (RCE). An attacker can read, modify, or destroy any data on the server, pivot to other internal network assets, and install persistent backdoors.

#### 3.4 Remediation
Never invoke underlying OS commands using user-supplied input. If system interaction is unavoidable, use language-specific, secure APIs (e.g., executing parameterized binaries where arguments are strictly passed as arrays, not concatenated strings).

#### 3.5 References
* **CWE-78:** https://cwe.mitre.org/data/definitions/78.html
* **OWASP Command Injection Prevention:** https://cheatsheetseries.owasp.org/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet.html