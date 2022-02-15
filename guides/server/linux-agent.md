---
description: Agent Setup Only Linux Systems
---

# Linux Agent

This policy we are going to create first is our **Linux systems.**

{% hint style="info" %}
Navigate to the three lines in the left hand corner click on them then scroll down to **Fleet.**

**Remember click to enlarge picture!**
{% endhint %}

![Home Dashboard](<../../.gitbook/assets/image (6).png>)

Then click on **Agent Policies.**

![Agent Policy Setup ](<../../.gitbook/assets/image (96).png>)

Then click on **Create agent policy.**

This part below you will provide the information that you want to input. Then create policy to save below.

![Policy Information](<../../.gitbook/assets/image (86).png>)

![Linux Policy Created](<../../.gitbook/assets/image (111).png>)

Now click on your new **Linux Policy** then go to **Add Integration** then search for **Endpoint Security**, then add that.

![Endpoint Integration](<../../.gitbook/assets/image (12).png>)

Now click on Endpoint Security and add.

![Endpoint Security Add](<../../.gitbook/assets/image (101).png>)

{% hint style="info" %}
Make sure you have **Linux** policy selected before saving!
{% endhint %}

![Saving Linux Policy](<../../.gitbook/assets/image (31).png>)

Now go back to **Integrations** and search for **Linux.**

![Linux Integration Add](<../../.gitbook/assets/image (106).png>)

![Add Linux Integration](<../../.gitbook/assets/image (19).png>)

{% hint style="info" %}
Make sure to fill out this policy information before saving.
{% endhint %}

![Information Added](<../../.gitbook/assets/image (66).png>)

Now click Add.

![Integrations Added](<../../.gitbook/assets/image (39).png>)

{% hint style="info" %}
Agent Enrollment Below Only For Linux System!
{% endhint %}

Click Add Integration.

![Add Agent](<../../.gitbook/assets/image (98).png>)

{% hint style="info" %}
**Note**: You can copy this script and it's contents then save it to the system if there is no GUI.
{% endhint %}

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
fleetIP='<IP of Fleet Server>'
token='<token from Fleet Manager>'

## Download the cert and add the the shared certs directory
# scp <username>@<Server IP>:<filename i.e. ca.crt> .
# mv ca.crt /usr/local/share/ca-certificates/
# OR #
scp $username@$certIP:/ca.crt /usr/local/share/ca-certificates/

## Add the cert
update-ca-certificates

## Download and install the tarball image
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.16.2-linux-x86_64.tar.gz
tar xzvf elastic-agent-7.16.2-linux-x86_64.tar.gz

## Since the agent was installed via apt we just need to enroll with the Fleet Server
cd elastic-agent-7.16.2-linux-x86_64
./elastic-agent install -f --url=https://$fleetIP:8220 --insecure --enrollment-token=$token

## Restart and enable the service
systemctl start elastic-agent.service
systemctl enable elastic-agent.service

cd ..
rm -rf elastic*
```

Now create a file with this command below.

```
touch agent-tar-install.sh
```

{% hint style="info" %}
Then open that file and paste the contents of config above in it.
{% endhint %}

```
sudo nano agent-tar-install.sh
```

Now go back to **Kibana** and grab that enrollment script token.

{% hint style="info" %}
Now lets enroll a **Linux Device** I will use my **Ubuntu Laptop** so scroll on over until you get to the end of: **--enrollment-token=**
{% endhint %}

![Enrollment Token](<../../.gitbook/assets/image (72).png>)

So now, we need to open that script again and paste that **Agent token** into the token field with our token and our fleet server IP address and certip.&#x20;

They should be the same if this build is on one system. Don't forget to add your ElasticXDR user name to ssh into that system.

```
sudo nano agent-tar-install.sh
```

![Agent Script](<../../.gitbook/assets/image (105).png>)

Now lets run that script.

```
sudo bash agent-tar-install.sh
```

{% hint style="success" %}
If everything worked you will see a response like this.
{% endhint %}

![Enrolled](<../../.gitbook/assets/image (108).png>)

![Linux Added](<../../.gitbook/assets/image (74).png>)

{% hint style="success" %}
Linux Laptop Added!
{% endhint %}
