<properties
pageTitle="RSS |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 RSS 连接器允许用户发布和检索源的项。 它还允许用户触发操作，新项发布到数据源时。"
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
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-rss-connector"></a>开始使用 RSS 连接器
RSS 是一种常用 web 联合格式，用于发布经常更新的内容 — 如网络日志项和头条新闻。  许多内容发行商提供允许用户订阅该 RSS 源。  使用 RSS 连接器 RSS 源中发布新项目时检索源的信息和触发流程。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

RSS 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 RSS 连接器具有以下操作和/或可用触发器︰

### <a name="rss-actions"></a>RSS 的操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|获取所有的 RSS 订阅源项。|
### <a name="rss-triggers"></a>RSS 触发器
您可以侦听这些事件︰

|触发器 | 说明|
|--- | ---|
|在发布新的订阅源的项|发布新的订阅源时触发工作流|


## <a name="create-a-connection-to-rss"></a>创建 rss 的连接

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="reference-for-rss"></a>RSS 的引用
适用于版本︰ 1.0

## <a name="onnewfeed"></a>OnNewFeed
在发布新的订阅源的项︰ 发布新的订阅源时触发工作流 

```GET: /OnNewFeed``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|feedUrl|字符串|是的|查询|无|数据源 url|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="listfeeditems"></a>ListFeedItems
列出所有的 RSS 订阅源项。: 获取所有 RSS 订阅源项。 

```GET: /ListFeedItems``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|feedUrl|字符串|是的|查询|无|数据源 url|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="object-definitions"></a>对象定义 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="feeditem"></a>FeedItem


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|是的 |
|标题|字符串|是的 |
|内容|字符串|是的 |
|链接|数组|不 |
|updatedOn|字符串|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)