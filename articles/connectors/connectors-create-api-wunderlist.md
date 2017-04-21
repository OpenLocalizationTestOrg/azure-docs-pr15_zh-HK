<properties
pageTitle="Wunderlist |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 Wunderlist 提供 todo 列表和任务管理器以帮助用户完成其任务。  无论您用一个亲人共享购物清单，从事一个项目，或计划休假，Wunderlist 容易地获取、 共享和完成您的 to¬dos。 Wunderlist 可立即同步您电话，tablet 计算机之间，因此您可以从任何位置访问您的所有任务。"
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

# <a name="get-started-with-the-wunderlist-connector"></a>开始使用 Wunderlist 连接器

Wunderlist 提供 todo 列表和任务管理器以帮助用户完成其任务。  无论您用一个亲人共享购物清单，从事一个项目，或计划休假，Wunderlist 容易地获取、 共享和完成您的 to¬dos。 Wunderlist 可立即同步您电话，tablet 计算机之间，因此您可以从任何位置访问您的所有任务。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

Wunderlist 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 Wunderlist 连接器具有以下操作和/或可用触发器︰

### <a name="wunderlist-actions"></a>Wunderlist 操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|检索与您的帐户相关联的列表。|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|创建一个列表。|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|检索特定列表中的任务。|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|创建任务|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|从特定的列表或特定任务检索子任务。|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|创建子任务内某项特定任务|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|检索对特定列表或特定任务的说明。|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|向特定任务添加备注|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|检索对特定列表或特定的任务的任务注释。|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|将注释添加到特定的任务|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|检索对特定列表或特定任务的提醒。|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|设置提醒。|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|检索文件的特定列表或特定的任务。|
|[GetList](connectors-create-api-wunderlist.md#getlist)|检索特定列表|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|删除列表|
|[UpdateList](connectors-create-api-wunderlist.md#updatelist)|更新特定列表|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|检索一个特定的任务|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|更新特定任务|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|删除特定的任务|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|检索一个特定的子任务|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|更新特定的子任务|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|删除一个特定的子任务|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|检索特定记录|
|[UpdateNote](connectors-create-api-wunderlist.md#updatenote)|更新特定的注解|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|删除特定的注解|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|检索特定任务注释|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|更新特定的提醒|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|删除特定的提醒|
### <a name="wunderlist-triggers"></a>Wunderlist 触发器
您可以侦听这些事件︰

|触发器 | 说明|
|--- | ---|
|当任务的截止日期|在列表中任务的截止日期时，将触发新的流|
|当创建新任务|在列表中创建新任务时，将触发新的流|
|当发生时提醒|当提醒发生时将触发新的流|


## <a name="create-a-connection-to-wunderlist"></a>创建到 Wunderlist 的连接
要使用 Wunderlist 创建逻辑的应用程序，必须先创建**连接**，然后提供下列属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供 Wunderlist 凭据|
创建此连接后，可以使用它来执行操作并聆听有这篇文章中所述的触发器。 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="reference-for-wunderlist"></a>Wunderlist 的引用
适用于版本︰ 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
当任务的截止日期︰ 列表中的任务的截止日期时，触发新的流 

```GET: /trigger/tasksdue``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|


## <a name="triggertasknew"></a>TriggerTaskNew
当创建一个新任务︰ 在列表中创建新任务时，将触发新的流 

```GET: /trigger/tasksnew``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|


## <a name="triggerreminder"></a>TriggerReminder
当发生时提醒︰ 提醒发生时将触发新的流 

```GET: /trigger/reminders``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|task_id|整数|不|查询|无|任务 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|


## <a name="retrievelists"></a>RetrieveLists
获取列表︰ 检索与您的帐户相关联的列表。 

```GET: /lists``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|400|错误的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="createlist"></a>CreateList
创建列表︰ 创建一个列表。 

```POST: /lists``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|发布| |是的|正文|无|要创建新列表|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|默认|操作失败。|


## <a name="listtasks"></a>ListTasks
获取任务︰ 从特定列表检索任务。 

```GET: /tasks``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|完成|布尔值|不|查询|无|完成|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|400|错误的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="createtask"></a>CreateTask
创建一个任务︰ 创建任务 

```POST: /tasks``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|发布| |是的|正文|无|创建新任务|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|201|创建|


## <a name="listsubtasks"></a>ListSubTasks
获取子任务︰ 从特定列表或特定任务检索子任务。 

```GET: /subtasks``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|task_id|整数|不|查询|无|任务 ID|
|完成|布尔值|不|查询|无|完成|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|400|错误的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="createsubtask"></a>CreateSubTask
创建子任务︰ 创建内某项特定任务的子任务 

```POST: /subtasks``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|发布| |是的|正文|无|新创建的子任务|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|201|创建|


## <a name="listnotes"></a>ListNotes
笔记︰ 检索对特定列表或特定任务的说明。 

```GET: /notes``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|task_id|整数|不|查询|无|任务 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|400|错误的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="createnote"></a>CreateNote
创建注释︰ 添加注释到特定的任务 

```POST: /notes``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|发布| |是的|正文|无|要创建的新注解|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|201|创建|


## <a name="listcomments"></a>ListComments
获取任务注释︰ 检索任务注释的特定列表或特定的任务。 

```GET: /task_comments``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|task_id|整数|不|查询|无|任务 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|400|错误的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="createcomment"></a>CreateComment
向任务添加注释︰ 添加注释到特定的任务 

```POST: /task_comments``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|发布| |是的|正文|无|要创建的新任务注释|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|201|创建|


## <a name="retrievereminders"></a>RetrieveReminders
得到提醒︰ 检索特定列表或特定任务的提醒。 

```GET: /reminders``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|task_id|整数|不|查询|无|任务 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|400|错误的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="createreminder"></a>CreateReminder
设置提醒︰ 设置提醒。 

```POST: /reminders``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|发布| |是的|正文|无|要创建的新提醒|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|默认|操作失败。|


## <a name="retrievefiles"></a>RetrieveFiles
获取文件︰ 检索文件的特定列表或特定的任务。 

```GET: /files``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|task_id|整数|不|查询|无|任务 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|操作成功|
|400|错误的请求|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|


## <a name="getlist"></a>GetList
获取列表︰ 检索特定列表 

```GET: /lists/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径|无|列表 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="deletelist"></a>DeleteList
删除列表︰ 删除列表 

```DELETE: /lists/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|列表 ID|
|修订|整数|是的|查询|无|修订|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|204|无内容|


## <a name="updatelist"></a>UpdateList
更新列表︰ 更新特定列表 

```PATCH: /lists/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|列表 ID|
|发布| |是的|正文|无|列表详细信息|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="gettask"></a>GetTask
获取任务︰ 检索特定的任务 

```GET: /tasks/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|标识|整数|是的|路径|无|任务 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="updatetask"></a>UpdateTask
更新任务︰ 更新特定任务 

```PATCH: /tasks/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|标识|整数|是的|路径|无|任务 ID|
|发布| |是的|正文|无|任务详细信息|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="deletetask"></a>DeleteTask
删除任务︰ 删除特定的任务 

```DELETE: /tasks/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|list_id|整数|是的|查询|无|列表 ID|
|标识|整数|是的|路径|无|任务 ID|
|修订|整数|是的|查询|无|修订|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|204|无内容|


## <a name="getsubtask"></a>GetSubTask
获取子任务︰ 检索特定的子任务 

```GET: /subtasks/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径|无|子任务 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="updatesubtask"></a>UpdateSubTask
更新子任务︰ 更新特定的子任务 

```PATCH: /subtasks/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|子任务 ID|
|发布| |是的|正文|无|子任务详细信息|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="deletesubtask"></a>DeleteSubTask
删除子任务︰ 删除一个特定的子任务 

```DELETE: /subtasks/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|子任务 ID|
|修订|整数|是的|查询|无|修订|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|204|无内容|


## <a name="getnote"></a>GetNote
获取注释︰ 检索特定记录 

```GET: /notes/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径|无|注意 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="updatenote"></a>UpdateNote
更新注释︰ 更新特定的注解 

```PATCH: /notes/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|注意 ID|
|发布| |是的|正文|无|注意详细信息|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="deletenote"></a>DeleteNote
删除注释︰ 删除特定的注解 

```DELETE: /notes/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|注意 ID|
|修订|整数|是的|查询|无|修订|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|204|无内容|


## <a name="getcomment"></a>GetComment
获取任务注释︰ 检索特定任务注释 

```GET: /task_comments/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|字符串|是的|路径|无|注释 ID|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="updatereminder"></a>UpdateReminder
更新提醒︰ 更新特定的提醒 

```PATCH: /reminders/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|提醒的 ID|
|发布| |是的|正文|无|提醒的详细信息|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|200|还行|


## <a name="deletereminder"></a>DeleteReminder
删除提醒︰ 删除特定的提醒 

```DELETE: /reminders/{id}``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|标识|整数|是的|路径|无|提醒的 ID。|
|修订|整数|是的|查询|无|修订|

#### <a name="response"></a>响应

|名称|说明|
|---|---|
|204|无内容|


## <a name="object-definitions"></a>对象定义 

### <a name="list"></a>列表


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|created_at|字符串|不 |
|标题|字符串|不 |
|list_type|字符串|不 |
|类型|字符串|不 |
|修订|整数|不 |



### <a name="createdlist"></a>CreatedList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|created_at|字符串|不 |
|标题|字符串|不 |
|修订|整数|不 |
|类型|字符串|不 |



### <a name="task"></a>任务


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|assignee_id|整数|不 |
|assigner_id|整数|不 |
|created_at|字符串|不 |
|created_by_id|整数|不 |
|due_date|字符串|不 |
|list_id|整数|不 |
|修订|整数|不 |
|星形|布尔值|不 |
|标题|字符串|不 |



### <a name="subtask"></a>子任务


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|task_id|整数|不 |
|created_at|字符串|不 |
|created_by_id|整数|不 |
|修订|字符串|不 |
|标题|字符串|不 |



### <a name="note"></a>请注意


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|task_id|整数|不 |
|内容|字符串|不 |
|created_at|字符串|不 |
|updated_at|字符串|不 |
|修订|整数|不 |



### <a name="comment"></a>注释


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|task_id|整数|不 |
|修订|整数|不 |
|文本|字符串|不 |
|类型|字符串|不 |
|created_at|字符串|不 |



### <a name="reminder"></a>提醒


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|日期|字符串|不 |
|task_id|整数|不 |
|修订|整数|不 |
|类型|字符串|不 |
|created_at|字符串|不 |
|updated_at|字符串|不 |



### <a name="createdreminder"></a>CreatedReminder


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|日期|字符串|不 |
|task_id|整数|不 |
|修订|整数|不 |
|created_at|字符串|不 |
|updated_at|字符串|不 |



### <a name="file"></a>文件


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标识|整数|不 |
|url|字符串|不 |
|task_id|整数|不 |
|list_id|整数|不 |
|user_id|整数|不 |
|文件名|字符串|不 |
|content_type|字符串|不 |
|file_size|整数|不 |
|local_created_at|字符串|不 |
|created_at|字符串|不 |
|updated_at|字符串|不 |
|类型|字符串|不 |
|修订|整数|不 |



### <a name="newtask"></a>NewTask


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|list_id|整数|是的 |
|标题|字符串|是的 |
|assignee_id|整数|不 |
|完成|布尔值|不 |
|recurrence_type|字符串|不 |
|recurrence_count|整数|不 |
|due_date|字符串|不 |
|星形|布尔值|不 |



### <a name="newlist"></a>NewList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标题|字符串|是的 |



### <a name="newsubtask"></a>NewSubtask


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|list_id|整数|是的 |
|task_id|整数|是的 |
|标题|字符串|是的 |
|完成|布尔值|不 |



### <a name="newnote"></a>NewNote


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|list_id|整数|是的 |
|task_id|整数|是的 |
|内容|字符串|是的 |



### <a name="newcomment"></a>NewComment


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|list_id|整数|是的 |
|task_id|整数|是的 |
|文本|字符串|是的 |



### <a name="newreminder"></a>NewReminder


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|list_id|整数|是的 |
|task_id|整数|是的 |
|日期|字符串|是的 |



### <a name="updatetask"></a>UpdateTask


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|修订|整数|不 |
|标题|字符串|不 |
|assignee_id|整数|不 |
|完成|布尔值|不 |
|recurrence_type|字符串|不 |
|recurrence_count|整数|不 |
|due_date|字符串|不 |
|星形|布尔值|不 |



### <a name="updatelist"></a>UpdateList


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|修订|整数|不 |
|标题|字符串|不 |



### <a name="updatesubtask"></a>UpdateSubtask


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|修订|整数|不 |
|标题|字符串|不 |
|完成|布尔值|不 |



### <a name="updatenote"></a>UpdateNote


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|修订|整数|不 |
|内容|字符串|不 |



### <a name="updatereminder"></a>UpdateReminder


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|日期|字符串|不 |
|修订|整数|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)