<properties
    pageTitle="Azure 堆栈的 Web 应用程序概述 |Microsoft Azure"
    description="在 Azure 堆栈中的 Web 应用程序的概述"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Azure 堆栈 Web 应用程序概述
    
> [AZURE.NOTE] 以下信息只适用于 Azure 堆栈 TP1 部署中。

Azure 堆栈 Web 应用程序是 Azure 应用程序服务产品进入 Azure 堆栈的第一个元素。 Azure 堆栈 Web 应用程序安装程序将创建五种所需的角色类型的每个实例，还将创建一个文件服务器。 尽管您可以为每种角色类型添加更多实例，请记住没有技术预览 1 中的虚拟机所需的空间大小。 Azure 堆栈 Web 应用程序的当前功能的主要基础来管理系统和主机 web 应用程序所需的功能。

![在 Azure 的 azure 堆栈的应用程序服务 Web 应用程序堆栈门户][1]

## <a name="limitations-of-the-technical-preview"></a>技术预览的限制

没有支持的 Azure 堆栈的应用程序服务的预览版本。 不要将此预览版本放生产工作负载。 此外，还有无 Azure 堆栈的应用程序服务预览版本之间的升级。 这些预览版本的主要目的是要显示我们提供并获得反馈。 

## <a name="what-is-an-app-service-plan"></a>什么是应用程序服务计划？

Azure 堆栈 Web 应用程序资源提供程序使用 Azure 应用程序服务中的 Azure Web 应用程序功能使用相同的代码。 因此，一些常见的概念，是值得描述。 在 Web 应用程序，web 应用程序的定价容器称为应用程序服务计划。 它表示的一套专门用来存放您的应用程序的虚拟机。 在给定的订阅，您可以有多个应用程序服务计划。 这也是在 Azure 堆栈 Web 应用程序中，则返回 true。 

在 Azure，有一些共享的和专用的工作人员。 共享工作人员支持高密度多租户 web 应用程序托管，没有一套共享的工作人员。 专用的服务器只由一个租户并有三种大小︰ 小、 中型和大型。 始终不能使用这些条款所述内部客户的需求。 在 Azure 堆栈 Web 应用程序，资源提供者管理员可以定义他们想要使他们有多组共享工作人员或不同的专门工作人员根据他们唯一的 web 托管需求集可用的辅助层。 使用这些辅助层定义，它们可以定义自己的定价 Sku。

## <a name="portal-features"></a>门户的功能

也会发生在后端，Azure 堆栈 Web 应用程序将使用 Azure Web 应用程序使用同一个用户界面。 某些功能处于禁用状态，还没有准备好在 Azure 堆栈功能。 这是因为 Azure 特有的期望或这些功能需要的服务还没有准备好在 Azure 堆栈中可用。 

## <a name="next-steps"></a>下一步行动

- [在您开始使用 Azure 堆栈 Web 应用程序之前](azure-stack-webapps-before-you-get-started.md)
- [安装 Web 应用程序资源提供程序](azure-stack-webapps-deploy.md)

您还可以尝试其他[平台即服务 (PaaS) 服务](azure-stack-tools-paas-services.md)，如[SQL Server 资源提供者](azure-stack-sql-rp-deploy-short.md)和[MySQL 资源提供程序](azure-stack-mysql-rp-deploy-short.md)。

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
