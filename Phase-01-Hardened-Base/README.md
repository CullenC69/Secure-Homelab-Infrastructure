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
Rather than relying on post-installation network reconfiguration utilities, the server's network profile was configured during the initial operating deployment phase. Binding the physical interface to a fixed local configuration layer ensures that downstream storage mappings (Samba) and SIEM log streams always target a predictable, unchanging destination. I also ensure that the IP assignment was outside my router's DHCP IP pool to prevent any IP address conflicts.
* **Target Subnet Layout:** 192.168.0.0/24
* **Assigned Host IP Location:** 192.168.0.x
* **Primary Gateway Interation:** 192.168.0.1

<br>

### 2. Network Layer Hardening (Stateful Firewall)

A strict default-deny firewall policy was implemented using UFW, blocking all incoming traffic which protecting the server from automated malicious port scanning or lateral indexing.

### Explicit Exceptions Allowed:
Port ```22/tcp``` (SSH)  
Port ```137,138/udp``` (Samba)  
Port ```139,445/tcp``` (Samba)  
Port ```53/udp``` (Pi-hole)  
Port ```53/tcp``` (Pi-hole)  
Port ```514/udp``` (Rsyslog Pipeline for SIEM ingestion)  
Port ```514/tcp``` (Rsyslog Pipeline for SIEM ingestion)  
Port ```5601/tcp``` (Kibana)

### 3. Cryptographic Identity & Access Management (SSH)
The secure remote configuration parameters inside ```/etc/ssh/sshd_config``` were audited to defend against automated dictionary and credential stuffing attacks targeting standard open administrative terminals.
#### Disabled Root Interactivity:
* Stopped administrative root users from establishing direct external connections (```PermitRootLogin no```)  

#### Enforced Public-Key Architecture:
* Swapped the standard, guessable alphanumeric string passwords for 4096-bit asymmetric key pairs (```PasswordAuthentication no```)



