# WEB APPLICATION PENETRATION TEST REPORT: IDOR (STATIC FILE ACCESS)

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 18 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details an Insecure Direct Object Reference (IDOR) vulnerability where sensitive static files (chat transcripts) are stored predictably and served without authorization checks, leading to credential leakage and account takeover.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — IDOR via Predictable Resource Location

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected URL** | `/download-transcript/[filename.txt]` |
| **CWE** | CWE-639: Authorization Bypass Through User-Controlled Key |
| **CVSSv3 Score** | 7.5 (High) |
| **OWASP Category**| A01:2021 – Broken Access Control |
| **Status** | Open |

#### 3.1 Description
The application saves sensitive live chat transcripts using predictable, sequential numerical filenames (e.g., `2.txt`). These static files are served directly to the client without verifying ownership or session authorization. An attacker can enumerate integer values to download historical chat logs belonging to other users.

#### 3.2 Proof of Concept (PoC)

**1. Baseline Request Identification**
Initiated a chat and downloaded the transcript, identifying the numerical naming convention.
<div align="center">
  <img src="./screenshots/identification.png" alt="Live Chat Transcript" width="85%">
  <p><i><b>Figure 1:</b> Accessing the chat transcript feature.</i></p>
</div>

**2. Analyzing the URL Pattern**
Observed the predictable sequence and manipulated the URL in the browser.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Sequential URL" width="85%">
  <p><i><b>Figure 2:</b> Identifying the predictable '2.txt' filename.</i></p>
</div>

**3. Unauthorized Data Retrieval**
Decremented the filename ID to `1.txt` to successfully retrieve a prior chat session containing sensitive credentials.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Carlos Password Leak" width="85%">
  <p><i><b>Figure 3:</b> Discovering Carlos's password within the leaked chat log.</i></p>
</div>

**4. Credential Verification**
Authenticated using the stolen password to successfully compromise the target account.
<div align="center">
  <img src="./screenshots/verification.png" alt="Account Access" width="85%">
  <p><i><b>Figure 4:</b> Successfully logging in as the target user.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
Predictable asset storage leads to mass data enumeration, privacy violations, and exposure of confidential communications, often containing actionable intelligence or credentials.

#### 3.4 Remediation
Use cryptographically secure, random, and non-sequential identifiers (like UUIDv4) for file storage. Additionally, do not serve sensitive files statically; route download requests through an application controller that verifies user authorization before serving the file.

#### 3.5 References
* **CWE-639:** https://cwe.mitre.org/data/definitions/639.html
* **OWASP IDOR Prevention:** https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html