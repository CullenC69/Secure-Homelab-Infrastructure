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
