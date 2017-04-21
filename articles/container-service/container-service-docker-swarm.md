<properties
   pageTitle="与 Docker 群的 azure 容器服务容器管理 |Microsoft Azure"
   description="将容器部署到 Docker 群在 Azure 容器服务"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker，容器，微服务 Mesos，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-with-docker-swarm"></a>容器管理与 Docker 群

Docker 蜂群提供用于部署跨 Docker 主机共用一套容器化工作负载环境。 Docker 蜂群使用本机 Docker API。 管理容器上 Docker 群的工作流是与它在单个容器主机上会几乎完全相同。 本文档提供了部署的 Docker 群 Azure 容器服务实例中的容器化工作负载的简单示例。 Docker 群上的更多详细文档，请参阅[Docker 群在 Docker.com 上](https://docs.docker.com/swarm/)。

本文中的练习的先决条件︰

[在 Azure 容器服务中创建的蜂群群集](container-service-deployment.md)

[在 Azure 容器服务了蜂群群集连接](container-service-connect.md)

## <a name="deploy-a-new-container"></a>部署新的容器

若要创建 Docker 群中一个新的容器，使用`docker run`（确保您已打开了到根据上述先决条件的母版的 SSH 隧道） 的命令。 本示例创建一个容器从`yeasy/simple-web`图像︰


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

在创建容器后，使用`docker ps`返回有关该容器的信息。 这里请注意，承载容器的蜂群代理列出︰


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

您现在可以访问通过蜂群代理负载平衡器的公共 DNS 名称容器中运行的应用程序。 您可以在 Azure 门户中找到此信息︰  


![真正的访问结果](media/real-visit.jpg)  

默认情况下，负载平衡器具有端口 80，8080 和 443 打开。 如果您想要在您需要为代理池打开该端口在 Azure 负载平衡器上的另一个端口上连接。

## <a name="deploy-multiple-containers"></a>部署多个容器

启动多个容器，通过执行 docker 运行多次，可以使用`docker ps`上运行命令来查看哪些主机容器。 在下面的示例中，三个容器是均匀地分布到三个蜂群代理︰  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>通过使用 Docker 撰写部署容器

您可以使用 Docker 撰写自动部署和配置的多个容器。 为此，请确保已创建了一个安全外壳协议 (SSH) 隧道和已 （请参见上面的必备），设置 DOCKER_HOST 变量。

创建 docker compose.yml 文件在您的本地系统上。 若要执行此操作，请使用此[示例](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml)。

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

运行`docker-compose up -d`启动容器部署︰


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

最后，将返回运行容器的列表。 此列表反映了已部署使用 Docker 撰写的容器︰


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

当然，您可以使用`docker-compose ps`检查仅在定义容器您`compose.yml`文件。

## <a name="next-steps"></a>下一步行动

[了解更多有关 Docker 群](https://docs.docker.com/swarm/)
