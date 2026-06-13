# Detection Engineering with Wazuh: Cryptomining Detection Lab

## Objective
This project demonstrates the implementation of detection engineering principles using Wazuh SIEM within a Linux VM home lab environment. The goal was to configure telemetry, author a custom detection rule for an active threat indicator (XMRig cryptomining malware), map the detection to the MITRE ATT&CK framework, and validate the end-to-end alerting pipeline on the Wazuh SOC Dashboard.

### Skills Documented:
* **SIEM Management:** Custom log analysis and dashboard navigation within Wazuh.
* **Detection Engineering:** Custom XML rule creation using parent-child rule architectures (`if_sid`).
* **Threat Mapping:** Aligning defensive alerts with industry-standard frameworks (MITRE ATT&CK).
* **Linux Security Operations:** Monitoring system telemetry, process tables, and raw log files.

---

## Architecture & Detection Pipeline

1. **Telemetry Generation:** An endpoint logs an indicator of compromise (IOC)—specifically the execution of a rogue cryptomining binary (`xmrig`).
2. **Log Pre-Decoding & Decoding:** The Wazuh Manager extracts crucial metadata (timestamp, hostname, and program name).
3. **Rules Engine & Threat Classification:** The custom rule evaluates the parsed log, matches the keyword signature, elevates the event severity, and tags it with a relevant MITRE ATT&CK technique.
4. **Dashboard Analysis:** The event is escalated to a Level 10 Critical Alert on the centralized security monitoring dashboard for analyst triage.

---

## Step-by-Step Implementation

### Phase 1: Telemetry Generation & Inspection
To simulate the attack footprint, process monitoring telemetry was directed to a dedicated local log file (`/var/log/malware.log`). Checking the raw file verified that the endpoint recorded the cryptographic execution behavior successfully.

* **Command Executed:**
  ```bash
  sudo tail -n 5 /var/log/malware.log
