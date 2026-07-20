# 1. Configuring System Memory Settings

Elasticsearch uses memory mapping (`mmapfs`) to manage its data. The default Linux limit on memory maps is too low and will cause Elasticsearch to crash during the startup, so we will need to increase this threshold

### Temporarily Increase the Limit

Running this command increases the maximum number of memory mappings that any single process can create from the default value to 262,144
```
sudo sysctl -w vm.max_map_count=262144
```
