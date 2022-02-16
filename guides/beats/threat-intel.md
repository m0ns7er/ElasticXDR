---
description: Configure Threat Intel
---

# Threat Intel

Now cd into your Filebeat folder.

```
cd /etc/filebeat/
```

Edit Filebeat

```
sudo nano filebeat.yml
```

Add these settings under **hosts: \["192.168.0.25:9200"]** and make sure **https** is un-commented and you put in your **elastic** password.

```
ssl.certificate_authorities: ["/etc/elasticsearch/certs/elasticsearch.crt"]
```

Don't forget to add: HTTPS to your config like so!

```
["https://192.168.0.25:9200"]
```

As well put in your Elastic Password as shown like sample config below.

#### Example Configs Below Before:

```
---------------------------- Elasticsearch Output ----------------------------
output.elasticsearch:
  #Array of hosts to connect to.
  hosts: ["https://192.168.0.25:9200"]

  #Protocol - either `http` (default) or `https`.
  #protocol: "https"

  # Authentication credentials - either API key or username/password.
  #api_key: "id:api_key"
  #username: "elastic"
  #password: "This is your elastic password"

```

#### After Configs Below:

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
  password: "MyPassword"

```

Now navigate to your filebeat modules.d location.

```
cd /etc/filebeat/modules.d
```

Next, enable the Filebeat module threatintel.

```
sudo filebeat modules enable threatintel
```

Before you setup Threat Intel, you will need an API key from OTX Alienvault.

Create your account then go to settings then copy your OTX API Key then add that key to this config below.

{% hint style="info" %}
**"The authentication token used to contact the OTX API, can be found on the OTX UI".**
{% endhint %}

{% embed url="https://otx.alienvault.com" %}
Alienvault API
{% endembed %}

Now edit your threatintel.yml file and erase contents with Ctrl + k then replace it with this config.

```
sudo nano threatintel.yml
```

{% hint style="info" %}
Look for this section in the script and replace my Demo API Key with your API.
{% endhint %}

```
- module: threatintel
  abuseurl:
    enabled: true

    # Input used for ingesting threat intel data.
    var.input: httpjson

    # The URL used for Threat Intel API calls.
    var.url: https://urlhaus-api.abuse.ch/v1/urls/recent/

    # The interval to poll the API for updates.
    var.interval: 10m

  abusemalware:
    enabled: true

    # Input used for ingesting threat intel data.
    var.input: httpjson

    # The URL used for Threat Intel API calls.
    var.url: https://urlhaus-api.abuse.ch/v1/payloads/recent/

    # The interval to poll the API for updates.
    var.interval: 10m

  malwarebazaar:
    enabled: true

    # Input used for ingesting threat intel data.
    var.input: httpjson

    # The URL used for Threat Intel API calls.
    var.url: https://mb-api.abuse.ch/api/v1/

    # The interval to poll the API for updates.
    var.interval: 10m

  misp:
    enabled: false

    # Input used for ingesting threat intel data, defaults to JSON.
    var.input: httpjson

    # The URL of the MISP instance, should end with "/events/restSearch".
    var.url: https://SERVER/events/restSearch

    # The authentication token used to contact the MISP API. Found when looking at user account in the MISP UI.
    var.api_token: API_KEY

    # Configures the type of SSL verification done, if MISP is running on self signed certificates
    # then the certificate would either need to be trusted, or verification_mode set to none.
    #var.ssl.verification_mode: none

    # Optional filters that can be applied to the API for filtering out results. This should support the majority of fields in a MISP context.
    # For examples please reference the filebeat module documentation.
    #var.filters:
    #  - threat_level: [4, 5]
    #  - to_ids: true

    # How far back to look once the beat starts up for the first time, the value has to be in hours. Each request afterwards will filter on any event newer
    # than the last event that was already ingested.
    var.first_interval: 300h

    # The interval to poll the API for updates.
    var.interval: 5m

  otx:
    enabled: false

    # Input used for ingesting threat intel data
    var.input: httpjson

    # The URL used for OTX Threat Intel API calls.
    var.url: https://otx.alienvault.com/api/v1/indicators/export

    # The authentication token used to contact the OTX API, can be found on the OTX UI.
    var.api_token: vc7fgdcosgaptb2rwrt83cbbzjvtrmggdghu73gzedejn8sx92y7eup6penio9qn

    # Optional filters that can be applied to retrieve only specific indicators.
    #var.types: "domain,IPv4,hostname,url,FileHash-SHA256"

    # The timeout of the HTTP client connecting to the OTX API
    #var.http_client_timeout: 120s

    # How many hours to look back for each request, should be close to the configured interval. Deduplication of events is handled by the module.
    var.lookback_range: 1h

    # How far back to look once the beat starts up for the first time, the value has to be in hours.
    var.first_interval: 400h

    # The interval to poll the API for updates
    var.interval: 5m

  anomali:
    enabled: true

    # Input used for ingesting threat intel data
    var.input: httpjson

    # The URL used for Threat Intel API calls. Limo has multiple different possibilities for URL's depending
    # on the type of threat intel source that is needed.
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/41/objects

    # The Username used by anomali Limo, defaults to guest.
    #var.username: guest

    # The password used by anomali Limo, defaults to guest.
    #var.password: guest

    # How far back to look once the beat starts up for the first time, the value has to be in hours.
    var.first_interval: 400h

    # The interval to poll the API for updates
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/107/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/135/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/136/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/150/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/200/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/209/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/31/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/313/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/68/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m
  anomali:
    enabled: true
    var.input: httpjson
    var.url: https://limo.anomali.com/api/v1/taxii2/feeds/collections/33/objects
    var.username: guest
    var.password: guest
    var.first_interval: 400h
    var.interval: 5m

  anomalithreatstream:
    enabled: false

    # Input used for ingesting threat intel data
    var.input: http_endpoint

    # Address to bind to in order to receive HTTP requests
    # from the Integrator SDK. Use 0.0.0.0 to bind to all
    # existing interfaces.
    var.listen_address: localhost

    # Port to use to receive HTTP requests from the
    # Integrator SDK.
    var.listen_port: 8080

    # Secret key to authenticate requests from the SDK.
    var.secret: '<Add your secret here>'

    # Uncomment the following and set the absolute paths
    # to the server SSL certificate and private key to
    # enable HTTPS secure connections.
    #
    # var.ssl_certificate: path/to/server_ssl_cert.pem
    # var.ssl_key: path/to/ssl_key.pem

  recordedfuture:
    enabled: false

    # Input used for ingesting threat intel data
    var.input: httpjson

    # The interval to poll the API for updates
    var.interval: 5m

    # How far back in time to start fetching intelligence when run for the
    # first time. Value must be in hours. Default: 168h (1 week).
    var.first_interval: 168h

    # The URL used for Threat Intel API calls.
    # Must include the `limit` parameter and at least `entity` and `timestamps` fields.
    # See the Connect API Explorer for a list of possible parameters.
    #
    # For `ip` entities:
    var.url: "https://api.recordedfuture.com/v2/ip/search?limit=200&fields=entity,timestamps,risk,intelCard,location&metadata=false"

    # For `domain` entities:
    # var.url: "https://api.recordedfuture.com/v2/domain/search?limit=200&fields=entity,timestamps,risk,intelCard,location&metadata=false"

    # For `hash` entities:
    # var.url: "https://api.recordedfuture.com/v2/hash/search?limit=200&fields=entity,fileHashes,timestamps,risk,intelCard,location&metadata=false"

    # For `url` entities:
    # var.url: "https://api.recordedfuture.com/v2/url/search?limit=200&fields=entity,timestamps,risk&metadata=false"

    # Set your API Token.
    var.api_token: "<RF_TOKEN>"
```

Once that is done restart filebeat.

```
sudo service filebeat restart
```

{% hint style="success" %}
Filebeat should restart!
{% endhint %}

Now load your dashboards with this command again.

```
sudo filebeat setup --dashboards
```

Now in your ElasticXDR Security dashboard you should see it enable like this.

![Enabled](<../../.gitbook/assets/image (57).png>)

{% hint style="warning" %}
If you don't see a caution sign on anything then it's working.
{% endhint %}

Now your Threat Intel module show be working.
