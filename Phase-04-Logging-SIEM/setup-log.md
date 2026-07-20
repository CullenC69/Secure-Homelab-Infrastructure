# 1. Configuring System Memory Settings

Elasticsearch uses memory mapping (`mmapfs`) to manage its data. The default Linux limit on memory maps is too low and will cause Elasticsearch to crash during the startup, so we will need to increase this threshold

