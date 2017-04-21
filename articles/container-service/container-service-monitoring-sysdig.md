<properties
   pageTitle="监视的 Azure 容器服务群集，Sysdig |Microsoft Azure"
   description="监视与 Sysdig Azure 容器服务群集。"
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="容器，DC/OS，Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>与 Sysdig Azure 容器服务群集监视器

在本文中，我们会将 Sysdig 代理部署到 Azure 容器服务群集中的所有代理节点。 对于此配置，您需要与 Sysdig 帐户。 

## <a name="prerequisites"></a>系统必备组件 

[部署](container-service-deployment.md)并[连接](container-service-connect.md)群集配置的 Azure 容器服务。 探讨[马拉松 UI](container-service-mesos-marathon-ui.md)。 转到[http://app.sysdigcloud.com](http://app.sysdigcloud.com) Sysdig 云帐户设置。 

## <a name="sysdig"></a>Sysdig

Sysdig 是允许您监视群集内的容器，您监视服务。 Sysdig 已知的帮助解决问题，但它还为 CPU、 网络、 内存和 I/O 具有基本监视度量标准。 Sysdig 容易地看到哪些容器正在使用 hardest 或实质上是最大的内存和 CPU。 此视图位于"概述"部分中，目前处于测试阶段。 

![Sysdig 用户界面](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Sysdig 将部署配置为使用马拉松比赛

这些步骤将向您介绍如何配置和 Sysdig 应用程序部署到您的群集的马拉松比赛。 

访问 DC/OS 用户界面通过[http://localhost:80 /](http://localhost:80/)次 DC/OS 用户界面中导航到"世界"，即在左下角，然后搜索"Sysdig"。

![Sysdig 直流/OS 世界中](./media/container-service-monitoring-sysdig/sysdig1.png)

现在要完成配置，您需要 Sysdig 云帐户或免费的试用帐户。 一旦您登录到 Sysdig 云网站，单击您的用户名，并在页上，您应看到您"访问键"。 

![Sysdig API 密钥](./media/container-service-monitoring-sysdig/sysdig2.png) 

接下来进入 DC/OS 宇宙内的 Sysdig 配置您的访问密钥。 

![DC/OS 世界中的 Sysdig 配置](./media/container-service-monitoring-sysdig/sysdig3.png)

现在设置为 10000000 实例所以每当一个新的节点添加到群集 Sysdig 将自动将代理部署到该新节点。 这是临时的解决方案，以确保 Sysdig 将部署到群集中的所有新的代理程序。 

![Sysdig 在 DC/OS 宇宙的实例的配置](./media/container-service-monitoring-sysdig/sysdig4.png)

一旦您已经安装了包向后定位到 Sysdig 用户界面，您将能够浏览您的群集中的容器的不同使用情况指标。 