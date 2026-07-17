# Phase 2: Network Storage (Samba) & Automated Container Management (Portainer)

## Summary  

With the physical host secured and firewalled, we can now establish the core utility layers of the homelab. We will be focusing on deploying two primary infrastructure capabilities, the setting up of a secure multi user Samba NAS and a Docker/Portainer environment to run and monitor future servises.  

A key design choice was seperating the roles, I ran Samba directly on the host for maximum storage performance, while hosting admin tools inside lightweight containers. This approach removes the need for cloud storage services while keeping full control of local data.  

<br>

## Core Tasks Achieved  

### Native Samba NAS Provisioning:
Configured a native Linux SMB server directly on the host, enabling cross-platform file sharing across the local network  

### Multi-User Access Controls:
Enforced Linux permissions (`chmod`, `chown`) amd Samba user accounts to keep data isloated between public and private shares  

### Docker Engine Deployment:
Installed Docker to run lightweight, container-based services instead of resource-heavy virtual machines  

### Portainer Management:
Added a web-based dashboard to easily manage, monitor and track containers

<br>

## Technical Implementations
### 1. Hybrid Storage & Virtualization Architecture
Rather than virtualizing the file storage system inside a heavy nested wrapper, Samba was installed directly onto the Ubuntu base which gives the storage direct and high performance access to the server's physical drives  

Conversely, my administrative tools like Portainer was intentionally encapsulated inside the isolated Docker environments. This ensures that if a containerized web tool is ever compromised, the attackers remained trapped within a restricted container and completely isolated from the host server's root directory.

<br>

### 2. File System Permission Matrix (Data-at-Rest Security)
To securely support multiple users on the local network, access controls were at enforced at both the Linux OS level and the Samba server level:
* **Public Share:** Anonymous users have read-only access, while authenticated users are required to modify or delete files.  
* **Private Shares:** Access is restricted to specific Linux user groups. Users outside these groups cannot access or browse these directories, preventing unauthorized data exposure.

<br>

## Verification & Operational Validation
The storage system and container environment were then validated to ensure they were operating correctly.  

### Container Environment Health
A system process audit was performed to verify that the Docker engine was running properly and that the Portainer management interface was successfully connected to its isolated web dashboard.  
<img width="2075" height="175" alt="Screenshot 2026-07-11 211417" src="https://github.com/user-attachments/assets/0b5ff111-eef3-4044-897f-0f789b7c3dec" />  

<br>

### Samba Shared Access Auditing
To validate access controls from the Windows testing workstation, `net` command was used to test the connection boundaries against the authenticated `NAS` storage pool  
#### Unauthenticated Access Validation (Client-Side Policy Enforcement):  
<img width="1452" height="220" alt="Screenshot 2026-07-17 230457" src="https://github.com/user-attachments/assets/66700446-2efa-4f00-a62c-1296d6afb253" />





