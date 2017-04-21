在本节中，您在您现有的手机应用程序后端项目每次添加新项时发送推式通知更新代码。 这被采用通知集线器[模板](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)功能，允许跨平台推进。 推式通知，使用模板注册了各种客户端和一个通用推可以获取到所有客户端平台。

选择下面与您的后端项目类型相匹配的过程&mdash; [.NET 后端](#dotnet)或[Node.js 后端](#nodejs)。

### <a name="dotnet"></a>后端的.NET 项目
1. 在 Visual Studio 中，用鼠标右键单击该服务器项目并单击**管理 NuGet 程序包**，搜索`Microsoft.Azure.NotificationHubs`，然后单击**安装**。 这将安装通知集线器库从您的后端服务器发送通知。

3. 在服务器项目中，打开**控制器** > **TodoItemController.cs**，然后添加以下使用语句︰

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. 在**PostTodoItem**方法中，对**InsertAsync**的调用之后添加以下代码︰  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    这将发送包含的项的模板通知。当插入新项的文本。

4. 重新发布服务器项目。 

### <a name="nodejs"></a>Node.js 后端项目

1. 如果您还没有做的话[下载快速入门后端项目](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart)，否则使用[在线编辑器的 Azure 的门户](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)。

2. Todoitem.js 中的现有代码替换为以下︰

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    这将发送包含 item.text 时插入新项模板通知。

2. 在编辑本地计算机上的文件时，将重新发布服务器项目。
