# Stateful Host Firewall Setup (UFW)

These were the following commands I ran directly on the Ubuntu Server terminal to establish a default-deny posture and open required ports for other services:  

#### This command blocks all incoming traffic by default, and allow outgoing traffic  
```
sudo ufw default deny incoming
sudo ufw default allow outgoing  
```
#### Open Port for SSH
`sudo ufw allow 22/tcp`  
