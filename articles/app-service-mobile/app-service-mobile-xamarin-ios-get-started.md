<properties
    pageTitle="Xamarin.iOS 应用程序的开始使用 Azure 应用程序服务移动应用程序 |Microsoft Azure"
    description="按照本教程中使用 Xamarin.iOS 开发的移动应用程序入门。"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>创建一个 Xamarin.iOS 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>概述

本教程展示如何使用 Azure 的移动应用程序的后端添加到 Xamarin.iOS 的移动应用程序的基于云的后端服务。  创建新的移动应用程序后端和一个简单的_Todo 列表_Xamarin.iOS 在 Azure 存储应用程序数据的应用程序。

学完本教程是在 Azure 应用程序服务使用移动应用程序功能有关的所有其他 Xamarin.iOS 教程的先决条件。

##<a name="prerequisites"></a>系统必备组件

若要完成本教程，您需要满足以下先决条件︰

* 活动的 Azure 帐户。 如果您没有帐户，注册 Azure 试用并获得最多 10 个免费的移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

* Xamarin 使用的 Visual Studio。 有关说明，请参阅[设置和安装的 Visual Studio 和 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) 。

* 一个 Mac Xcode 7.0 版或更高版本和安装的 Xamarin Studio 社区。 请参阅[设置和 Visual Studio 和 Xamarin 的安装](https://msdn.microsoft.com/library/mt613162.aspx)[设置、 安装和 Mac 用户的验证](https://msdn.microsoft.com/library/mt488770.aspx)(MSDN)。

>[AZURE.NOTE]如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](https://tryappservice.azure.com/?appServiceName=mobile)。 可以立即应用程序服务中创建短期的初学者移动应用程序 — 需要，没有信用卡，没有承诺。

## <a name="create-an-azure-mobile-app-backend"></a>创建 Azure 的移动应用程序的后端

请按照以下步骤创建移动应用程序的后端。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>配置服务器项目

现在已设置可以通过您的移动客户端应用程序使用 Azure 的移动应用程序后端。 接下来，下载服务器项目"todo 列表"一个简单的后端，并将其发布到 Azure。

请按照以下步骤来配置要使用的 Node.js 或.NET 的后端服务器项目。

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>下载并运行 Xamarin.iOS 的应用程序

1. 在浏览器窗口中打开[Azure 的门户]。

2. 在为您的移动应用程序设置刀片式服务器，请单击**开始** > **Xamarin.iOS**。 在步骤 3 中，单击**创建新的应用程序**，如果它还没有被选定。  接下来，单击**下载**按钮。

    将下载的客户端应用程序连接到您的移动后端。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

3. 提取已下载，该项目，然后将其打开 Xamarin Studio （或 Visual Studio） 中。

    ![][9]

    ![][8]

4. 按 F5 键以生成项目，然后启动该应用程序在 iPhone 模拟器。

5. 在应用程序中，键入有意义的文本，如_学习 Xamarin_，，然后单击**+**按钮。

    ![][10]

    从请求的数据插入到 TodoItem 表中。 通过移动应用程序的后端，返回项存储在表中，数据显示在列表中。

>[AZURE.NOTE]您可以查看访问您的移动应用程序后端查询并将数据插入 QSTodoService.cs C# 文件中的代码。

##<a name="next-steps"></a>下一步行动

* [添加到您的应用程序的脱机同步](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [添加到您的应用程序的身份验证](app-service-mobile-xamarin-ios-get-started-users.md)
* [向您的 Xamarin.Android 应用程序添加推式通知](app-service-mobile-xamarin-ios-get-started-push.md)
* [如何使用 Azure 移动应用程序的托管客户端](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure 门户]: https://portal.azure.com/
