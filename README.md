# Secure Homelab Infrastructure


## Overview
This repository documents the setup and evolution of my home lab, built to mirror a small-scale enterprise environment. It starts with a basic Ubuntu server hosted on a repurposed old PC, and grows into a system that includes private storage, network-level ad blocking, firewalls, and centralized logging with a SIEM stack.
The goal is to move beyond theory and actually build, break, and secure systems in a hands-on way.


## Why I Built This
Public cloud services are convenient, but they come with trade-offs:

- Cost over time – Storage and logging costs can grow quickly and become unpredictable.
- Privacy concerns – Personal data and logs are stored with third parties under their policies.
- Limited visibility – Managed services hide the underlying infrastructure, making it harder to learn and experiment.

Instead, this lab is fully self-hosted. Everything runs on local hardware, so I have full control over data, configuration, and security.


## Architecture
The lab runs on a single Ubuntu server acting as a mini data center. Services are isolated using Docker and basic network segmentation.
High-level layout:


<img width="851" height="875" alt="Screenshot 2026-06-21 064953" src="https://github.com/user-attachments/assets/e8add94a-72b4-4a52-8118-3d21f3a60613" />


## Skills Demonstrated

1.   **Linux Administration** – System hardening, Networking, User Access Controls and File Permissions
2.   **Networking** – Layer 3/4 Firewall Policies, DNS control and Packet Routing Analysis
3.   **Docker** – Designing Isolated Virtual bridges, Volume Mapping, and Multi-Service Management via Docker Compose and Portainer
4.   **Security monitoring** – log collection, parsing, and visualization with ELK


## Project Phases

### Phase 1: Hardened Linux Base
- Provisioned a headless Ubuntu Server on dedicated hardware with static IP assignments
- Applied a stateful host firewall ( UFW ) implementing a strict default-deny on all unauthorized incoming traffic
- Hardened remote administratice access lines by restricting login verification solely to SSH keys


### Phase 2: Network Storage (Samba)

Deploy a Samba container for local file storage
Replace reliance on cloud drives
Separate shared folders and private user directories
Enforce permissions using Linux file controls

### Phase 3: DNS Filtering (Pi-hole)

Deploy Pi-hole as a network-wide DNS sinkhole
Block ads, trackers, and telemetry at the DNS level
Configure upstream DNS securely
Apply curated blocklists

### Phase 4: Logging & SIEM (ELK Stack)

Build an ELK stack for centralized logging
Forward logs from the host via rsyslog
Parse logs using Logstash
Store and query them in Elasticsearch
Visualize activity in Kibana dashboards


## Key Issues & Fixes
IPv6 DNS Bypass (Phase 3)
Problem: Ads were still getting through even after configuring Pi-hole.
Cause: The system was using IPv6 DNS instead of IPv4. Many OSes prefer IPv6 when available, which bypassed Pi-hole.
Fix: Disabled IPv6 on the client during testing to ensure all traffic went through Pi-hole.

### Docker Network Isolation (Phase 4)
Problem: DNS requests weren’t reaching Pi-hole correctly from outside the container.
Cause: Docker’s NAT makes traffic appear as if it’s coming from a different subnet. Pi-hole blocks these by default (“allow only local requests”).
Fix: Changed Pi-hole to accept all origins. This was safe because:

The server is behind a router firewall
UFW is still enforcing strict rules


## Summary
This lab is a practical way to learn how systems actually behave under real conditions—especially when things go wrong. It replaces managed services with self-hosted ones to give full visibility and control over the entire stack.
