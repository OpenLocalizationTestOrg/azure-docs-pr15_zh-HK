<properties
pageTitle="了解如何在您的应用程序逻辑中使用 Azure 服务总线连接器 |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 连接到 Azure 服务总线发送和接收消息。 您可以执行操作，如发送队列、 主题、 从队列中接收从发送和接收订阅。"
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>开始使用 Azure 服务总线连接器

连接到 Azure 服务总线发送和接收消息。 您可以执行操作，如发送队列、 主题、 从队列中接收从发送和接收订阅。

使用[任何连接器](./apis-list.md)，您首先需要创建一个逻辑应用程序。 您可以通过[创建一个逻辑应用程序现在](../app-service-logic/app-service-logic-create-a-logic-app.md)开始。

## <a name="connect-to-service-bus"></a>连接到服务总线

逻辑应用程序可以访问的任何服务之前，您首先需要创建一个连接到的服务。 [连接](./connectors-overview.md)提供一个逻辑应用程序和其他服务之间的连接。  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>使用服务总线触发器

触发器是可以用来启动工作流的逻辑应用程序中定义的事件。 [了解更多有关触发器](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)的信息。  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>使用服务总线操作

操作是由逻辑应用程序中定义的工作流执行的操作。 [了解有关操作的详细](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)信息。

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>技术详细信息

以下是有关触发器、 操作和支持此连接的响应的详细信息。

### <a name="service-bus-triggers"></a>服务总线触发器

服务总线具有以下触发器︰  

|触发器 | 说明|
|--- | ---|
|[当队列中接收消息](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|此操作会触发流时队列中接收消息。|
|[当主题订阅中收到一条消息](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|在主题订阅收到一条消息时，此操作会触发流。|


### <a name="service-bus-actions"></a>服务总线操作

服务总线具有以下操作︰


|操作|说明|
|--- | ---|
|[发送消息](connectors-create-api-servicebus.md#send-message)|此操作将消息发送到队列或主题。|
### <a name="action-and-trigger-details"></a>操作和触发器的详细信息

以下是操作的详细信息以及此连接器，以及他们的响应触发器。



#### <a name="send-message"></a>发送消息

|属性名称| 显示名称|说明|
| ---|---|---|
|ContentData *|内容|该邮件的内容。|
|ContentType|内容类型|消息内容的内容类型。|
|属性|属性|每个键 / 值对通过代理属性。|
|实体 *|队列/主题名称|队列或主题的名称。|

这些高级的参数也是可用的︰

|属性名称| 显示名称|说明|
| ---|---|---|
|邮件 Id|消息 Id|服务总线可以使用来标识重复的邮件，如果启用了用户定义的值。|
|自|自|要发送到的地址。|
|回复|回复|要回复的队列地址。|
|ReplyToSessionId|会话 Id 的答复|要回复的会话标识符。|
|标签|标签|特定于应用程序的标签。|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|日期和时间以 utc 格式时，邮件将被添加到队列。|
|会话标识符|会话 Id|会话的标识符。|
|都会|相关性 Id|关联的标识符。|
|TimeToLive|生存的时间|在刻度，消息的有效期限。 期间开始时将消息发送到服务总线。|



* 指示属性是必需。


#### <a name="when-a-message-is-received-in-a-queue"></a>当队列中接收消息

|属性名称| 显示名称|说明|
| ---|---|---|
|queueName *|队列名称|队列的名称。|


* 指示属性是必需。


##### <a name="output-details"></a>输出详细信息

ServiceBusMessage︰ 此对象具有的内容和服务总线消息的属性。


| 属性名称 | 数据类型 | 说明 |
|---|---|---|
|ContentData|字符串|该邮件的内容。|
|ContentType|字符串|消息内容的内容类型。|
|属性|对象|每个键 / 值对通过代理属性。|
|邮件 Id|字符串|服务总线可以使用来标识重复的邮件，如果启用了用户定义的值。|
|自|字符串|将发送到地址。|
|回复|字符串|要回复的队列地址。|
|ReplyToSessionId|字符串|要回复的会话标识符。|
|标签|字符串|特定于应用程序的标签。|
|ScheduledEnqueueTimeUtc|字符串|日期和时间以 utc 格式时，邮件将被添加到队列。|
|会话标识符|字符串|会话的标识符。|
|都会|字符串|关联的标识符。|
|TimeToLive|字符串|在刻度，消息的有效期限。 期间开始时将消息发送到服务总线。|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>当主题订阅中收到一条消息

|属性名称| 显示名称|说明|
| ---|---|---|
|topicName *|主题名称|主题的名称。|
|subscriptionName *|主题订阅名称|该主题的订阅的名称。|


* 指示属性是必需。


##### <a name="output-details"></a>输出详细信息

ServiceBusMessage︰ 此对象具有的内容和服务总线消息的属性。


| 属性名称 | 数据类型 | 说明 |
|---|---|---|
|ContentData|字符串|该邮件的内容。|
|ContentType|字符串|消息内容的内容类型。|
|属性|对象|每个键 / 值对通过代理属性。|
|邮件 Id|字符串|服务总线可以使用来标识重复的邮件，如果启用了用户定义的值。|
|自|字符串|将发送到地址。|
|回复|字符串|要回复的队列地址。|
|ReplyToSessionId|字符串|要回复的会话标识符。|
|标签|字符串|特定于应用程序的标签。|
|ScheduledEnqueueTimeUtc|字符串|日期和时间以 utc 格式时，邮件将被添加到队列。|
|会话标识符|字符串|会话的标识符。|
|都会|字符串|关联的标识符。|
|TimeToLive|字符串|在刻度，消息的有效期限。 期间开始时将消息发送到服务总线。|



### <a name="http-responses"></a>HTTP 响应

上述的操作和触发器可以返回一个或多个以下 HTTP 状态代码︰

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误。|
|默认|操作失败。|

## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。
