---
description: Agent Setup
---

# Windows Agent

Now we can install our windows agent policy. This policy is like Linux but, it will have the windows integration installed instead of Linux.

So, go back to **Agent policies** then click on **Create agent policy** again.

![Windows Policy](<../../.gitbook/assets/image (17).png>)

Create agent policy, then click on that policy the click **Add Integration.**

![Add Integration](../../.gitbook/assets/image.png)

Search for **Endpoint Security** and add it.

![Endpoint Security](<../../.gitbook/assets/image (40).png>)

Then add it and put in your information like so.oo

![Add Endpoint](<../../.gitbook/assets/image (110).png>)

Now provide your info and save.

![Agent Policy](<../../.gitbook/assets/image (78).png>)

Then go back and search for **windows integration** and add it.oo

![Add Windows](<../../.gitbook/assets/image (18).png>)

![Add Integration](<../../.gitbook/assets/image (49).png>)

Provide your information then save the policy.

{% hint style="info" %}
On this screen expand the settings look over your collections and configure it to what you need.
{% endhint %}

![Configure Integration](<../../.gitbook/assets/image (42).png>)

Now click on **Actions** & **Add agent.**

![Ad Windows Agent](<../../.gitbook/assets/image (94).png>)

{% hint style="danger" %}
This script is for Servers as Well Windows Clients.
{% endhint %}

For Server Version, modify this lines and just un-comment these lines:

```
#Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine

#[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
#Invoke-WebRequest -Uri https://gist.github.com/darkoperator/6152630/raw/c67de4f7cd780ba367cccbc2593f38d18ce6df89/instposhsshdev

#Set-ExecutionPolicy -ExecutionPolicy Undefined -Scope LocalMachine
```

{% hint style="success" %}
Windows Client use script below as designed.

Don't for get to change IP, and username and password for your ElasticXDR Server.
{% endhint %}

Copy Powershell script.&#x20;

```
#########################################################
#  Elastic Agent Windows Powershell Install Script
#		by David Walden (Rainiur)
#  This script will download the certificate from the server via scp
#       (This script will register a self-signed cert if you are using a 3rd party signed cert
#       delete --insecure from the elastic-agent install)
#  Install the certificate into the LocalMachine Root
#  Download the agent from elastic.co 
#  And run the install to register it to your Fleet manager
#
#  You will need the following
#		- ssh credentials, filename and IP for the server with your cert 
#			(best to copy the cert to the root directory of the user)
#		- IP address of your Fleet Server
#		- Token for the policy to apply to the system
#
#########################################################

#Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine

#[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
#Invoke-WebRequest -Uri https://gist.github.com/darkoperator/6152630/raw/c67de4f7cd780ba367cccbc2593f38d18ce6df89/instposhsshdev

## Change to the user directory and install SSH module
Set-Location ~
Install-Module -Name Posh-SSH -Force

## Credentials to download cert file from server
#$credential = Get-Credential

## Use this instead if you do not want to be propmted for a login
$username = 'myname'
$password = 'password' | ConvertTo-SecureString -asPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential($username,$password)

## Download the cert and close the SSH session
Get-SCPItem -ComputerName 172.16.100.6 -AcceptKey -Credential $credential -Path '/ca.crt' -PathType File -Destination ./
Get-SSHSession | Remove-SSHSession

## Import the cert into the Root of the LocalMachine
Import-Certificate -FilePath ca.crt  -CertStoreLocation 'Cert:\LocalMachine\Root' -Verbose

# Remove copied cert file and uninstall module
Remove-Item ca.crt
Remove-Module -Name Posh-SSH -Force

## Verify the cert is installed
Get-ChildItem Cert:\LocalMachine\Root\ | Where-Object { $_.Subject -like '*Elastic*'}

## Download the agent
$filename = "elastic-agent-7.16.3-windows-x86_64.zip"
$ProgressPreference = 'SilentlyContinue'
$url = "https://artifacts.elastic.co/downloads/beats/elastic-agent/" + $filename
Invoke-WebRequest $url -OutFile $filename
$ProgressPreference = 'Continue'

## Unzip the agent
Expand-Archive $filename

## Change into agent directory and install (Don't know why is creates 2 subdirectories to store the files)
Set-Location $filename.Replace(".zip","")
Set-Location $filename.Replace(".zip","")
./elastic-agent install -f --url=https://172.16.100.6:8220 --insecure --enrollment-token=TkdSRnlud0JGRWRqNkxGTXpfekQ6c1p0ZGVtVWhRYUs3UktWbWlqVmZPUQ==


## Cleanup
Set-Location ../..
Remove-Item -Recurse -Force $filename.Replace(".zip","")
Remove-Item $filename

#Set-ExecutionPolicy -ExecutionPolicy Undefined -Scope LocalMachine
```

{% embed url="https://www.elastic.co/downloads/past-releases#elastic-agent" %}
Elastic Agent Releases Just an FYI
{% endembed %}

Now login into your windows system or server.

![Windows Server 2016](<../../.gitbook/assets/image (123).png>)

Now open Powershell ISE as Admin

![Powershel ISE](<../../.gitbook/assets/image (58).png>)

Now paste that script into a new tab into powershell.

![Scripted Added to Powershell](<../../.gitbook/assets/image (52).png>)

Then change your Fleet IP Address: **IP of Fleet Server** to your **ElasticXDR** and then go copy that **Token** from **kibana** and place that token into **token field**:

e.g: TUtXeUVId0J0N3R3S0wybnF6cjI6YmdwRWswcHpUTU9sZXAyQ0VYMS0yZw==

![Token Enrollment Process](<../../.gitbook/assets/image (87).png>)

Now save that script and hit the green button to run.

{% hint style="info" %}
Remember click on image to enlarge!
{% endhint %}

Now if **NuGet** warning pops up, click yes to install it

![Powershell Run](<../../.gitbook/assets/image (120).png>)

{% hint style="warning" %}
Microsoft Notice

As of April 2020, the PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. If you are not using TLS 1.2 or higher, you will receive an error when trying to access the PowerShell Gallery. Use the following command to ensure you are using TLS 1.2: PowerShell
{% endhint %}

{% hint style="info" %}
```
[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12
```
{% endhint %}

{% embed url="https://docs.microsoft.com/en-us/powershell/scripting/gallery/installing-psget?view=powershell-7.1" %}
Powershell SCP FYI Informational
{% endembed %}

{% hint style="info" %}
This statement has been added above in the script for you to edit but just in case it's better to understand if that error pops up for Server Build Agent.
{% endhint %}

Now the script should be running.

![Agent Install](<../../.gitbook/assets/image (114).png>)

{% hint style="info" %}
Don't worry if you get a warning status about **TLS.**
{% endhint %}

![Successful Install](<../../.gitbook/assets/image (113).png>)

Then Windows client or Server should be installed if you see this message at the bottom on the screen.

![Installed](<../../.gitbook/assets/image (46).png>)

{% hint style="success" %}
Windows Client or Server Installed.
{% endhint %}

![Installed](<../../.gitbook/assets/image (85).png>)

{% hint style="success" %}
That should do it, agents & fleet server setup!
{% endhint %}

