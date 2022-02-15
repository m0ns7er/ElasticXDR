---
description: Configure Encryption
---

# Encryption

### Filebeat Setup

Navigate to your **filebeat** location.

```
cd /etc/filebeat
```

Next, enable the Filebeat module threatintel.

```
sudo filebeat modules enable threatintel
```

{% hint style="info" %}
**Note**: If you have a device that supports netflow traffic, like: **Meraki**, **pfSense** then enabled this module.
{% endhint %}

```
sudo filebeat modules enable netflow
```

Now edit your **filebeat** config.

```
sudo nano filebeat.yml
```

### Filebeat Settings

{% hint style="info" %}
Enable TLS/SSL Encryption
{% endhint %}

Enable HTTPS settings for Kibana.

{% hint style="info" %}
Look for these setting for Kibana and change it to HTTPS as seen below.
{% endhint %}

```
setup.kibana:

  # Kibana Host
  # Scheme and port can be left out and will be set to the default (http and 56>
  # In case you specify and additional path, the scheme is required: http://loc>
  # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
  host: "https://192.168.0.25:5601"
```

SSL Settings for **filebeat** to connect to kibana

Add this settings under **hosts: \["**[**https://192.168.0.25:9200**](https://192.168.0.25:9200)**"]** and make sure **HTTPS** is uncommitted and you put in your **elastic** password.

```
ssl.certificate_authorities: ["/etc/elasticsearch/certs/elasticsearch.crt"]
```

Make sure you configs look like this with the field uncommitted as so.

```
---------------------------- Elasticsearch Output ----------------------------
output.elasticsearch:
  #Array of hosts to connect to.
  hosts: ["https://192.168.0.25:9200"]
  ssl.certificate_authorities: ["/etc/elasticsearch/certs/elasticsearch.crt"]
  #Protocol - either `http` (default) or `https`.
  protocol: "https"

  # Authentication credentials - either API key or username/password.
  #api_key: "id:api_key"
  username: "elastic"
  password: "This is your elastic password"
```

Once that is all done, you will need to load your dashboards.

```
sudo filebeat setup --dashboards
```

{% hint style="success" %}
That is it, if you see that it loaded then it worked.
{% endhint %}
