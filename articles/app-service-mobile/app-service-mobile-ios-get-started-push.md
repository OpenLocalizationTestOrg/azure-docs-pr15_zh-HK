<properties
    pageTitle="将推送通知添加到 iOS Azure 移动应用程序的应用程序"
    description="了解如何使用 Azure 移动应用程序可于 iOS 应用程序发送推式通知。"
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>添加到您的 iOS 应用程序推送通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>概述
在本教程中，您添加推式通知到[iOS 快速启动]项目，以便每次插入记录到设备发送推式通知。

如果您不使用下载快速入门服务器项目，将需要推送通知扩展包。 有关详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 。

[IOS 模拟器不支持推式通知](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html)。 您需要物理的 iOS 设备和一个[苹果开发商计划的成员资格](https://developer.apple.com/programs/ios/)。

##<a name="configure-hub"></a>配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>推式通知有关注册应用程序

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>配置 Azure 发送推式通知

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>更新后端发送推式通知

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>向应用程序添加推式通知

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>测试推式通知

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>更多

* 模板为您提供了灵活地发送跨平台推进和本地化的推进。 [如何使用 iOS Azure 移动应用程序的客户端库](app-service-mobile-ios-how-to-use-client-library.md#templates)将显示如何注册模板。

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS 快速入门]: app-service-mobile-ios-get-started.md
