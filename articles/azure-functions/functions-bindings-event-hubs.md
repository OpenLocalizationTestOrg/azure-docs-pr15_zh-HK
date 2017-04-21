<properties
    pageTitle="Azure 函数事件中心绑定 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用 Azure 事件中心绑定。"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure 函数事件中心绑定

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍了如何配置和代码[Azure 事件中心](../event-hubs/event-hubs-overview.md)Azure 函数绑定。 Azure 的函数对 Azure 事件集线器支持触发器和输出绑定。

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

如果您是新到 Azure 事件集线器，请参阅[Azure 事件中心概述](../event-hubs/event-hubs-overview.md)。

## <a name="azure-event-hub-trigger-binding"></a>Azure 事件中心触发绑定

Azure 事件中心触发器可用于响应事件发送到事件中心事件流。 您必须具有读取访问权限若要设置触发器绑定事件中心。

#### <a name="functionjson-for-event-hub-trigger-binding"></a>对于事件中心的 function.json 触发绑定

Azure 事件中心触发器的*function.json*文件中指定以下属性︰

- `type`︰ 必须设置为*eventHubTrigger*。
- `name`︰ 在函数代码中使用事件中心消息变量名。 
- `direction`︰ 必须设置为*中*。 
- `path`︰ 事件中心的名称。
- `consumerGroup`︰ 这是一个用来设置用于订阅事件使中心的[使用者组](../event-hubs-overview.md#consumer-groups)的可选属性。 如果省略，`$Default`使用的使用者组。 
- `connection`︰ 包含事件中心所在的命名空间的连接字符串的应用程序设置的名称。 通过单击**连接信息**按钮的命名空间，不事件中心本身复制此连接字符串。  此连接字符串必须至少具有读取权限才能激活触发器。

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure 事件中心触发器 C# 示例
 
使用上面的示例 function.json，事件消息的正文将记录使用 C# 函数下面的代码︰
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Azure 事件中心触发器 F# 示例

使用上面的示例 function.json，事件消息的正文将记录使用 F# 函数下面的代码︰

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure 事件中心触发器 Node.js 示例
 
使用上面的示例 function.json，事件消息的正文将记录下面的 Node.js 函数代码︰
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure 事件中心输出绑定

Azure 事件中心输出绑定用于将事件写入事件中心事件流。 您必须将事件写入事件中心的发送权限。 

#### <a name="functionjson-for-event-hub-output-binding"></a>对于事件中心的 function.json 输出绑定

Azure 事件中心的*function.json*文件输出绑定指定以下属性︰

- `type`︰ 必须设置为*eventHub*。
- `name`︰ 在函数代码中使用事件中心消息变量名。 
- `path`︰ 事件中心的名称。
- `connection`︰ 包含事件中心所在的命名空间的连接字符串的应用程序设置的名称。 通过单击**连接信息**按钮的命名空间，不事件中心本身复制此连接字符串。  此连接字符串必须具有将消息发送到事件中心流发送权限。
- `direction`︰ 必须给*出*设置。 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure 事件中心 C# 代码示例的输出绑定
 
下面 C# 函数的代码示例演示如何将事件写入事件中心事件流。 此事件中心输出绑定如上所示的示例表示应用到 C# 计时器触发。  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure 事件中心 F# 代码示例的输出绑定

下面 F# 函数的代码示例演示如何将事件写入事件中心事件流。 此事件中心输出绑定如上所示的示例表示应用到 C# 计时器触发。

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure 事件中心 Node.js 代码示例的输出绑定
 
下面 Node.js 函数的代码示例演示事件中心事件流中写入一个事件。 此事件中心输出绑定如上所示的示例表示应用 Node.js 计时器触发。  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
