<properties
pageTitle="Outlook.com |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 Outlook.com 连接器允许您管理您的邮件、 日历和联系人。 您可以执行各种操作，如发送邮件、 安排会议、 添加联系人等。"
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

# <a name="get-started-with-the-outlookcom-connector"></a>开始使用 Outlook.com 连接器

Outlook.com 连接器允许您管理您的邮件、 日历和联系人。 您可以执行各种操作，如发送邮件、 安排会议、 添加联系人等。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

Outlook.com 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 Outlook.com 连接器具有以下操作和/或可用触发器︰

### <a name="outlookcom-actions"></a>Outlook.com 操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|从文件夹中检索电子邮件|
|[SendEmail](connectors-create-api-outlook.md#SendEmail)|发送电子邮件|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|按 id 删除电子邮件|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|将标记为已读的电子邮件|
|[回复](connectors-create-api-outlook.md#ReplyTo)|对电子邮件的答复|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|按 id 检索电子邮件的附件|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|发送一封电子邮件有多个选项，并等待收件人响应回用另一种|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|发送电子邮件的审批，并等待来自收件人的响应|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|检索日历|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|从日历中检索项|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|创建新的事件|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|从日历中检索特定项|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|删除日历项目|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|部分更新的日历项目|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|检索联系人文件夹|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|从联系人文件夹中检索联系人|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|创建新联系人|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|从联系人文件夹中检索特定的联系人|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|删除联系人|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|部分更新联系人|
### <a name="outlookcom-triggers"></a>Outlook.com 触发器
您可以侦听这些事件︰

|触发器 | 说明|
|--- | ---|
|在事件即将开始|在即将到来的日历事件启动时触发流程|
|在新的电子邮件|新电子邮件到达时触发流程|
|上新项目|当创建新的日历项时触发|
|更新项目|在日历项目被修改时触发|


## <a name="create-a-connection-to-outlookcom"></a>创建到 Outlook.com 的连接
要使用 Outlook.com 创建逻辑的应用程序，必须先创建**连接**，然后提供下列属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供 Outlook.com 的凭据|
创建此连接后，可以使用它来执行操作并聆听有这篇文章中所述的触发器。

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。  

## <a name="reference-for-outlookcom"></a>Outlook.com 的引用
适用于版本︰ 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
在事件即将开始︰ 在即将到来的日历事件启动时触发流程 

```GET: /Events/OnUpcomingEvents``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|查询|无|日历的唯一标识符|
|lookAheadTimeInMinutes|整数|不|查询|15|要展望即将到来的事件的时间 （以分钟为单位）|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|202|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="getemails"></a>GetEmails
获取电子邮件︰ 从文件夹中检索电子邮件 

```GET: /Mail``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|采用文件夹路径|字符串|不|查询|收件箱|要检索电子邮件的文件夹的路径 (默认: 收件箱)|
|返回页首|整数|不|查询|10|要检索的电子邮件的数量 (默认值︰ 10)|
|fetchOnlyUnread|布尔值|不|查询|真|检索只未读电子邮件吗？|
|includeAttachments|布尔值|不|查询|假|如果设置为 true，附件也将随电子邮件一起检索|
|searchQuery|字符串|不|查询|无|搜索查询来筛选电子邮件|
|跳过|整数|不|查询|0|若要跳过的电子邮件的数量 (默认值︰ 0)|
|skipToken|字符串|不|查询|无|跳到提取新页的标记|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="sendemail"></a>SendEmail
发送电子邮件︰ 发送电子邮件 

```POST: /Mail``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|emailMessage| |是的|正文|无|电子邮件|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="deleteemail"></a>DeleteEmail
删除电子邮件︰ 按 id 删除电子邮件 

```DELETE: /Mail/{messageId}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|邮件 Id|字符串|是的|路径|无|若要删除该电子邮件的 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="markasread"></a>MarkAsRead
标记为已读︰ 标记为已读的电子邮件 

```POST: /Mail/MarkAsRead/{messageId}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|邮件 Id|字符串|是的|路径|无|Id 的电子邮件，标记为读取|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="replyto"></a>回复
电子邮件的回复︰ 回复一封电子邮件 

```POST: /Mail/ReplyTo/{messageId}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|邮件 Id|字符串|是的|路径|无|若要回复的电子邮件的 id|
|注释|字符串|是的|查询|无|答复批注|
|全部答复|布尔值|不|查询|假|答复所有收件人|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="getattachment"></a>GetAttachment
获取附件︰ 按 id 检索电子邮件的附件 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|邮件 Id|字符串|是的|路径|无|电子邮件 id|
|attachmentId|字符串|是的|路径|无|若要下载该附件的 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="onnewemail"></a>OnNewEmail
在新的电子邮件︰ 新电子邮件到达时触发流程 

```GET: /Mail/OnNewEmail``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|采用文件夹路径|字符串|不|查询|收件箱|要检索的电子邮件文件夹 (默认︰ 收件箱)|
|自|字符串|不|查询|无|收件人电子邮件地址|
|从|字符串|不|查询|无|发件人地址|
|重要性|字符串|不|查询|普通|（高、 普通、 低） 的电子邮件的重要性 (默认︰ 正常)|
|fetchOnlyWithAttachment|布尔值|不|查询|假|检索只带有附件的电子邮件|
|includeAttachments|布尔值|不|查询|假|包含附件|
|subjectFilter|字符串|不|查询|无|要在主题中查找字符串|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作已成功完成|
|202|接受|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
使用选项发送电子邮件︰ 发送一封电子邮件有多个选项，并等待收件人响应回用另一种 

```POST: /mailwithoptions/$subscriptions``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |是的|正文|无|订阅请求电子邮件选项|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|201|创建预订|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="sendapprovalmail"></a>SendApprovalMail
发送审批电子邮件︰ 发送电子邮件的审批，并等待来自收件人的响应 

```POST: /approvalmail/$subscriptions``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |是的|正文|无|审批电子邮件订阅请求|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|201|创建预订|
|400|BadRequest|
|401|未经授权|
|403|禁止访问|
|500|内部服务器错误|
|默认|操作失败。|


## <a name="calendargettables"></a>CalendarGetTables
获取日历︰ 检索日历 

```GET: /datasets/calendars/tables``` 

此调用没有参数
#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="calendargetitems"></a>CalendarGetItems
获取事件︰ 将项从一个日历检索 

```GET: /datasets/calendars/tables/{table}/items``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|要检索的日历的唯一标识符|
|$filter|字符串|不|查询|无|ODATA 筛选查询以限制的条目数|
|$orderby|字符串|不|查询|无|指定项的顺序的 ODATA orderBy 查询|
|$skip|整数|不|查询|无|若要跳过的条目数 (默认值 = 0)|
|$top|整数|不|查询|无|要检索的项的最大数量 (默认值 = 256)|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="calendarpostitem"></a>CalendarPostItem
创建事件︰ 创建新的事件 

```POST: /datasets/calendars/tables/{table}/items``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|日历的唯一标识符|
|项目| |是的|正文|无|要创建的日历项目|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="calendargetitem"></a>CalendarGetItem
获取事件︰ 从日历中检索特定项 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|日历的唯一标识符|
|标识|字符串|是的|路径|无|要检索的日历项的唯一标识符|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
删除事件︰ 删除日历项目 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|日历的唯一标识符|
|标识|字符串|是的|路径|无|要删除的日历项的唯一标识符|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="calendarpatchitem"></a>CalendarPatchItem
更新事件︰ 部分更新的日历项目 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|日历的唯一标识符|
|标识|字符串|是的|路径|无|要更新的日历项的唯一标识符|
|项目| |是的|正文|无|要更新的日历项|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
在新项目上︰ 在创建新的日历项时触发 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|日历的唯一标识符|
|$filter|字符串|不|查询|无|ODATA 筛选查询以限制的条目数|
|$orderby|字符串|不|查询|无|指定项的顺序的 ODATA orderBy 查询|
|$skip|整数|不|查询|无|若要跳过的条目数 (默认值 = 0)|
|$top|整数|不|查询|无|要检索的项的最大数量 (默认值 = 256)|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
更新项目︰ 在日历项目被修改时触发 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|日历的唯一标识符|
|$filter|字符串|不|查询|无|ODATA 筛选查询以限制的条目数|
|$orderby|字符串|不|查询|无|指定项的顺序的 ODATA orderBy 查询|
|$skip|整数|不|查询|无|若要跳过的条目数 (默认值 = 0)|
|$top|整数|不|查询|无|要检索的项的最大数量 (默认值 = 256)|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="contactgettables"></a>ContactGetTables
获取联系人文件夹︰ 检索联系人文件夹 

```GET: /datasets/contacts/tables``` 

此调用没有参数
#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="contactgetitems"></a>ContactGetItems
获取联系人︰ 从联系人文件夹中检索联系人 

```GET: /datasets/contacts/tables/{table}/items``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|要检索的联系人文件夹的唯一标识符|
|$filter|字符串|不|查询|无|ODATA 筛选查询以限制的条目数|
|$orderby|字符串|不|查询|无|指定项的顺序的 ODATA orderBy 查询|
|$skip|整数|不|查询|无|若要跳过的条目数 (默认值 = 0)|
|$top|整数|不|查询|无|要检索的项的最大数量 (默认值 = 256)|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="contactpostitem"></a>ContactPostItem
创建联系人︰ 创建新的联系人 

```POST: /datasets/contacts/tables/{table}/items``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|联系人文件夹的唯一标识符|
|项目| |是的|正文|无|若要创建的联系人|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="contactgetitem"></a>ContactGetItem
获取联系人︰ 从联系人文件夹中检索特定的联系人 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|联系人文件夹的唯一标识符|
|标识|字符串|是的|路径|无|要检索的联系人的唯一标识符|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="contactdeleteitem"></a>ContactDeleteItem
删除联系人︰ 删除联系人 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|联系人文件夹的唯一标识符|
|标识|字符串|是的|路径|无|若要删除的联系人的唯一标识符|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="contactpatchitem"></a>ContactPatchItem
更新联系人︰ 部分更新联系人 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|表|字符串|是的|路径|无|联系人文件夹的唯一标识符|
|标识|字符串|是的|路径|无|若要更新的联系人的唯一标识符|
|项目| |是的|正文|无|要更新的联系人项|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|默认|操作失败。|


## <a name="object-definitions"></a>对象定义 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [较 [字符串、 对象]]


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="object"></a>对象


| 属性名称 | 数据类型 | 必填 |
|---|---|---|



### <a name="sendmessage"></a>发送消息


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|附件|数组|不 |
|从|字符串|不 |
|抄送|字符串|不 |
|密件抄送|字符串|不 |
|主题|字符串|是的 |
|正文|字符串|是的 |
|重要性|字符串|不 |
|IsHtml|布尔值|不 |
|自|字符串|是的 |



### <a name="sendattachment"></a>SendAttachment


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|@odata.type|字符串|不 |
|名称|字符串|是的 |
|ContentBytes|字符串|是的 |



### <a name="receivemessage"></a>ReceiveMessage


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|IsRead|布尔值|不 |
|HasAttachment|布尔值|不 |
|DateTimeReceived|字符串|不 |
|附件|数组|不 |
|从|字符串|不 |
|抄送|字符串|不 |
|密件抄送|字符串|不 |
|主题|字符串|是的 |
|正文|字符串|是的 |
|重要性|字符串|不 |
|IsHtml|布尔值|不 |
|自|字符串|是的 |



### <a name="receiveattachment"></a>ReceiveAttachment


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|是的 |
|ContentType|字符串|是的 |
|@odata.type|字符串|不 |
|名称|字符串|是的 |
|ContentBytes|字符串|是的 |



### <a name="digestmessage"></a>DigestMessage


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|主题|字符串|是的 |
|正文|字符串|不 |
|重要性|字符串|不 |
|摘要|数组|是的 |
|附件|数组|不 |
|自|字符串|是的 |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|表格格式|未定义|不 |
|blob|未定义|不 |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|来源|字符串|不 |
|显示名称|字符串|不 |
|urlEncoding|字符串|不 |
|tableDisplayName|字符串|不 |
|tablePluralName|字符串|不 |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|来源|字符串|不 |
|显示名称|字符串|不 |
|urlEncoding|字符串|不 |



### <a name="tablemetadata"></a>TableMetadata


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|不 |
|标题|字符串|不 |
|ms 的 x 权限|字符串|不 |
|ms 的 x 功能|未定义|不 |
|架构|未定义|不 |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|sortRestrictions|未定义|不 |
|filterRestrictions|未定义|不 |
|filterFunctions|数组|不 |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|可排序|布尔值|不 |
|unsortableProperties|数组|不 |
|ascendingOnlyProperties|数组|不 |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|可筛选|布尔值|不 |
|nonFilterableProperties|数组|不 |
|requiredProperties|数组|不 |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|NotificationUrl|字符串|不 |
|消息|未定义|不 |



### <a name="messagewithoptions"></a>MessageWithOptions


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|主题|字符串|是的 |
|选项|字符串|是的 |
|正文|字符串|不 |
|重要性|字符串|不 |
|附件|数组|不 |
|自|字符串|是的 |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|资源|字符串|不 |
|notificationType|字符串|不 |
|notificationUrl|字符串|不 |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|NotificationUrl|字符串|不 |
|消息|未定义|不 |



### <a name="approvalmessage"></a>ApprovalMessage


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|主题|字符串|是的 |
|选项|字符串|是的 |
|正文|字符串|不 |
|重要性|字符串|不 |
|附件|数组|不 |
|自|字符串|是的 |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|SelectedOption|字符串|不 |



### <a name="tableslist"></a>TablesList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="table"></a>表


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|不 |
|显示名称|字符串|不 |



### <a name="item"></a>项目


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|ItemInternalId|字符串|不 |



### <a name="calendaritemslist"></a>CalendarItemsList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="calendaritem"></a>日历项目


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|ItemInternalId|字符串|不 |



### <a name="contactitemslist"></a>ContactItemsList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="contactitem"></a>联系人


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|ItemInternalId|字符串|不 |



### <a name="datasetslist"></a>DataSetsList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="dataset"></a>数据集


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|不 |
|显示名称|字符串|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)