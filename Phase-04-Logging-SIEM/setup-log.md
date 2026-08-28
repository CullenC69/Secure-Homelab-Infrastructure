# 1. Configuring System Memory Settings

Elasticsearch uses memory mapping (`mmapfs`) to manage its data. The default Linux limit on memory maps is too low and will cause Elasticsearch to crash during the startup, so we will need to increase this threshold

### Temporarily Increase the Limit

Running this command increases the maximum number of memory mappings that any single process can create from the default value to 262,144
```
sudo sysctl -w vm.max_map_count=262144
```

We will then also should lock the setting permanently by editing the `sysctl.conf` and adding this line to the bottom of the file:
```
vm.max_map_count=262144
```

<br>

# 2. Create the Logstash Pipeline Configuration

We will need to create a config file to tell Logstash where to receive logs, how to process them, and where to send them.

### Create the Pipeline Configuration File

We will first create the directory where the Logstash pipeline configuration
```
sudo mkdir -p /etc/logstash/pipeline
```

Now we can create and open the config file:
```
sudo nano /etc/logstash/pipeline/logstash.conf
```

Add the following configuration:

```
input {
  udp {
    port => 5140
    type => "syslog"
  }
  tcp {
    port => 5140
    type => "syslog"
  }
}

filter {
  if [type] == "syslog" {
    grok {
      match => { "message" => "%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:syslog_hostname} %{DATA:syslog_program}(?:\[%{POSINT:syslog_pid}\])?: %{GREEDYDATA:syslog_message}" }
    }
    date {
      match => [ "syslog_timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
  }
}

output {
  elasticsearch {
    hosts => ["http://elasticsearch:9200"]
    index => "syslog-logs-%{+YYYY.MM}"
  }
}
```

<br>

# 3. Run the ELK Containers

The ELK stack is run using Docker containers. This keeps Elasticsearch, Logstash, and Kibana separate and makes the setup easier to manage.

### Create a Docker Network

Create a network so the three containers can communicate with each other.

```
sudo docker network create elk-network
```

This allows the containers to connect using their container names, such as `elasticsearch`, instead of needing to use IP addresses.

### Start Elasticsearch

Run Elasticsearch as a Docker container.

```
sudo docker run -d \
  --name elasticsearch \
  --net elk-network \
  -p 9200:9200 \
  -e "discovery.type=single-node" \
  -e "ES_JAVA_OPTS=-Xms1g -Xmx1g" \
  --restart=always \
  elasticsearch:8.12.2
```

Some important options:

* `-d` runs the container in the background.
* `--name elasticsearch` gives the container an easy name to identify.
* `--net elk-network` connects it to the ELK network.
* `-p 9200:9200` allows Elasticsearch to receive connections on port `9200`.
* `discovery.type=single-node` is used because this setup only has one Elasticsearch node.
* `ES_JAVA_OPTS=-Xms1g -Xmx1g` limits Elasticsearch to **1 GB of memory**, which helps reduce resource usage.
* `--restart=always` makes Docker restart the container if it stops.

### Start Logstash

Run Logstash and connect it to the same Docker network.

```
sudo docker run -d \
  --name logstash \
  --net elk-network \
  -p 5140:5140/udp \
  -p 5140:5140/tcp \
  -v /etc/logstash/pipeline:/usr/share/logstash/pipeline \
  -e "LS_JAVA_OPTS=-Xms1g -Xmx1g" \
  --restart=always \
  logstash:8.12.2
```

The pipeline folder is mounted into the container using:

```
-v /etc/logstash/pipeline:/usr/share/logstash/pipeline
```

This lets Logstash use the configuration file created in the previous step.

Both TCP and UDP port `5140` are exposed so Logstash can receive Syslog messages using either protocol.

The memory limit is also set to **1 GB** to keep resource usage under control.

### Start Kibana

Run Kibana and connect it to Elasticsearch.

```
sudo docker run -d \
  --name kibana \
  --net elk-network \
  -p 5601:5601 \
  -e "ELASTICSEARCH_HOSTS=http://elasticsearch:9200" \
  --restart=always \
  kibana:8.12.2
```

The `ELASTICSEARCH_HOSTS` setting tells Kibana where to find Elasticsearch.

Port `5601` is used to access the Kibana web interface.

<br>

# 4. Configure rsyslog to Send Logs to Logstash

The server uses `rsyslog` to collect system logs. In this step, we configure it to send authentication-related logs to Logstash.

### Create an rsyslog Rule

Create a new configuration file for the Logstash forwarding rule.

```
sudo nano /etc/rsyslog.d/60-elk-forward.conf
```

The `/etc/rsyslog.d/` folder is used for additional rsyslog configuration files. Keeping this rule in a separate file makes it easier to manage.

### Forward Authentication Logs to Logstash

Add the following line:

```
auth,authpriv.* @@127.0.0.1:5140
```

This tells rsyslog to:

* `auth,authpriv.*` — collect authentication and privilege-related logs.
* `@@` — send the logs using **TCP**.
* `127.0.0.1` — send them to the local server.
* `5140` — send them to the Logstash listener on port `5140`.

This allows authentication events, such as login attempts, to be sent to Logstash for processing and storage in Elasticsearch.

Save the file and exit the editor.

### Restart rsyslog

Restart rsyslog so that it loads the new configuration.

```
sudo systemctl restart rsyslog
```


