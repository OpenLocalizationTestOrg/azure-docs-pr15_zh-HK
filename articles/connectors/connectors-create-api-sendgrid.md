<properties
pageTitle="SendGrid |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 SendGrid 连接提供程序允许您发送电子邮件和管理收件人列表。"
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

# <a name="get-started-with-the-sendgrid-connector"></a>开始使用 SendGrid 连接器

SendGrid 连接提供程序允许您发送电子邮件和管理收件人列表。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

SendGrid 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 SendGrid 连接器具有以下操作可用。 没有触发器。

### <a name="sendgrid-actions"></a>SendGrid 操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[SendEmail](connectors-create-api-sendgrid.md#sendemail)|发送一封电子邮件，使用 SendGrid API （限于 10000 个收件人）|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|将单独的收件人添加到收件人列表|


## <a name="create-a-connection-to-sendgrid"></a>创建到 SendGrid 的连接
要使用 SendGrid 创建逻辑的应用程序，必须先创建**连接**，然后提供下列属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|ApiKey|是的|提供 SendGrid 的 Api 密钥|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

创建此连接后，可以使用它来执行操作并聆听有这篇文章中所述的触发器。

## <a name="reference-for-sendgrid"></a>SendGrid 的引用
适用于版本︰ 1.0

## <a name="sendemail"></a>SendEmail
发送电子邮件︰ 发送电子邮件使用 SendGrid API （限于 10000 个收件人） 

```POST: /api/mail.send.json``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|请求| |是的|正文|无|要发送的电子邮件消息|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|429|太多的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="addrecipienttolist"></a>AddRecipientToList
将收件人添加到列表中︰ 将单独的收件人添加到收件人列表 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|listId|字符串|是的|路径|无|收件人列表的唯一 id|
|recipientId|字符串|是的|路径|无|收件人的唯一 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="object-definitions"></a>对象定义 

### <a name="emailrequest"></a>EmailRequest


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|从|字符串|是的 |
|fromname|字符串|不 |
|自|字符串|是的 |
|toname|字符串|不 |
|主题|字符串|是的 |
|正文|字符串|是的 |
|ishtml|布尔值|不 |
|抄送|字符串|不 |
|ccname|字符串|不 |
|密件抄送|字符串|不 |
|bccname|字符串|不 |
|回复|字符串|不 |
|日期|字符串|不 |
|邮件头|字符串|不 |
|文件|数组|不 |
|文件名|数组|不 |



### <a name="emailresponse"></a>EmailResponse


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|消息|字符串|不 |



### <a name="recipientlists"></a>RecipientLists


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|列表|数组|不 |



### <a name="recipientlist"></a>RecipientList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|名称|字符串|不 |
|recipient_count|整数|不 |



### <a name="recipients"></a>收件人


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|收件人|数组|不 |



### <a name="recipient"></a>收件人


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|电子邮件|字符串|不 |
|姓|字符串|不 |
|名|字符串|不 |
|标识|字符串|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)