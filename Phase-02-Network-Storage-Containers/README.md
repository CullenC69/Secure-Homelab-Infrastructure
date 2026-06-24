# Phase 2: Samba (Network Storage) & Automated Container Management (Portainer)

## Summary  

With the physical host secured and firewalled, we can now establish the core utility layers of the homelab. We will be focusing on deploying two primary infrastructure capabilities, the setting up of a secure multi user Samba NAS and a Docker/Portainer environment to run and monitor future servises.  

A key design choice was seperating the roles, I ran Samba directly on the host for maximum storage performance, while hosting admin tools inside lightweight containers. This approach removes the need for cloud storage services while keeping full control of local data.
