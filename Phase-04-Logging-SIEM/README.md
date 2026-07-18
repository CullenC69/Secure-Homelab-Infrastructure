# Summary
In Phase 4 we created created a centralized monitoring and logging solution using the ELK stack (Elasticsearch, Logstash, Kibana). With this setup, instead of manually reviewing log files on individual systems, this setup automatically collects, processes, and visualizes security and system events in real time.  

We accomplished this by configuring the host's logging service (rsyslog) to forward authentication and system logsto a dedicated Logstash pipeline. Logstash will then process the data, Elasticsearch stores and indexes it and Kibana provides dashboards for visualisation and analysis. Creating a centralized platform for monitoring login activity, access attempts, and important system changes.

<br>

## Core Tasks Achieved

#### Optimized ELK Deployment
Deployed the ELK stack in Docker with resource limits suitable for an 8GB RAM environment

#### Centralized Log Collection
I configured `rsyslog` to forward authentication logs from the host system to Logstash

#### Log Parsing & Indexing
I created a custom Logstash pipeline to parse syslog data and store it in Elasticsearch as structured records

#### Network & Firewall Security
Restricted access to management interfaces while keeping log collection and ingestion services operational

<br>

## Technical Implementation Details

### 1. Managing Resource Usage
The ELK stack is notorious for its heavy memory requirements, particularly because Elasticsearch and Logstash rely on the Java Virtual Machine (JVM). Having previously experienced VM instability and crashes caused by ELK's resource consumption, I prioritized memory management during deployment. JVM heap allocations (ES_JAVA_OPTS) were limited to 1GB, preventing excessive RAM usage and ensuring the stack could operate reliably without impacting other services running on the host, such as the NAS and Pi-hole DNS server.

### 2. Connecting the Host to the Container Network
Since the ELK services run inside a Docker bridge network, the host system cannot send logs to them directly. To enable communication, Logstash was configured to listen on a custom non-privileged port (5140). The host's rsyslog service was then configured to forward log data to this port, allowing events to flow reliably into the ELK stack.

