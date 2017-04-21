<properties
pageTitle=" 逻辑应用程序中使用的可宽延时间的接口 |Microsoft Azure"
description="开始使用您的 Microsoft Azure 应用程序服务逻辑应用程序中的可宽延时间连接器"
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

# <a name="get-started-with-the-slack-connector"></a>开始可宽延时间的连接器

可宽延时间是在一起的团队通信工具在一个团队的通信的所有放置，立即搜索和使用随身。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。

可宽延时间的连接器，您可以︰

* 使用它来构建应用程序逻辑

若要在应用程序逻辑中添加操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="lets-talk-about-triggers-and-actions"></a>让我们谈一谈触发器和操作

可宽延时间的连接器可以用作操作;没有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 可宽延时间连接器具有以下操作和/或可用触发器︰

### <a name="slack-actions"></a>可宽延时间的操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|传递|投递邮件到指定的频道。|
## <a name="create-a-connection-to-slack"></a>创建连接到可宽延时间
若要使用的可宽延时间的连接器，第一次创建的**连接**中，然后提供这些属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供可宽延时间的凭据|

请按照下列步骤登录可宽延时间和完成可宽延时间**连接**逻辑应用程序中的配置︰

1. 选择**重复周期**
2. 选择一个**频率**和输入的**时间间隔**
3. 选择**添加操作**  
![配置可宽延时间][1]  
4. 在搜索框中输入可宽延时间和等待搜索返回名称中的可宽延时间的所有项
5. 选择**可宽延时间的开机自检消息**
6. 选择**登录以可宽延时间**︰  
![配置可宽延时间][2]
7. 提供您可宽延时间的凭据才能登录授权应用程序    
![配置可宽延时间][3]  
8. 您将被重定向到您的组织的登录页。 **授权**可宽延时间与您的逻辑应用程序进行交互︰      
![配置可宽延时间][5] 
9. 授权完成后您将被重定向到您的逻辑应用程序可通过配置**可宽延时间-收到的所有邮件**部分中完成它。 添加其他触发器和所需的操作。  
![配置可宽延时间][6]
10. 通过在上面的菜单栏上选择**保存**保存您的工作。


>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="slack-rest-api-reference"></a>可宽延时间的 REST API 参考
#### <a name="this-documentation-is-for-version-10"></a>本文档是为版本︰ 1.0


### <a name="post-a-message-to-a-specified-channel"></a>投递邮件到指定的频道。
**```POST: /chat.postMessage```** 



| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|通道|字符串|是的|查询|无|通道、 专用组或 IM 通道向其发送消息。 可以是一个名称 (例如︰ #general) 或一个已编码的 id。|
|文本|字符串|是的|查询|无|要发送的消息的文本。 要看到格式选项，请参阅 https://api.slack.com/docs/formatting。|
|用户名|字符串|不|查询|无|Bot 的名称|
|as_user|布尔值|不|查询|无|通过将邮件标记为已通过身份验证的用户，而不是作为 bot，则返回 true|
|分析|字符串|不|查询|无|更改邮件的处理方式。 有关详细信息，请参阅 https://api.slack.com/docs/formatting。|
|link_names|整数|不|查询|无|查找和链接频道名称和用户名。|
|unfurl_links|布尔值|不|查询|无|通过设置为 true 将启用展开的主要是基于文本的内容。|
|unfurl_media|布尔值|不|查询|无|通过为 false，则禁用媒体内容的展开。|
|icon_url|字符串|不|查询|无|要用作此消息图标的图像 URL|
|icon_emoji|字符串|不|查询|无|Emoji 以用作此邮件的图标|


### <a name="here-are-the-possible-responses"></a>以下是可能的响应︰

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|408|请求超时|
|429|请求太多|
|500|内部服务器错误。 出现未知的错误|
|503|可宽延时间服务不可用|
|第 504|网关超时|
|默认|操作失败。|
------



## <a name="object-definitions"></a>对象定义︰ 

 **消息**︰ Yammer 消息

邮件的必需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|标识|整数|
|content_excerpt|字符串|
|sender_id|整数|
|replied_to_id|整数|
|created_at|字符串|
|network_id|整数|
|message_type|字符串|
|sender_type|字符串|
|url|字符串|
|web_url|字符串|
|group_id|整数|
|正文|未定义|
|thread_id|整数|
|direct_message|布尔值|
|client_type|字符串|
|client_url|字符串|
|语言|字符串|
|notified_user_ids|数组|
|隐私|字符串|
|liked_by|未定义|
|system_message|布尔值|



 **PostOperationRequest**︰ 表示 yammer Yammer 连接器发送 post 请求

PostOperationRequest 所需的属性︰

正文

**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|正文|字符串|
|group_id|整数|
|replied_to_id|整数|
|direct_to_id|整数|
|广播|布尔值|
|topic1|字符串|
|topic2|字符串|
|topic3|字符串|
|topic4|字符串|
|topic5|字符串|
|topic6|字符串|
|topic7|字符串|
|topic8|字符串|
|topic9|字符串|
|topic10|字符串|
|topic11|字符串|
|topic12|字符串|
|topic13|字符串|
|topic14|字符串|
|topic15|字符串|
|topic16|字符串|
|topic17|字符串|
|topic18|字符串|
|topic19|字符串|
|topic20|字符串|



 **MessageList**︰ 消息列表

MessageList 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|消息|数组|



 **MessageBody**︰ 邮件正文

MessageBody 所必需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|分析|字符串|
|纯|字符串|
|富|字符串|



 **LikedBy**︰ 喜欢的

LikedBy 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|计数|整数|
|名称|数组|



 **YammmerEntity**︰ 喜欢的

YammmerEntity 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|类型|字符串|
|标识|整数|
|full_name|字符串|


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>对象定义︰ 

 **WebResultModel**: Bing web 搜索结果

WebResultModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|标题|字符串|
|说明|字符串|
|DisplayUrl|字符串|
|标识|字符串|
|FullUrl|字符串|



 **VideoResultModel**: Bing 视频搜索结果

VideoResultModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|标题|字符串|
|DisplayUrl|字符串|
|标识|字符串|
|MediaUrl|字符串|
|运行时|整数|
|缩略图|未定义|



 **ThumbnailModel**︰ 多媒体元素的缩略图属性

ThumbnailModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|MediaUrl|字符串|
|ContentType|字符串|
|宽度|整数|
|高度|整数|
|文件大小|整数|



 **ImageResultModel**: Bing 图像搜索结果

ImageResultModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|标题|字符串|
|DisplayUrl|字符串|
|标识|字符串|
|MediaUrl|字符串|
|SourceUrl|字符串|
|缩略图|未定义|



 **NewsResultModel**: Bing 新闻搜索结果

NewsResultModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|标题|字符串|
|说明|字符串|
|DisplayUrl|字符串|
|标识|字符串|
|来源|字符串|
|日期|字符串|



 **SpellResultModel**: Bing 拼写建议结果

SpellResultModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|标识|字符串|
|值|字符串|



 **RelatedSearchResultModel**: Bing 相关搜索结果

RelatedSearchResultModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|标题|字符串|
|标识|字符串|
|BingUrl|字符串|



 **CompositeSearchResultModel**: Bing 复合搜索结果

CompositeSearchResultModel 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|WebResultsTotal|整数|
|ImageResultsTotal|整数|
|VideoResultsTotal|整数|
|NewsResultsTotal|整数|
|SpellSuggestionsTotal|整数|
|WebResults|数组|
|ImageResults|数组|
|VideoResults|数组|
|NewsResults|数组|
|SpellSuggestionResults|数组|
|RelatedSearchResults|数组|


## <a name="object-definitions"></a>对象定义︰ 

 **PostOperationResponse**︰ 用于过帐到可宽延时间表示的可宽延时间连接器的 post 操作的响应

PostOperationResponse 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|还行|布尔值|
|通道|字符串|
|ts|字符串|
|消息|未定义|
|错误|字符串|



 **MessageItem**︰ 频道消息。

MessageItem 所需的属性︰


任何属性是必需的。 


**所有属性**︰ 


| 名称 | 数据类型 |
|---|---|
|文本|字符串|
|标识|字符串|
|用户|字符串|
|创建|整数|
|is_user 删除|布尔值|


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
