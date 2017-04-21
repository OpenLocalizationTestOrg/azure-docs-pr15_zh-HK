<properties
   pageTitle="DC/OS CLI 安装 |Microsoft Azure"
   description="安装 DC/OS CLI。"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="容器，微服务 DC/OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] 这是用于处理基于 DC/OS 的 ACS 群集。 没有必要为此基于蜂群的 ACS 群集。

第一，[连接到基于 DC/OS 的 ACS 群集](../articles/container-service/container-service-connect.md)。 完成连接后，您可以在客户端计算机使用下面的命令安装 DC/OS CLI:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

如果您使用的 Python 的旧版本，可能会发现一些"InsecurePlatformWarnings"。 您可以放心地忽略这些。

首先不需要重新启动您的外壳，运行︰

```bash
source ~/.bashrc
```

此步骤时将不需要启动新的外壳。

现在，您可以确认安装 CLI:

```bash
dcos --help
```