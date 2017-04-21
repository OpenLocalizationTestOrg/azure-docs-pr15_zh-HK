<properties
pageTitle="逻辑应用程序中添加 Yammer 连接器 |Microsoft Azure"
description="使用 REST API 的参数 Yammer 连接器的概述"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-yammer-connector"></a>开始使用 Yammer 连接器

连接到 Yammer 访问企业网络中的对话。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。

与 Yammer，您可以︰

- 建立基于获得 Yammer 的数据业务流。 
- 在没有新消息组或源中的您以下时，使用触发的。
- 使用动作发布一条消息，得到的所有消息，以及更多。 这些操作获得的响应，并使输出可用于其他操作。 例如，出现一个新的邮件时，您可以发送电子邮件使用 Office 365。

若要在应用程序逻辑中添加操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作
Yammer 包括下面的触发器和操作。 

触发器 | 操作
--- | ---
<ul><li>当组中没有新邮件</li><li>我下面中的新邮件时进</li></ul>| <ul><li>获取所有邮件</li><li>获取组中的邮件</li><li>我下面的消息源的获取</li><li>开机自检信息</li><li>当组中没有新邮件</li><li>我下面中的新邮件时进</li></ul>

所有连接器都支持 JSON 和 XML 格式的数据。 

## <a name="create-a-connection-to-yammer"></a>创建到 Yammer 的连接
要使用 Yammer 连接器，请首先创建一个**连接**中，然后提供这些属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供 Yammer 凭据|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="yammer-rest-api-reference"></a>Yammer REST API 参考
本文档是为版本︰ 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>已登录的用户的 Yammer 网络中获取所有公共消息
对应于 Yammer web 界面中的"全部"对话。  
```GET: /messages.json```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|older_then|整数|不|查询|无|返回时间超过指定为数字字符串的邮件 ID 的邮件。 这可用于对消息进行分页。 例如，如果您当前正在查看 20 个邮件，最早是数字 2912，无法追加"？ older_than = 2912″ 为您的请求以获取您会看到 20 个邮件之前的那些。|
|newer_then|整数|不|查询|无|返回消息的消息 ID 指定为数值的字符串比。 在轮询新邮件时应使用此。 如果您正在查看的邮件，并为 3516 返回的最新消息，您可以发出一个请求参数与"？ newer_than = 3516″，以确保，没有得到消息的复制副本已在页面上。|
|限制|整数|不|查询|无|返回仅指定的邮件数。|
|页面|整数|不|查询|无|获取指定的页。 如果返回的数据大于此限制，可以使用此字段访问后续页|


### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|408|请求超时|
|429|请求太多|
|500|内部服务器错误。 出现未知的错误|
|503|Yammer 服务不可用|
|第 504|网关超时|
|默认|操作失败。|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>将邮件张贴到组或所有公司都进
如果提供组 ID，则消息将过帐到指定的组其他将会刊登在下所有公司送。    
```POST: /messages.json``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|输入| |是的|正文|无|开机自检消息请求|


### <a name="response"></a>响应

|名称|说明|
|---|---|
|201|创建|



## <a name="object-definitions"></a>对象定义

#### <a name="message-yammer-message"></a>消息︰ Yammer 消息

| 名称 | 数据类型 | 必填 |
|---|---| --- | 
|标识|整数|不|
|content_excerpt|字符串|不|
|sender_id|整数|不|
|replied_to_id|整数|不|
|created_at|字符串|不|
|network_id|整数|不|
|message_type|字符串|不|
|sender_type|字符串|不|
|url|字符串|不|
|web_url|字符串|不|
|group_id|整数|不|
|正文|未定义|不|
|thread_id|整数|不|
|direct_message|布尔值|不|
|client_type|字符串|不|
|client_url|字符串|不|
|语言|字符串|不|
|notified_user_ids|数组|不|
|隐私|字符串|不|
|liked_by|未定义|不|
|system_message|布尔值|不|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest︰ 表示 yammer Yammer 连接器发送 post 请求

| 名称 | 数据类型 | 必填 |
|---|---| --- | 
|正文|字符串|是的|
|group_id|整数|不|
|replied_to_id|整数|不|
|direct_to_id|整数|不|
|广播|布尔值|不|
|topic1|字符串|不|
|topic2|字符串|不|
|topic3|字符串|不|
|topic4|字符串|不|
|topic5|字符串|不|
|topic6|字符串|不|
|topic7|字符串|不|
|topic8|字符串|不|
|topic9|字符串|不|
|topic10|字符串|不|
|topic11|字符串|不|
|topic12|字符串|不|
|topic13|字符串|不|
|topic14|字符串|不|
|topic15|字符串|不|
|topic16|字符串|不|
|topic17|字符串|不|
|topic18|字符串|不|
|topic19|字符串|不|
|topic20|字符串|不|

#### <a name="messagelist-list-of-messages"></a>MessageList︰ 列表中的邮件

| 名称 | 数据类型 | 必填 |
|---|---| --- | 
|消息|数组|不|


#### <a name="messagebody-message-body"></a>MessageBody︰ 邮件正文

| 名称 | 数据类型 | 必填 |
|---|---| --- | 
|分析|字符串|不|
|纯|字符串|不|
|富|字符串|不|

#### <a name="likedby-liked-by"></a>LikedBy︰ 非常喜欢的

| 名称 | 数据类型 | 必填 |
|---|---| --- | 
|计数|整数|不|
|名称|数组|不|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity︰ 非常喜欢的

| 名称 | 数据类型 | 必填 |
|---|---| --- | 
|类型|字符串|不|
|标识|整数|不|
|full_name|字符串|不|


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
