# Summary
In Phase 4 we created created a centralized monitoring and logging solution using the ELK stack (Elasticsearch, Logstash, Kibana). With this setup, instead of manually reviewing log files on individual systems, this setup automatically collects, processes, and visualizes security and system events in real time.  
We accomplished this by configuring the host's logging service (rsyslog) to forward authentication and system logsto a dedicated Logstash pipeline. Logstash will then process the data, Elasticsearch stores and indexes it and Kibana provides dashboards for visualisation and analysis. Creating a centralized platform for monitoring login activity, access attempts, and important system changes.


