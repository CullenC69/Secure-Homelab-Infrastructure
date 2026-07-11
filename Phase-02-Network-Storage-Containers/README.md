# Phase 2: Network Storage (Samba) & Automated Container Management (Portainer)

## Summary  

With the physical host secured and firewalled, we can now establish the core utility layers of the homelab. We will be focusing on deploying two primary infrastructure capabilities, the setting up of a secure multi user Samba NAS and a Docker/Portainer environment to run and monitor future servises.  

A key design choice was seperating the roles, I ran Samba directly on the host for maximum storage performance, while hosting admin tools inside lightweight containers. This approach removes the need for cloud storage services while keeping full control of local data.  

<br>

## Core Tasks Achieved  

### Native Samba NAS Provisioning:
Configured a native Linux SMB server directly on the host, enabling cross-platform file sharing across the local network

<br>

### Multi-User Access Controls:
Enforced Linux permissions (`chmod`, `chown`) amd Samba user accounts to keep data isloated between public and private shares

<br>

### Docker Engine Deployment:
Installed Docker to run lightweight, container-based services instead of resource-heavy virtual machines

<br>

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
* **Public Share:** Anonymus users have read-only access, while authenticated users are required to modify or delete files.
* **Private Shares:** Access is restricted to specific Linux user groups. Users outside these groups cannot access or browse these directories, preventing unauthorized data exposure.
