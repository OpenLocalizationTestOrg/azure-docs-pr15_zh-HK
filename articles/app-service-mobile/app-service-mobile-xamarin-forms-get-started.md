<properties
    pageTitle="通过使用 Xamarin.Forms 开始使用移动应用程序"
    description="按照本教程中若要开始使用 Xamarin.Forms 开发的 Azure 移动应用程序"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>创建一个 Xamarin.Forms 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>概述

本教程展示如何使用 Azure 的移动应用程序的后端 Xamarin.Forms 移动应用程序添加一个基于云的后端服务。 您将创建新的移动应用程序后端和一个简单的_Todo 列表_在 Azure 存储应用程序数据的 Xamarin.Forms 应用程序。

学完本教程是为 Xamarin.Forms 的所有其他移动应用程序教程的先决条件。

##<a name="prerequisites"></a>系统必备组件

若要完成本教程，您需要以下各项︰

* 活动的 Azure 帐户。 如果您没有帐户，可以注册 Azure 试用并获得最多 10 自由移动应用程序，您可以继续使用您试用结束后甚至。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

* Xamarin 使用的 Visual Studio。 有关说明，请参阅[设置和安装的 Visual Studio 和 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) 。 

* 一个 Mac Xcode 7.0 版或更高版本和安装的 Xamarin Studio 社区。 请参阅[设置和 Visual Studio 和 Xamarin 的安装](https://msdn.microsoft.com/library/mt613162.aspx)[设置、 安装和 Mac 用户的验证](https://msdn.microsoft.com/library/mt488770.aspx)(MSDN)。
 
>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](https://tryappservice.azure.com/?appServiceName=mobile)，立即可以在此创建短期的初学者移动应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="create-a-new-azure-mobile-app-backend"></a>创建新 Azure 的移动应用程序的后端

请按照以下步骤创建新的移动应用程序后端。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


现在已设置可以通过您的移动客户端应用程序使用 Azure 的移动应用程序后端。 接下来，您将下载服务器项目"todo 列表"一个简单的后端，并将其发布到 Azure。

## <a name="configure-the-server-project"></a>配置服务器项目

请按照以下步骤来配置要使用的 Node.js 或.NET 的后端服务器项目。

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>下载并运行 Xamarin.Forms 解决方案

在此，您有以下两个选项。 可以下载到 Mac 的解决方案和 Xamarin Studio 中将其打开或下载到 Windows 计算机的解决方案，并在构建 iOS 应用程序使用网络的 Mac 的 Visual Studio 中打开它。 如果您需要 Xamarin 安装程序方案的详细的说明，请参阅[设置和安装的 Visual Studio 和 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) 。

让我们继续︰

 1. 您的 Mac 或 Windows 计算机上，请在浏览器窗口中打开[Azure 门户]。
 2. 在为您的移动应用程序设置刀片式服务器，请单击 （手机） 下的**入门**> **Xamarin.Forms**。 在步骤 3 中，单击**创建新的应用程序**，如果它还没有被选定。  接下来，单击**下载**按钮。

    此下载包含的客户端应用程序连接到您的移动应用程序的项目。 压缩的项目文件保存到本地计算机，并记下其保存的位置。

 3. 解压缩下载的该项目，然后在 Xamarin Studio 或 Visual Studio 中打开它。

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>（可选）运行 iOS 项目

本部分是用于运行 iOS 设备 Xamarin iOS 项目。 如果您不使用 iOS 设备，您可以跳过本节。

####<a name="in-xamarin-studio"></a>在 Xamarin Studio 中

1. IOS 项目中，用鼠标右键单击，然后单击**设为启动项目**。
2. 在**运行**菜单上，单击**开始调试**生成项目，然后启动该应用程序在 iPhone 模拟器。

####<a name="in-visual-studio"></a>在 Visual Studio 中
1. IOS 项目中，用鼠标右键单击，然后单击**设为启动项目**。
2. 单击**生成**菜单上的**配置管理器**。
3. 在**配置管理器**对话框中，选择 iOS 项目的**生成**和**部署**的复选框。
4. 按**F5**键以生成项目，然后启动该应用程序在 iPhone 模拟器。

    >[AZURE.NOTE] 如果您有问题的构建，运行 NuGet 程序包管理器和更新到最新版本的 Xamarin 支持包。 有时快速入门项目可能会滞后被更新为最新。    

在应用程序中，键入有意义的文本，如_学习 Xamarin_ ，然后单击**+**按钮。

![][10]

这将 POST 请求发送到 Azure 中承载新的移动应用程序后端。 从请求的数据插入到 TodoItem 表中。 通过移动应用程序的后端，返回项存储在表中，数据显示在列表中。

>[AZURE.NOTE]
> 您会发现访问您移动应用程序后端解决方案的可移植类库项目的 TodoItemManager.cs C# 文件中的代码。

##<a name="optional-run-the-android-project"></a>（可选）运行 Android 的项目

本部分是运行 Android Xamarin droid 项目。 如果您不使用 Android 设备，您可以跳过本节。

####<a name="in-xamarin-studio"></a>在 Xamarin Studio 中

1. Android 的项目中，用鼠标右键单击，然后单击**设为启动项目**。
2. 在**运行**菜单上单击**启动调试**生成项目并在 Android 模拟器中启动该应用程序。

####<a name="in-visual-studio"></a>在 Visual Studio 中
1. Android (Droid) 项目中，用鼠标右键单击，然后单击**设为启动项目**。
4. 单击**生成**菜单上的**配置管理器**。
5. 在**配置管理器**对话框中，选择 Android 项目的**生成**和**部署**的复选框。
6. 按**F5**键以生成项目并在 Android 模拟器中启动该应用程序。

    >[AZURE.NOTE] 如果您有问题的构建，运行 NuGet 程序包管理器和更新到最新版本的 Xamarin 支持包。 有时快速入门项目可能会滞后被更新为最新。    


在应用程序中，键入有意义的文本，如_学习 Xamarin_ ，然后单击**+**按钮。

![][11]

这将 POST 请求发送到 Azure 中承载新的移动应用程序后端。 从请求的数据插入到 TodoItem 表中。 通过移动应用程序的后端，返回项存储在表中，数据显示在列表中。

> [AZURE.NOTE]
> 您会发现访问您移动应用程序后端解决方案的可移植类库项目的 TodoItemManager.cs C# 文件中的代码。


##<a name="optional-run-the-windows-project"></a>（可选）运行 Windows 项目


本部分适用于运行 Windows 设备 Xamarin WinApp 项目。 如果您不使用 Windows 设备，您可以跳过本节。


####<a name="in-visual-studio"></a>在 Visual Studio 中
1. 用鼠标右键单击任何 Windows 项目，然后单击**设为启动项目**。
4. 单击**生成**菜单上的**配置管理器**。
5. 在**配置管理器**对话框中，选择所选的 Windows 项目**生成**和**部署**的复选框。
6. 按**F5**键以生成项目，然后在 Windows 仿真程序中启动该应用程序。

    >[AZURE.NOTE] 如果您有问题的构建，运行 NuGet 程序包管理器和更新到最新版本的 Xamarin 支持包。 有时快速入门项目可能会滞后被更新为最新。    


在应用程序中，键入有意义的文本，如_学习 Xamarin_ ，然后单击**+**按钮。

这将 POST 请求发送到 Azure 中承载新的移动应用程序后端。 从请求的数据插入到 TodoItem 表中。 通过移动应用程序的后端，返回项存储在表中，数据显示在列表中。

![][12]

> [AZURE.NOTE]
> 您会发现访问您移动应用程序后端解决方案的可移植类库项目的 TodoItemManager.cs C# 文件中的代码。

##<a name="next-steps"></a>下一步行动

* [添加到您的应用程序的身份验证](app-service-mobile-xamarin-forms-get-started-users.md)  
了解如何标识提供程序与应用程序的用户进行身份验证。

* [将推式通知添加到您的应用程序](app-service-mobile-xamarin-forms-get-started-push.md)  
了解如何添加的推送通知到您的应用程序支持和配置您的移动应用程序后端使用 Azure 通知集线器发送推式通知。

* [使您的应用程序的脱机同步](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  了解如何添加您使用移动应用程序的后端的应用程序的脱机支持。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。

* [如何使用 Azure 移动应用程序的托管客户端](app-service-mobile-dotnet-how-to-use-client-library.md)  
了解如何使用 Xamarin 应用程序中的托管客户端 SDK。 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure 门户]: https://portal.azure.com/

