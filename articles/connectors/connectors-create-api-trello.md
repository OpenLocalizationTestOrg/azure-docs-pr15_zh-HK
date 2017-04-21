<properties
pageTitle="Trello |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 Trello 提供在所有项目中，在工作和家庭的角度来看。  它是轻松、 自由、 灵活和视觉方法管理项目和组织的任何内容。  连接到 Trello 来管理您的主板列表和卡"
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

# <a name="get-started-with-the-trello-connector"></a>开始使用 Trello 连接器

Trello 提供在所有项目中，在工作和家庭的角度来看。  它是轻松、 自由、 灵活和视觉方法管理项目和组织的任何内容。  连接到 Trello 来管理您的主板列表和卡。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

Trello 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 Trello 连接器具有以下操作和/或可用触发器︰

### <a name="trello-actions"></a>Trello 操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[ListCards](connectors-create-api-trello.md#listcards)|在板中的列表卡|
|[GetCard](connectors-create-api-trello.md#getcard)|通过 id 获取卡|
|[UpdateCard](connectors-create-api-trello.md#updatecard)|更新显卡|
|[DeleteCard](connectors-create-api-trello.md#deletecard)|删除卡|
|[CreateCard](connectors-create-api-trello.md#createcard)|您的 trello 帐户中创建一个新卡|
|[ListBoards](connectors-create-api-trello.md#listboards)|列表的主板|
|[GetBoard](connectors-create-api-trello.md#getboard)|通过 Id 获取主板|
|[ListLists](connectors-create-api-trello.md#listlists)|列出在板卡列表|
|[GetList](connectors-create-api-trello.md#getlist)|获取按 Id 列表|
### <a name="trello-triggers"></a>Trello 触发器
您可以侦听这些事件︰

|触发器 | 说明|
|--- | ---|
|当新的卡被添加到一个板|新卡添加到板时，将触发一个流|
|当向列表中添加新卡|向列表添加新卡时触发流程|


## <a name="create-a-connection-to-trello"></a>创建到 Trello 的连接
要使用 Trello 创建逻辑的应用程序，必须先创建**连接**，然后提供下列属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供 Trello 凭据|
创建此连接后，可以使用它来执行操作并聆听有这篇文章中所述的触发器。 

>[AZURE.INCLUDE [Steps to create a connection to Trello](../../includes/connectors-create-api-trello.md)]

>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="reference-for-trello"></a>Trello 的引用
适用于版本︰ 1.0

## <a name="onnewcardinboard"></a>OnNewCardInBoard
当新的卡被添加到一个板︰ 新卡添加到板时，将触发一个流 

```GET: /trigger/boards/{board_id}/cards``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|路径|无|获取在卡板的唯一 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="onnewcardinlist"></a>OnNewCardInList
当向列表中添加新卡︰ 新卡被添加到列表时，将触发一个流 

```GET: /trigger/lists/{list_id}/cards``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|查询|无|获取在卡板的唯一 id|
|list_id|字符串|是的|路径|无|要获取卡中的列表的唯一 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="listcards"></a>ListCards
列表中板卡︰ 列表中板卡 

```GET: /boards/{board_id}/cards``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|路径|无|获取所有卡板的 id|
|操作|字符串|不|查询|无|列出要返回的操作。 指定全部或无效值的逗号分隔列表|
|附件|布尔值|不|查询|无|显示附件|
|attachment_fields|字符串|不|查询|无|列出要返回的附件字段。 指定全部或无效值的逗号分隔列表|
|不干胶标签|布尔值|不|查询|无|显示标签|
|成员|布尔值|不|查询|无|显示成员|
|memeber_fields|字符串|不|查询|无|列出要返回的成员字段。 指定全部或无效值的逗号分隔列表|
|CheckItemStates|布尔值|不|查询|无|返回卡状态|
|检查表|字符串|不|查询|无|显示清单|
|限制|整数|不|查询|无|要返回的结果中，1 和 1000年之间的最大数量|
|因为|字符串|不|查询|无|在此日期之后获取所有卡|
|之前|字符串|不|查询|无|获取在此日期之前的所有卡|
|筛选器|字符串|不|查询|无|筛选器的响应|
|字段|字符串|不|查询|无|列出卡字段返回。 指定全部或无效值的逗号分隔列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="getcard"></a>GetCard
通过 id 获取卡︰ 通过 id 获取卡 

```GET: /cards/{card_id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|查询|无|获取在卡板的 id|
|card_id|字符串|是的|路径|无|Id 卡要获取的|
|操作|字符串|不|查询|无|列出要返回的操作。 指定全部或无效值的逗号分隔列表|
|actions_entities|布尔值|不|查询|无|退货处理实体|
|actions_display|布尔值|不|查询|无|退货处理显示|
|actions_limit|整数|不|查询|无|操作返回的最大数目|
|action_fields|字符串|不|查询|无|要返回的每个操作的操作域的列表。 指定全部或无效值的逗号分隔列表|
|action_memberCreator_fields|字符串|不|查询|无|操作返回的成员创建者字段的列表。 指定全部或无效值的逗号分隔列表|
|附件|布尔值|不|查询|无|返回附件|
|attachement_fields|字符串|不|查询|无|要返回的每个附件的附件字段的列表。 指定全部或无效值的逗号分隔列表|
|成员|布尔值|不|查询|无|返回成员|
|member_fields|字符串|不|查询|无|要返回的每个成员的成员字段的列表。 指定全部或无效值的逗号分隔列表|
|membersVoted|布尔值|不|查询|无|返回投票成员|
|memberVoted_fields|字符串|不|查询|无|返回每个 voted 成员的投票成员字段的列表。 指定全部或无效值的逗号分隔列表|
|checkItemStates|布尔值|不|查询|无|返回卡状态|
|checkItemState_fields|字符串|不|查询|无|若要为每个卡项状态返回的状态字段的列表。 指定全部或无效值的逗号分隔列表|
|检查表|字符串|不|查询|无|返回清单|
|checklist_fields|字符串|不|查询|无|检查表字段要返回的每个检查表的列表。 指定全部或无效值的逗号分隔列表|
|主板|布尔值|不|查询|无|返回属于卡板|
|board_fields|字符串|不|查询|无|列出台式机主板字段返回。 指定全部或无效值的逗号分隔列表|
|列表|布尔值|不|查询|无|返回列表属于卡|
|list_fields|字符串|不|查询|无|列表返回的列表域。 指定全部或无效值的逗号分隔列表|
|不干胶标签|布尔值|不|查询|无|返回的不干胶标签|
|sticker_fields|字符串|不|查询|无|列出要返回的每个标签的不干胶标签字段。 指定全部或无效值的逗号分隔列表|
|字段|字符串|不|查询|无|列出卡字段返回。 指定全部或无效值的逗号分隔列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="updatecard"></a>UpdateCard
更新卡︰ 更新卡 

```PUT: /cards/{card_id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|查询|无|获取从卡板的 id|
|card_id|字符串|是的|路径|无|Id 卡要更新的|
|updateCard| |是的|正文|无|更新的卡值|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="deletecard"></a>DeleteCard
删除卡︰ 删除卡 

```DELETE: /cards/{card_id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|查询|无|获取从卡板的 id|
|card_id|字符串|是的|路径|无|要删除的卡的 id|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="createcard"></a>CreateCard
创建卡︰ 在您的 trello 帐户中创建一个新卡 

```POST: /cards``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|查询|无|若要创建在卡板的唯一 id|
|newCard| |是的|正文|无|新卡，以添加到 trello 板|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="listboards"></a>ListBoards
列出主板︰ 列出主板 

```GET: /member/me/boards``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|筛选器|字符串|不|查询|无|列表筛选器将应用于板的结果。 指定全部或无效值的逗号分隔列表|
|字段|字符串|不|查询|无|列出台式机主板字段返回。 指定全部或无效值的逗号分隔列表|
|操作|字符串|不|查询|无|列出操作字段返回。 指定全部或无效值的逗号分隔列表|
|actions_entities|布尔值|不|查询|无|退货处理实体|
|actions_limit|整数|不|查询|无|操作返回的最大数目|
|actions_format|字符串|不|查询|无|指定的操作后，返回的格式|
|actions_since|字符串|不|查询|无|在指定的日期之后返回操作|
|action_fields|字符串|不|查询|无|列出该操作返回的字段。 指定全部或无效值的逗号分隔列表|
|成员资格|字符串|不|查询|无|指定要返回的成员资格信息。 指定全部或无效值的逗号分隔列表|
|组织|布尔值|不|查询|无|指定要返回的组织信息|
|organization_fields|字符串|不|查询|无|列出组织字段返回。 指定全部或无效值的逗号分隔列表|
|列表|字符串|不|查询|无|指定是否返回属于主板列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="getboard"></a>GetBoard
获取主板 id︰ 获取主板 id 

```GET: /boards/{board_id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|路径|无|获取该板的唯一 id|
|操作|字符串|不|查询|无|列出要返回的操作。 指定全部或无效值的逗号分隔列表|
|action_entities|布尔值|不|查询|无|指定是否返回操作的实体|
|actions_display|布尔值|不|查询|无|指定是否返回操作显示|
|actions_format|字符串|不|查询|无|指定的操作后，返回的格式|
|actions_since|字符串|不|查询|无|只有在此日期之后返回操作|
|actions_limit|整数|不|查询|无|操作返回的最大数目|
|action_fields|字符串|不|查询|无|列表返回的每个字段的字段。 指定全部或无效值的逗号分隔列表|
|action_memeber|布尔值|不|查询|无|指定是否返回操作成员|
|action_member_fields|字符串|不|查询|无|列出成员字段返回操作的每个成员。 指定全部或无效值的逗号分隔列表|
|action_memberCreator|布尔值|不|查询|无|指定是否返回操作成员创建者|
|action_memberCreator_fields|字符串|不|查询|无|列出要返回的操作成员创建者字段。 指定全部或无效值的逗号分隔列表|
|卡|字符串|不|查询|无|指定要返回的卡|
|card_fields|字符串|不|查询|无|列出为每个卡返回的字段。 指定全部或无效值的逗号分隔列表|
|card_attachments|布尔值|是的|查询|无|指定是否以卡返回附件|
|card_attachment_fields|字符串|不|查询|无|列出要返回的每个附件的附件字段。 指定全部或无效值的逗号分隔列表|
|card_checklists|字符串|不|查询|无|指定要为每个卡返回的清单|
|card_stickers|布尔值|不|查询|无|指定是否返回卡贴纸|
|boardStarts|字符串|不|查询|无|指定要返回板星|
|标签|字符串|不|查询|无|指定要返回的标签|
|label_fields|字符串|不|查询|无|列出要返回的每个标签的标签字段。 指定全部或无效值的逗号分隔列表|
|labels_limits|整数|不|查询|无|标签可返回的最大数目|
|列表|字符串|不|查询|无|指定要返回的列表|
|list_fields|字符串|不|查询|无|列表返回的每个列表的列表域。 指定全部或无效值的逗号分隔列表|
|成员资格|字符串|不|查询|无|列出要返回的成员资格。 指定全部或无效值的逗号分隔列表|
|memberships_member|布尔值|不|查询|无|指定是否返回成员资格的成员|
|memberships_member_fields|字符串|不|查询|无|列出要返回的每个成员资格成员的成员字段。 指定全部或无效值的逗号分隔列表|
|成员|字符串|不|查询|无|列出了要返回的成员|
|member_fields|字符串|不|查询|无|列出成员字段要返回的每个成员。 指定全部或无效值的逗号分隔列表|
|membersInvited|字符串|不|查询|无|指定要返回的被邀请的成员|
|membersInvited_fields|字符串|不|查询|无|列出每个返回的字段。 指定全部或无效值的逗号分隔列表|
|检查表|字符串|不|查询|无|指定要返回的清单|
|checklist_fields|字符串|不|查询|无|列出清单字段要返回的每个检查表。 指定全部或无效值的逗号分隔列表|
|组织|布尔值|不|查询|无|指定是否返回的组织信息|
|organization_fields|字符串|不|查询|无|列出组织字段返回每个组织。 指定全部或无效值的逗号分隔列表|
|organization_memberships|字符串|不|查询|无|列表返回的组织成员身份。 指定全部或无效值的逗号分隔列表|
|myPerfs|布尔值|不|查询|无|指定是否返回我 perfs|
|字段|字符串|不|查询|无|列出要返回的字段。 指定全部或无效值的逗号分隔列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="listlists"></a>ListLists
列出在板卡列表︰ 列出在板卡列表 

```GET: /boards/{board_id}/lists``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|路径|无|列出的主板以获取唯一 id|
|卡|字符串|不|查询|无|指定要返回的卡|
|card_fields|字符串|不|查询|无|列出从返回卡字段。 指定全部或无效值的逗号分隔列表|
|筛选器|字符串|不|查询|无|指定列表的筛选器属性|
|字段|字符串|不|查询|无|列出要返回的字段。 指定全部或无效值的逗号分隔列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="getlist"></a>GetList
通过 Id 获取列表︰ 通过 Id 获取列表 

```GET: /lists/{list_id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|board_id|字符串|是的|查询|无|获取列表从板的唯一 id|
|list_id|字符串|是的|路径|无|要读取的列表的唯一 id|
|卡|字符串|不|查询|无|指定要返回的卡|
|card_fields|字符串|不|查询|无|列出为每个卡返回卡字段。 指定全部或无效值的逗号分隔列表|
|主板|布尔值|不|查询|无|指定是否返回服务器主板的信息。|
|board_fields|字符串|不|查询|无|列出台式机主板字段返回。 指定全部或无效值的逗号分隔列表|
|字段|字符串|不|查询|无|列表返回的列表域。 指定全部或无效值的逗号分隔列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败|


## <a name="object-definitions"></a>对象定义 

### <a name="card"></a>卡


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|checkItemStates|数组|不 |
|关闭|布尔值|不 |
|dateLastActivity|字符串|不 |
|降序|字符串|不 |
|idBoard|字符串|不 |
|idList|字符串|不 |
|idMembersVoted|数组|不 |
|idShort|整数|不 |
|idAttachmentCover|字符串|不 |
|manualCoverAttachment|布尔值|不 |
|idLabels|数组|不 |
|名称|字符串|不 |
|pos|整数|不 |
|shortLink|字符串|不 |
|徽章|未定义|不 |
|截止|字符串|不 |
|电子邮件|字符串|不 |
|shortUrl|字符串|不 |
|订阅|布尔值|不 |
|url|字符串|不 |



### <a name="badges"></a>徽章


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|投票|整数|不 |
|ViewingMemberVoted|布尔值|不 |
|订阅|布尔值|不 |
|Fogbugz|字符串|不 |
|CheckItems|整数|不 |
|CheckItemsChecked|整数|不 |
|注释|整数|不 |
|附件|整数|不 |
|说明|布尔值|不 |
|截止|字符串|不 |



### <a name="object"></a>对象


| 属性名称 | 数据类型 | 必填 |
|---|---|---|



### <a name="createcard"></a>CreateCard


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|idList|字符串|是的 |
|名称|字符串|是的 |
|降序|字符串|不 |
|pos|字符串|不 |
|idMembers|字符串|不 |
|idLabels|字符串|不 |
|urlSource|字符串|不 |
|文件源|字符串|不 |
|idCardSource|字符串|不 |
|keepFromSource|字符串|不 |



### <a name="updatecard"></a>UpdateCard


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|不 |
|降序|字符串|不 |
|关闭|布尔值|不 |
|idMembers|字符串|不 |
|idAttachmentCover|字符串|不 |
|idList|字符串|不 |
|idBoard|字符串|不 |
|pos|字符串|不 |
|截止|字符串|不 |
|订阅|布尔值|不 |



### <a name="board"></a>主板


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|关闭|布尔值|不 |
|dateLastActivity|字符串|不 |
|dateLastView|字符串|不 |
|降序|字符串|不 |
|idOrganization|字符串|不 |
|邀请|数组|不 |
|邀请|布尔值|不 |
|labelNames|未定义|不 |
|成员资格|数组|不 |
|名称|字符串|不 |
|固定|布尔值|不 |
|powerUps|数组|不 |
|perfs|未定义|不 |
|shortLink|字符串|不 |
|shortUrl|字符串|不 |
|星形|字符串|不 |
|订阅|字符串|不 |
|url|字符串|不 |



### <a name="label"></a>标签


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|绿色|字符串|不 |
|黄色|字符串|不 |
|橙色|字符串|不 |
|红色|字符串|不 |
|紫色|字符串|不 |
|蓝色|字符串|不 |
|天空|字符串|不 |
|酸橙色|字符串|不 |
|粉红色|字符串|不 |
|黑色|字符串|不 |



### <a name="membership"></a>成员资格


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|idMember|字符串|不 |
|memberType|字符串|不 |
|未确认|布尔值|不 |



### <a name="perfs"></a>Perfs


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|permissionLevel|字符串|不 |
|投票|字符串|不 |
|注释|字符串|不 |
|邀请|字符串|不 |
|selfJoin|布尔值|不 |
|cardCovers|布尔值|不 |
|calendarFeedEnabled|布尔值|不 |
|背景|字符串|不 |
|背景|字符串|不 |
|backgroundImage|字符串|不 |
|backgroundImageScaled|字符串|不 |
|backgroundTile|布尔值|不 |
|backgroundBrightness|字符串|不 |
|canBePublic|布尔值|不 |
|canBeOrg|布尔值|不 |
|canBePrivate|布尔值|不 |
|canInvite|布尔值|不 |



### <a name="list"></a>列表


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|字符串|不 |
|名称|字符串|不 |
|关闭|布尔值|不 |
|idBoard|字符串|不 |
|pos|编号|不 |
|订阅|布尔值|不 |
|卡|数组|不 |
|主板|未定义|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)