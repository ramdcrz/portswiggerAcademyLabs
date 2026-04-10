# WEB APPLICATION PENETRATION TEST REPORT: WEBSOCKET XSS

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 19 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report highlights a High-severity vulnerability involving the manipulation of WebSocket messages. The application incorrectly relies on client-side protections, allowing an attacker to intercept and inject a Stored Cross-Site Scripting (XSS) payload that executes in the context of administrative support agents.

### 2.1 Overall Risk Rating
**Rating: HIGH**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Stored XSS via WebSocket Frame Manipulation

| Attribute | Detail |
| :--- | :--- |
| **Severity** | High |
| **Finding ID** | VULN-001 |
| **Affected Endpoint** | `/chat` (WebSocket endpoint) |
| **CWE** | CWE-79: Improper Neutralization of Input During Web Page Generation |
| **CVSSv3 Score** | 7.6 (High) |
| **OWASP Category**| A03:2021 – Injection |
| **Status** | Open |

#### 3.1 Description
The application uses WebSockets for real-time live chat communication. The client-side JavaScript attempts to sanitize inputs by HTML-encoding special characters before transmission. By enabling WebSocket interception in a proxy tool, an attacker can bypass this client-side defense, modifying the JSON frame mid-transit to inject raw HTML payloads. The server broadcasts this unsanitized frame to all chat participants, leading to Stored XSS.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Analysis**
Sent a standard message and identified the JSON structure in Burp Suite's WebSockets history. Observed that client-side HTML encoding neutralizes `<script>` tags entered into the browser UI.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Identifying the basic JSON message structure in the WebSocket history.</i></p>
</div>
<div align="center">
  <img src="./screenshots/proxy.png" alt="Proxy" width="85%">
  <p><i><b>Figure 2:</b> Observing client-side HTML encoding in the WebSocket history.</i></p>
</div>

**2. Execution of XSS Traversal (Injection)**
Intercepted the outgoing WebSocket frame and manually replaced the encoded string with a raw HTML event handler: `<img src=1 onerror='alert(1)'>`.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Traversal" width="85%">
  <p><i><b>Figure 3:</b> Injecting the raw XSS payload into the intercepted WebSocket frame.</i></p>
</div>

**3. Verification & Confirmation**
The server echoed the raw HTML back to the browser and to the support agent, triggering the execution of the injected JavaScript alert.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Confirming the execution of the injected JavaScript alert.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Confirmation" width="85%">
  <p><i><b>Figure 5:</b> Final confirmation of lab completion with the 'Solved' banner.</i></p>
</div>

#### 3.3 Business Impact
Stored XSS executed within the context of an administrative support agent's browser can lead to session hijacking, forced administrative actions, or exposure of internal support systems, severely compromising backend integrity.

#### 3.4 Remediation
Never rely on client-side encoding for security controls. Treat all data received via WebSockets as untrusted. Implement robust, server-side context-aware output encoding before broadcasting messages to clients.

#### 3.5 References
* **CWE-79:** https://cwe.mitre.org/data/definitions/79.html
* **OWASP WebSocket Security:** https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html#websockets