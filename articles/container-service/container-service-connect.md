<properties
   pageTitle="连接到 Azure 容器服务群集 |Microsoft Azure"
   description="通过使用 SSH 隧道连接到 Azure 容器服务的群集。"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker，容器，微-服务，DC/OS，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>连接到 Azure 容器服务群集

DC/OS 和 Azure 容器服务部署的 Docker 群簇公开其余的终结点。 但是，这些终结点并不向外界开放。 为了管理这些终结点，则必须创建安全外壳协议 (SSH) 隧道。 一个 SSH 隧道建立之后，可以对群集的终结点运行命令并查看群集用户界面通过浏览器在您自己的系统上。 本文档将指导您完成从 Linux、 OS X 和 Windows 创建 SSH 隧道。

>[AZURE.NOTE] 您可以使用群集管理系统创建 SSH 会话。 但是，我们不建议这样。 直接从事管理系统公开无意配置更改的风险。   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>在 Linux 或 OS X 上创建 SSH 隧道

在 Linux 或 OS X 中创建 SSH 隧道时需要执行的第一件事是找到负载平衡主机的公共 DNS 名称。 为此，展开资源组，以便显示每个资源。 查找并选择的主服务器的公共 IP 地址。 这将打开刀片式服务器包含公用 IP 地址，其中包括 DNS 名称有关的信息。 保存以供将来使用此名称。 <br />


![公共 DNS 名称](media/pubdns.png)

现在打开外壳程序并运行下面的命令在其中︰

**端口**是要公开的终结点的端口。 对于的蜂群，这是 2375年。 对 DC/OS 中，使用端口 80。  
**用户名**是您部署群集时提供的用户名。  
**DNSPREFIX**为您提供部署群集时 DNS 前缀。  
**区域**是资源组所在的地区。  
**PATH_TO_PRIVATE_KEY**[可选] 是时创建容器服务群集提供的公钥相对应的私钥的路径。 使用此选项与-i 标志。

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> SSH 连接端口是 2200-不是标准端口 22。

## <a name="dcos-tunnel"></a>DC/OS 隧道

若要打开通往 DC/OS 相关端点的隧道，执行类似下面的命令︰

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

现在，您可以访问在 DC/OS 相关端点︰

- DC/操作系统︰`http://localhost/`
- 马拉松比赛︰`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

同样，可以通过该隧道的每个应用程序的其余部分 Api 来访问。

## <a name="swarm-tunnel"></a>蜂群隧道

若要打开通往蜂群端点的隧道，执行类似以下的命令︰

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

现在可以设置您的 DOCKER_HOST 环境变量，如下所示。 您可以继续照常使用您 Docker 命令行界面 (CLI)。

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>在 Windows 上创建 SSH 隧道

有多个选项用于创建在 Windows 上的 SSH 隧道。 本文档将描述如何使用 PuTTY 来执行此操作。

下载到您的 Windows 系统的 PuTTY 并运行该应用程序。

输入的主机名，群集管理员的用户名称和群集中的第一个主控的公共 DNS 名称组成。 **主机名称**将如下所示︰ `adminuser@PublicDNS`。 输入**端口**2200年。

![PuTTY 配置 1](media/putty1.png)

选择**SSH**和**身份验证**。 添加您的私钥文件进行身份验证。

![PuTTY 配置 2](media/putty2.png)

选择**隧道**并配置端口转发如下︰
- **的源端口︰**您的首选项-80 用于 DC/OS 或 2375 个蜂群。
- **目标︰**使用 DC/OS 或 localhost:2375 localhost:80 的蜂群。

下面的示例配置的直流/OS 中，但将寻找类似 Docker 群。

>[AZURE.NOTE] 当您创建此隧道端口 80 必须不会被使用。

![PuTTY 配置 3](media/putty3.png)

完成后，保存该连接配置，并连接 PuTTY 会话。 在连接时，您可以看到 PuTTY 事件日志中的端口配置。

![PuTTY 事件日志](media/putty4.png)

当您已为 DC/OS 配置隧道时，可以访问在相关的终结点︰

- DC/操作系统︰`http://localhost/`
- 马拉松比赛︰`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

当您已为 Docker 群配置隧道时，您可以通过 Docker CLI 访问蜂群群集。 您需要先配置 Windows 环境变量名为`DOCKER_HOST`值为` :2375`。

## <a name="next-steps"></a>下一步行动

部署和管理与 DC/OS 或蜂群的容器︰

- [使用 Azure 容器服务和 DC/OS](container-service-mesos-marathon-rest.md)
- [使用 Azure 容器服务和 Docker 蜂群](container-service-docker-swarm.md)
