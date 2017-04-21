<properties 
    pageTitle="如何将事件记录到 Azure 事件集线器在 Azure API 管理 |Microsoft Azure" 
    description="了解如何将事件记录到 Azure 事件集线器在 Azure API 管理。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>如何将事件记录到 Azure API 管理 Azure 事件集线器

Azure 事件集线器是一种高度可扩展的数据入口服务，可以摄取数以百万计，每秒钟的事件，以便您可以处理和分析大量数据连接的设备和应用程序所产生。 事件集线器充当事件管道，"前门"，一旦数据被收集在一个事件集线器，它可以转换并存储使用任何实时分析提供程序或批处理存储适配器。 这样的事件使用者可以访问他们自己的日程安排上的事件，事件集线器将生产从这些事件中，消耗的事件流的分离。

本文[采用事件中枢集成 Azure 的 API 管理](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/)视频一起并描述如何使用 Azure 事件集线器的 API 管理事件日志。

## <a name="create-an-azure-event-hub"></a>创建一个 Azure 事件集线器

若要创建新的事件中心，登录到[Azure 的传统门户网站](https://manage.windowsazure.com)，并单击**新建**->**应用程序服务**->**服务总线**->**事件中心**->**快速创建**。 输入事件中心的名称，选择订阅，地区和选择一个命名空间。 如果您以前没有创建一个命名空间，则可以创建一个在**Namespace**文本框中键入一个名称。 一旦配置了所有的属性，请单击**创建新的事件中心**创建事件中心。

![创建事件中心][create-event-hub]

下一步，为您的新事件中心导航到**配置**选项卡并创建两个**共享的访问策略**。 第一个**发送**名称并将其**发送**权限。

![发送策略][sending-policy]

命名第二个**接收**、 授予它**侦听**的权限，并单击**保存**。

![接收的策略][receiving-policy]

每个共享的访问策略允许应用程序发送和接收事件和事件中心。 要访问这些策略的连接字符串，请导航到事件中心的**仪表板**选项卡上，单击**连接信息**。

![连接字符串][event-hub-dashboard]

当记录事件时，用于**发送**连接字符串和**接收**连接字符串用于从事件中心下载事件时。

![连接字符串][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>创建 API 管理记录器

现在，您有事件中心下, 一步是将[记录器](https://msdn.microsoft.com/library/azure/mt592020.aspx)配置 API 管理服务中，以便它可以将事件记录到事件中心。

使用[API 管理 REST API](http://aka.ms/smapi)来配置 API 管理记录器。 之前第一次使用 REST API，查看[系统必备组件](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites)并确保已[启用对 REST API 的访问](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI)。

若要创建记录器，请使用以下 URL 模板将 HTTP 请求。

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   更换`{your service}`API 管理服务实例的名称。
-   更换`{new logger name}`为您新记录器的所需名称。 配置[日志到 eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)策略时，将引用此名称

将以下标头添加到请求中。

-   内容类型︰ 应用程序/json
-   授权︰ SharedAccessSignature uid =...
    -   有关生成`SharedAccessSignature` [Azure API 管理 REST API 身份验证](https://msdn.microsoft.com/library/azure/dn798668.aspx)，请参阅。

指定请求正文使用以下模板。

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`必须设置为`AzureEventHub`。
-   `description`提供记录器的可选说明，如果需要，可以是一个零长度字符串。
-   `credentials`包含`name`和`connectionString`的 Azure 事件网络集线器。

当您进行请求，如果记录器创建状态代码`201 Created`将返回。 

>[AZURE.NOTE] 对于其他可能的返回代码和它们的原因，请参阅[创建记录器](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT)。 若要查看如何执行其他操作，例如列表、 更新和删除，请参阅[记录器](https://msdn.microsoft.com/library/azure/mt592020.aspx)实体文档。

## <a name="configure-log-to-eventhubs-policies"></a>配置日志到 eventhubs 策略

一旦您记录器配置 API 管理中，您可以配置日志到 eventhubs 策略来记录所需的事件。 在入站的策略一节或出站策略部分可用日志到 eventhubs 策略。

配置策略，登录到[Azure 的传统门户网站](https://manage.windowsazure.com)，定位您的 API 管理服务，并单击**发布服务器入口**或**管理**访问该发布服务器的门户。

![出版商门户][publisher-portal]

左侧的 API 管理菜单中单击**策略**，选择所需的产品和 API，并单击**添加策略**。 在此示例中我们将**回显 API** **限制**产品中添加策略。

![添加策略][add-policy]

将光标放在`inbound`策略部分，单击要插入的**记录到 EventHub**策略`log-to-eventhub`策略语句模板。

![策略编辑器][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

更换`logger-id`API 管理记录器上一步骤中配置的名称。

您可以使用任何表达式都返回一个字符串的值作为`log-to-eventhub`元素。 在本示例中将记录一个包含日期和时间、 服务名称、 请求 id、 请求的 ip 地址，以及操作名称的字符串。

单击**保存**以保存更新的策略配置。 只要它保存该策略处于活动状态和事件记录到指定的事件中心。

## <a name="next-steps"></a>下一步行动

-   了解更多有关 Azure 事件集线器
    -   [开始使用 Azure 事件集线器](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [接收邮件 EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [事件集线器编程指南](../event-hubs/event-hubs-programming-guide.md)
-   了解更多关于 API 管理和事件集线器集成
    -   [记录器实体引用](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [日志对 eventhub 策略引用](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [监视您的 Api 使用 Azure API 管理、 事件集线器和 Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>观看视频演练

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






