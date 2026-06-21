# Summary

This phase details the deployment, static provisioning and defensive configuration of a headless Ubuntu Server instance on bare-metal legacy hardware. As the foundation of any secure enterprise infrastructure relies on the absolute integrity of its base operating system, this phase is extremely important.  
By stripping away unnecessary graphical subsystems and desktop software, the platform's initial *attack surface* was minimized. This set up a secure, default-deny baseline to protect the system before launching storage and containerized services.

<br>

## Core Tasks Achieved  

### Minimal Operating System Provisioning:
Deployed a minimal, headless Linux environment to restrict backgroud resource overhead and eliminate target software vulnerabilities  

<br>

### Pre-Boot Static Networking Configuration:
Provisioned a predictable static IPv4 network interface during the OS installation phase and ensuring the IP address is outside of router's DHCP pool to ensure persistent LAN routing 

<br>

### Stateful Host Firewall Implementation:
Built a stateful packet-filtering architecture using ```UFW``` to prevent lateral network reconnaissance  

<br>

### Cryptographic Access Control Hardening:
Removed legacy, brute-forceable password authentication and enforcing exclusive asymmetric public-key authentication for secure access.

<br>

## Technical Implementation Details

### 1. Pre-Boot Static Networking Provisioning
Rather than relying on post-installation network reconfiguration utilities, the server's network profile was configured during the initial operating deployment phase. Isolating the host's static IP assignment away from the router's dynamic DHCP pool to prevent the risk of IP duplication conflicts and guarantees that downstream storage mappings (Samba) and SIEM log streams always target an unchanging destination. 
* **Target Subnet Layout:** 192.168.0.0
* **Assigned Host IP Location:** 192.168.0.x (Outside dynamic DHCP scope)
* **Primary Gateway Interation:** 192.168.0.1

<br>

### 2. Network Layer Hardening (Stateful Firewall)

A strict default-deny firewall policy was implemented using UFW, blocking all incoming traffic which protecting the server from automated malicious port scanning or lateral indexing.

#### Explicit Exceptions Allowed:
| Service | Port / Protocol | Traffic Type | Purpose |
| :--- | :--- | :--- | :--- |
| **SSH** | `22/TCP` | Inbound | Secure Remote Systems Administration |
| **Samba NAS** | `137/UDP`, `138/UDP` | Inbound | NetBIOS Name Service & Datagram Distribution |
| **Samba NAS** | `139/TCP`, `445/TCP` | Inbound | Session Service & Direct SMB File Sharing |
| **Pi-hole** | `53/UDP`, `53/TCP` | Inbound | Network-Wide DNS Resolution Queries |
| **Rsyslog** | `514/UDP`, `514/TCP` | Inbound | Centralized Inbound Logging Pipeline for SIEM |
| **Kibana** | `5601/TCP` | Inbound | SIEM Security Data Analytics Visual Web UI |

<br>

### 3. Cryptographic Identity & Access Management (SSH)
The secure remote configuration parameters inside ```/etc/ssh/sshd_config``` were audited to defend against automated dictionary and credential stuffing attacks targeting standard open administrative terminals.
#### Disabled Root Interactivity:
* Stopped administrative root users from establishing direct external connections (```PermitRootLogin no```)  

#### Enforced Public-Key Architecture:
* Swapped the standard, guessable alphanumeric string passwords for 4096-bit asymmetric key pairs (```PasswordAuthentication no```)

<br>

## Verification & Operational Validation
To comfirm the defensive integrity of the deployment baseline prior to launching subsequent architecture phases, the following validations were executed:  

### Firewall Policy Auditing
Running ```sudo ufw status verbose``` outputs an explicit validation statement verifying the firewall is active, operating on system boot and dropping unexpected incoming packets while opening only designated ports.  

<img width="539" height="471" alt="Screenshot 2026-06-22 064705" src="https://github.com/user-attachments/assets/5fe35485-3208-41a1-8dcc-ed8618b759e1" />

<br>

### Remote Authentication Auditing
Attempting to connect to the host without an explicitly mapped and verified cryptographic token drops the session at the handshake stage without processing standard passwords, neutralizing credential stuffing vectors.  

<img width="687" height="81" alt="Screenshot 2026-06-22 070710" src="https://github.com/user-attachments/assets/217c667f-2fbf-4f80-9dfa-587b5cea7db2" />










