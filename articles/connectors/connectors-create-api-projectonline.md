<properties
pageTitle="ProjectOnline |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 项目在线是一个灵活的在线解决方案，为项目组合管理 (PPM) 和来自 Microsoft 的日常工作。 通过 Office 365 提供，在线项目使公司能够快速开始使用强大的项目管理能力，以规划、 设置优先级和管理项目和项目组合投资 — — 从几乎任何地方在几乎任何设备上。"
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

# <a name="get-started-with-the-projectonline-connector"></a>开始使用 ProjectOnline 连接器

项目在线是一个灵活的在线解决方案，为项目组合管理 (PPM) 和来自 Microsoft 的日常工作。 通过 Office 365 提供，在线项目使公司能够快速开始使用强大的项目管理能力，以规划、 设置优先级和管理项目和项目组合投资 — — 从几乎任何地方在几乎任何设备上。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

ProjectOnline 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 ProjectOnline 连接器具有以下操作和/或可用触发器︰

### <a name="projectonline-actions"></a>ProjectOnline 操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|列出项目在线网站中的项目|
|[创建项目](connectors-create-api-projectonline.md#createproject)|在您项目的在线站点中创建新项目|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|在您的项目中创建新任务|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|创建项目在线站点中的企业资源|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|列出了项目中的已发布的任务|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|签出网站中的项目|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|签入并发布现有网站中的项目|
### <a name="projectonline-triggers"></a>ProjectOnline 触发器
您可以侦听这些事件︰

|触发器 | 说明|
|--- | ---|
|当创建新项目|每次创建新项目时触发流程|
|当创建新的资源|在创建新的资源时，将触发新的流|
|当创建新任务|创建新任务时触发流程|


## <a name="create-a-connection-to-projectonline"></a>创建到 ProjectOnline 的连接
要使用 ProjectOnline 创建逻辑的应用程序，必须先创建**连接**，然后提供下列属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供 ProjectOnline 的凭据|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="reference-for-projectonline"></a>ProjectOnline 的引用
适用于版本︰ 1.0

## <a name="onnewproject"></a>OnNewProject
当创建一个新项目︰ 每次创建新项目时触发流程 

```GET: /trigger/_api/ProjectData/Projects``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
当创建新的资源︰ 在创建新的资源时，将触发新的流 

```GET: /trigger/_api/ProjectData/Resources``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
当创建一个新任务︰ 创建新任务时触发流程 

```GET: /trigger/_api/ProjectData/Tasks``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
列出项目︰ 列出项目在线网站中的项目 

```GET: /_api/ProjectServer/Projects``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>创建项目
创建新的项目︰ 项目在线站点中创建新项目 

```POST: /_api/ProjectServer/Projects``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|
|项目| |是的|正文|无|要创建新项目|

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


## <a name="createtask"></a>CreateTask
创建新的任务︰ 在您的项目中创建新任务 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|字符串|是的|路径|无|该项目的唯一 ID 添加到任务|
|任务| |是的|正文|无|向项目中添加新任务|

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


## <a name="createresource"></a>CreateResource
创建新的资源︰ 项目在线网站中创建企业资源 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|
|资源| |是的|正文|无|新的企业资源向项目添加|

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


## <a name="listtasks"></a>ListTasks
列出了任务︰ 列出了项目中的已发布的任务 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|字符串|是的|路径|无|项目的唯一 ID 来获取任务|

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


## <a name="checkoutproject"></a>CheckoutProject
签出项目︰ 签出网站中的项目 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|字符串|是的|路径|无|该项目的唯一 ID 添加到任务|

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


## <a name="publishproject"></a>PublishProject
签入并发布项目︰ 签入并发布及现有网站中的项目 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|siteUrl|字符串|是的|查询|无|根网站的项目网站的 url (示例︰ https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|字符串|是的|路径|无|签入项目的唯一 ID|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="triggerproject"></a>TriggerProject


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|ProjectStartDate|字符串|不 |
|ProjectFinishDate|字符串|不 |
|ProjectCreatedDate|字符串|不 |
|ProjectId|字符串|不 |
|ProjectModifiedDate|字符串|不 |
|项目类型|整数|不 |
|项目名称|字符串|不 |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="triggerresource"></a>TriggerResource


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|资源 Id|字符串|不 |
|ResourceBaseCalendar|字符串|不 |
|ResourceBookingType|整数|不 |
|ResourceCanLevel|布尔值|不 |
|ResourceCostPerUse|编号|不 |
|ResourceCreatedDate|字符串|不 |
|ResourceEarliestAvailableFrom|字符串|不 |
|ResourceEmail|字符串|不 |
|ResourceInitials|字符串|不 |
|ResourceIsActive|布尔值|不 |
|ResourceIsGeneric|布尔值|不 |
|ResourceLatestAvailableTo|字符串|不 |
|ResourceModifiedDate|字符串|不 |
|资源名称|字符串|不 |
|ResourceStatsuName|字符串|不 |
|资源类型|整数|不 |
|TypeDescription|字符串|不 |
|类型名称|字符串|不 |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="triggertask"></a>TriggerTask


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|ProjectId|字符串|不 |
|TaskId|字符串|不 |
|项目名称|字符串|不 |
|任务名|字符串|不 |
|TaskCreatedDate|字符串|不 |
|TaskModifieddate|字符串|不 |
|TaskStartDate|字符串|不 |
|TaskFinishDate|字符串|不 |
|TaskPriority|整数|不 |
|TaskIsActive|布尔值|不 |



### <a name="newproject"></a>新建项目


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|是的 |
|说明|字符串|不 |
|启动|字符串|不 |



### <a name="newreource"></a>NewReource


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|是的 |
|IsBudget|布尔值|不 |
|IsGeneric|布尔值|不 |
|IsInactive|布尔值|不 |



### <a name="project"></a>项目


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|ApprovedStart|字符串|不 |
|ApprovedEnd|字符串|不 |
|CheckedOutDate|字符串|不 |
|CheckOutDescription|字符串|不 |
|CheckOutId|字符串|不 |
|CreatedDate|字符串|不 |
|标识|字符串|不 |
|IsCheckedOut|布尔值|不 |
|LastPublishedDate|字符串|不 |
|上次保存|字符串|不 |
|OptimizerDecision|整数|不 |
|PlannerDecision|整数|不 |
|项目类型|整数|不 |
|名称|字符串|不 |
|WinprojVersion|字符串|不 |



### <a name="projectswrapper"></a>ProjectsWrapper


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="newtask"></a>NewTask


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|参数|未定义|是的 |



### <a name="taskparameters"></a>TaskParameters


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|名称|字符串|是的 |
|备注|字符串|不 |
|启动|字符串|不 |
|持续时间|字符串|不 |



### <a name="enterpriseresource"></a>EnterpriseResource


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|CanLevel|布尔值|不 |
|代码|字符串|不 |
|CostAccrual|整数|不 |
|CostCenter|字符串|不 |
|创建|字符串|不 |
|DefaultBookingType|整数|不 |
|电子邮件|字符串|不 |
|ExternalId|字符串|不 |
|组|字符串|不 |
|Hiredate|字符串|不 |
|标识|字符串|不 |
|首字母缩写|字符串|不 |
|IsActive|布尔值|不 |
|IsBudget|布尔值|不 |
|IsCheckedOut|布尔值|不 |
|IsGeneric|布尔值|不 |
|IsTeam|布尔值|不 |
|MaterialLabel|字符串|不 |
|修改|字符串|不 |
|名称|字符串|不 |
|拼音|字符串|不 |
|资源类型|整数|不 |
|TerminationDate|字符串|不 |



### <a name="taskswrapper"></a>TasksWrapper


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|值|数组|不 |



### <a name="task"></a>任务


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|创建|字符串|不 |
|修改|字符串|不 |
|启动|字符串|不 |
|完成|字符串|不 |
|名称|字符串|不 |
|标识|字符串|不 |
|优先级|整数|不 |
|完成百分比|整数|不 |
|备注|字符串|不 |
|联系人|字符串|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)