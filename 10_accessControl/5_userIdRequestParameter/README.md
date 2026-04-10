# WEB APPLICATION PENETRATION TEST REPORT: INSECURE DIRECT OBJECT REFERENCE

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 18 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details an Insecure Direct Object Reference (IDOR) vulnerability. The application uses predictable URL parameters to fetch private account data without verifying if the authenticated session holds the authorization to access the requested resource.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Horizontal Privilege Escalation via IDOR

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected URL** | `/my-account?id=[username]` |
| **CWE** | CWE-639: Authorization Bypass Through User-Controlled Key |
| **CVSSv3 Score** | 7.5 (High) |
| **OWASP Category**| A01:2021 – Broken Access Control |
| **Status** | Open |

#### 3.1 Description
The application uses a predictable identifier (the username) in the URL parameter to determine which user's data to display. The backend explicitly trusts this parameter without validating it against the active session token, allowing horizontal privilege escalation by simply swapping the username.

#### 3.2 Proof of Concept (PoC)

**1. Baseline Observation**
Identified the `id` GET parameter dictating the account data retrieval.
<div align="center">
  <img src="./screenshots/identification.png" alt="Wiener Account Page" width="85%">
  <p><i><b>Figure 1:</b> Identifying the 'id' parameter used for account data retrieval.</i></p>
</div>

**2. Executing IDOR**
Modified the URL parameter from `id=wiener` to target `id=carlos`.
<div align="center">
  <img src="./screenshots/proxy.png" alt="URL Manipulation" width="85%">
  <p><i><b>Figure 2:</b> Modifying the request to target the account of user 'carlos'.</i></p>
</div>

**3. Unauthorized Data Access & Verification**
The server successfully processed the IDOR request, returning the full account page and private API key for `carlos`.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Carlos API Key" width="85%">
  <p><i><b>Figure 3:</b> Successfully exfiltrating the API key for user 'carlos'.</i></p>
</div>
<div align="center">
  <img src="./screenshots/verification.png" alt="Submission" width="85%">
  <p><i><b>Figure 4:</b> Preparing the exfiltrated key for solution submission.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion.</i></p>
</div>

#### 3.3 Business Impact
Allows attackers to systematically access, scrape, or modify any user's private data, leading to massive data breaches and privacy violations.

#### 3.4 Remediation
Implement robust server-side authorization checks. Ensure that the resource requested in the `id` parameter strictly matches the identity verified by the active session token.

#### 3.5 References
* **CWE-639:** https://cwe.mitre.org/data/definitions/639.html
* **OWASP IDOR Prevention:** https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html