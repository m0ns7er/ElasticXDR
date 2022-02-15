---
description: Configure Elasticsearch
---

# Elasticsearch

Install Elasticsearch.

```
sudo apt-get install elasticsearch -y
```

Edit elasticsearch.yml file.

```
sudo nano /etc/elasticsearch/elasticsearch.yml
```

You should see a configuration file with several different entries and descriptions.

```
cluster.name: my-application
network.host: localhost
http.port: 9200
```

Uncomment, edit and change values to match your information and cluster name then save it.

Example Below:

```
cluster.name: ElasticXDR
network.host: 192.168.0.25
http.port: 9200
```

Search for Discovery Type settings section add this line between lines.

```
discovery.type: single-node
```

Set your Java JVM Heap Options to balance Elasticsearch Resopurces.

{% embed url="https://www.elastic.co/guide/en/elasticsearch/reference/current/advanced-configuration.html#set-jvm-heap-size" %}
Set JVM Heap Size Information
{% endembed %}

{% hint style="info" %}
Here are examples of how to set the heap size via the jvm.options file:



\-Xms6g - Min

\-Xmx6g - Max



Set the minimum heap size to 4g.&#x20;

Set the maximum heap size to 4g.
{% endhint %}

Un Comment file and make changes to it then save.

```
sudo su
```

```
cd /etc/elasticsearch/
```

```
nano jvm.options
```

Start the Elasticsearch service by running a systemctl command.

```
sudo systemctl start elasticsearch.service
```

Now verify that the service is running.

```
sudo service elasticsearch status
```

Verify Elasticsearch is running throught the web browser.

```
http://XDR_ip_address_here:9200
```

Now you should see something like this.

```
{
  "name" : "xdr",
  "cluster_name" : "ElasticXDR",
  "cluster_uuid" : "I2EDGxkoRD27wvVsGSFEyA",
  "version" : {
    "number" : "7.14.1",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "66b55ebfa59c92c15db3f69a335d500018b3331e",
    "build_date" : "2021-08-26T09:01:05.390870785Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

{% hint style="success" %}
If you are seeing this message, then its working correctly!
{% endhint %}

Now enable Elasticsearch on Boot.

```
sudo systemctl enable elasticsearch.service
```
