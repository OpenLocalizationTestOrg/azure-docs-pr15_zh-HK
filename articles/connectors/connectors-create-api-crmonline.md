<properties
    pageTitle="将 Dynamics CRM Online 的连接器添加到您的逻辑应用程序 |Microsoft Azure"
    description="Azure 应用程序服务创建的应用程序逻辑。 Dynamics CRM 联机连接提供程序提供了一个 API，用于处理 Dynamics CRM Online 上的实体。"
    services="logic-apps"    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/15/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-dynamics-crm-online-connector"></a>开始使用 Dynamics CRM Online 的连接器
连接到 Dynamics CRM Online 来创建新记录，更新项目，等等。 使用 CRM Online，您可以︰

- 构建业务流从 CRM Online 获得的数据。 
- 删除一条记录的使用操作获取实体，等等。 这些操作获得的响应，并使输出可用于其他操作。 例如，在 CRM 中更新项目时，可以发送一封电子邮件，使用 Office 365。

本主题演示如何在逻辑应用程序中，使用 Dynamics CRM Online 的连接器，同时列出触发器和操作。

>[AZURE.NOTE] 此版本的文章将应用于逻辑应用程序一般可用性 (GA)。

要了解更多有关逻辑应用程序，请参见[什么是逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="connect-to-dynamics-crm-online"></a>连接到 Dynamics CRM 联机

逻辑应用程序可以访问的任何服务之前，您首先创建一个*连接*到的服务。 连接提供一个逻辑应用程序和其他服务之间的连接。 例如，若要连接到动态，必须先 Dynamics CRM Online 的*连接*。 若要创建连接，输入通常用于访问要连接到的服务的凭据。 因此用动态，您 Dynamics CRM Online 的帐户来创建连接到输入的凭据。


### <a name="create-the-connection"></a>创建连接

>[AZURE.INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]

## <a name="use-a-trigger"></a>使用触发器

触发器是可以用来启动工作流的逻辑应用程序中定义的事件。 触发器"轮询"服务间隔和所需的频率。 [了解更多有关触发器](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)的信息。

1. 在逻辑应用程序中，键入"dynamics"获取触发器的列表︰  

    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)

2. 选择**Dynamics CRM Online-创建一条记录时**。 如果已存在一个连接，然后选择组织和实体，从下拉列表中。

    ![](./media/connectors-create-api-crmonline/select-organization.png)

    如果提示您登录，然后输入符号来创建该连接的详细信息中。 [创建该连接](connectors-create-api-crmonline.md#create-the-connection)，本主题中列出的步骤。 

    > [AZURE.NOTE] 在此示例中，逻辑应用程序运行时将创建一个记录。 若要查看此触发器的结果，添加另一个向您发送一封电子邮件的操作。 例如，添加电子邮件您添加新记录时 Office 365*发送一封电子邮件*的操作。 

3. 选择**编辑**按钮并设置的**频率**和**时间间隔**的值。 例如，如果您想要轮询的触发器每隔 15 分钟，然后将**频率**设置为**分钟**，并将**间隔**设置为**15**。 

    ![](./media/connectors-create-api-crmonline/edit-properties.png)

4. **保存**所做的更改 （左上的角的工具栏）。 逻辑应用程序保存，并且可能会自动被启用。


## <a name="use-an-action"></a>使用的操作

操作是由逻辑应用程序中定义的工作流执行的操作。 [了解有关操作的详细](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)信息。

1. 选择加号。 看了几种选择︰**将操作添加**、**添加一个条件**，或**多个**选项之一。

    ![](./media/connectors-create-api-crmonline/add-action.png)

2. 选择**添加操作**。

3. 在文本框中，键入"dynamics"以获取所有可用操作的列表。

    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)

4. 在我们的示例中，选择**Dynamics CRM Online 的更新记录**。 如果已存在一个连接，然后选择**组织名称**、**实体名称**和其他属性︰  

    ![](./media/connectors-create-api-crmonline/sample-action.png)

    如果系统提示您输入连接信息，然后输入要创建的连接的详细信息。 本主题中[创建的连接](connectors-create-api-crmonline.md#create-the-connection)描述了这些属性。 

    > [AZURE.NOTE] 在此示例中，我们会更新在线 CRM 中的现有记录。 从另一个触发器的输出可用于更新记录。 例如，添加 SharePoint*修改现有项目时*触发。 然后将添加使用 SharePoint 字段来更新现有记录中 CRM Online CRM Online*更新记录*的操作。 

5. **保存**所做的更改 （左上的角的工具栏）。 逻辑应用程序保存，并且可能会自动被启用。


## <a name="technical-details"></a>技术详细信息

## <a name="triggers"></a>触发器

|触发器 | 说明|
|--- | ---|
|[当创建一条记录](connectors-create-api-crmonline.md#when-a-record-is-created)|在 CRM 中创建对象时将触发一个流。|
|[更新记录时](connectors-create-api-crmonline.md#when-a-record-is-updated)|在 CRM 中修改对象时，将触发流。|
|[当记录被删除时](connectors-create-api-crmonline.md#when-a-record-is-deleted)|在 CRM 中删除对象时将触发一个流。|


## <a name="actions"></a>操作

|操作|说明|
|--- | ---|
|[列表中的记录](connectors-create-api-crmonline.md#list-records)|此操作获取实体的记录。|
|[创建新记录](connectors-create-api-crmonline.md#create-a-new-record)|此操作将创建新的实体记录。|
|[获取记录](connectors-create-api-crmonline.md#get-record)|此操作获取实体指定的记录。|
|[删除一条记录](connectors-create-api-crmonline.md#delete-a-record)|此操作从一个实体集合中删除的记录。|
|[更新记录](connectors-create-api-crmonline.md#update-a-record)|此操作会更新现有记录的实体。|

### <a name="trigger-and-action-details"></a>触发器和操作详细信息

在此部分，查看有关每个触发器和操作，包括任何必需的还是可选的输入的属性，以及任何相应的输出与连接器相关联的特定详细信息。

#### <a name="when-a-record-is-created"></a>当创建一条记录
在 CRM 中创建对象时将触发一个流。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|
|$skip|跳过计数|若要跳过的条目数 (默认值 = 0)|
|$top|获取最大计数|要获取的项的最大数量 (默认值 = 256)|
|$filter|筛选器查询|ODATA 筛选查询来限制返回的项|
|$orderby|排序依据|指定项的顺序的 ODATA orderBy 查询|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
ItemsList

| 属性名称 | 数据类型 |
|---|---|
|值|数组|


#### <a name="when-a-record-is-updated"></a>更新记录时
在 CRM 中修改对象时，将触发流。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|
|$skip|跳过计数|若要跳过的条目数 (默认值 = 0)|
|$top|获取最大计数|要获取的项的最大数量 (默认值 = 256)|
|$filter|筛选器查询|ODATA 筛选查询来限制返回的项|
|$orderby|排序依据|指定项的顺序的 ODATA orderBy 查询|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
ItemsList

| 属性名称 | 数据类型 |
|---|---|
|值|数组|


#### <a name="when-a-record-is-deleted"></a>当记录被删除时
在 CRM 中删除对象时将触发一个流。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|
|$skip|跳过计数|若要跳过的条目数 (默认值 = 0)|
|$top|获取最大计数|要获取的项的最大数量 (默认值 = 256)|
|$filter|筛选器查询|ODATA 筛选查询来限制返回的项|
|$orderby|排序依据|指定项的顺序的 ODATA orderBy 查询|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
ItemsList

| 属性名称 | 数据类型 |
|---|---|
|值|数组|


#### <a name="list-records"></a>列表中的记录
此操作获取实体的记录。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|
|$skip|跳过计数|若要跳过的条目数 (默认值 = 0)|
|$top|获取最大计数|要获取的项的最大数量 (默认值 = 256)|
|$filter|筛选器查询|ODATA 筛选查询来限制返回的项|
|$orderby|排序依据|指定项的顺序的 ODATA orderBy 查询|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
ItemsList

| 属性名称 | 数据类型 |
|---|---|
|值|数组|


#### <a name="create-a-new-record"></a>创建新记录
此操作将创建新的实体记录。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
无。


#### <a name="get-record"></a>获取记录
此操作获取实体指定的记录。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|
|id *|项目标识符|指定记录的标识符|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
无。


#### <a name="delete-a-record"></a>删除一条记录
此操作从一个实体集合中删除的记录。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|
|id *|项目标识符|指定记录的标识符|

星号 （*） 表示该属性是必需的。


#### <a name="update-a-record"></a>更新记录
此操作会更新现有记录的实体。 

|属性名称| 显示名称|说明|
| ---|---|---|
|数据集 *|组织名称|如 Contoso 的 CRM 组织的名称|
|表 *|实体名称|实体的名称|
|id *|记录的标识符|指定记录的标识符|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
无。


## <a name="http-responses"></a>HTTP 响应

操作和触发器可以返回一个或多个以下 HTTP 状态代码︰ 

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误。|
|默认|操作失败。|


## <a name="next-steps"></a>下一步行动

[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 浏览其他可用的连接器，在我们的[Api 列表](apis-list.md)的逻辑应用程序中。

