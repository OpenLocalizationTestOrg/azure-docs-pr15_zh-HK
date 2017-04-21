<properties
pageTitle="GitHub |Microsoft Azure"
description="Azure 应用程序服务创建的应用程序逻辑。 GitHub 是承载服务的基于 web 的 Git 存储库。 它提供的所有分布式的版本控制和源代码代码管理 (SCM) 功能 Git 以及添加自己的功能。"
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

# <a name="get-started-with-the-github-connector"></a>开始使用 GitHub 连接器

GitHub 是承载服务的基于 web 的 Git 存储库。 它提供的所有分布式的版本控制和源代码代码管理 (SCM) 功能 Git 以及添加自己的功能。

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2015年-08-01-预览架构版本。 

您可以通过创建一个逻辑应用程序现在开始，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="triggers-and-actions"></a>触发器和操作

GitHub 连接器可以用作操作;它具有触发器。 所有连接器都支持 JSON 和 XML 格式的数据。 

 GitHub 连接器具有以下操作和/或可用触发器︰

### <a name="github-actions"></a>GitHub 操作
您可以执行下列操作︰

|操作|说明|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|创建一个问题|
### <a name="github-triggers"></a>GitHub 触发器
您可以侦听这些事件︰

|触发器 | 说明|
|--- | ---|
|当打开一个问题|打开一个问题|
|当关闭问题|关闭问题|
|当分配问题|分配问题|


## <a name="create-a-connection-to-github"></a>创建到 GitHub 的连接
要使用 GitHub 创建逻辑的应用程序，必须先创建**连接**，然后提供下列属性的详细信息︰ 

|属性| 必填|说明|
| ---|---|---|
|标记|是的|提供 GitHub 的凭据|
创建此连接后，可以使用它来执行操作并聆听有这篇文章中所述的触发器。 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] 您可以在其他逻辑应用程序中使用此连接。

## <a name="reference-for-github"></a>GitHub 的引用
适用于版本︰ 1.0

## <a name="createissue"></a>CreateIssue
创建问题︰ 创建一个问题 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| 名称| 数据类型|必填|位于|默认值|说明|
| ---|---|---|---|---|---|
|repositoryOwner|字符串|是的|路径|无|存储库所有者|
|repositoryName|字符串|是的|路径|无|存储库名称|
|issueBasicDetails| |是的|正文|无|问题详细信息|

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


## <a name="issueopened"></a>IssueOpened
当打开一个问题︰ 打开一个问题 

```GET: /trigger/issueOpened``` 

此调用没有参数
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


## <a name="issueclosed"></a>IssueClosed
当关闭一个问题︰ 关闭问题 

```GET: /trigger/issueClosed``` 

此调用没有参数
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


## <a name="issueassigned"></a>IssueAssigned
当分配问题︰ 一个问题分配 

```GET: /trigger/issueAssigned``` 

此调用没有参数
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标题|字符串|是的 |
|正文|字符串|是的 |
|受托人|字符串|是的 |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| 属性名称 | 数据类型 | 必填 |
|---|---|---|
|标题|字符串|是的 |
|正文|字符串|是的 |
|受托人|字符串|是的 |
|编号|字符串|不 |
|状态|字符串|不 |
|created_at|字符串|不 |
|repository_url|字符串|不 |


## <a name="next-steps"></a>下一步行动
[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)