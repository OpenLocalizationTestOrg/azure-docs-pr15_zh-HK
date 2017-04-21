<properties
    pageTitle="将推式通知添加到您的通用 Windows 平台 (UWP) 应用程序 |Azure 的移动应用程序"
    description="了解如何使用 Azure 应用程序服务移动应用程序和 Azure 通知集线器于通用 Windows 平台 (UWP) 应用程序发送推式通知。"
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>向您的 Windows 应用程序中添加推式通知

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>概述

在本教程中，您添加推式通知到[Windows 快速启动](app-service-mobile-windows-store-dotnet-get-started.md)项目，以便每次插入记录到设备发送推式通知。

如果您不使用下载快速入门服务器项目，将需要推送通知扩展包。 有关详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 。

##<a name="configure-hub"></a>配置通知中心

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>注册您的推送通知的应用程序

您需要提交到 Windows 应用商店，您的应用程序，然后配置您的服务器项目集成与 Windows 通知服务 (WNS) 发送推式。

1. 在 Visual Studio 解决方案资源管理器中，右击 UWP 应用程序项目，单击**存储** > **相关联的应用程序与存储...**。 

    ![将应用程序与 Windows 存储关联](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. 在向导中，单击**下一步**，使用您的 Microsoft 帐户登录、**保留新的应用程序名称**，键入您的应用程序的名称，然后单击**保留**。

3. 成功创建的应用程序注册后，选择新的应用程序名称，请单击**下一步**，，然后单击**关联**。 将所需的 Windows 应用商店注册信息添加到应用程序清单。  

7. 导航到[Windows 开发人员中心](https://dev.windows.com/en-us/overview)，登录与您的 Microsoft 帐户，请单击新的应用程序注册，在**我的应用程序**，然后展开**服务** > **推式通知**。 

8. 在**推式通知**页上，单击**Microsoft Azure 移动服务**下的**Live 服务网站**。

9. 在注册页上，记下**应用程序机密**和**包 SID**的下一步将用于配置移动应用程序端的值。 

    ![将应用程序与 Windows 存储关联](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] 客户的机密和包 SID 是重要的安全凭据。 不要与任何人共享这些值或将其与您的应用程序一起分发。 **应用程序 Id**与密码用于配置 Microsoft 客户身份验证。

##<a name="configure-the-backend-to-send-push-notifications"></a>配置后端发送推式通知

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>更新服务器发送推式通知

使用下面的过程与后端项目类型&mdash; [.NET 后端](#dotnet)或[Node.js 后端](#nodejs)。

### <a name="dotnet"></a>后端的.NET 项目

1. 在 Visual Studio 中，用鼠标右键单击该服务器项目**管理 NuGet 程序包**、 搜索 Microsoft.Azure.NotificationHubs，然后单击**安装**。 这将安装集线器通知客户端库。

2. 展开**控制器**，请打开 TodoItemController.cs，然后添加以下使用语句︰

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. 在**PostTodoItem**方法中，对**InsertAsync**的调用之后添加以下代码︰

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    此代码指示通知的集线器，以插入一个新项目之后，将发送推式通知。

4. 重新发布服务器项目。

### <a name="nodejs"></a>Node.js 后端项目

1. 如果您还没有做的话[下载快速启动项目](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart)，否则使用[在线编辑器的 Azure 的门户](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)。

2. 在 todoitem.js 文件中的现有代码替换为以下︰

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    当插入新的 todo 项包含 item.text 的 WNS toast 通知将发送。

2. 在编辑本地计算机上的文件时，将重新发布服务器项目。

##<a id="update-app"></a>将推式通知添加到您的应用程序

接下来，您的应用程序必须注册启动时的推式通知。 当您已经启用身份验证，请确保，用户迹象中尝试注册的推式通知之前。

1. 打开**App.xaml.cs**项目文件，然后添加以下`using`语句︰

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. 在同一文件中，对**应用程序**类中添加下面的**InitNotificationsAsync**方法定义︰

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    此代码从 WNS，检索应用程序 ChannelURI，然后您的应用程序服务移动的应用程序中注册的 ChannelURI。

3. 在**App.xaml.cs**的**OnLaunched**事件处理程序的顶部，将**异步**修饰符添加到方法定义并添加以下调用到新的**InitNotificationsAsync**方法，如以下示例所示︰

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    这可以保证短期 ChannelURI 已注册，每次启动应用程序。

4. 重新生成应用程序项目，UWP。 您的应用程序现在就可以收到 toast 通知。

##<a id="test"></a>测试您的应用程序中的推式通知

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>下一步行动

了解有关推式通知的详细信息︰

* [如何使用 Azure 移动应用程序的托管客户端](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
模板为您提供了灵活地发送跨平台推进和本地化的推进。 了解如何注册模板。

* [推式通知问题诊断](../notification-hubs/notification-hubs-push-notification-fixer.md)  
有通知可能会被丢弃或没有停止在设备上的各种原因。 本主题演示如何分析和计算出的推式通知失败的根本原因。 

请考虑继续下面的教程之一︰

+ [添加到您的应用程序的身份验证](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  了解如何标识提供程序与应用程序的用户进行身份验证。

+ [使您的应用程序的脱机同步](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  了解如何添加您使用移动应用程序的后端的应用程序的脱机支持。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

