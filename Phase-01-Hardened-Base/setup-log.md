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

# 2. SSH Key Hardening
