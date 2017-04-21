<properties
    pageTitle="Azure 函数通知中心绑定 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用 Azure 通知中心绑定。"
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="azure 函数，函数、 事件处理、 动态计算、 无服务器体系结构"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure 函数通知中心输出绑定

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍了如何配置和 Azure 函数中的代码 Azure 通知中心绑定。 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

这些函数可以发送推式通知配置的 Azure 通知中心使用几行代码。 但是，Azure 通知中心必须配置为平台通知服务 （企业型） 要使用。 配置 Azure 通知中心和开发客户端应用程序的注册，以接收通知的详细信息，请参阅[入门通知集线器](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)和单击顶部您目标客户机平台。

您发送的通知可以是本机的通知模板。 本机的通知目标具体通知平台配置在`platform`的输出绑定的属性。 可以使用模板通知要面向多个平台。   

## <a name="notification-hub-output-binding-properties"></a>通知中心输出绑定属性

Function.json 文件提供了以下属性︰

- `name`︰ 在函数代码中使用的通知中心消息变量的名称。
- `type`︰ 必须设置为*"notificationHub"*。
- `tagExpression`︰ 标记表达式允许您指定通知传递到一组已注册接收通知与标记表达式匹配的设备。  有关详细信息，请参阅[路由和标记表达式](../notification-hubs/notification-hubs-tags-segment-push-message.md)。
- `hubName`︰ 在 Azure 门户的通知中心资源的名称。
- `connection`︰ 此连接字符串必须为通知中心的*DefaultFullSharedAccessSignature*值设置为**应用程序设置**连接字符串。
- `direction`︰ 必须设置为*"出"*。 
- `platform`︰ 该平台属性指示通知平台通知目标。 必须是以下值之一︰
    - `template`︰ 从输出绑定中省略平台属性这是缺省平台。 用于面向任何平台上 Azure 通知集线器配置模板的通知。 一般情况下使用模板发送跨平台 Azure 通知中心通知的详细信息，请参阅[模板](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)。
    - `apns`︰ 苹果推送通知服务。 配置详细信息 APNS 和在客户端应用程序中，一旦收到通知的通知中心请参阅[发送推式通知到 Azure 通知集线器的 Io](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`︰[消息 Amazon 设备](https://developer.amazon.com/device-messaging)。 配置通知中心的 ADM 和 Kindle 应用程序在接收到此通知的详细信息，请参阅[入门的 Kindle 应用程序通知集线器](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google 云消息](https://developers.google.com/cloud-messaging/)。 此外支持 firebase 云消息，即 GCM 的新版本。 GCM/FCM 和 Android 的客户端应用程序中，在接收到此通知的通知中心有关配置的详细信息请参阅[发送推式通知到 Android 使用 Azure 通知集线器](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`︰[窗口推送通知服务](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)面向 Windows 平台。 8.1 版及更高版本的 Windows Phone 还受 WNS 支持。 有关配置的详细信息 WNS 和通用 Windows 平台 (UWP) 应用程序中，在接收到此通知的通知中心请参见[通知集线器的 Windows 通用平台应用程序入门](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Microsoft 推送通知服务](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx)。 Windows Phone 8 和更早的 Windows Phone 平台，此平台支持。 有关配置的详细信息 MPNS 和在 Windows Phone 应用程序中，一旦收到通知的通知中心看到[与在 Windows Phone Azure 通知集线器发送推式通知](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
示例 function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>通知中心连接字符串设置

若要使用通知中心输出绑定，必须配置集线器的连接字符串。 进行这种*集成*选项卡上选择通知中心或创建一个新的。 

通过将连接字符串用于*DefaultFullSharedAccessSignature*添加到通知中心，可以同时手动添加现有集线器的连接字符串。 此连接字符串中提供您的函数访问权限发送通知消息。 可以从主刀片式服务器的通知中心资源在 Azure 门户中的**键**按钮访问*DefaultFullSharedAccessSignature*连接字符串值。 若要手动添加网络集线器的连接字符串，请使用以下步骤︰ 

1. 在 Azure 门户的**应用程序函数**刀片式服务器，请单击**函数的应用程序设置 > 转到应用程序服务设置**。

2. 在**设置**刀片式服务器，请单击**应用程序设置**。

3. 向下滚动到**连接字符串**部分，然后添加通知中心的*DefaultFullSharedAccessSignature*值的命名的项。 将类型更改为**自定义**。
4. 引用连接字符串的名称输出绑定中。 类似于上面的示例中使用的**MyHubConnectionString** 。

## <a name="native-notification-examples"></a>本机的通知示例

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>APNS 与 C# 队列触发器的本机通知

此示例演示如何使用[Microsoft Azure 通知集线器库](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)中定义的类型来发送本机 APNS 通知。 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM 本机 C# 队列触发器通知

此示例演示如何使用[Microsoft Azure 通知集线器库](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)中定义的类型来发送本机 GCM 通知。 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>与 C# 队列触发器 WNS 本机通知

此示例演示如何使用[Microsoft Azure 通知集线器库](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)中定义的类型来发送本机 WNS toast 通知。 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>模板通知示例

#### <a name="template-example-for-nodejs-timer-triggers"></a>Node.js 计时器触发器的模板示例 

本示例将发送包含[模板注册](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)通知`location`， `message`。

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>F# 计时器触发器的模板示例

本示例将发送包含[模板注册](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)通知`location`， `message`。

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>使用 out 参数的模板示例 

本示例将发送包含[模板注册](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)通知`message`在模板中的占位符。

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>使用异步函数模板示例

如果使用异步代码时，输出参数不允许。 在这种情况下使用`IAsyncCollector`返回模板通知。 下面的代码是代码的异步的上面示例。 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>使用 JSON 的模板示例 

本示例将发送包含[模板注册](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)通知`message`使用有效的 JSON 字符串的模板中的占位符。

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>使用通知集线器的库类型的模板示例

本示例演示如何使用[Microsoft Azure 通知集线器库](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)中定义的类型。 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
