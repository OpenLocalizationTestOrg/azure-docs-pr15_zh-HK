<properties
    pageTitle="创建在移动应用程序使用通用 Windows 平台 (UWP) |Microsoft Azure"
    description="按照本教程中使用 Azure 的移动应用程序 backends C#、 Visual Basic 或 JavaScript 中的通用 Windows 平台 (UWP) 应用程序开发的入门。"
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>创建一个 Windows 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>概述

本教程演示了如何将基于云的后端服务添加到通用 Windows 平台 (UWP) 应用程序。 有关详细信息，请参阅[什么是移动应用程序](app-service-mobile-value-prop.md)。 从已完成的应用程序的屏幕捕获如下︰

![已完成的桌面应用程序](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
在台式机上运行。 

![已完成的电话应用程序](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
在手机上运行

学完本教程是为 UWP 应用程序的所有其他移动应用程序教程的先决条件。 

##<a name="prerequisites"></a>系统必备组件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 的试验，并获得最多 10 个自由移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

* [Visual Studio 社区 2015年]或更高版本。

>[AZURE.NOTE] 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](https://tryappservice.azure.com/?appServiceName=mobile)。 那里，您立即可以应用程序服务中创建短期的初学者移动应用程序 — 需要，没有信用卡，没有承诺。

##<a name="create-a-new-azure-mobile-app-backend"></a>创建新 Azure 的移动应用程序的后端

请按照以下步骤创建新的移动应用程序后端。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

现在已设置可以通过您的移动客户端应用程序使用 Azure 的移动应用程序后端。 接下来，您将下载服务器项目"todo 列表"一个简单的后端，并将其发布到 Azure。

## <a name="configure-the-server-project"></a>配置服务器项目

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>下载并运行客户端项目

一旦您移动应用程序的后端配置，您可以创建新的客户端应用程序或修改现有应用程序连接到 Azure。 在本节中，您将下载自定义连接到您的移动应用程序后端的 UWP 应用程序模板项目。

1. 在您移动应用程序的后端的**快速入门**刀片式服务器，请单击**创建新应用程序** > **下载**，然后解压缩压缩的项目文件复制到本地计算机。

    ![下载 Windows 快速启动项目](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. （可选）将 UWP 应用程序项目添加到服务器项目的同一个解决方案中。 这使得它容易调试和测试应用程序和后端在相同的 Visual Studio 解决方案中，如果您选择这样做。 要将 UWP 应用程序项目添加到解决方案中，您必须使用 Visual Studio 2015年或更高版本。

4. 作为启动项目的 UWP 应用程序，按 F5 键来部署和运行应用程序。

5. 在应用程序中，**将 TodoItem 插入**的文本框中键入有意义的文本，例如，*完成本教程*，，然后单击**保存**。

    ![Windows 快速入门完整桌面](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    这将 POST 请求发送到位于 Azure 中的新的移动应用程序后端。

6. （可选）停止应用程序并重新启动它在不同的设备或移动仿真器上。

    ![Windows 快速入门完整的电话](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    请注意，在 UWP 应用程序启动后从 Azure 加载上一步中保存的数据。 

##<a name="next-steps"></a>下一步行动

* [添加到您的应用程序的身份验证](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  了解如何标识提供程序与应用程序的用户进行身份验证。

* [将推式通知添加到您的应用程序](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  了解如何添加的推送通知到您的应用程序支持和配置您的移动应用程序后端使用 Azure 通知集线器发送推式通知。

* [使您的应用程序的脱机同步](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  了解如何添加您使用移动应用程序的后端的应用程序的脱机支持。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio 社区 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
