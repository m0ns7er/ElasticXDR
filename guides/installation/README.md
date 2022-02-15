---
description: >-
  This setup guide will show you how to create your ElasticXDR Platform.  I have
  listed some install steps below to help you re-create the same install build
  that I created.
---

# 🏗 Installation

### Server Requirements

* **8GB - 12GB Memory**
* **2CPU Processors**
* **Ubuntu** **20.04.3 LTS ISO**

### **Server Software**

* **Java** **8**
* **Git Software**
* **Elastic repository**
* **Putty or SSH Terminal**



{% hint style="info" %}
**Good to know:** Most of the software needed for this build will already be installed on the Server. Except for Java, Elastic Repos etc.
{% endhint %}

### The basics

Lets configure our Server with a static IP Address.

```markup
cd /etc/netplan/
```

Now install Networking Tools.

```
sudo apt install net-tools -y
```

Now Display Networking Information and copy your Network ID.

```
ifconfig -v
```

Example:&#x20;

```
ens160: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.16.100.6  netmask 255.255.255.0  broadcast 172.16.100.255
        inet6 fe80::250:56ff:fea9:2e97  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:a9:2e:97  txqueuelen 1000  (Ethernet)
        RX packets 40640203  bytes 122931198356 (122.9 GB)
        RX errors 2  dropped 344284  overruns 0  frame 0
        TX packets 20887562  bytes 23620529090 (23.6 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 352143408  bytes 194568613700 (194.5 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 352143408  bytes 194568613700 (194.5 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

<mark style="color:green;">**ens160**</mark> will be mines to use so copy what yours is you will need it for the next part.

You should see a file with this name below.

<mark style="color:green;">**00-installer-config.yaml**</mark>

Now edit this file below.

```
sudo nano 00-installer-config.yaml
```

Now add this config and make changes to it for you IP information after you erase everything. Then save with CTRL + x.

{% hint style="warning" %}
**Caution**: Make sure you know you Networking information like Default gateway, Subnet, IP etc.
{% endhint %}

```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
     dhcp4: no
     addresses: [192.168.0.25/24]
     gateway4: 192.168.0.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]
```

{% hint style="info" %}
You can use you own DNS Server Addresses if you would like.
{% endhint %}

### <mark style="color:red;">Warning Before Doing Next Step!!!</mark>

<mark style="color:red;">If you changed your IP, your connection will drop! If so ssh back into the Server with new IP.</mark>

Apply settings so Network manager can accept the new IP Information.

```
sudo netplan apply
```

<mark style="color:red;">If Server session disconnected then skip this below</mark><mark style="color:green;">.</mark>

Check settings with debug and make sure everything is showing like you want.

```
sudo netplan --debug apply
```
