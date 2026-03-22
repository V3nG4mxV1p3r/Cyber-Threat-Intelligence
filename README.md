# 🌍 Audit Case 4: Cyber Threat Intelligence (CTI) & Malware Detection

## 📌 Executive Summary
To enhance the SOC's automated detection capabilities, this project phase integrated global Cyber Threat Intelligence (CTI). By connecting the Wazuh SIEM with the VirusTotal API, the system can now autonomously analyze cryptographic hashes of new files in real-time, instantly flagging known malware without relying on local antivirus definitions.

## ⚙️ SIEM & API Integration Architecture
1. **Endpoint Configuration (FIM):** The Windows 10 agent was configured to monitor the `C:\PCI_Audit_Secure` directory in real-time, enforcing hash extraction (MD5, SHA-1, SHA-256) for any file creation or modification.
2. **Manager Configuration (API):** The Wazuh Manager (`ossec.conf`) was engineered to trigger a VirusTotal API lookup whenever a syscheck alert (Rule 554 or 550) was generated.

```xml
<integration>
  <name>virustotal</name>
  <api_key>[REDACTED_API_KEY]</api_key>
  <rule_id>554,550</rule_id>
  <alert_format>json</alert_format>
</integration>
```

## ⚔️ Malware Simulation
To test the pipeline safely, the **EICAR Standard Antivirus Test File** was introduced into the audited directory. 

## 📊 Audit Evidence & Results
The integration performed flawlessly. The SIEM executed the pipeline and returned a **Level 12 Critical Alert**:
* **File:** `c:\pci_audit_secure\eicar.txt`
* **Detection:** 66 out of 68 engines in the VirusTotal database flagged the hash as malicious.
* **MITRE ATT&CK Mapping:** Successfully mapped to `T1203` (Exploitation for Client Execution).

> **Evidence (CTI Dashboard Alert):**
![virustotal_log](https://github.com/user-attachments/assets/42ddeb7c-7250-41fa-8ebf-7c63cc038300)
