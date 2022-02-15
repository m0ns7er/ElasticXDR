---
description: Configure Filebeat
---

# Filebeat

Install Filebeat by running the following command.

```
sudo apt-get install filebeat -y
```

Configure Filebeat.

```
sudo nano /etc/filebeat/filebeat.yml
```

Under the Elasticsearch output section, search for the commented outlines.

```
output.elasticsearch:
hosts: ["localhost:9200"]
```

Replace the output above with your information like I did below, then save the file.

```
output.elasticsearch
hosts: ["192.168.0.25:9200"]
```

Next, load the index template.

{% hint style="warning" %}
Remember to replace your IP in this config.
{% endhint %}

```
sudo filebeat setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["192.168.0.25:9200"]'
```

The system will do some work on setting it up.

Start and enable the Filebeat service.

```
sudo systemctl start filebeat
```

```
sudo systemctl enable filebeat
```

Verify Elasticsearch Reception of Data.

```
curl -XGET http://192.168.0.25:9200/_cat/indices?v
```

{% hint style="success" %}
Now you should see some Elastic Indices Displayed.
{% endhint %}
