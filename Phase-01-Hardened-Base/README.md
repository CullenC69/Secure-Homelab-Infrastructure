# Summary

This phase details the deployment, static provisioning and defensive configuration of a headless Ubuntu Server instance on bare-metal legacy hardware. As the foundation of any secure enterprise infrastructure relies on the absolute integrity of its base operating system, this phase is extremely important.  
By stripping away unnecessary graphical subsystems and desktop software, the platform's initial *attack surface* was minimized. This set up a secure, default-deny baseline to protect the system before launching storage and containerized services.

<br>

## Core Tasks Achieved  

### Minimal Operating System Provisioning:
Deployed a minimal, headless Linux environment to restrict backgroud resource overhead and eliminate target software vulnerabilities  

### Pre-Boot Static Networking Configuration:
Provisioned a predictable static IPv4 network interface during the OS installation phase and ensuring the IP address is outside of router's DHCP pool to ensure persistent LAN routing 

### Stateful Host Firewall Implementation:
Built a stateful packet-filtering architecture using ```UFW``` to prevent lateral network reconnaissance  

### Cryptographic Access Control Hardening:
Removed legacy, brute-forceable password authentication and enforcing exclusive asymmetric public-key authentication for secure access.

<br>

## Technical Implementation Details





