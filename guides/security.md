---
description: Configure Security
---

# 🔐 Security

{% hint style="info" %}
**Good to know:** This section will help Secure your ElasticXDR Server.
{% endhint %}

## Certificates Setup

First step is to navigate to the Elasticsearch folder location.

```
cd /usr/share/elasticsearch/
```

Now create this file: **instances.yml.**

```
sudo nano instances.yml
```

Now add these lines with your IP address.

```
instances:
    - name: "elasticsearch"
      ip:
        - "192.168.0.25"
    - name: "kibana"
      ip:
        - "192.168.0.25"
```

Now execute elasticsearch-certutil and create a root CA --pem zip file.

```
sudo bin/elasticsearch-certutil ca --pem
```

Install unzip.

```
sudo apt install zip unzip -y
```

Then unzip the Certificate.

```
sudo unzip elastic-stack-ca.zip
```

Then run this command to create the Elasticsearch & Kibana Certificates.

```
sudo /usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca-cert ca/ca.crt --ca-key ca/ca.key --pem --in instances.yml --out certs.zip

```

Then unzip certs.zip file.

```
sudo unzip certs.zip
```

Now create the **certs** directory.

```
sudo mkdir certs
```

Now we need to move the Elasticsearch certificates to certs.

```
sudo mv /usr/share/elasticsearch/elasticsearch/* certs/
```

Now we need to move the Kibana certificates to certs.

```
sudo mv /usr/share/elasticsearch/kibana/* certs/
```

Make these directories for the certificate move.

```
sudo mkdir /etc/kibana/certs
sudo mkdir /etc/kibana/certs/ca
sudo mkdir /etc/elasticsearch/certs
sudo mkdir /etc/elasticsearch/certs/ca
```

Now copy the certificates to them directories.

```
sudo cp ca/ca.* /etc/kibana/certs/ca
sudo cp ca/ca.* /etc/elasticsearch/certs/ca
sudo cp certs/elasticsearch.* /etc/elasticsearch/certs/
sudo cp certs/kibana.* /etc/kibana/certs/
```

Also copy the ca.crt to the home directory we will need this later for auto deployment.

```
sudo cp ca/ca.crt /
```

Now lets clean up a bit.

```
sudo rm -r elasticsearch/ kibana/
```

Now lets change some permissions.

```
cd /usr/share
```

Change folder permissions.

```
sudo chown -R elasticsearch:elasticsearch elasticsearch/
sudo chown -R elasticsearch:elasticsearch /etc/elasticsearch/
cd elasticsearch
sudo chown -R elasticsearch:elasticsearch certs/
```

Now lets change the permissions for CA/ directory.

```
sudo chown -R elasticsearch:elasticsearch ca/
```

{% hint style="success" %}
Once you are here your are just about able to login.
{% endhint %}

## Verify Your Certificates

We must Verify our certificates are correct just to make sure we got it right.

Your Root CA should be the: **ca.crt.**

```
sudo openssl x509 -in /etc/elasticsearch/certs/elasticsearch.crt -text -noout
```

{% hint style="info" %}
Look for: Subject: CN = elasticsearch | Subject Alternative Name: IP Address: e.g 192.168.0.25
{% endhint %}

```
sudo openssl x509 -in /etc/kibana/certs/kibana.crt -text -noout
```

{% hint style="info" %}
Look for: Subject: Subject: CN = kibana | Subject Alternative Name: IP Address: e.g 192.168.0.25
{% endhint %}

```
sudo openssl x509 -in /etc/kibana/certs/ca/ca.crt -text -noout
```

{% hint style="info" %}
Look for: Basic Constraints: critical CA:TRUE
{% endhint %}

This is your Root CA!.

Since we created the **Elasticsearch** & **Kibana** certs, we now have to add the certs path to the config file.

## **Add Certificates To YML Files**

Lets go back and edit you Kibana file again**.**

```
sudo nano /etc/kibana/kibana.yml
```

Now un-comment (#) hash signs and edit the values below to match.

Change the values to match the location path of the certs you created.

```
server.ssl.enabled: true
server.ssl.certificate: "/etc/kibana/certs/kibana.crt"
server.ssl.key: "/etc/kibana/certs/kibana.key"
```

Now copy & make the changes to the IP address for you**r** server IP configs below**.**

```
elasticsearch.hosts: ["https://192.168.0.25:9200"]
elasticsearch.ssl.certificateAuthorities: ["/etc/kibana/certs/ca/ca.crt"]
elasticsearch.ssl.certificate: "/etc/kibana/certs/kibana.crt"
elasticsearch.ssl.key: "/etc/kibana/certs/kibana.key"
```

Server PublicBaseUrl.

```
server.publicBaseUrl: "https://192.168.0.25:5601"
```

Now save the file with the changes and exit.

Now restart Kibana & check status.

```
sudo systemctl restart kibana
```

```
sudo systemctl status kibana
```

Now restart Elasticsearch & check status.

```
sudo systemctl restart elasticsearch
```

```
sudo systemctl status elasticsearch
```

Now edit your Elasticsearch yml file.

```
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Security Feature to add at the end of the file.

```
xpack.security.enabled: true
xpack.security.authc.api_key.enabled: true

xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.transport.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.transport.ssl.certificate_authorities: [ "/etc/elasticsearch/certs/ca/ca.crt" ]

xpack.security.http.ssl.enabled: true
xpack.security.http.ssl.verification_mode: certificate
xpack.security.http.ssl.key: /etc/elasticsearch/certs/elasticsearch.key
xpack.security.http.ssl.certificate: /etc/elasticsearch/certs/elasticsearch.crt
xpack.security.http.ssl.certificate_authorities: [ "/etc/elasticsearch/certs/ca/ca.crt" ]
```

Now reboot Elasticsearch & check status.

```
sudo systemctl restart elasticsearch
```

```
sudo systemctl status elasticsearch
```

{% hint style="info" %}
Please change **"something\_at\_least\_32\_characters"** to a Random String!
{% endhint %}

{% embed url="https://bitwarden.com/password-generator" %}
Use this for your password gen if you don't have one.
{% endembed %}

Now edit your Kibana.yml file add this security feature at the end of the file.

```
sudo nano /etc/kibana/kibana.yml
```

Security feature and timeout settings below plus encryption password.

{% hint style="warning" %}
Remember: 32\_characters is needed!
{% endhint %}

```
xpack.security.enabled: true
xpack.security.session.idleTimeout: "30m"
xpack.encryptedSavedObjects.encryptionKey: "something_at_least_32_characters"
```

Now lets restart Kibana & Elasticsearch.

```
sudo systemctl restart kibana
```

```
sudo systemctl status kibana
```

```
sudo systemctl restart elasticsearch
```

```
sudo systemctl status elasticsearch
```

## Create System Users

{% hint style="danger" %}
Please take note of these User names & Passwords. You cannot repeat this again!
{% endhint %}

```
cd /usr/share/elasticsearch/bin
```

```
sudo ./elasticsearch-setup-passwords auto
```

{% hint style="success" %}
Once these Usernames are created we can login to the systems after this setup.
{% endhint %}

Now we must go back again and add our **Kibana\_system** user login credentials.

```
sudo nano /etc/kibana/kibana.yml
```

Now look for this field and un-comment the values and add your password for this account.

```
elasticsearch.username: "kibana_system"
elasticsearch.password: "oZdTyaMMxCLaNmaW9udf"
```

{% hint style="info" %}
This will authorize the kibana\_system user.
{% endhint %}

Now restart Kibana service.

```
sudo systemctl restart kibana
```

Status Check of Kibana.

```
sudo systemctl status kibana
```

Now to access Kibana open you browser and type in this web address.

```
https://Your-IP-Here:5601
```

{% hint style="success" %}
Once this process is done your ElasticXDR will be working!!&#x20;
{% endhint %}

You can login with the username: **elastic** & **Password** that was generated for you.

## Conclusion

We now have our security features enabled and working, so let's add our **ElaticXDR Fleet Server.**
