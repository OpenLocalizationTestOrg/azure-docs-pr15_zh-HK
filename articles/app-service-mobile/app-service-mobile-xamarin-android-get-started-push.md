<properties
    pageTitle="将推式通知添加到您的 Xamarin.Android 应用程序 |Azure 应用程序服务"
    description="了解如何使用 Azure 应用程序服务和 Azure 通知集线器于 Xamarin.Android 应用程序发送推式通知"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>向您的 Xamarin.Android 应用程序添加推式通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>概述


在本教程中，您添加推式通知到[Xamarin.Android 快速启动](app-service-mobile-windows-store-dotnet-get-started.md)项目，以便每次插入记录到设备发送推式通知。

如果您不使用下载快速入门服务器项目，将需要推送通知扩展包。 有关详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 。


##<a name="prerequisites"></a>系统必备组件

本教程要求如下︰

+ 活动的 Google 帐户。 您可以在[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302)的 Google 帐户注册。
+ [Google 云消息传递客户端组件](http://components.xamarin.com/view/GCMClient/)。

##<a name="configure-hub"></a>配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>启用 Firebase 云消息

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>配置 Azure 发送推送请求

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>更新发送推式通知的服务器项目

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>推式通知的客户端项目配置

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>将推式通知代码添加到您的应用程序

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>测试您的应用程序中的推式通知

您可以测试该应用程序在仿真程序中使用的虚拟设备。 有在仿真器上运行时，需要额外的配置步骤。

1. 请确保您正在部署到或调试上了 Google Api 集作为目标，如下所示在 Android 虚拟设备 (AVD) 管理器的虚拟设备。

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. 通过单击**应用程序**到 Android 设备添加 Google 帐户 > **设置** > **添加帐户**，然后按照提示进行操作。

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. 运行任务列表应用程序作为之前，插入一个新的 todo 项。 这一次，是在通知区域显示一个通知图标。 您可以打开通知抽屉查看通知的全文。

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
