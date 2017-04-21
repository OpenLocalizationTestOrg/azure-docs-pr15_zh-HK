<properties
pageTitle="逻辑应用程序在使用 Office 365 视频连接器 |Microsoft Azure"
description="开始在 Microsoft Azure 应用程序服务的逻辑应用程序中使用 Office 365 视频连接器"
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

# <a name="get-started-with-the-office365-video-connector"></a>开始使用 Office365 视频连接器
连接到 Office 365 视频来获取有关 Office 365 的信息视频，获得一份视频，等等。 可以从使用 Office 365 视频连接器︰

- 逻辑应用程序 

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 所有以前的架构版本不支持此连接器。

使用 Office 365 视频中，您可以︰

- 建立基于获得 Office 365 视频数据业务流。 
- 使用操作检查视频门户状态，请获取列表的所有视频通道，等等。 这些操作获得的响应，并使输出可用于其他操作。 例如，可以使用 Bing 搜索连接器搜索有关 Office 365 的视频，并将 Office 365 视频接口以获得关于该视频的信息。 如果视频符合您的要求，则可以在 Facebook 上张贴该视频。 

若要在应用程序逻辑中添加操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

Office 365 视频连接器具有以下操作可用。 没有触发器。

| 触发器 | 操作|
| --- | --- |
| 无 | <ul><li>检查视频门户状态</li><li>获取所有可查看频道</li><li>获取视频的播放 url 的 Azure 媒体服务清单</li><li>获取要获得解密视频的持有者标记</li><li>获取有关特定 office365 的信息视频</li><li>列出所有通道中存在 office365 视频</li></ul>

所有连接器都支持 JSON 和 XML 格式的数据。 

## <a name="create-a-connection-to-office365-video-connector"></a>创建一个连接到 Office365 视频连接器
当此连接器添加到您的逻辑应用程序时，您必须登录到您的 Office 365 视频帐户并允许逻辑应用程序连接到您的帐户。

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

在创建连接后，您输入 Office 365 的视频属性，如承租人姓名或通道 id。 本主题中的**其余部分 API 参考**描述了这些属性。

>[AZURE.TIP] 在其他逻辑应用程序中，您可以使用此相同的 Office 365 视频连接。

## <a name="swagger-rest-api-reference"></a>Swagger REST API 参考
适用于版本︰ 1.0。

### <a name="checks-video-portal-status"></a>检查视频门户状态 
检查以查看视频服务已经启用了视频门户状态。  
```GET: /{tenant}/IsEnabled``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|租户|字符串|是的|路径|无|目录用户的租户名称是的一部分|


#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|404|找不到|
|500|内部服务器错误|
|默认|操作失败。|



### <a name="get-all-viewable-channels"></a>获取所有可查看频道 
获取用户有权查看的所有通道。  
```GET: /{tenant}/Channels``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|租户|字符串|是的|路径|无|目录用户的租户名称是的一部分|


#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|404|找不到|
|500|内部服务器错误|
|默认|操作失败。|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>列出所有通道中存在 office365 视频 
列出所有通道中存在 office365 视频。  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|租户|字符串|是的|路径|无|目录用户的租户名称是的一部分|
|channelId|字符串|是的|路径|无|需要以获取视频通道 id|


#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|404|找不到|
|500|内部服务器错误|
|默认|操作失败。|




### <a name="gets-information-about-a-particular-office365-video"></a>获取有关特定 office365 的信息视频 
获取有关特定 office365 的信息视频。  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|租户|字符串|是的|路径|无|目录用户的租户名称是的一部分|
|channelId|字符串|是的|路径|无|通道 id|
|videoId|字符串|是的|路径|无|视频的 id|


#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|404|找不到|
|500|内部服务器错误|
|默认|操作失败。|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>获取视频的播放 url 的 Azure 媒体服务清单 
获取视频播放的 Azure 媒体服务清单的 url。  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|租户|字符串|是的|路径|无|目录用户的租户名称是的一部分|
|channelId|字符串|是的|路径|无|通道 id|
|videoId|字符串|是的|路径|无|视频的 id|
|streamingFormatType|字符串|是的|查询|无|流格式类型。 1-流的平滑或 MPEG-短划线。 0-HLS 流|


#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|404|找不到|
|500|内部服务器错误|
|默认|操作失败。|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>获取要获得解密视频的持有者标记 
获取要获得解密视频的持有者标记。  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|租户|字符串|是的|路径|无|目录用户的租户名称是的一部分|
|channelId|字符串|是的|路径|无|通道 id|
|videoId|字符串|是的|路径|无|视频的 id|


#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|404|找不到|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="object-definitions"></a>对象定义

#### <a name="channel-channel-class"></a>通道︰ 通道类

| 名称 | 数据类型 | 必填|
|---|---|---|
|标识|字符串|不|
|标题|字符串|不|
|说明|字符串|不|


#### <a name="video"></a>视频 

| 名称 | 数据类型 |必填|
|---|---|---|
|标识|字符串|不|
|标题|字符串|不|
|说明|字符串|不|
|CreationDate|字符串|不|
|所有者|字符串|不|
|ThumbnailUrl|字符串|不|
|VideoUrl|字符串|不|
|VideoDuration|整数|不|
|VideoProcessingStatus|整数|不|
|ViewCount|整数|不|


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。
