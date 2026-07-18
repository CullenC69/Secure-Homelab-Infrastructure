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
