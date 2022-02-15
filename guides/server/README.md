---
description: Configure Server
---

# 🖥 Server

![Default Dashboard Login](<../../.gitbook/assets/image (76).png>)

Now click on Explorer on my own!

{% hint style="info" %}
Lets Setup Our Elastic Fleet Server.
{% endhint %}

![Dashboard](<../../.gitbook/assets/image (81).png>)

Now we need to navigate to our Fleet Integrations.

So click on the 3 line at the Left Hand Corner.

![7.16.3 Build](<../../.gitbook/assets/image (3).png>)

Now navigate to Management, then Fleet.

![](<../../.gitbook/assets/image (97).png>)

Then go to Fleet Settings.

![Fleet Settings](<../../.gitbook/assets/image (2).png>)

Now change the Fleet Host IP to look likes mines with your IP Information.

![Fleet Server IP Settings](<../../.gitbook/assets/image (21).png>)

{% hint style="info" %}
Now lets add our own Flee Server Policy!
{% endhint %}

Now click on Agent Policies.

![Policies](<../../.gitbook/assets/image (15).png>)

Now Click on Create agent policy.

![New policy](<../../.gitbook/assets/image (119).png>)

Now add your information to the policy what you like then click create agent policy.

![Setup Policy](<../../.gitbook/assets/image (26).png>)

{% hint style="info" %}
Now we need to add our Flee Server Integration to this policy.
{% endhint %}

So click on Fleet Server name.

![Fleet Server](<../../.gitbook/assets/image (70).png>)

Now click on Add integration in the middle.

![Add integration](<../../.gitbook/assets/image (5).png>)

Now search for Fleet Server in the searchbox.

![Search](<../../.gitbook/assets/image (4).png>)

![Fleet Add On](<../../.gitbook/assets/image (121).png>)

![Add Fleet](<../../.gitbook/assets/image (51).png>)

Just Follow Steps The Steps To Complete!

![Settings](<../../.gitbook/assets/image (67).png>)

![Settings2](<../../.gitbook/assets/image (43).png>)

Make sure you choose FleetServer policy at the end fore Agent policy

Now Choose Add Elastic Agent Later.

![Added](<../../.gitbook/assets/image (22).png>)

Now click on Got It to close that message.

![Close Message](<../../.gitbook/assets/image (122).png>)

![Add integration](<../../.gitbook/assets/image (75).png>)

Now Add your Fleet Server.

![Agent Select](<../../.gitbook/assets/image (25).png>)

{% hint style="danger" %}
Make sure you select **Quick start** deployment mode for security!
{% endhint %}

![Quick Start](<../../.gitbook/assets/image (73).png>)

Now lets generate **Fleet Server** Token.

{% hint style="info" %}
**Important Note:** Please save that enrollment token. You will not see it again!
{% endhint %}

Generate Service Token.

![Token Generate](<../../.gitbook/assets/image (104).png>)

Save Token.

![Service Token](<../../.gitbook/assets/image (69).png>)

{% hint style="info" %}
Server-tar-install Config Fleet Server Below.
{% endhint %}

Now run this command in your Fleet Server Terminal.

```
sudo nano server-tar-install.sh
```

Remember:  Change these settings for you Username & Password then save it for server-tar-install.sh

```
username='<ssh username>'
certIP='<IP to cert server>'
```

Now copy this config below to a notepad editor and make changes to it then save it.

```
#!/bin/bash
#########################################################
#  Elastic Agent Ubuntu Install Steps using apt
#		by David Walden (Rainiur)
#  These steps will: 
#  Add the Elastic Repository to apt
#  Download and install the elastic agent
#  Download the certificate from the server via scp
#       (This script will register a self-signed cert if you are using a 3rd party signed cert delete --insecure from the elastic-agent install)
#  Add the certificate
#  And run the install to register it to your Fleet manager
#
#  You will need the following
#		- ssh credentials, filename and IP for the server with your cert 
#			(best to copy the cert to the root directory of the user)
#		- IP address of your Fleet Server
#		- Token for the policy to apply to the system
#
#########################################################

username='<ssh username>'
certIP='<IP to cert server>'

## Download the cert and add the the shared certs directory
# scp <username>@<Server IP>:<filename i.e. ca.crt> .
# mv ca.crt /usr/local/share/ca-certificates/
# OR #
scp $username@$certIP:/ca.crt /usr/local/share/ca-certificates/

## Add the cert
update-ca-certificates

## Download and install the tarball image
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.16.3-linux-x86_64.tar.gz
tar xzvf elastic-agent-7.16.3-linux-x86_64.tar.gz

## Since the agent was installed via apt we just need to enroll with the Fleet Server
cd elastic-agent-7.16.3-linux-x86_64

```

Example Config click to enlarge picture.

![Fleet Server Script](<../../.gitbook/assets/image (107).png>)

{% hint style="info" %}
Optional Link Reference Only
{% endhint %}

{% embed url="https://www.elastic.co/downloads/past-releases#elastic-agent" %}
Elastic Agents
{% endembed %}

Now run that script with.

```
sudo bash server-tar-install.sh
```

{% hint style="danger" %}
Note: If you do not change back in to Elastic folder, then "cd" in that folder again and run script to install agent.
{% endhint %}

Once you run that script go back to your **ElasticXDR** in kibana, and grab this below script and paste it into the terminal and run it.

{% hint style="info" %}
**Note**: Choose **Linux/macOS** Platform and copy that command and paste into your **ElasticXDR** Server and run the command
{% endhint %}

![Enroll & Connect Fleet Server](<../../.gitbook/assets/image (37).png>)

{% hint style="success" %}
Now your **ElasticXDR** Fleet Server should be connected!
{% endhint %}

![Installed](<../../.gitbook/assets/image (103).png>)

![Agent Enrolled](<../../.gitbook/assets/image (63).png>)
