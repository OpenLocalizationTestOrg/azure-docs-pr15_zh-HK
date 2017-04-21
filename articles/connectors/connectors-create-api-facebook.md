<properties
    pageTitle="逻辑应用程序中添加 Facebook 连接器 |Microsoft Azure"
    description="与 REST API，参数的 Facebook 连接器的概述"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>开始使用 Facebook 连接器
连接到 Facebook 和张贴到时间线，获得单页送纸，等等。 

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。


使用 Facebook，您可以︰

- 构建业务流从 Facebook 获得的数据。 
- 当接收到新的帖子时，请使用触发器。
- 张贴到时间轴中，使用操作获取单页送纸，等等。 这些操作获得的响应，并使输出可用于其他操作。 例如，当在时间线上有一篇文章时，可以采取该张贴内容并将其推到 Twitter 订阅源。 



若要在应用程序逻辑中添加操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作
Facebook 连接器包括以下触发器和操作。 

| 触发器 | 操作|
| --- | --- |
| <ul><li>当在时间线上的一篇文章</li></ul> |<ul><li>获取源从时间线</li><li>过帐到时间线</li><li>当在时间线上的一篇文章</li><li>获取源页</li><li>获取用户的时间轴</li><li>发送到页</li></ul>

所有连接器都支持 JSON 和 XML 格式的数据。

## <a name="create-a-connection-to-facebook"></a>创建到 Facebook 的连接
当此连接器添加到您的逻辑应用程序时，您必须授权逻辑应用程序连接到您的 Facebook。

1. 登录到 Facebook 帐户
2. 选择**授权**，并允许您的逻辑应用程序连接并使用您的 Facebook。 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] 在其他逻辑应用程序中，您可以使用此相同的 Facebook 连接。

## <a name="swagger-rest-api-reference"></a>Swagger REST API 参考
适用于版本︰ 1.0。

### <a name="get-feed-from-my-timeline"></a>获取源从时间线
获取源从登录的用户的时间轴。  
```GET: /me/feed```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|字段|字符串|不|查询|无 |指定要返回的字段。 示例 （id、 名称、 图片）。|
|限制|整数|不|查询| 无|文章要检索的最大数量|
|使用|字符串|不|查询| 无|只有那些具有附加的位置限制的文章的列表。|
|筛选器|字符串|不|查询| 无|检索与特定流筛选器匹配的帖子。|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|500|内部服务器错误|
|默认|操作失败。|


### <a name="post-to-my-timeline"></a>过帐到时间线
发布状态消息向已登录的用户的时间线。  
```POST: /me/feed```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|发布|字符串 |是的|正文|无 |要发布新消息|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|500|内部服务器错误|
|默认|操作失败。|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>当在时间线上的一篇文章
在没有登录的用户的时间轴上的一篇文章时才会触发新的流。  
```GET: /trigger/me/feed```

没有任何参数。 

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|500|内部服务器错误|
|默认|操作失败。|


### <a name="get-page-feed"></a>获取源页
指定页面的源获得的帖子。  
```GET: /{pageId}/feed```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|pageId|字符串|是的|路径| 无|已检索公告的页面 id。|
|限制|整数|不|查询| 无|文章要检索的最大数量|
|include_hidden|布尔值|不|查询|无 |是否包括任何隐藏页的帖子|
|字段|字符串|不|查询|无 |指定要返回的字段。 示例 （id、 名称、 图片）。|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|500|内部服务器错误|
|默认|操作失败。|


### <a name="get-user-timeline"></a>获取用户的时间轴
帖子得到用户的时间轴。  
```GET: /{userId}/feed```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|用户 Id|字符串|是的|路径|无 |其时间线需要检索用户 id。|
|限制|整数|不|查询|无 |文章要检索的最大数量|
|使用|字符串|不|查询|无 |只有那些具有附加的位置限制的文章的列表。|
|筛选器|字符串|不|查询| 无|检索与特定流筛选器匹配的帖子。|
|字段|字符串|不|查询| 无|指定要返回的字段。 示例 （id、 名称、 图片）。|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|500|内部服务器错误|
|默认|操作失败。|


### <a name="post-to-page"></a>发送到页
登录的用户作为一个 Facebook 网页发布的消息。  
```POST: /{pageId}/feed```

| 名称|数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|pageId|字符串|是的|路径|无 |要发布的页的 id。|
|发布|很多 |是的|正文|无 |新消息发布。|

#### <a name="response"></a>响应
|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="object-definitions"></a>对象定义

#### <a name="getfeedresponse"></a>GetFeedResponse

|属性名称 | 数据类型 | 必填|
|---|---|---|
|数据|数组|不|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|属性名称 | 数据类型 |必填|
|---|---|---|
|数据|数组|不|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem︰ 配置文件中的单个项的源
配置文件可以是用户、 页面、 应用程序或组。 

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|
|admin_creator|数组|不|
|标题|字符串|不|
|created_time|字符串|不|
|说明|字符串|不|
|feed_targeting|未定义|不|
|从|未定义|不|
|图标|字符串|不|
|is_hidden|布尔值|不|
|is_published|布尔值|不|
|链接|字符串|不|
|消息|字符串|不|
|名称|字符串|不|
|省略 object_id|字符串|不|
|图片|字符串|不|
|位置|未定义|不|
|隐私|未定义|不|
|属性|数组|不|
|来源|字符串|不|
|status_type|字符串|不|
|故事|字符串|不|
|目标|未定义|不|
|自|数组|不|
|类型|字符串|不|
|updated_time|字符串|不|
|with_tags|未定义|不|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem︰ 配置文件中的单个项的源
配置文件可以是用户、 页面、 应用程序或组。

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|
|created_time|字符串|不|
|从|未定义|不|
|消息|字符串|不|
|类型|字符串|不|

#### <a name="adminitem"></a>AdminItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|
|链接|字符串|不|

#### <a name="propertyitem"></a>PropertyItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|名称|字符串|不|
|文本|字符串|不|
|href|字符串|不|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|属性名称 | 数据类型 |必填|
|---|---|---|
|消息|字符串|是的|
|链接|字符串|不|
|图片|字符串|不|
|名称|字符串|不|
|标题|字符串|不|
|说明|字符串|不|
|位置|字符串|不|
|标记|字符串|不|
|隐私|未定义|不|
|object_attachment|字符串|不|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|属性名称 | 数据类型 |必填|
|---|---|---|
|消息|字符串|是的|
|链接|字符串|不|
|图片|字符串|不|
|名称|字符串|不|
|标题|字符串|不|
|说明|字符串|不|
|操作|数组|不|
|位置|字符串|不|
|标记|字符串|不|
|object_attachment|字符串|不|
|目标|未定义|不|
|feed_targeting|未定义|不|
|发布|布尔值|不|
|scheduled_publish_time|字符串|不|
|backdated_time|字符串|不|
|backdated_time_granularity|字符串|不|
|child_attachments|数组|不|
|multi_share_end_card|布尔值|不|

#### <a name="postfeedresponse"></a>PostFeedResponse

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|

#### <a name="profilecollection"></a>ProfileCollection

|属性名称 | 数据类型 |必填|
|---|---|---|
|数据|数组|不|

#### <a name="useritem"></a>UserItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|
|名|字符串|不|
|姓|字符串|不|
|名称|字符串|不|
|性别|字符串|不|
|有关|字符串|不|

#### <a name="actionitem"></a>ActionItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|名称|字符串|不|
|链接|字符串|不|

#### <a name="targetitem"></a>TargetItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|国家/地区|数组|不|
|区域设置|数组|不|
|地区|数组|不|
|城市|数组|不|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem︰ 控制新闻的对象源目标对此文章
这些组中的任何人都更有可能看到这篇文章，其他人不太可能。 仅适用于页面。

|属性名称 | 数据类型 |必填|
|---|---|---|
|国家/地区|数组|不|
|地区|数组|不|
|城市|数组|不|
|age_min|整数|不|
|age_max|整数|不|
|个性|数组|不|
|relationship_statuses|数组|不|
|interested_in|数组|不|
|college_years|数组|不|
|兴趣爱好|数组|不|
|relevant_until|整数|不|
|education_statuses|数组|不|
|区域设置|数组|不|

#### <a name="placeitem"></a>PlaceItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|
|名称|字符串|不|
|overall_rating|编号|不|
|位置|未定义|不|

#### <a name="locationitem"></a>LocationItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|市/县|字符串|不|
|国家/地区|字符串|不|
|纬度|编号|不|
|located_in|字符串|不|
|经度|编号|不|
|名称|字符串|不|
|地区|字符串|不|
|状态|字符串|不|
|街道|字符串|不|
|压缩|字符串|不|

#### <a name="privacyitem"></a>PrivacyItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|说明|字符串|不|
|值|字符串|是的|
|允许|字符串|不|
|拒绝|字符串|不|
|朋友|字符串|不|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|链接|字符串|不|
|图片|字符串|不|
|image_hash|字符串|不|
|名称|字符串|不|
|说明|字符串|不|

#### <a name="postphotorequest"></a>PostPhotoRequest

|属性名称 | 数据类型 |必填|
|---|---|---|
|url|字符串|是的|
|标题|字符串|不|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|是的|
|post_id|字符串|是的|

#### <a name="postvideorequest"></a>PostVideoRequest

|属性名称 | 数据类型 |必填|
|---|---|---|
|videoData|字符串|是的|
|说明|字符串|是的|
|标题|字符串|是的|
|uploadedVideoName|字符串|不|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|属性名称 | 数据类型 |必填|
|---|---|---|
|数据|未定义|是的|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|url|字符串|是的|
|is_silhouette|布尔值|是的|
|高度|字符串|不|
|宽度|字符串|不|

#### <a name="geteventresponse"></a>GetEventResponse

|属性名称 | 数据类型 |必填|
|---|---|---|
|数据|数组|是的|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|属性名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|是的|
|名称|字符串|是的|
|start_time|字符串|不|
|end_time|字符串|不|
|时区|字符串|不|
|位置|字符串|不|
|说明|字符串|不|
|ticket_uri|字符串|不|
|rsvp_status|字符串|是的|


## <a name="next-steps"></a>下一步行动

[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。
