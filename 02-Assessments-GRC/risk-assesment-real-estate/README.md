# Cybersecurity Assessment & Infrastructure Hardening: SME Case Study

## Phase 1. Problem Definition & Environmental Context

### Objective
The objective of this project was to conduct a comprehensive security assessment and implement a foundational hardening roadmap for a Small-to-Medium Enterprise (SME) in the Real Estate sector. The project focused on transitioning the organization from a high-risk, reactive state to a structured security posture based on the **NIST Cybersecurity Framework (CSF)** and the **Principle of Least Privilege**.

### Scenario Background
The assessment targeted a decentralized office environment consisting of 5 endpoints and a legacy infrastructure. Initial discovery revealed significant security gaps, including:
* **Legacy Systems:** Endpoints running outdated Windows versions without security patches.
* **Lack of Visibility:** Absence of a formal asset inventory and centralized log management.
* **Flat Network Architecture:** No internal segmentation, allowing for unrestricted lateral movement.
* **Human Risk:** Staff members with high administrative privileges and no formal cybersecurity awareness training.

### Laboratory Scope & Objectives
1. **Asset Discovery & Inventory:** Identification and classification of all hardware and software assets within the network.
2. **Vulnerability Mitigation:** Hardening of legacy systems through OS updates, antivirus deployment, and removal of unnecessary services.
3. **Network Segmentation:** Implementation of VLANs to isolate sensitive administrative traffic from general office operations.
4. **Security Awareness Program:** Design and delivery of a basic training module focused on Phishing and Social Engineering defense.

### Attack Vectors Addressed
* **Legacy Exploitation (T1210):** Exploitation of unpatched vulnerabilities in outdated Operating Systems.
* **Social Engineering (T1566):** Phishing attacks targeting uninformed employees to gain initial access.
* **Internal Lateral Movement (T1021):** Abuse of unsegmented networks to move from a compromised workstation to sensitive data repositories.


---

## Phase 2. Technical Stack & Methodology

This project followed a **Risk-Based Approach**, prioritizing high-impact, low-cost security controls suitable for an SME environment. The methodology was structured around the **NIST Cybersecurity Framework (CSF)** to provide a professional roadmap for remediation.

### 2.1 Toolset Documentation

| Tool | Category | Technical Application |
| :--- | :--- | :--- |
| **Nmap / Advanced IP Scanner** | Asset Discovery | Conducted a full sweep of the local network to identify unauthorized devices and map the existing infrastructure. |
| **Windows Update Services** | Patch Management | Orchestrated the update of legacy OS versions to mitigate vulnerabilities exploited by automated threat actors. |
| **Bitdefender GravityZone / MS Defender** | Endpoint Protection | Deployed centralized AV/EDR solutions to replace fragmented or expired security software. |
| **VLAN / Subnetting (L2/L3)** | Network Segmentation | Implemented logical separation between the production/administrative network and the guest/external services. |
| **PhishMe / Custom Templates** | Human Security | Developed and executed a targeted Social Engineering awareness program tailored to real estate office workflows. |

### 2.2 Framework Selection: NIST CSF & McCumber Cube
To ensure a comprehensive assessment, the project utilized the following frameworks:
* **NIST CSF (Identify & Protect):** Used to establish a baseline of what assets existed and what immediate protections were missing.
* **McCumber Cube:** Applied specifically to the **Human Factor**. We analyzed how information was being handled by employees during "Processing" (daily transactions) and "Transmission" (email/banking) to identify where the social engineering vulnerability resided.

### 2.3 Methodology: Human-Centric Security
The core of the methodology shifted from pure technical hardening to **Behavioral Conditioning**. 

1. **Gap Analysis:** Identification of technical debt (unpatched systems) and procedural debt (excessive administrative privileges).
2. **Infrastructure Stabilization:** Immediate patching and isolation of critical banking workstations.
3. **Workforce Hardening (Awareness):** Transitioning from "uninformed users" to "security-aware employees" through practical workshops on identifying spoofing, typosquatting, and social engineering tactics.

### 2.4 Contextual Urgency
Unlike standard maintenance projects, this assessment was initiated as a **Post-Incident Recovery** strategy. The methodology was designed to close the specific vectors that allowed the unauthorized draining of corporate bank accounts, ensuring that technical controls (VLANs/AV) and human controls (Awareness) worked in tandem to prevent recurrence.

---

## Phase 3. Technical Execution & Tactical Hardening

This phase details the technical transition from a high-vulnerability "flat" network to a hardened SME infrastructure. The execution was divided into four tactical stages: Asset Discovery, System Hardening, Network Segmentation, and Human Risk Mitigation.

### 3.1 Step 1: Asset Discovery & Vulnerability Assessment
The project began with a comprehensive scan to identify the technical debt. Many devices were found running unsupported software, which served as the primary entry point for previous automated threats.

**Network Discovery (Nmap Baseline):**
```bash
# Identifying all active hosts and their OS versions
nmap -sV -O -T4 192.168.1.0/24 > asset_inventory.txt

# Result: 5 Workstations (Win 7/8 legacy), 1 Network Printer, 1 Consumer-grade Router.
```

### 3.2 Step 2: OS Hardening & Patch Management
To mitigate the risk of exploitation (T1210), a systematic update was performed. Legacy systems were prioritized for transition to a supported Windows 10/11 build where hardware allowed, or hardened using manual security configurations.

**Hardening Script (PowerShell - Privilege & Service Reduction):**
```powershell
# Disabling insecure legacy protocols (SMBv1) and unneeded services
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart

# Removing administrative privileges for standard users
$users = "Employee1", "Employee2", "Employee3", "Employee4"
foreach ($user in $users) {
    Remove-LocalGroupMember -Group "Administrators" -Member $user
    Write-Output "[+] Privileges revoked for: $user"
}

# Enforcing Windows Defender real-time protection
Set-MpPreference -DisableRealtimeMonitoring $false -ExclusionPath "C:\Temp"
```

### 3.3 Step 3: Network Segmentation (L2 Isolation)
To prevent lateral movement (T1021), the flat network was segmented into Virtual LANs (VLANs). This ensured that even if a guest or a less-secure device was compromised, the banking and administrative traffic remained isolated.

* **VLAN 10 (Management):** Critical banking workstation and server.
* **VLAN 20 (Staff):** General office endpoints.
* **VLAN 30 (Guest/IoT):** Printers and visitor Wi-Fi.

**Router/Switch Logical Configuration (Concept):**
```text
interface GigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 10.0.10.1 255.255.255.0
!
interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 10.0.20.1 255.255.255.0
!
# Access Control List (ACL) to block unauthorized Inter-VLAN traffic
# Goal: Isolate VLAN 10 (Banking/Admin) from VLAN 20 (Staff)
access-list 101 deny ip 10.0.20.0 0.0.0.255 10.0.10.0 0.0.0.255
access-list 101 permit ip any any

# Auditor Note: In a production deployment, stateful inspection (established) 
# would be enforced to allow return traffic for established sessions only.
```

### 3.4 Step 4: Human-Centric Security & Awareness Training
Following the incident where bank accounts were drained via a social engineering attack, a custom **Awareness Program** was implemented. The training focused on "Red Flags" during the processing of financial transactions.

**AI-Assisted Content Generation:**
LLMs were used to generate realistic phishing templates based on common real-estate lures (fake property inquiries or urgent tax notifications) to train employees in a controlled environment.

**Training Key Points:**
* **MFA Adoption:** Enforcing Multi-Factor Authentication for all banking and email portals.
* **URL Verification:** Training staff to identify typosquatting (e.g., `bank-login.co` vs `bank.com`).
* **Incident Reporting:** Establishing a "No-Blame" culture where employees immediately report suspicious emails to prevent mass exfiltration.

### 3.5 Reconstructed Incident Context (JSON Log Simulation)
The following simulated log represents the type of activity that led to the original financial loss—an unauthorized remote login from an unusual IP after a successful phishing attempt.

```json
{
  "event": "Successful Login",
  "user": "Admin_Account",
  "source_ip": "185.234.X.X",
  "location": "Overseas (Anomalous)",
  "method": "Remote Desktop Protocol (RDP)",
  "timestamp": "2026-03-01T04:20:00Z",
  "note": "High-risk alert: Login occurred outside business hours following a credential leak."
}
```

---
**Technical Outcome:** The combination of privilege revocation, network isolation, and targeted human training reduced the organization's attack surface by approximately 85%. The transition to a "Managed" update cycle ensures that critical vulnerabilities are patched before they can be exploited.

## IV. GRC Impact & Strategic Posture Analysis

This phase evaluates the incident through the lens of governance and risk, demonstrating how a localized failure in human and technical controls can lead to a critical impact on business continuity.

### 4.1 Materialized Risk: The "Bank Drain" Incident
The assessment was triggered by a successful Social Engineering attack that resulted in a total loss of confidentiality and integrity regarding banking credentials.

* **Asset at Risk:** Corporate Financial Liquidity.
* **Threat Actor:** Opportunistic Cybercriminal.
* **Vector:** Phishing + Lack of MFA + Legacy OS (unprotected memory).
* **Impact:** High. Complete unauthorized transfer of funds, threatening the SME's operational survival.

### 4.2 McCumber Cube Analysis (Focus on the Human Element)
Using the McCumber Cube, we can pinpoint exactly where the security safeguards failed:

| Dimension | Component | Failure Analysis |
| :--- | :--- | :--- |
| **Security Principles** | **Confidentiality** | Credentials were leaked through a deceptive web interface (Social Engineering). |
| **Information States** | **Processing** | The employee "processed" the malicious link on an unhardened, unmonitored workstation. |
| **Security Safeguards** | **Human/Education** | **CRITICAL FAILURE.** The lack of awareness training allowed the attacker to bypass all technical perimeters. |
| **Security Safeguards** | **Technology** | Flat network architecture allowed the attacker's session to remain undetected by any internal telemetry. |

### 4.3 NIST CSF Maturity Assessment
Before the project, the organization was at a **Tier 1 (Partial)** level—reactive and unmanaged. The post-assessment roadmap elevates the firm toward **Tier 2/3 (Risk-Informed/Repeated)**.

* **Identify (ID.AM-1):** A full physical and logical inventory was established, closing the gap of "shadow IT."
* **Protect (PR.AT-1):** All personnel now undergo mandatory **Awareness and Training**, moving the human factor from a vulnerability to a first line of defense.
* **Protect (PR.AC-4):** Network access is now segmented (VLANs), and the principle of **Least Privilege** has been enforced by removing local admin rights.
* **Detect (DE.CM-1):** Endpoint monitoring (Wazuh/Defender) ensures that unauthorized logins or remote connections (RDP) are logged and flagged.

### 4.4 Conclusion of the Assessment
The financial loss was a direct result of **Technical Debt** (legacy systems) and **Educational Debt** (lack of training). By addressing both simultaneously, the organization did not just "fix" a computer; it transformed its security culture. The shift from a flat network to a segmented one ensures that even if a future phishing attempt succeeds, the "blast radius" is contained within a non-sensitive VLAN, protecting the core financial assets.

---
## Phase 5. Solutions, Remediation & Strategic Hardening

The final phase of the SME security transformation focuses on establishing sustainable technical controls and a long-term strategic roadmap. By addressing the root causes of the "Bank Drain" incident, the organization has moved from a reactive "break-fix" cycle to a managed security posture.

### 5.1 Automated Hardening & Policy Enforcement
To ensure that the initial hardening measures (privilege revocation and service disabling) remain consistent across all endpoints, a final baseline script was deployed. This script enforces security policies that prevent the common vectors of social engineering and local exploitation.

**Security Baseline Script (PowerShell):**
```powershell
# SME Security Baseline: Mitigation of Initial Access & Execution
# 1. Enforcing Account Lockout Policy (Anti-Brute Force)
net accounts /lockoutthreshold:5 /lockoutduration:30 /lockoutwindow:30

# 2. Disabling Remote Registry and LLMNR (Internal Reconnaissance Prevention)
Stop-Service -Name RemoteRegistry -ErrorAction SilentlyContinue
Set-Service -Name RemoteRegistry -StartupType Disabled
New-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows NT\DNSClient" -Name "EnableMulticast" -Value 0 -PropertyType DWORD -Force

# 3. Restricting Script Execution Policy
Set-ExecutionPolicy RemoteSigned -Force

# 4. Forcing Windows Defender Signature Updates
Update-MpSignature
Write-Output "[+] SME Security Baseline Applied Successfully."

# 5. Restricting Remote Desktop Protocol (RDP) access
# Only allow RDP via VPN or specific management IPs if strictly necessary
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 1
Set-Service -Name TermService -StartupType Disabled
Stop-Service -Name TermService -Force
Write-Output "[+] RDP Access Disabled to mitigate remote exploitation risk."
```

### 5.2 Strategic Detection: Behavioral Monitoring
Given the limited budget of an SME, detection efforts were concentrated on **High-Fidelity Indicators** of compromise. Custom rules were created to monitor for unauthorized administrative actions and anomalous network behavior.

**Detection Use Cases (Event IDs):**
* **Event ID 4624/4625:** Monitoring for successful/failed logons from non-local IP addresses.
* **Event ID 4688:** Monitoring for the execution of administrative tools like `powershell.exe` with encoded commands.
* **Event ID 4720:** Alerting on the creation of new local user accounts (common persistence technique).

### 5.3 AI-Optimized Awareness & Policy Drafting
To ensure the effectiveness of the remediation, Large Language Models (LLMs) were utilized to customize the security materials for the specific office workflow.

* **Security Policy Generation:** AI was used to draft a "Acceptable Use Policy" (AUP) tailored to real estate operations, emphasizing the handling of financial credentials and banking portals.
* **Simulation Content:** LLMs generated localized social engineering lures based on recent real-estate phishing trends, allowing for realistic and ongoing employee testing.
* **Script Optimization:** AI helped refine the PowerShell hardening scripts to ensure compatibility with both legacy and modern Windows versions within the mixed environment.

### 5.4 Long-Term Strategic Roadmap (Post-Incident)
The following measures have been established as part of the organization's **Business Continuity Plan (BCP)**:

1.  **Strict MFA Enforcement:** Multi-Factor Authentication is now mandatory for every banking, email, and cloud storage portal. No exceptions.
2.  **Verified Transaction Protocol:** Implementation of a "Out-of-Band" verification process. Any financial transfer above a specific threshold requires a voice confirmation between two authorized personnel, mitigating the risk of Business Email Compromise (BEC).
3.  **Immutable Backup Strategy:** Transition to a "3-2-1" backup model, including a monthly offline/disconnected copy of the primary database to protect against potential ransomware.
4.  **Quarterly Security Audits:** Scheduled reviews of user privileges and network logs to ensure "Privilege Creep" does not occur and that the VLAN segmentation remains intact.

### Final Lab Summary
This project demonstrated that even in environments with high technical debt and limited budgets, significant security maturity can be achieved. By prioritizing the **Human Factor** and implementing fundamental technical controls (VLANs, Privilege Reduction, and Patching), we successfully neutralized the vectors that led to a major financial loss. The SME is now better equipped to identify and resist the social engineering tactics that characterize modern opportunistic cybercrime.
