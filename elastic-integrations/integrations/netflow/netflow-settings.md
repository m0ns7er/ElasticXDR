---
description: Configure Network Settings
---

# Netflow Settings

{% hint style="info" %}
Netflow Enabled Device Only!
{% endhint %}

Netflow Integration is an Elastic module that will capture all Network flow data that is directed to the system. Once this is setup you will see rich information about your network and what systems it's communicating to and with.

On the Elastic Home Screen go to Add Integrations.

![Home Screen Elastic](<../../../.gitbook/assets/image (30).png>)

Now you should see this screen below.

![Integrations Screen](<../../../.gitbook/assets/image (61).png>)

Here you wanna search for **Netflow** in the search box.

![Netflow Integration](<../../../.gitbook/assets/image (112).png>)

Now click on that Integration and click add once that is done. You will see informaton that is generic so put in your informaton like so as mines.

![Add](<../../../.gitbook/assets/image (68).png>)

{% hint style="info" %}
I put **xdr** in the Namespace so that I can track it later in Discover.
{% endhint %}

![Edit](<../../../.gitbook/assets/image (34).png>)

{% hint style="danger" %}
**Important:** replace localhost with 0.0.0.0 and give it a port number to receive data on.
{% endhint %}

Setting it to 0.0.0.0 opens all interfaces to reviece a connect to this server.

![Fill Out](<../../../.gitbook/assets/image (33).png>)

Here you can put TAGS here to later search for in Discover to find certain strings.

![Tags](<../../../.gitbook/assets/image (48).png>)

{% hint style="danger" %}
**Important**: Add you policy to your **ElasticXDR** Policy you created.
{% endhint %}

![Default](<../../../.gitbook/assets/image (116).png>)

![Change to ElasticXDR](<../../../.gitbook/assets/image (93).png>)

Once you have finish click save & Continue to add.

Once you go back to your Policies you will see under your ElasticXDR Integrations netflow.

{% hint style="info" %}
These are some of the ones I have added to mines to get data. The process is all the same as Netflow.
{% endhint %}

![Integration Added](<../../../.gitbook/assets/image (7).png>)

{% hint style="warning" %}
Only certain Integrations can be added to your ElasticXDR Server Policy, not all are meant for this policy. Only Syslog types of Integration are really needed.
{% endhint %}
