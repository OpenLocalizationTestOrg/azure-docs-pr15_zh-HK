<properties
   pageTitle="监视的 Azure 容器服务群集，Datadog |Microsoft Azure"
   description="监视与 Datadog Azure 容器服务群集。 使用 DC/OS web 用户界面将 Datadog 代理部署到您的群集。"
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="DC/OS Docker 蜂群 Azure 的容器"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>与 Datadog Azure 容器服务群集监视器

在这篇文章中我们会将 Datadog 代理部署到 Azure 容器服务群集中的所有代理节点。 对于此配置，您将需要具有 Datadog 的帐户。 

## <a name="prerequisites"></a>系统必备组件 

[部署](container-service-deployment.md)并[连接](container-service-connect.md)群集配置的 Azure 容器服务。 探讨[马拉松 UI](container-service-mesos-marathon-ui.md)。 转到[http://datadoghq.com](http://datadoghq.com)来设置 Datadog 帐户。 

## <a name="datadog"></a>Datadog 

Datadog 是从 Azure 容器服务群集内的容器，您收集监控数据监视服务。 Datadog 有一个 Docker 集成仪表板，您可以在您的容器内看到的具体标准。 从您的容器收集度量标准组织的 CPU、 内存、 网络和 I/O。 Datadog 将指标分为容器和图像。 下面是一种用户界面如下所示的 CPU 使用率。

![Datadog 用户界面](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Datadog 将部署配置为使用马拉松比赛

这些步骤将向您介绍如何配置和 Datadog 应用程序部署到您的群集的马拉松比赛。 

访问 DC/OS 用户界面通过[http://localhost:80 /](http://localhost:80/)。 一次 DC/OS 用户界面中导航到"世界"在左下角，然后搜索"Datadog"并单击"安装"。

![Datadog 包中的 DC/OS 宇宙](./media/container-service-monitoring/datadog1.png)

现在要完成配置，您需要的 Datadog 帐户或免费的试用帐户。 一旦您正在登录到 Datadog 网站外观的左侧并转到集成-> 然后 API。 

![Datadog API 密钥](./media/container-service-monitoring/datadog2.png)

接下来进入 DC/OS 宇宙内的 Datadog 配置 API 密钥。 

![DC/OS 世界中的 Datadog 配置](./media/container-service-monitoring/datadog3.png) 

在上述配置实例设置为 10000000，每当一个新的节点添加到群集 Datadog 会自动将代理部署到该节点。 这是一个临时解决方案。 一旦您已经安装了的程序包应该向后定位到 Datadog 网站并查找"仪表板"。 从那里，您会看到自定义和集成仪表板。 Docker 集成仪表板将拥有所需的监视群集中的所有容器指标。 
