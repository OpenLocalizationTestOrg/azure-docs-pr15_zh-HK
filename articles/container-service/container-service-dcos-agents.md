<properties
   pageTitle="公用和专用直流/OS 代理池 ACS 的 |Microsoft Azure"
   description="公用和专用代理池如何使用 Azure 容器服务群集。"
   services="container-service"
   documentationCenter=""
   authors="Thraka"
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
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Azure 容器服务 DC/OS 代理池

DC/操作系统 Azure 容器服务划分为公用或专用池代理。 可以到任何一个池，影响容器服务中的计算机之间的可访问性做出部署。 机器可以暴露给 internet （公共） 或保持内部 （专用）。 这篇文章概括的简要概述，为什么存在公用和专用池。

### <a name="private-agents"></a>专用的代理

通过非路由网络运行专用代理节点。 管理区域或经过公共区域的区域边界路由器，才可以访问此网络。 默认情况下，直流/OS 启动专用代理节点上的应用程序。 有关网络安全的详细信息，请查阅[DC/OS 文档](https://dcos.io/docs/1.7/administration/securing-your-cluster/)。

### <a name="public-agents"></a>公共代理

公共代理节点通过一个可公开访问的网络运行 DC/OS 的应用程序和服务。 有关网络安全的详细信息，请查阅[DC/OS 文档](https://dcos.io/docs/1.7/administration/securing-your-cluster/)。

## <a name="using-agent-pools"></a>使用代理池

默认情况下，**马拉松比赛**将部署任何新的应用程序*专用*的代理节点。 您必须显式地将应用部署到*公共*节点在该应用程序的创建过程。 选择**可选**的选项卡上，输入**slave_public**作为**接受资源角色**值。 此过程介绍了[这里](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container)并在[DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/)文档中。

## <a name="next-steps"></a>下一步行动

阅读有关[管理 DC/OS 容器](container-service-mesos-marathon-ui.md)的详细信息。

了解如何[打开防火墙](container-service-enable-public-access.md)提供的 Azure 允许公共访问 DC/OS 容器。