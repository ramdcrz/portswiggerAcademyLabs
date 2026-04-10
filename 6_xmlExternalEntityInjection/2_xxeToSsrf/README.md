# WEB APPLICATION PENETRATION TEST REPORT: XXE-BASED SSRF

## 1. Document Control

| Detail | Value |
| :--- | :--- |
| **Report Date** | 16 March 2026 |
| **Report Version** | 1.0 |
| **Classification** | CONFIDENTIAL |
| **Prepared By** | Ramil V. Deocariza Jr. |

---

## 2. Executive Summary
This assessment identified a Critical vulnerability where an XML External Entity (XXE) flaw was chained into a Server-Side Request Forgery (SSRF) attack. This allowed for the exfiltration of highly sensitive internal AWS IAM credentials.

### 2.1 Overall Risk Rating
**Rating: CRITICAL**

### 2.2 Risk Summary
| Critical | High | Medium | Low | Info | Total Findings |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 0 | 0 | 0 | 1 |

---

## 3. Detailed Findings

### VULN-001 — XXE Chained to SSRF Exfiltrating IAM Credentials

| Attribute | Detail |
| :--- | :--- |
| **Severity** | Critical |
| **Finding ID** | VULN-001 |
| **Affected Component** | "Check stock" functionality (XML Parser) |
| **CWE** | CWE-611 (XXE) / CWE-918 (SSRF) |
| **CVSSv3 Score** | 9.1 (Critical) |
| **OWASP Category**| A10:2021 – Server-Side Request Forgery (SSRF) |
| **Status** | Open |

#### 3.1 Description
The XML parser processing the "Check stock" feature insecurely resolves external entities. Instead of pointing the entity to a local file, it was pointed to an internal HTTP endpoint (`http://169.254.169.254/`). This forced the server to act as a proxy (SSRF), allowing manual crawling of the EC2 metadata service to retrieve the cloud environment's administrative IAM credentials.

#### 3.2 Proof of Concept (PoC)

**1. Identification**
Intercepted the XML-based "Check stock" request.
<div align="center">
  <img src="./screenshots/identification.png" alt="Identification" width="85%">
  <p><i><b>Figure 1:</b> Intercepting the baseline XML request.</i></p>
</div>

**2. Initial SSRF Discovery**
Injected a `SYSTEM` entity targeting the base AWS metadata URL. The server reflected the API directory structure.
<div align="center">
  <img src="./screenshots/proxy.png" alt="SSRF Discovery" width="85%">
  <p><i><b>Figure 2:</b> Probing the internal network via the XML parser.</i></p>
</div>

**3. Intermediate Path Traversal**
Iteratively updated the DTD URL to navigate the internal API tree to `latest/meta-data/iam/security-credentials/admin`.
<div align="center">
  <img src="./screenshots/traversal.png" alt="Traversal" width="85%">
  <p><i><b>Figure 3:</b> Evidence of manual traversal to the admin credentials folder.</i></p>
</div>

**4. Data Exfiltration & Verification**
Targeting the final endpoint successfully exfiltrated the `SecretAccessKey` JSON object to the external session.
<div align="center">
  <img src="./screenshots/verification.png" alt="Verification" width="85%">
  <p><i><b>Figure 4:</b> Successfully retrieving the IAM SecretAccessKey.</i></p>
</div>
<div align="center">
  <img src="./screenshots/confirmation.png" alt="Confirmation" width="85%">
  <p><i><b>Figure 5:</b> Lab solved confirmation.</i></p>
</div>

#### 3.3 Business Impact
Retrieval of internal cloud IAM credentials leads to total infrastructure compromise. Attackers can utilize these keys to pivot into the AWS environment, manipulate infrastructure, access databases, and exfiltrate entire datasets.

#### 3.4 Remediation
1. **Primary:** Disable DTDs and external entity resolution completely within the XML processing library.
2. **Defense-in-Depth:** Configure network-level firewall rules or IMDSv2 (Instance Metadata Service Version 2) on cloud instances to restrict unauthorized access to the `169.254.169.254` endpoint.

#### 3.5 References
* **CWE-918:** https://cwe.mitre.org/data/definitions/918.html
* **AWS IMDSv2 Documentation:** https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html