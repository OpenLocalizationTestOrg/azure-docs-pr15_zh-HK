<properties
    pageTitle="将推式通知添加到您的 Xamarin.iOS 应用程序与 Azure 应用程序服务"
    description="了解如何使用 Azure 应用程序服务于 Xamarin.iOS 应用程序发送推式通知"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>向您的 Xamarin.iOS 应用程序添加推式通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>概述

在本教程中，您添加推式通知到[Xamarin.iOS 快速启动](app-service-mobile-xamarin-ios-get-started.md)项目，以便每次插入记录到设备发送推式通知。

如果您不使用下载快速入门服务器项目，将需要推送通知扩展包。 有关详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 。

##<a name="prerequisites"></a>系统必备组件

* 完成[Xamarin.iOS 快速入门](app-service-mobile-xamarin-ios-get-started.md)教程。

* 一种物理的 iOS 设备。 IOS 模拟器不支持推式通知。

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>注册为苹果的开发人员门户网站上的推式通知应用程序

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>配置您的移动应用程序发送推式通知

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>更新发送推式通知的服务器项目

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>对 Xamarin.iOS 项目进行配置

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>将推式通知添加到您的应用程序

1. 在**QSTodoService**，以便**AppDelegate**可以获取移动客户端添加以下属性︰

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. 添加以下`using`到**AppDelegate.cs**文件的开头的语句。

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. 在**AppDelegate**，重写**FinishedLaunching**事件︰

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. 在同一文件中，重写**RegisteredForRemoteNotifications**事件。 在此代码中您正在注册的简单模板通知，将由服务器发送所有受支持的平台上。

    带有通知集线器的模板的详细信息，请参阅[模板](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)。


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. 然后，重写**DidReceivedRemoteNotification**事件︰

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

现在，您的应用程序将更新以支持推送通知。

## <a name="test"></a>测试您的应用程序中的推式通知

1. 按**运行**按钮以生成项目并在 iOS 设备，启动应用程序，然后单击**确定**以接受推式通知。

    > [AZURE.NOTE] 从您的应用程序，您必须显式地接受推式通知。 此请求仅发生第一次应用程序运行。

2. 在应用程序中，键入任务，然后单击加号 (**+**) 图标。

3. 请验证通知收到后，然后单击**确定**以关闭通知。

4. 重复步骤 2，并立即关闭应用程序，然后验证显示通知。

您已成功完成本教程。

<!-- Images. -->

<!-- URLs. -->



