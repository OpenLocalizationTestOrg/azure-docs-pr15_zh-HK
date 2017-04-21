<properties
   pageTitle="Azure 容器服务简介 |Microsoft Azure"
   description="Azure 容器服务提供一种方法来简化创建、 配置和运行容器化应用程序预配置的虚拟机群集的管理。"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker，容器，微服务 Mesos，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Azure 容器服务简介

Azure 容器服务使您可以创建、 配置和管理群集运行容器化应用程序预配置的虚拟机的更简单。 它使用流行的开源计划和协调工具的优化的配置。 这使您可以利用现有的技能，或利用庞大且不断壮大主体社区专业技能，来部署和管理基于容器的 Microsoft Azure 上的应用程序。


![Azure 容器服务提供的方法来管理容器化在 Azure 上的多个主机上的应用程序。](./media/acs-intro/acs-cluster.png)


Azure 容器服务利用 Docker 容器格式，以确保您的应用程序容器是完全可移植的。 这样，您可以扩展到数千个容器或甚至数以万计的这些应用程序，它还支持您选择的马拉松比赛和 DC/OS 或 Docker 群。

通过使用 Azure 容器服务，您可以利用 Azure，企业级功能的同时保持应用程序的可移植性，包括在业务流程层的可移植性。

<a name="using-azure-container-service"></a>使用 Azure 容器服务
-----------------------------

Azure 容器服务与我们的目标是提供宿主环境使用的开源工具和技术，目前我们的客户之间流行的容器。 为此，我们为您选的控制器 （DC/OS 或 Docker 群） 公开标准的 API 端点。 通过使用这些终结点，您可以利用能够交谈这些终结点的任何软件。 例如，对于 Docker 群终结点，您可以选择使用 Docker 命令行界面 (CLI)。 对 DC/OS 中，您可以选择使用 DCOS CLI。

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>通过使用 Azure 容器服务创建 Docker 群集
-------------------------------------------------------

要开始使用 Azure 容器服务，您部署门户 （搜索 Azure 容器服务），通过 Azure 容器服务群集使用 Azure 资源管理器模板 （[Docker 群](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)或[DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)） 或[CLI](/documentation/articles/xplat-cli-install/)。 提供快速启动模板可以进行修改，以包含附加或高级 Azure 的配置。 部署一个 Azure 容器服务群集的详细信息，请参阅[部署群集 Azure 容器服务](container-service-deployment.md)。

<a name="deploying-an-application"></a>部署应用程序
------------------------

Azure 容器服务提供业务流程 Docker 群或 DC/OS 选项。 部署应用程序的方式取决于所选的控制器。

### <a name="using-dcos"></a>使用 DC/OS

DC/OS 是分布式的操作系统基于 Apache Mesos 分布式的系统内核。 Apache Mesos 收容在 Apache 软件基金会，并列出了一些[中的最大名称 IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/)作为用户和贡献者。

![配置为显示代理和基础的蜂群的 azure 容器服务。](media/acs-intro/dcos.png)

DC/OS 和 Apache Mesos 包括令人印象深刻的功能集︰

-   经验证的可扩展性

-   主服务器和从属服务器使用 Apache ZooKeeper 的容错复制

-   对 Docker 格式化容器的支持

-   本机与 Linux 容器的任务之间的隔离

-   Multiresource 计划 （，内存、 CPU、 磁盘，并端口）

-   Java、 Python 和 c + + Api，可用于开发新的并行应用程序

-   查看群集状态的 web 用户界面

默认情况下，DC/OS 在 Azure 容器服务上运行包括安排工作负载的马拉松业务流程平台。 但是，附带 ACS 的 DC/OS 部署是 Mesosphere 宇宙的可以添加到您的服务，其中包括触发，Hadoop，卡桑德拉和其他更多的服务。

![在 Azure 容器服务中的 DC/OS 世界](media/dcos/universe.png)

#### <a name="using-marathon"></a>使用马拉松比赛

马拉松比赛是群集范围内的初始化和服务 cgroups — 中，或者在 Azure 容器服务，Docker 格式化容器的控制系统。 马拉松比赛提供 web 用户界面可以从中部署应用程序。 您可以在如下所示的 URL 访问这`http://DNS_PREFIX.REGION.cloudapp.azure.com`在 DNS\_前缀和地区同时在部署时定义。 当然，您还可以提供您自己的 DNS 名称。 有关运行容器使用马拉松 web 用户界面的详细信息，请参阅[通过 web 用户界面的容器管理](container-service-mesos-marathon-ui.md)。

![马拉松比赛应用程序列表](media/dcos/marathon-applications-list.png)

此外可以使用 REST Api 与马拉松进行通信。 有大量的客户端库所提供的每一种工具。 它们涵盖多种语言 — 而且，当然，在任何语言中使用的 HTTP 协议。 此外，许多流行 DevOps 工具提供支持的马拉松比赛。 当您正在使用 Azure 容器服务群集操作团队为提供最大的灵活性。 通过 REST API，马拉松比赛运行容器的详细信息，请参阅[使用 REST API 的容器管理](container-service-mesos-marathon-rest.md)。

### <a name="using-docker-swarm"></a>使用 Docker 蜂群

Docker 蜂群提供本机群集 Docker。 因为 Docker 群，填补标准 Docker API 已经与 Docker 守护进程进行通信的任何工具可以使用蜂群透明地扩展到多个主机上 Azure 容器服务。

![Azure 容器服务配置为使用 DC/OS-显示 jumpbox、 代理和主控形状。](media/acs-intro/acs-swarm2.png)

管理蜂群群集上的容器支持的工具包括，但不是限于以下︰

-   Dokku

-   Docker CLI 和 Docker 撰写

-   Krane

-   Jenkins

<a name="videos"></a>视频
------

开始使用 Azure 容器服务 (101):  

> [AZURE.VIDEO azure-container-service-101]

生成应用程序使用 Azure 容器服务 (生成 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
