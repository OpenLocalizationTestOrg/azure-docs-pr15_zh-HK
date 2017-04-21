<properties
    pageTitle="Azure 服务总线功能触发器和绑定 |Microsoft Azure"
    description="了解如何在 Azure 的函数中使用 Azure 服务总线触发器和绑定。"
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
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
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure 服务总线功能触发器和队列和主题的绑定

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍了如何配置和代码 Azure 服务总线触发器和 Azure 函数中的绑定。 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>服务总线队列或主题触发器

#### <a name="functionjson"></a>function.json

*Function.json*文件指定下列属性。

- `name`︰ 在函数代码中使用的队列或主题或队列或主题消息变量名。 
- `queueName`︰ 为队列会触发，轮询队列的名称。
- `topicName`︰ 为主题会触发，要轮询的主题的名称。
- `subscriptionName`︰ 为主题会触发，订阅名称。
- `connection`︰ 包含服务总线连接字符串的应用程序设置的名称。 连接字符串必须为服务总线命名空间中，不局限于特定的队列或主题。 如果连接字符串不具有管理权限，设置`accessRights`属性。 如果将`connection`为空或多个绑定将使用函数的应用程序，由 AzureWebJobsServiceBus 应用程序设置指定的默认服务总线连接字符串。
- `accessRights`︰ 指定用于连接字符串的访问权限。 默认值是`manage`。 设置为`listen`如果您正在使用一个连接字符串，并不提供管理权限。 否则可能会尝试运行时函数和无法执行要求的操作管理权限。
- `type`︰ 必须设置为*serviceBusTrigger*。
- `direction`︰ 必须设置为*中*。 

服务总线队列触发器示例*function.json* :

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>处理服务总线队列消息的 C# 代码示例

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>处理服务总线队列消息的 F# 代码示例

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>处理服务总线队列消息的 Node.js 代码示例

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>受支持的类型

服务总线队列消息可以反序列化到任何以下类型︰

* 对象 （从 JSON)
* 字符串
* 字节数组 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock 的行为

函数运行时收到一条消息在`PeekLock`模式和调用`Complete`上时，如果此函数成功完成的消息或调用`Abandon`如果该功能失败。 如果函数运行超过`PeekLock`自动续订超时，该锁。

#### <a id="sbpoison"></a>有害消息处理

服务总线实现自己有害消息处理的不能控制或配置在 Azure 功能配置或代码中。 

#### <a id="sbsinglethread"></a>单线程

默认情况下，函数运行时同时处理多个队列的消息。 若要直接运行时，若要一次处理一个队列或主题消息、 设置`serviceBus.maxConcurrrentCalls`到*host.json*文件中的 1。 *Host.json*文件的信息，请参阅中的开发人员参考文章和[host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) WebJobs.Script 资料库 wiki 中的[文件夹结构](functions-reference.md#folder-structure)。

## <a id="sboutput"></a>服务总线队列或主题输出绑定

#### <a name="functionjson"></a>function.json

*Function.json*文件指定下列属性。

- `name`︰ 在函数代码中使用的队列或队列消息变量名。 
- `queueName`︰ 为队列会触发，轮询队列的名称。
- `topicName`︰ 为主题会触发，要轮询的主题的名称。
- `subscriptionName`︰ 为主题会触发，订阅名称。
- `connection`︰ 与相同服务总线触发。
- `accessRights`︰ 指定用于连接字符串的访问权限。 默认值是`manage`。 设置为`send`如果您正在使用一个连接字符串，并不提供管理权限。 否则可能会尝试运行时函数和无法执行要求的操作管理权限，如创建队列。
- `type`︰ 必须设置为*serviceBus*。
- `direction`︰ 必须给*出*设置。 

使用计时器触发编写服务总线队列消息的示例*function.json* :

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>受支持的类型

Azure 函数可以从任何以下类型创建服务总线队列消息。

* 对象 （总是创建 JSON 消息，带有 null 对象创建邮件，如果值为 null，在函数结束后）
* 字符串 （如果值为非空值在函数结束后将产生一条消息）
* 字节数组 （像字符串一样有效） 
* `BrokeredMessage`(C#，运作字符串)

您可以使用 C# 函数中创建多个邮件，`ICollector<T>`或`IAsyncCollector<T>`。 创建一个邮件，当您调用`Add`方法。

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>创建服务总线队列消息的 C# 代码示例

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>创建服务总线队列消息的 F# 代码示例

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>创建服务总线队列消息的 Node.js 代码示例

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>下一步行动

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
