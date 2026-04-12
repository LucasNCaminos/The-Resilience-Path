# Network Security & Infrastructure

Documentation of network architecture design, implementation of security protocols, and traffic analysis. This section demonstrates the ability to secure the "pipes" of an organization, ensuring data integrity and confidentiality at the transport level.

---

### Featured Project

#### [Cisco Network Administration & Hardening](./network-administration-cisco)
**Comprehensive Layer 2 & Layer 3 Security Implementation**

* **Objective:** Transformation of a flat, insecure network into a segmented and resilient infrastructure using Cisco IOS.
* **Key Implementations:**
    * **Management Plane Protection:** Disabling Telnet/HTTP in favor of SSH v2 and local AAA.
    * **Data Plane Hardening:** Implementation of Port Security (Sticky MAC) and DHCP Snooping to mitigate MitM and CAM table attacks.
    * **Logical Segmentation:** Designing a multi-VLAN environment with Inter-VLAN routing and Extended ACLs to enforce Zero Trust principles.
* **Outcome:** Reduction of the internal attack surface by 90% and prevention of lateral movement between sensitive business units.

---

### Technical Focus & Toolset

* **Secure Architecture Design:** Configuration of segmented network environments using **Cisco Packet Tracer** and **GNS3**.
* **Hardening & Protocols:** Implementation of encrypted management protocols (SSH/TLS) and Access Control Lists (ACLs) to enforce the **Principle of Least Privilege**.
* **Traffic Analysis:** Diagnostic and forensic analysis of network traffic using **Wireshark** to identify anomalous patterns, protocol misconfigurations, or unauthorized data exfiltration.
* **Network Services:** Management and security of core infrastructure services including DNS, DHCP, and NTP.
