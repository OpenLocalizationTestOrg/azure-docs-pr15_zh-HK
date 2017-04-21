<properties
    pageTitle="在 Azure 应用程序服务移动应用程序上创建一个 Cordova 应用程序 |Microsoft Azure"
    description="按照本教程中使用 Azure 的移动应用程序 backends 对 Apache Cordova 开发入门"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova，javascript，移动客户端" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>创建 Apache Cordova 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>概述

本教程展示如何使用 Azure 的移动应用程序的后端向 Apache Cordova 移动应用程序添加一个基于云的后端服务。  您将创建新的移动应用程序后端和一个简单的_Todo 列表_Apache Cordova 在 Azure 存储应用程序数据的应用程序。

学完本教程是在 Azure 应用程序服务使用移动应用程序功能有关的所有其他 Apache Cordova 教程的先决条件。

## <a name="prerequisites"></a>系统必备组件

若要完成本教程，您需要以下各项︰

* PC 与[Visual Studio 社区 2015年]或更高版本。
* [Apache Cordova visual Studio 工具]。
* [活动的 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)。

您还可能会绕过 Visual Studio 并直接使用 Apache Cordova 命令行。  在完成本教程在 Mac 计算机上的时，这非常有用。  本教程不包括 Apache Cordova 客户端应用程序使用命令行进行编译。

## <a name="create-a-new-azure-mobile-app-backend"></a>创建新 Azure 的移动应用程序的后端

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[观看视频显示相似的步骤](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>配置服务器项目

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>下载并运行 Apache Cordova 应用程序

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>下一步行动

现在，您已完成本快速入门教程，转到下面的教程之一︰

* [添加身份验证]您 Apache Cordova 应用程序。
* [添加推式通知]您 Apache Cordova 应用程序。

了解有关使用 Azure 应用程序服务的关键概念。

* [身份验证]
* [推式通知]

了解如何使用 Sdk。

* [Apache Cordova SDK]
* [ASP.NET 服务器 SDK]
* [Node.js SDK 服务器]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio 社区 2015]: http://www.visualstudio.com/
[Apache Cordova visual Studio 工具]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[添加身份验证]: app-service-mobile-cordova-get-started-users.md
[添加推式通知]: app-service-mobile-cordova-get-started-push.md
[身份验证]: app-service-mobile-auth.md
[推式通知]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET 服务器 SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js SDK 服务器]: app-service-mobile-node-backend-how-to-use-server-sdk.md
