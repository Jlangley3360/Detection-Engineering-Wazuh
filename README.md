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

Figure 1: Verification of the simulated attack telemetry containing the 'xmrig' malicious process signature.
Phase 2: Authoring the Custom Detection Rule

To move past default telemetry monitoring to actionable intelligence, I configured a custom rule in the Wazuh manager's local rule set. The rule watches for specific process strings and explicitly maps the threat to MITRE ATT&CK Technique T1496 (Resource Hijacking) at a critical severity level.

    File Location: /var/ossec/etc/rules/local_rules.xml

Figure 2: Custom XML structure for Rule 100002, detailing rule dependencies, match criteria, and threat framework integration.
Phase 3: SIEM Validation & Triage

Once the updated alerting logic was initialized, the pipeline was triggered. Filtering for rule.id: "100002" inside the Wazuh Discover interface confirmed that the SIEM successfully processed the raw telemetry data and generated a high-priority alert.

Figure 3: Main event alert visible on the Wazuh timeline showcasing the correlated metadata and Level 10 severity status.

Expanding the document payload reveals the granular field breakdown. The engine successfully extracted fields like rule.mitre.id and populated the full_log telemetry parameters cleanly, preparing the event for a SOC analyst's investigation workflow.

Figure 4: Expanded metadata document mapping out defensive details, host data, and tactical framework criteria.
Conclusion & Defenses Demonstrated

By bridging endpoint telemetry collection with custom logic parsing, this project simulates a core responsibility of a Security Operations Center analyst: turning noisy host activity into actionable security events. The rule accurately identifies illegal mining scripts running in background spaces, minimizing dwell time and ensuring immediate tracking against the corporate technical baseline.
