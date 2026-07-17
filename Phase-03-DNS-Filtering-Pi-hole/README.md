# Summary
In this phase it focuses on network-wide defense and local traffic orhestration by deploying a Pi-hole DNS sinkhole. By intercepting DNS requests at the network perimeter, this layer blocks telemetry, advertisements, and known malicious domains for all connected assets without requiring device-specific software. 
Additionally we also did local DNS mapping allowing the infrastructure to resolve friendly domain names like (portainer.local) directly to the host IP. Expanding on the virtualization foundation in Phase 2, the DNS architecture is fully deployed as an isolated Docker container, maintaining a strict separation between core network utilities and the host operating system.

## Core Tasks Achieved
### **Containerized DNS Sinkholing:** 
Deployed Pi-hole in Docker to provide fast, network wide DNS filtering.  

### **Port Conflict Resolution:** 
Disabled Ubuntu's default DNS service to free Port 53 for Pi-hole  

### **Local Domain Resolution:** 
Created custom local DNS records to eliminate the need for manial IP address tracking across the homelab network  

### **Stateful Firewall Integration:** 
Updated UFW rules to allow DNS traffic across the local network while keeping management interfaces protected.

<br>

## Technical Implementation Details

### 1. The Port 53 Challenge
While setting up Pi-hole, I discovered that Ubuntu's built-in systemd-resolved service was already occupying Port 53, creating a conflict with the Pi-hole container. To resolve this I disabled the system's DNS stub listener and redirected DNS requests to the Docker-hosted Pi-hole service which allowed it to manage DNS traffic across the network.  

### 2. Network-Wide Ad Blocking and Tracking Protection
One of the key benefits of deploying Pi-hole was gaining centralized control over outbound DNS traffic. By filtering requests for known advertising and tracking domains and redirecting them to 0.0.0.0, I was able to stop unnecessary connections before they left the network. This not only improved privacy and bandwidth efficiency but also provided valuable insight into the types of external services devices were attempting to access.

<br>

## Verification & Operational Validation

### Active Container Auditing
To verify the container status to comfirm the Pi-hole instance is active and correctly mapping external DNS requests (Port 53) and web administration controls (Port 8080) to the container environment we will use the `sudo docker ps` command:

<img width="2814" height="236" alt="Screenshot 2026-07-18 034412" src="https://github.com/user-attachments/assets/82df1c29-7b82-45f2-9ad1-2c2c05e38043" />  

### DNS Resolution Verification (Client Testing)
Running a targeted name resolution test from a client workstation validates that the server is successfully parsing queries and blocking unwanted tracking nodes:
<img width="841" height="156" alt="Screenshot 2026-07-18 035713" src="https://github.com/user-attachments/assets/d076584a-de9d-44e4-ac0b-ffc167ae2753" />





