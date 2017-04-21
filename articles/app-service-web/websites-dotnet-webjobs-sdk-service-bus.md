<properties 
    pageTitle="如何使用 WebJobs SDK Azure 服务总线" 
    description="了解如何使用 WebJobs SDK Azure 服务总线队列和主题。" 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>如何使用 WebJobs SDK Azure 服务总线

## <a name="overview"></a>概述

本指南提供了 C# 的代码示例演示如何接收到 Azure 服务总线消息时触发流程。 代码示例使用[WebJobs SDK](websites-dotnet-webjobs-sdk.md)版本 1.x。

该指南假定您知道[如何创建一个在 Visual Studio 中指向您的存储帐户连接字符串的 WebJob 项目](websites-dotnet-webjobs-sdk-get-started.md)。

代码段仅显示未创建的代码的函数`JobHost`对象如本示例所示︰

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

[服务总线的完整代码示例](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs)是在 GitHub.com azure webjobs sdk 示例存储库中。

## <a id="prerequisites"></a>系统必备组件

使用服务总线，您需要安装[Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet 程序包除了其他 WebJobs SDK 程序包。 

您还必须设置 AzureWebJobsServiceBus 连接字符串以及存储连接字符串。  你可以`connectionStrings`的 App.config 文件，如下面的示例中所示的部分︰

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

在 App.config 文件中包括的服务总线连接字符串设置的示例项目，请参阅[服务总线的示例](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus)。 

此外可以在 Azure 的运行时环境中，然后将 App.config 设置重写 WebJob Azure; 在运行时设置连接字符串有关详细信息，请参阅[开始使用 WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account)。

## <a id="trigger"></a>如何接收服务总线队列邮件时触发一个函数

若要编写一个函数，WebJobs SDK 调用接收队列邮件时，使用`ServiceBusTrigger`属性。 属性构造函数采用一个参数以指定轮询队列的名称。

### <a name="how-servicebustrigger-works"></a>ServiceBusTrigger 的工作原理

SDK 接收的邮件中`PeekLock`模式和调用`Complete`上的邮件时，如果此函数成功，完成或调用`Abandon`如果该功能失败。 如果函数运行超过`PeekLock`自动续订超时，该锁。

服务总线进行自己毒信队列处理无法控制或通过 WebJobs SDK 配置。 

### <a name="string-queue-message"></a>字符串队列消息

下面的代码示例读取队列消息包含一个字符串并将该字符串写入 WebJobs SDK 的仪表板。

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**注意︰**如果您正在创建队列消息的应用程序不使用 WebJobs SDK 中，请确保将[BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx)设置为"纯文本"。

### <a name="poco-queue-message"></a>POCO 队列消息

SDK 将自动反序列化包含 JSON POCO 队列消息[(普通旧 CLR 对象](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) 类型。 下面的代码示例读取队列消息包含`BlobInformation`对象具有`BlobName`属性︰

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

有关代码示例，显示如何使用 POCO 属性来使用 blob 和表中相同的功能，请参阅[存储队列版本的这篇文章](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs)。

如果您创建的队列消息的代码不使用 WebJobs SDK，使用类似于下面的示例代码︰

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>类型 ServiceBusTrigger 的合作

除了`string`POCO 类型，您可以使用`ServiceBusTrigger`属性使用一个字节数组或`BrokeredMessage`对象。

## <a id="create"></a>如何创建服务总线队列消息

若要编写一个函数来创建新的队列消息使用`ServiceBus`特性，并将在队列名称中传递给属性构造函数。 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>在非异步函数中创建单个队列消息

下面的代码示例使用输出参数创建一封新邮件中名为"outputqueue"具有相同的内容，作为在名为"inputqueue"的队列中接收消息的队列。

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

创建单个队列消息的输出参数可以是以下类型︰

* `string`
* `byte[]`
* `BrokeredMessage`
* 可序列化的 POCO 类型定义。 自动序列化为 JSON。

对于 POCO 类型参数，队列消息始终创建函数的结束;如果该参数为 null，SDK 创建的队列消息，消息是接收和反序列化时将返回空值。 对于其他类型，如果该参数为 null 被创建队列消息。

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>创建多个队列消息或异步函数

若要创建多个邮件，请使用`ServiceBus`特性与`ICollector<T>`或`IAsyncCollector<T>`，如下面的代码示例所示︰

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

立即创建每个队列消息时`Add`调用方法。

## <a id="topics"></a>如何使用服务总线主题

若要编写一个函数，SDK 调用服务总线主题上收到一条消息时，请使用`ServiceBusTrigger`采用主题名称和订阅名称，如下面的代码示例中所示的构造函数的属性︰

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

创建一封邮件的主题，使用`ServiceBus`使用主题名称相同的方式使用队列名称的属性。

## <a name="features-added-in-release-11"></a>在 1.1 版中的新增功能

在 1.1 版中添加了以下功能︰

* 允许深自定义消息处理通过的`ServiceBusConfiguration.MessagingProvider`。
* `MessagingProvider`支持自定义服务总线的`MessagingFactory`， `NamespaceManager`。
* A`MessageProcessor`战略模式允许您指定每个队列/主题的处理器。
* 默认情况下支持消息处理并发性。 
* 易于自定义`OnMessageOptions`通过`ServiceBusConfiguration.MessageOptions`。
* 允许在上指定[AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) `ServiceBusTriggerAttribute` / `ServiceBusAttribute` （的情况下，您可能不具有管理权限）。 

## <a id="queues"></a>通过存储队列操作方法文章所涉及的相关的主题

非特定于服务总线的 WebJobs SDK 方案有关的信息，请参阅[如何使用 WebJobs sdk 的 Azure 队列存储](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)。 

在这篇文章中所涉及的主题包括︰

* 异步函数
* 多个实例
* 正常关机
* 使用 WebJobs SDK 的函数体中的属性
* 在代码中设置 SDK 连接字符串
* 值为 WebJobs SDK 构造函数参数在代码中设置
* 手动触发函数
* 写入日志

## <a id="nextsteps"></a>下一步行动

本指南提供了代码示例来说明如何处理使用 Azure 服务总线的通用方案。 有关如何使用 Azure WebJobs 和 WebJobs SDK 的详细信息，请参阅[Azure WebJobs 推荐资源](http://go.microsoft.com/fwlink/?linkid=390226)。
 
