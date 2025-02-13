---
title: Ship Apache Cassandra metrics via Telegraf
logo:
  logofile: cassandra-logo.png
  orientation: vertical
data-source: Apache Cassandra 
data-for-product-source: Metrics
templates: ["docker"]
contributors:
  - daniel-tk
  - nshishkin
shipping-tags:  
  - prometheus
  - prebuilt-dashboards
order: 800
---

## Overview

[Apache Cassandra](https://cassandra.apache.org/) is an open source NoSQL distributed database management system designed to process large amounts of data across commodity servers.

Telegraf is a plug-in driven server agent for collecting and sending metrics and events from databases, systems and IoT sensors.

To send your JMX-format Apache Cassandra metrics to Logz.io, you need to add the **inputs.jolokia2_agent** and **outputs.http** plug-ins to your Telegraf configuration file.

#### Configuring Telegraf to send your metrics data to Logz.io

<div class="tasklist">
  
##### Install Jolokia agent on your Cassandra server
  
<!-- info-box-start:info -->
You need to install and enable Jolokia on every Cassandra server.
{:.info-box.note}
<!-- info-box-end -->
    
Download Jolokia agent to `/usr/share/java`:
  
```shell
RUN curl -L http://search.maven.org/remotecontent?filepath=org/jolokia/jolokia-jvm/1.6.0/jolokia-jvm-1.6.0-agent.jar
```

##### Enable Jolokia agent on your Cassandra server
    
```shell
JVM_OPTS="$JVM_OPTS -javaagent:/usr/share/java/jolokia-jvm-1.6.0-agent.jar=port=8778,host=localhost"
```
  
##### Restart Cassandra
  
```shell
sudo service cassandra restart
```

##### Verify Jolokia is accessible

```shell
curl http://localhost:8778/jolokia/
```

##### Set up Telegraf v1.17 or higher on your Cassandra server

<!-- info-box-start:info -->
You need to install Telegraf on every Cassandra server.
{:.info-box.note}
<!-- info-box-end -->

  
{% include metric-shipping/telegraf-setup.md %}

##### Add the inputs.jolokia2_agent plug-in

First you need to configure the input plug-in to enable Telegraf to scrape the Apache Cassandra data from your hosts. To do this, add the following code to the configuration file:


``` ini
[[inputs.jolokia2_agent]]
  urls = ["http://localhost:8778/jolokia"]
  name_prefix = "java_"
  [[inputs.jolokia2_agent.metric]]
    name  = "Memory"
    mbean = "java.lang:type=Memory"
  [[inputs.jolokia2_agent.metric]]
    name  = "GarbageCollector"
    mbean = "java.lang:name=*,type=GarbageCollector"
    tag_keys = ["name"]
    field_prefix = "$1_"
[[inputs.jolokia2_agent]]
  urls = ["http://localhost:8778/jolokia"]
  name_prefix = "cassandra_"
  [[inputs.jolokia2_agent.metric]]
    name  = "Cache"
    mbean = "org.apache.cassandra.metrics:name=*,scope=*,type=Cache"
    tag_keys = ["name", "scope"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "Client"
    mbean = "org.apache.cassandra.metrics:name=*,type=Client"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "ClientRequestMetrics"
    mbean = "org.apache.cassandra.metrics:name=*,type=ClientRequestMetrics"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "ClientRequest"
    mbean = "org.apache.cassandra.metrics:name=*,scope=*,type=ClientRequest"
    tag_keys = ["name", "scope"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "ColumnFamily"
    mbean = "org.apache.cassandra.metrics:keyspace=*,name=*,scope=*,type=ColumnFamily"
    tag_keys = ["keyspace", "name", "scope"]
    field_prefix = "$2_"
  [[inputs.jolokia2_agent.metric]]
    name  = "CommitLog"
    mbean = "org.apache.cassandra.metrics:name=*,type=CommitLog"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "Compaction"
    mbean = "org.apache.cassandra.metrics:name=*,type=Compaction"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "CQL"
    mbean = "org.apache.cassandra.metrics:name=*,type=CQL"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "DroppedMessage"
    mbean = "org.apache.cassandra.metrics:name=*,scope=*,type=DroppedMessage"
    tag_keys = ["name", "scope"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "FileCache"
    mbean = "org.apache.cassandra.metrics:name=*,type=FileCache"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "ReadRepair"
    mbean = "org.apache.cassandra.metrics:name=*,type=ReadRepair"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "Storage"
    mbean = "org.apache.cassandra.metrics:name=*,type=Storage"
    tag_keys = ["name"]
    field_prefix = "$1_"
  [[inputs.jolokia2_agent.metric]]
    name  = "ThreadPools"
    mbean = "org.apache.cassandra.metrics:name=*,path=*,scope=*,type=ThreadPools"
    tag_keys = ["name", "path", "scope"]
    field_prefix = "$1_"
```

<!-- info-box-start:info -->
The full list of data scraping and configuring options can be found [here](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/jolokia2).
{:.info-box.note}
<!-- info-box-end -->

##### Add the outputs.http plug-in

{% include metric-shipping/telegraf-outputs.md %}
{% include general-shipping/replace-placeholders-prometheus.html %}

##### Start Telegraf

{% include metric-shipping/telegraf-run.md %}

##### Check Logz.io for your metrics

{% include metric-shipping/custom-dashboard.html %} Install the pre-built dashboard to enhance the observability of your metrics.

<!-- logzio-inject:install:grafana:dashboards ids=["5oCUt52hGJu6LmVGHPOktr"] --> 

{% include metric-shipping/generic-dashboard.html %} 

</div>
