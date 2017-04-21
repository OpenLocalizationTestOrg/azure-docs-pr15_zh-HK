<properties
   pageTitle="通过 web 用户界面的 azure 容器服务容器管理 |Microsoft Azure"
   description="部署到 Azure 容器服务群集服务通过马拉松 web UI 容器。"
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
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>通过 web 用户界面的容器管理

DC/OS 提供了部署和调整聚集的工作负载，同时抽象底层硬件的环境。 在 DC/OS，没有管理计划和执行计算的工作负载的框架。

框架可用于许多常见的工作负载时，本文将介绍如何创建和扩展容器部署与马拉松比赛。 这些示例之前，您将需要的 DC/OS 群集的 Azure 容器服务中配置。 您还需要具有远程连接到该群集。 这些项目的详细信息，请参阅下列文章︰

- [部署群集 Azure 容器服务](container-service-deployment.md)
- [连接到 Azure 容器服务群集](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>研究直流/OS 用户界面

用建立的安全外壳协议 (SSH) 隧道，浏览到 http://localhost/。 这将加载 DC/OS web 用户界面并显示有关群集，如使用的资源、 活动代理和正在运行的服务的信息。

![DC/OS 用户界面](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>研究马拉松 UI

要看到马拉松 UI，请浏览到 http://localhost/Marathon。 在此屏幕中，可以在 Azure 容器服务 DC/OS 群集中启动一个新的容器或另一个应用程序。 您还可以查看有关运行容器和应用程序的信息。  

![马拉松比赛 UI](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>部署 Docker 格式化的容器

要通过马拉松比赛部署一个新的容器，单击**创建应用程序**按钮和窗体中输入以下信息︰

字段           | 值
----------------|-----------
标识              | nginx
图像           | nginx
网络         | 桥接
主机端口       | 80
协议        | TCP

![新应用程序 UI-常规](media/dcos/dcos4.png)

![新的应用程序用户界面 — Docker 容器](media/dcos/dcos5.png)

![新的应用程序用户界面-端口和服务发现](media/dcos/dcos6.png)

如果希望以静态映射到代理上的端口的容器端口，您需要使用 JSON 模式。 为此，请在新应用程序向导使用切换到**JSON 模式**切换。 然后输入下`portMappings`应用程序定义的部分。 此示例绑定到端口 80 的 DC/OS 代理端口 80 的容器。 进行完此更改后，您可以切换从 JSON 模式此向导。

```none
"hostPort": 80,
```

![新的应用程序用户界面 — 80 端口的示例](media/dcos/dcos13.png)

DC/OS 群集组的专用和公用代理与部署。 可以从 Internet 访问应用程序的群集，您需要应用程序部署到一个公用的代理。 为此，请选择新建应用程序向导的**可选**选项卡和**slave_public**输入**接受资源角色**。

![新应用程序 UI-公用代理设置](media/dcos/dcos14.png)

在马拉松比赛主页面中，您可以看到容器的部署状态。

![马拉松比赛主页面 UI-容器部署状态](media/dcos/dcos7.png)

在切换回 DC/OS web 用户界面 (http://localhost/) 时，您会看到 （在此例中为 Docker 格式化的容器） 的任务运行于 DC/OS 群集。

![DC/OS web 用户界面-在群集上运行的任务](media/dcos/dcos8.png)

您还可以查看任务运行在群集节点。

![DC/OS web 用户界面 — 任务群集节点](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>扩展您的容器

马拉松比赛用户界面可用于扩展容器的实例计数。 若要执行此操作，导航到**马拉松**页上，选择容器，您想要扩展，单击**小数位数**按钮。 在**缩放应用程序**对话框中，输入所需的容器实例数并选择**缩放的应用程序**。

![马拉松比赛 UI-缩放应用程序对话框](media/dcos/dcos10.png)

缩放操作完成后，您将看到相同的任务分布在 DC/OS 代理的多个实例。

![DC/OS web UI 仪表板--任务代理之间的跨页](media/dcos/dcos11.png)

![DC/OS web 用户界面 — 节点](media/dcos/dcos12.png)

## <a name="next-steps"></a>下一步行动

- [使用 DC/OS 和马拉松 API](container-service-mesos-marathon-rest.md)

在使用 Mesos 的 Azure 容器服务深入探究

> [AZURE。视频] azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos]
