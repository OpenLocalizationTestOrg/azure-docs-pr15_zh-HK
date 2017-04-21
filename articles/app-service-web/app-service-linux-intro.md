<properties 
    pageTitle="在 Linux 上的应用程序服务简介 |Microsoft Azure" 
    description="了解如何在 Linux 上的应用程序服务。" 
    keywords="azure 应用程序服务、 linux 操作系统"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>在 Linux 上的应用程序服务简介
在 Linux 上的应用程序服务正在公共预览和在 Linux 上以本机方式支持运行 web 应用程序。 

## <a name="overview"></a>概述 ##
客户可以使用应用程序服务在 Linux 上到宿主 web 应用程序在 Linux 上以本机方式支持的应用程序堆栈。 下面的功能部分列出了当前支持的应用程序堆栈。

## <a name="features"></a>功能 ##
在 Linux 上的应用程序服务目前支持下列应用程序堆栈

- Node.js
- PHP

客户可以部署自己的应用程序使用

- FTP。
- 本地 Git。
- GitHub 或 BitBucket。

应用程序扩展


- 客户可以通过更改其应用程序服务计划中的层向上和向下扩展他们的 web 应用程序。 
- 客户可以横向扩展其应用程序和运行其应用程序的多个实例范围内的 SKU。

Kudu 的一些基本功能将起作用

- 环境。
- 部署。
- 基本的控制台。

## <a name="limitations"></a>限制 ##

Azure 管理门户将只显示当前受支持的功能，在 Linux 上的应用程序服务，并隐藏其余部分。 为使更多的功能我们团队我们将保持反射这管理门户网站上。 一些功能，如 VNET 集成和 AAD/第三方身份验证或 Kudu 网站扩展当前无法工作。 但随着我们这些工作我们会更新我们的文档和更改有关的博客。

此公共预览目前只能用在以下地区

-   西部美国。
-   西部欧洲。
-   东南亚。

在 Linux 上的 web 应用程序只支持专用应用程序服务计划中并没有自由或共享层。 此外，针对常规和 Linux web 应用程序的应用程序服务计划是互相排斥，因此不能在非 Linux 应用程序服务计划中创建一个 Linux web 应用程序。

在 Linux 上的 web 应用程序必须创建不包含非 Linux 在同一区域中的 web 应用程序的资源组中。

由于缺乏重叠回收的 web 应用程序，客户应小的停机时间，在 web 应用程序被重新启动。 

## <a name="next-steps"></a>下一步行动 ##

请按照以下链接以开始使用 Linux 上的应用程序服务。 请在[我们的论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview)上发布问题和顾虑。

* [在 Linux 上的应用程序服务中创建 Web 应用程序](./app-service-linux-how-to-create-a-web-app.md)
* [用于在 Web 应用程序在 Linux 上的 Node.js PM2 配置](./app-service-linux-using-nodejs-pm2.md)

