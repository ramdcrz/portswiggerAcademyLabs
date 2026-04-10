# WEB APPLICATION PENETRATION TEST REPORT: CORS MISCONFIGURATION (ORIGIN REFLECTION)

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 16 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details the discovery of a critical Cross-Origin Resource Sharing (CORS) misconfiguration. The server blindly trusts arbitrary origin headers, allowing malicious third-party sites to exfiltrate sensitive authenticated data.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Arbitrary Origin Reflection in CORS Policy

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected URL** | `/accountDetails` |
| **CWE** | CWE-942: Permissive Cross-domain Policy with Untrusted Domains |
| **CVSSv3 Score** | 8.1 (High) |
| **OWASP Category**| A05:2021 – Security Misconfiguration |
| **Status** | Open |

#### 3.1 Description
The application reads the `Origin` header from incoming requests and reflects it verbatim in the `Access-Control-Allow-Origin` response header, while simultaneously setting `Access-Control-Allow-Credentials: true`. This allows any malicious website to execute authenticated AJAX/XHR requests against the application and read the sensitive responses.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Analysis**
Analyzed the `/accountDetails` endpoint, identifying sensitive JSON data and the presence of CORS headers.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Intercepting the account details API request.</i></p>
</div>

**2. Proving Origin Reflection**
Injected a fake `Origin` header via Burp Repeater. The server mirrored the untrusted origin back in its access control headers.
<div align="center">
  <img src="./screenshots/proxy.png" alt="Origin Reflection" width="85%">
  <p><i><b>Figure 2:</b> Evidence of the server reflecting an untrusted Origin.</i></p>
</div>

**3. Crafting the Exfiltration Script**
Hosted a malicious JavaScript payload that issues an XHR request with `withCredentials = true`, parsing the response and forwarding the API key to an external log.
<div align="center">
  <img src="./screenshots/payload.png" alt="CORS Exploit" width="85%">
  <p><i><b>Figure 3:</b> The malicious XHR script designed to steal the API key.</i></p>
</div>

**4. Execution & Verification**
Delivered the exploit to the administrator and retrieved their private API key from the external access log.
<div align="center">
  <img src="./screenshots/verification.png" alt="Log Analysis" width="85%">
  <p><i><b>Figure 4:</b> Successfully exfiltrating the admin API key into the exploit log.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of the solved CORS lab.</i></p>
</div>

#### 3.3 Business Impact
Allows an attacker to bypass the Same-Origin Policy (SOP) and read sensitive data, such as API keys, PII, or CSRF tokens, leading to account compromise or further unauthorized API access.

#### 3.4 Remediation
Never dynamically reflect the `Origin` header. Implement a strict **whitelist** of explicitly trusted domains. If an incoming origin does not match the whitelist exactly, deny the CORS request.

#### 3.5 References
* **CWE-942:** https://cwe.mitre.org/data/definitions/942.html
* **OWASP CORS Misconfigurations:** https://owasp.org/www-community/attacks/CORS_OriginHeaderScrutiny