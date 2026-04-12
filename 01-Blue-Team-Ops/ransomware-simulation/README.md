# Behavioral Ransomware Detection & Incident Response Lab

## Phase 1. Problem Definition & Environmental Context

### Objective
The primary goal of this laboratory is to detect, analyze, and document the lifecycle of a ransomware infection by focusing on **behavioral anomalies** rather than static signatures. This project demonstrates the ability to establish containment and recovery protocols aligned with the **NIST Incident Response Life Cycle (SP 800-61 Rev. 2)**.

### Scenario Background
This simulation replicates a high-impact security incident within a Financial Services environment. A malicious binary, masquerading as a legitimate financial reporting tool, bypassed initial perimeter defenses and was executed on a production endpoint. The malware initiated an automated encryption routine, targeting critical user directories (PII and financial records) while attempting to inhibit system recovery.

The lab focuses on the role of a **SOC Tier 1/2 Analyst** in identifying the infection through telemetry correlation and mitigating the impact before the exfiltration phase.

### Laboratory Architecture
The environment follows a "Victim-Analyst" isolated architecture to ensure safe execution and high-fidelity telemetry collection.

* **Endpoint (Victim):** Windows 10 Pro (Build 19044). Security controls (Tamper Protection/Real-time Scan) were intentionally tuned to simulate a signature-evasive threat.
* **Security Orchestration (SIEM/XDR):** Ubuntu Server 22.04 LTS hosting a **Wazuh Manager** instance. This node handles log aggregation, File Integrity Monitoring (FIM), and Sysmon integration.
* **Analysis & C2 Simulation:** Kali Linux 2024.1, utilized for forensic network analysis, traffic interception via Wireshark, and identifying Command & Control (C2) beacons.

### Attack Lifecycle & MITRE ATT&CK Mapping
The simulation covers the following technical milestones:
* **Execution (T1204.002):** Malicious File execution via user interaction.
* **Defense Evasion (T1562.001):** Disabling local monitoring tools.
* **Inhibit System Recovery (T1490):** Deletion of Volume Shadow Copies (VSS) using `vssadmin`.
* **Impact (T1486):** Data Encrypted for Impact (Detection through high-entropy file write operations).

### AI-Enhanced Detection Strategy
This project incorporates **LLM-assisted log normalization and analysis**. By utilizing AI to parse complex JSON telemetry from Sysmon and Wazuh, the detection window was significantly reduced, allowing for automated pattern recognition of "Encryption-like" behavior that standard heuristic engines might miss.

---

## Phase 2. Technical Stack & Methodology

The simulation follows the **NIST SP 800-61 Rev. 2** Incident Handling Guide, focusing on the **Detection and Analysis** and **Containment, Eradication, and Recovery** phases.

### 2.1 Toolset Documentation

| Tool | Category | Technical Application |
| :--- | :--- | :--- |
| **Wazuh** | SIEM / XDR | Centralized log aggregation and real-time File Integrity Monitoring (FIM). Used for automated behavioral alerts. |
| **Microsoft Sysmon** | Endpoint Telemetry | Granular logging of process creation (EID 1), file system changes (EID 11), and system recovery tampering (EID 1 - vssadmin). |
| **Splunk (KQL/SPL)** | Log Management | Advanced querying to reconstruct the attack timeline and identify parent-child process anomalies. |
| **Wireshark** | Network Forensics | Traffic analysis to identify Command & Control (C2) beaconing and data exfiltration patterns. |
| **LLM (AI Analysis)** | Automated Triage | Custom Python scripts utilizing LLMs for high-speed normalization and risk-scoring of Sysmon JSON logs. |

### 2.2 Defense-in-Depth Strategy

The laboratory architecture implements a layered defense model to evaluate security controls at multiple levels:

* **Endpoint Layer:** Sysmon is configured with a high-visibility schema to capture "Living off the Land" (LotL) techniques that bypass traditional AV signatures.
* **Network Layer:** Monitoring of "East-West" traffic to detect lateral movement and "North-South" traffic for C2 communication detection.
* **Intelligence Layer:** Integration of AI-driven log parsing to reduce the signal-to-noise ratio, allowing the analyst to focus on high-entropy file modifications typical of ransomware encryption.

### 2.3 Methodology: Behavioral Over Signatures

Unlike legacy systems that rely on known file hashes, this lab utilizes **Behavioral Indicators of Compromise (BIoCs)**. The methodology focuses on detecting the *consequences* of the execution, such as:
1.  Massive file renaming patterns.
2.  Unusual process hierarchy (e.g., an Office document spawning a PowerShell instance).
3.  Unauthorized modification of system backup configurations.

