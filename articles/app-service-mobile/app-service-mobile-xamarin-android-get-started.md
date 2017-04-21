<properties
    pageTitle="开始使用 Azure 的 Xamarin.Android 应用程序的移动应用程序"
    description="请按照 Xamarin Android 开发使用 Azure 移动应用程序开始在本教程"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>创建一个 Xamarin.Android 应用程序

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>概述

本教程展示如何为 Xamarin.Android 应用程序添加基于云的后端服务。 有关详细信息，请参阅[什么是移动应用程序](app-service-mobile-value-prop.md)。

下面是从已完成的应用程序的一个屏幕快照︰

![][0]

学完本教程是为 Xamarin.Android 应用程序的所有其他移动应用程序教程的先决条件。

##<a name="prerequisites"></a>系统必备组件

若要完成本教程，您需要满足以下先决条件︰

* 活动的 Azure 帐户。 如果您没有注册试用 Azure 并且获得最多 10 个免费的手机应用程序的帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

* Xamarin 使用的 Visual Studio。 有关说明，请参阅[设置和安装的 Visual Studio 和 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) 。

>[AZURE.NOTE]如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](https://tryappservice.azure.com/?appServiceName=mobile)。  您立即可以应用程序服务中创建短期初学者移动应用程序。 没有信用卡，所需;没有承诺。

## <a name="create-an-azure-mobile-app-backend"></a>创建 Azure 的移动应用程序的后端

请按照以下步骤创建移动应用程序的后端。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

现在已设置可以通过您的移动客户端应用程序使用 Azure 的移动应用程序后端。 接下来，下载服务器项目"todo 列表"一个简单的后端，并将其发布到 Azure。

## <a name="configure-the-server-project"></a>配置服务器项目

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>下载并运行 Xamarin.Android 的应用程序

1. 在**下载并运行 Xamarin.Android 项目**，请单击**下载**按钮。

    压缩的项目文件保存到本地计算机，并记下其保存的位置。

2. 按**F5**键以生成项目，然后启动应用程序。

3. 在应用程序中，键入有意义的文本，例如_完成本教程_，然后单击**添加**按钮。

    ![][10]

    从请求的数据插入到 TodoItem 表中。 在表中存储的项目由移动应用程序的后端，和出现在列表中的数据。

    > [AZURE.NOTE] 您可以查看您的移动应用程序后端来查询、 插入 ToDoActivity.cs C# 文件中找到的数据访问的代码。

##<a name="next-steps"></a>下一步行动

* [添加到您的应用程序的脱机同步](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [添加到您的应用程序的身份验证](app-service-mobile-xamarin-android-get-started-users.md)
* [向您的 Xamarin.Android 应用程序添加推式通知](app-service-mobile-xamarin-android-get-started-push.md)
* [如何使用 Azure 移动应用程序的托管客户端](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
