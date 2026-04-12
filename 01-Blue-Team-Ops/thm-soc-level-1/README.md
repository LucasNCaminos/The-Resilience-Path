# SOC Level 1: Practical Laboratory (TryHackMe Path)

## Status: Work In Progress (WIP)
> **Note:** This repository is currently being updated as I progress through the **SOC Level 1 Learning Path**. Here, I document the practical application of the concepts learned.

---

## I. Objective & Methodology
The goal of this folder is to provide a "Proof of Work" for my technical skills in Security Operations. Instead of just completing modules, I am documenting the scripts, logs, and methodologies used to solve real-world cybersecurity challenges.

### Core Focus Areas:
* **Cyber Threat Intelligence:** Analyzing adversary TTPs to enhance proactive defense.
* **Network Security & Traffic Analysis:** Identifying anomalous patterns in PCAP data using **Wireshark**, **Zeek**, and **Brim/Zui**.
* **Endpoint Security Monitoring:** Auditing system integrity and process behavior with **Sysmon** and **OSQuery**.
* **SIEM Operations:** Correlating multi-source logs within **Splunk** and **Wazuh** to reduce False Positives.
* **Digital Forensics & Incident Response (DFIR):** Reconstructing attack timelines through memory and artifact analysis (**Volatility/Autopsy**).
* **Phishing Analysis:** Performing header analysis and URL/Attachment sandboxing for malicious email triage.

---

## II. Repository Contents
As I advance through the path, this folder will be populated with:

* **`/scripts`**: Custom automation scripts (Bash/Python/PowerShell) developed to streamline log analysis or evidence gathering.
* **`/lab-notes`**: Tactical breakdowns of complex rooms, focusing on the *how* and *why* of the investigative process.
* **`/tool-configs`**: Configuration files and custom dashboard queries for SIEM tools.
* **`/phishing-cases`**: Documented analysis of simulated malicious emails.

---

## III. Why This Matters
While my academic background and preparation for certifications like **CompTIA Security+**, **Network+**, and **ISC2 CC** provided the strategic framework, these labs represent the **tactical execution**. This repository serves as the bridge between theoretical security principles and the hands-on proficiency required in a modern **Security Operations Center**.

---

## IV. Tactical Toolset
* **SIEM/XDR:** Splunk (Search & Reporting), Wazuh (Manager & Agents), ELK Stack.
* **Network Forensics:** Wireshark, Tshark, Zeek, Brim/Zui.
* **Host & Memory Forensics:** Autopsy, Volatility 3, FTK Imager.
* **Threat Intel & OSINT:** VirusTotal, Any.Run, AbuseIPDB, URLScan.io.

---
**Technical Standard:** All laboratory activities are documented following the **NIST SP 800-61 Rev. 2** Incident Handling guidelines where applicable.

