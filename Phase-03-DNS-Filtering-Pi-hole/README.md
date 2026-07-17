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
