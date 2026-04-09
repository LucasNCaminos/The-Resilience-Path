# Incident Response Simulation: Ransomware Scenario Analysis

## I. Problem Definition & Environmental Context

### Objective
To detect, analyze, and document the lifecycle of a ransomware infection within a controlled Windows environment, establishing effective containment and recovery protocols based on NIST standards.

### Scenario Background
This project simulates a high-impact security incident where an unauthorized binary, masquerading as a financial update, was executed on a production endpoint. The malware initiated an automated encryption routine, targeting critical user directories and modifying file extensions to `.crypt`. The focus of this simulation is the role of a SOC Tier 1 Analyst in identifying the infection vector and mitigating lateral movement.

### Laboratory Environment
The environment was structured using a virtualized "Victim-Analyst" architecture to ensure safe execution and telemetry isolation.

* **Endpoint (Victim):** Windows 10 Pro (Build 19044). Real-time protections were adjusted to simulate a successful initial execution.
* **Management Node:** Ubuntu Server 22.04 LTS hosting a Wazuh Manager instance for log aggregation and File Integrity Monitoring (FIM).
* **Analysis Station:** Kali Linux 2024.1, utilized for forensic network analysis and Command & Control (C2) identification.

---

## II. Technical Stack & Methodology

The simulation followed the **NIST SP 800-61 Rev. 2** Incident Handling Guide, specifically focusing on the Detection and Analysis phases.

### Toolset Documentation

| Tool | Category | Technical Application |
| :--- | :--- | :--- |
| **Wazuh** | SIEM / XDR | [cite_start]Real-time File Integrity Monitoring (FIM) and Sysmon event correlation[cite: 20, 22]. |
| **Splunk** | Log Management | [cite_start]Advanced querying (KQL/SPL) for identifying process trees and execution timelines[cite: 71, 73]. |
| **Wireshark** | Network Forensics | [cite_start]Pcap analysis to identify outbound C2 communication and DNS exfiltration patterns[cite: 22, 73]. |
| **Sysmon** | Endpoint Telemetry | Detailed event logging (Event ID 1, 11, and 7) for process and file creation tracking. |

### Defense-in-Depth Strategy
The analysis incorporates principles of layered defense, evaluating how perimeter controls and endpoint detection failed to prevent the initial execution, and how internal monitoring (East-West traffic) was used for detection.
