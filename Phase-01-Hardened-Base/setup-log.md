# 1. Stateful Host Firewall Setup (UFW)

These were the following commands I ran directly on the Ubuntu Server terminal to establish a default-deny posture and open required ports for other services:  

#### This command blocks all incoming traffic by default, and allow outgoing traffic  
```
sudo ufw default deny incoming
sudo ufw default allow outgoing  
```

#### Open port for SSH
```
sudo ufw allow 22/tcp  
```

#### Open ports for Samba NAS Storage (Phase 2)
```
sudo ufw allow 137,138/udp
sudo ufw allow 139,445/tcp  
```

#### Open ports for Pi-hole DNS (Phase 3)
```
sudo ufw allow 53/udp
sudo ufw allow 53/tcp  
```

#### Open ports for Centralized Rsyslog SIEM Ingestion (Phase 4)
```
sudo ufw allow 514/udp
sudo ufw allow 514/tcp  
```

#### Open port for Kibana Web Analytics Interface (Phase 4)
```
sudo ufw allow 5601/tcp
```

#### Enable the firewall configuration to run on system boot
```
sudo ufw enable  
```
---

<br>

# 2. Secure Remote Configuration (SSH Key Hardening)
To replace the vulnerable password logins with cryptographic authentication, the public key was generated in the Windows workstation and was transfered and authorized in this following process:  

<br>

## Generating the SSH Key Pair (Windows Workstation)
From a Windows Powershell terminal, I first start with generating a modern and highly secure Ed25519 key pair on my Windows workstation:
```
ssh-keygen -t ed25519 -C "homelab-workstation"  
```

<br>

## View and Copy the Public Key (Windows Workstation)
As the native Linux utility command ```ssh-copy-id``` is unavailable on Windows key, we will have to manually view and copy the public key onto the Ubuntu Server:
```
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub"
```

<br>

## Create the Remote SSH Directory (Ubuntu Server)
We then need to login into the server and initialize the hidden SSH configuration directory in the home directory:
```
mkdir -p ~/.ssh
```

<br>

## Authorize the Workstation Key (Ubuntu Server)
We then append the coppied public key from the workstation into the server's authorized deployment file using a text editor, I used ```nano``` to achieve this:
```
nano ~/.ssh/authorized_keys
```

<br>

## Enforce Strict Linux File Permissions (Ubuntu Server)
Due to strict access controls required by Linux, we need to change the permissions of the directory and keys file:
```
#Only the owner can read/write/execute
chmod 700 ~/.ssh

#Only the owner can read/write
chmod 600 ~/.ssh/authorized_keys
```

<br>

# 3. Disable Password Logins Globally (Ubuntu Server)






