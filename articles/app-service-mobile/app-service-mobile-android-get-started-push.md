<properties
    pageTitle="推式通知添加使用 Azure 的移动应用程序的 Android 应用程序"
    description="了解如何使用 Azure 移动应用程序到 Android 的应用程序发送推式通知。"
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Android 应用程序中添加推式通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>概述
在本教程中，您添加推式通知到[Android 快速启动]项目，以便每次插入记录到设备发送推式通知。

如果您不使用下载快速入门服务器项目，则需要推送通知扩展包。 有关详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

## <a name="prerequisites"></a>系统必备组件

您需要以下各项︰

* 这取决于项目的后端的 IDE:

    * [Android 的 Studio](https://developer.android.com/sdk/index.html)如果此应用程序具有 Node.js 后端。

    * [Visual Studio 社区 2013年](https://go.microsoft.com/fwLink/p/?LinkID=391934)或以后如果此应用程序具有.Net 后端。

* Android 2.3 或更高版本、 Google 资源库修订 27 或更高版本，以及 Google 播放服务 9.0.2 或更高版本的 Firebase 云消息。

* 完成[Android 快速入门]。

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>创建一个项目来支持 Firebase 云消息

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>配置 Azure 发送推式通知

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>启用推式通知的服务器项目

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>将推式通知添加到您的应用程序

在本节中，您将更新客户端 Android 应用程序来处理推式通知。

### <a name="verify-android-sdk-version"></a>验证 Android SDK 版本

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

下一步是安装 Google 播放服务。 Google 云消息有某些最低 API 级别要求开发和测试，该清单中的**minSdkVersion**属性必须符合。

如果您使用较旧的设备进行测试，然后咨询[设置了 Google 播放服务 SDK]来确定如何低可以设置此值，并进行适当设置。

### <a name="add-google-play-services-to-the-project"></a>向项目中添加 Google 播放服务

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>添加代码

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>测试针对手机信息发布服务应用程序

通过直接附加 Android 使用 USB 电缆，电话或通过在仿真程序中使用的虚拟设备，可以测试该应用程序。

## <a name="more"></a>更多

<!-- URLs -->
[Android 的快速入门]: app-service-mobile-android-get-started.md

[安装了 Google 播放服务 SDK]:https://developers.google.com/android/guides/setup
