# Advanced DMVPN & Network Automation (NetDevOps)

![Cisco](https://img.shields.io/badge/Platform-Cisco_IOS--XE-blue.svg)
![Ansible](https://img.shields.io/badge/Automation-Ansible-red.svg)
![Python](https://img.shields.io/badge/Code-Python_3-yellow.svg)
![Architecture](https://img.shields.io/badge/Architecture-Dual_Hub_DMVPN-orange.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

## 📋 Project Overview
This project demonstrates a production-grade implementation of a **Dual Hub DMVPN Phase 3** architecture, integrated with a modern **NetDevOps automation pipeline**.

The primary goal was to design a scalable, redundant WAN solution and a fully automated **Network Validation** process using Ansible. A key engineering challenge addressed in this project is the seamless integration of modern Python automation tools with legacy cryptographic standards (Legacy SSH) often found in brownfield enterprise environments.

## 🏗️ Network Architecture

### Topology & Design
* **Underlay Network:** Simulated WAN utilizing Cisco CSR1000v routers.
* **Overlay Network:** Dual Hub Single Cloud (mGRE) architecture for high availability.
* **Routing Protocol:** iBGP with Route Reflectors (Hubs) and Dynamic Neighbors (Listen Range) for scalability.
* **Security:** IKEv2 with IPsec Profiles (VTI) for encrypted transport.

### Key Technologies
* **DMVPN Phase 3:** Implements NHRP Redirect and Shortcut to optimize traffic flow (direct Spoke-to-Spoke communication).
* **Redundancy:** Automatic failover capabilities between Primary and Secondary Hubs.
* **BGP Optimization:** Dynamic peering configuration allows adding new Spokes without modifying Hub configuration.
* **NAT/PAT:** Internet edge simulation via ISP router.

## 🚀 Automation & DevOps

The control plane is managed via an **Ubuntu Control Node** running **Ansible**. The automation strategy focuses on "Intent-Based Networking" — validating that the operational state matches the design intent.

### Playbook Capabilities (`site.yaml`)
1.  **Smart Inventory Management:** Dynamic grouping of Hubs, Spokes, and ISP nodes.
2.  **Legacy Systems Integration:** Custom SSH and Paramiko configuration to support legacy Key Exchange (Kex) algorithms and Ciphers (e.g., `diffie-hellman-group1-sha1`, `3des-cbc`), bridging the gap between modern DevOps tools and older hardware.
3.  **Automated Network Validation:**
    * **Reachability:** Verifies ICMP/SSH connectivity.
    * **Overlay Health:** Audits DMVPN tunnel status and NHRP peerings.
    * **Routing Health:** Validates BGP session states (Established/Active).
4.  **Compliance Reporting:** Generates timestamped, structured text reports for each network node.

## 📂 Project Structure

```text
.
├── inventory/
│   └── hosts.yaml       # Infrastructure definition and connection variables
├── playbooks/
│   └── site.yaml        # Main validation and reporting playbook
├── reports/             # Generated compliance reports (git-ignored)
├── ansible.cfg          # Ansible tuning (SSH host key checking disabled for lab)
└── README.md            # Project documentation
```
🛠️ Getting Started
Prerequisites
Cisco CML, GNS3, or EVE-NG environment with CSR1000v/IOSv images.
Linux Control Node (Ubuntu/Debian recommended) with network access to the management plane.

# Installation
Clone the repository:
```
git clone [https://github.com/](https://github.com/)wmateusz1212-cell/cisco-dmvpn-automation.git
cd cisco-dmvpn-automation
```
# Install dependencies:
System dependencies for Python crypto libraries
`sudo apt update && sudo apt install python3-paramiko sshpass -y`

Python dependencies
`pip3 install ansible netmiko`

Run the Validation Playbook:
`ansible-playbook playbooks/site.yaml`

📝 Sample Output (Report)
`The playbook generates granular reports in the reports/ directory:`

```
=======================================================
REPORT FOR HOST: spoke1
DATE: 2023-10-27T10:30:00+00:00
=======================================================

[1] DEVICE INFO
Model: CSR1000V
Serial: 9XLM...
IOS Version: 16.12.0

[2] DMVPN STATUS
Interface: Tunnel100, IPv4 NHRP Details 
Type:Spoke, NHRP Peers:2
...

[3] BGP SESSION STATUS
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
172.16.0.1      4        65000      12      13        5    0    0 00:03:12        3
172.16.0.2      4        65000      11      13        5    0    0 00:03:10        3

Analysis:
[OK] BGP Sessions appear healthy
=======================================================
```
Author: Mateusz W
