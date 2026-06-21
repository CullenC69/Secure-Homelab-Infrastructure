# Secure Homelab Infrastructure

<br>

# Overview
This repository documents the setup and evolution of my home lab, built to mirror a small-scale enterprise environment. It starts with a basic Ubuntu server hosted on a repurposed old PC, and grows into a system that includes private storage, network-level ad blocking, firewalls, and centralized logging with a SIEM stack.
The goal is to move beyond theory and actually build, break, and secure systems in a hands-on way.

<br>

## Why I Built This
Public cloud services are convenient, but they come with trade-offs:

- Cost over time – Storage and logging costs can grow quickly and become unpredictable.
- Privacy concerns – Personal data and logs are stored with third parties under their policies.
- Limited visibility – Managed services hide the underlying infrastructure, making it harder to learn and experiment.

Instead, this lab is fully self-hosted. Everything runs on local hardware, so I have full control over data, configuration, and security.

<br>

## Architecture
The lab runs on a single Ubuntu server acting as a mini data center. Services are isolated using Docker and basic network segmentation.
High-level layout:


<img width="638" height="656" alt="Screenshot 2026-06-21 064953" src="https://github.com/user-attachments/assets/e8add94a-72b4-4a52-8118-3d21f3a60613" />

<br>

## Skills Demonstrated

1.   **Linux Administration** – System hardening, Networking, User Access Controls and File Permissions
2.   **Networking** – Layer 3/4 Firewall Policies, DNS control and Packet Routing Analysis
3.   **Docker** – Designing Isolated Virtual bridges, Volume Mapping, and Multi-Service Management via Docker Compose and Portainer
4.   **Security monitoring** – log collection, parsing, and visualization with ELK

<br>

## Project Phases

### Phase 1: Hardened Linux Base
- Provisioned a headless Ubuntu Server on dedicated hardware with static IP assignments
- Applied a stateful host firewall ( ```UFW``` ) implementing a strict default-deny on all unauthorized incoming traffic
- Hardened remote administratice access lines by restricting login verification solely to SSH keys

### Phase 2: Network Storage (Samba)

- Deployed a native, multi-tenant Samba Network-Attached Storage directly on the host to eliminate needing cloud drive subscription
- Enforced underlying Linux file system controls (```chmod```/```chown```) to gurantee security at the data-at-rest layer
- Installed Portainer to have a centralized web dashboard for managing container lifecycles and real-time environment logs


### Phase 3: DNS Filtering (Pi-hole)

- Deployed Pi-hole as a network-wide DNS sinkhole to intercept ads, malicious trackers and hardware telemetry
- Configured strict, secure upstream DNS forwarders and applied curated ad-blocking gravity databases


### Phase 4: Logging & SIEM (ELK Stack)

- Constructed an ELK Stack ( Elasticsearch, Logstash, Kibana ) engine to centralize system notifications
- Configured the host's ```rsyslog``` daemon to pipe the background kernal updates, authentication states and ```sudo``` actions directly into the containerized infrastructure
- Engineered a visual analytics dashboard inside Kibana to track login anomalies and process execution trails in real-time

<br>

## Key Issues & Fixes

### 1. The Dual-Stack IPv6 DNS Bypass ( Phase 3 )

**Problem**: Ads were still getting through to client machine even after configuring the Pi-hole IPv4 address.  

**Cause**:  Modern OSes inherently favor the IPv6 protocol stack over IPv4, hence the client system completely bypassed the local IPv4 Pi-hole resolver  

**Fix**: Disabled IPv6 on the client during testing to ensure all traffic went through Pi-hole.

<br>

### 2. Docker Network Isolation (Phase 4)
**Problem**: DNS requests weren’t reaching Pi-hole correctly from outside the container.  

**Cause**: Docker’s NAT makes traffic appear as if it’s coming from a different subnet. Pi-hole blocks these by default (“allow only local requests”).  

**Fix**: Modified the container interface listening behavior to "Permit All Origins." Assessing the risk, this was safe because: the server is behind a edge router firewall and UFW is still enforcing strict rules. Hence opening this interface does not expose the server to public internet exploitation vectors like DDoS attacks.

<br>

## Future Roadmap & Scaling Plans

With the foundational infrastructure stable and monitored, the future iterations will focus more on scaling automation, threat emulation and advanced access control hardening:

* **Phase 5: Centralized Identity & Access Management (IAM):** Integrate a centralized authentication provider like an open-source LDAP or Keycloak service to replace static Linux user accounts with localized *Single Sign-On* (SSO) and *Role-Based Access Controls* (RBAC) across my Samba NAS and Portainer environments.
* **Phase 6: Network Segmentation (VLANs):** Transition from a single host-managed subnet to a managed Layer 2 swith environment to physically isolate the Docker testing bridge network from the primary home user LAN traffic.
* **Phase 7: Active Threat Emulation:** Deploy a deliberate, isolated vulnerable container like a *honeypot* or *Metasploitable* instance inside the network to generate malicious traffic profiles to test the Kibana dashboard's capacity to alert on real-time port scans, brute-force attempts, and privilege escalations.

<br>

## Summary
This home lab serves as a self-directed, hands-on project designed to bridge the gap between theoretical cybersecurity and computer science concepts to real-world systems architecture. By building, deploying and securing this multi-tier infrastructure on physical hardware, I translated abstract ideas into a functional secure system which achieved several measurable outcomes:

### Applied Protocol Analysis & Optimazation:

Setting up the DNS-layer defense gave he hands-on experience with DNS routing and traffic analysis. By configuring it to block tracking and telemetry traffic at the source, I noticed improvements in network performance with faster page load times and reduced bandwidth usage.

### Validating Local Data Control:

Building the Samba NAS provided me with practical experience with data security, *role-based access control* (RBAC), and storage virtualization. This setup also replaced my reliance on cloud services like Google Drive demonstrating that full data control can be achieved on local infrastructure.

### Advanced Systems Visibility:

Creating the ELK logging pipeline gave me insight into system monitoring, event correlation, and data ingestion. It transformed large volumes of raw system logs into structured visualizations which simulates an enterprise-level Security Operations Center (SOC).







