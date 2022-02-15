---
description: Additional Software Install
---

# Prerequisites

Install Java 8.

```
sudo apt-get install openjdk-8-jdk -y
```

Add Elastic Repository GPG Key.

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

Install Apt HTTPS Transports Package.

```
sudo apt-get install apt-transport-https -y
```

Add Elastic Repository.

```
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```

Update System Again.

```
sudo apt-get update -y
```

