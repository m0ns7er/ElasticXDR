---
description: Configure Kibana
---

# Kibana

Install KIbana.

```
sudo apt-get install kibana -y
```

Edit Kibana.yml file.

```
sudo nano /etc/kibana/kibana.yml
```

Un-comment these lines and make changes for your Information.

```
server.port: 5601
server.host: "192.168.0.25"
elasticsearch.hosts: ["http://192.168.0.25:9200"]
```

Now start Kibana service.

```
sudo systemctl start kibana
```

Now enable Kibana on Boot.

```
sudo systemctl enable kibana
```

Access Kibana from web browser, also ignore warning about "public URL'.

```
http://192.168.0.25:5601
```

{% hint style="warning" %}
Important: Ignore Warning Below!!
{% endhint %}

![Warning Sign](<../../.gitbook/assets/image (14).png>)

{% hint style="success" %}
You should now be able to explore Kibana.
{% endhint %}

{% hint style="danger" %}
If you receive a **"Kibana server not ready yet"** error, check if the Elasticsearch and Kibana services are active.
{% endhint %}

```
sudo systemctl status kibana
```

```
sudo systemctl status elasticsearch
```

{% hint style="danger" %}
If the services are not running then you will need to restart or start the services with these commands.
{% endhint %}

```
sudo systemctl restart kibana
```

```
sudo systemctl restart elasticsearch
```
