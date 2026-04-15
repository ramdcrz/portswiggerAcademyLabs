# WEB APPLICATION PENETRATION TEST REPORT: LLM EXCESSIVE AGENCY

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 1 April 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This report details a Critical vulnerability concerning Excessive Agency in an integrated Large Language Model (LLM). The application's customer-facing AI chatbot has been granted unrestricted access to a highly privileged internal API capable of executing raw SQL queries against the production database. By utilizing natural language prompts, an attacker can coerce the LLM into executing arbitrary SQL commands, leading to complete database compromise, unauthorized data extraction, and destructive actions.

### 2.1 Overall Risk Rating
**Rating: CRITICAL**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — Unrestricted Database Compromise via LLM Excessive Agency

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Critical |
| **Finding ID** | VULN-001 |
| **Affected Endpoint** | `/chat` (Live Chat API integrations) |
| **CWE** | CWE-1202: Excessive Privileges / Agency in AI Systems<br>CWE-89: Improper Neutralization of Special Elements used in an SQL Command |
| **CVSSv3 Score** | 9.8 (Critical) |
| **OWASP Category**| LLM06: Excessive Agency |
| **Status** | Open |

#### 3.1 Description
The application implements an AI-driven live chat feature. To enhance the chatbot's utility, developers integrated it with backend tools, including a `Debug SQL API`. This architectural decision violates the Principle of Least Privilege. The LLM is granted excessive agency to call this API with user-controlled string arguments without any intermediate authorization checks, strict parameter typing, or human-in-the-loop (HITL) approval. An attacker can map the LLM's available tools through conversational reconnaissance and subsequently instruct the LLM to pass malicious SQL payloads to the backend API, effectively using the AI as a proxy for SQL Injection.

#### 3.2 Proof of Concept (PoC)

**1. Identification & Reconnaissance**
Initiated a conversation with the live chat agent, probing for integrated capabilities. The LLM disclosed access to the `Debug SQL API` and confirmed it accepts raw SQL string arguments.
<div align="center">
  <img src="./screenshots/identification.png" alt="LLM Tool Leak" width="85%">
  <p><i><b>Figure 1:</b> The LLM disclosing its internal API access and parameter requirements.</i></p>
</div>

**2. Enumeration**
Instructed the LLM to call the API with a benign reconnaissance query (`SELECT * FROM users`). The LLM executed the query and returned sensitive internal data, confirming full read access to the database.
<div align="center">
  <img src="./screenshots/enumeration.png" alt="Database Dump via LLM" width="85%">
  <p><i><b>Figure 2:</b> The LLM executing the query and disclosing the contents of the users table.</i></p>
</div>

**3. Execution**
Instructed the LLM to execute a destructive state-changing operation: `DELETE FROM users WHERE username='carlos'`. The LLM processed the natural language command, formulated the API call, and executed the database deletion.
<div align="center">
  <img src="./screenshots/execution.png" alt="Destructive SQL Execution" width="85%">
  <p><i><b>Figure 3:</b> The LLM successfully deleting a user account based on natural language instructions.</i></p>
</div>

**4. Confirmation**
The successful deletion of the target user was verified, confirming total database compromise.
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Lab Solved" width="85%">
  <p><i><b>Figure 4:</b> Final confirmation of successful exploitation via Excessive Agency.</i></p>
</div>

#### 3.3 Business Impact
Granting an LLM unrestricted execution rights over a database represents a catastrophic security failure. Attackers do not need specialized technical knowledge to exploit this; they simply ask the AI to perform the hack. This allows any user to drop entire database tables, exfiltrate sensitive customer PII, modify financial records, or destroy application availability.

#### 3.4 Remediation
1. **Principle of Least Privilege for LLMs:** Never grant an LLM access to raw execution APIs (like SQL execution, OS command execution, or raw file system access).
2. **Strict Tool Scoping:** If an LLM must interact with a database, it should only be