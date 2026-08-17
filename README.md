# 🖥️ Physical & Virtualized Enterprise Security Testbed

## 📊 Infrastructure Summary
```yaml
Host System:           Windows 11 Pro (Samsung SSD Storage)
Hypervisor:            VMware Workstation Pro
Virtual Switches:      NAT, Corporate LAN (Custom; Guest-only)
Virtual Machines:      Kali Linux (Attacker), Windows Targets (Victim), Wazuh SIEM (Collector)
Physical Hardware:     Cisco Catalyst 2960G Switch, HP Stream 11 (Parrot OS / SSH Management)
```

## 🌐 VMware Workstation Virtual Network Topology
​To simulate an enterprise environment while strictly containing attack traffic, the internal virtual network is split across two distinct virtual software switches:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│WINDOWS 11 HOST SYSTEM (Bare-Metal Engine)                                                                    │
│                                                                                                              │
│   ┌──────────────────────────────────────────────────────────────────────────────────────┐   │
│   │ VMWARE WORKSTATION PRO HYPERVISOR                                                                   │    │
│   │                                                                                                     │    │
│   │    NAT Subnet  ─── (Outbound Internet Updates Only)                                                 │    │
│   │            │                                                                                        │    │
│   │    Corporate LAN: Host-Only Isolated Network  ──────────────────────────────────────────┐    │    │
│   │            │                                                                                   │    │    │
│   │  ┌───────┴────────────┐ ┌─────────────────┐   ┌────────────────┐   ┌──────────────┴─┐  │    │
│   │  │ Windows Server 2019    │ │ Kali Linux Node    │   │ Windows Target    │   │ Wazuh SIEM Server │ │    │
│   │  │ (AD Domain Controller) │ │ (Offensive/Triage) │   │ (Audited Machine) │   │ (Log Ingestion)   │ │    │
│   │  └────────────────────┘ └─────────────────┘   └────────────────┘   └────────────────┘  │    │
│   └──────────────────────────────────────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────────────────────────────────┘
                                     │
                Physical Ethernet / PuTTY Console 
                                     │
                      ┌────────────┴───────────┐
                      │ HP Stream 11               │
                      │ (Parrot OS via SSH)        │
                      └────────────────────────┘
```

## 📝 Lab Architecture & Design Rationale

1. **Primary Virtualization Host**
   * **Bare-Metal Baseline:** Assembled a budget high-spec desktop machine and configured a Windows 11 Pro host utilizing high-speed Samsung SSD storage to ensure minimal I/O latency during multi-VM deployment.
   * **Hypervisor Layer:** VMware Workstation Pro hosting isolated attack/defense nodes (Kali Linux) configured with custom Host-Only and NAT virtual networks to contain testing traffic.

2. **Physical Network Infrastructure**
   * **Layer 2/3 Switching:** Integrated a dedicated Cisco Catalyst 2960G series gigabit switch to practice hardware configuration, port security, and VLAN isolation.

3. **Dedicated Administration Console**
   * **Endpoint:** Windows 11 Pro.
   * **Remote Management:** Configured terminal-based administration utilizing PuTTY and SSH to simulate headless command-line administration of network appliances and remote Linux virtual machines.

## ⚙️ Hypervisor Configuration & Guest Machine Roles
  * **1. Offensive Security Node (Kali Linux)**
​Network Adapter: Dual-homed ('NAT' for software updates; 'Corporate LAN' for targeted local attacks).
​Role: Serves as the primary security testing platform for running network discovery scans, credential auditing, and command-line traffic generation.
​
  * **2. Vulnerable Endpoint Node (Windows Target)**
​Network Adapter: Single-homed (Corporate LAN).
​Role: Acts as an isolated target endpoint used for file system access control audits, SQL database logging, and system event generation. Completely isolated from the host machine and external internet.
​
  * **3. Centralized Telemetry & SIEM Node (Wazuh Server)**
​Network Adapter: Single-homed (Corporate LAN).
​Role: Collects syslogs, security event logs, and authentication telemetry from internal virtual endpoints to practice live threat detection and rule parsing.
  
  * **4. Active Directory Domain-Controller (Windows Server 2019)**
​Network Adapter: Single-homed (Corporate LAN).
​Role: Provides domain services to the Active Directory of the simulated enterprise CYSEC, hosting domain-administrator, admin, users and guest accounts
Domain-name: cysec.local
​
## 🛠️ Security Exercises Conducted in Lab
  * **Network Layer Management:** Command-line network configuration and SSH session management across physical and virtual endpoints.
  * **​Active Directory & Identity Governance (cysec.local):** Structured Organizational Units (OUs), provisioned Role-Based Access Controls (RBAC), and managed Group Policy Objects (GPOs) to enforce baseline user and machine security policies.
  * **SIEM Telemetry & Threat Detection:** Configured Wazuh agents across domain-joined Windows endpoints and Linux nodes to ingest security event logs (Sysmon / Windows Event Logs), baseline operational activity, and trigger alert notifications on anomalous logins.
 

## 🛠️ Key Virtualization Skills Demonstrated
  * **Traffic Containment:** Enforcing strict Host-Only isolation on victim VMs to eliminate accidental leakages during testing.

  * **Virtual Subnet Routing:** Customizing DHCP scope parameters, default gateways, and static IP assignments inside VMware Virtual Network Editor.
  
  * **Snapshot Management:** Creating clean post-installation baseline snapshots for instant VM rollback following destructive testing or malware analysis.

## 📈 Planned Topology Upgrades
 * **PfSense / OPNSense Firewall:** Deploying a dedicated virtual firewall instance to enforce micro-segmentation between attack and target networks.
