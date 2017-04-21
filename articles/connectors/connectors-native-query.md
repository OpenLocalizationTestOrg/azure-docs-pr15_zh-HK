<properties
    pageTitle="在应用程序逻辑中添加的查询操作 |Microsoft Azure"
    description="执行滤镜数组等操作的查询操作的概述。"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>入门的查询操作

通过使用查询操作时，您可以使用批处理和阵列，以实现对工作流︰

- 从数据库创建的高优先级的所有记录的任务。
- 保存所有 PDF 附件的电子邮件到 Azure 的 blob。

若要开始使用逻辑应用程序中的查询操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="use-the-query-action"></a>使用查询操作

对策是由逻辑应用程序中定义的工作流执行的操作。 [了解有关操作的详细](connectors-overview.md)信息。  

查询操作目前有一个称为滤镜数组，公开设计器中的操作。 这允许您查询数组并返回经过筛选的结果集。

下面是如何您可以将其添加在应用程序中的逻辑︰

1. 选择**新步骤**按钮。
2. 选择**添加操作**。
3. 在操作搜索框中，键入**筛选器**列表的**筛选器数组**操作。

    ![选择查询操作](./media/connectors-native-query/using-action-1.png)

4. 选择要筛选的数组。 （下面的屏幕快照显示从 Twitter 搜索结果的数组）。
5. 创建一个条件来评估每个项目上。 （下面的屏幕快照筛选器从拥有 100 多个用户的用户 tweets。）

    ![完成的查询操作](./media/connectors-native-query/using-action-2.png)

    该操作将输出新数组，其中包含符合筛选要求的结果。
6. 单击工具栏上要保存，左上角和逻辑应用程序会进行保存和发布 （激活）。

## <a name="query-action"></a>查询操作

以下是该连接器所支持的操作的详细信息。 该连接器有可能的操作。

|操作|说明|
|---|---|
|滤镜数组|计算数组中的每个项的条件，并返回结果|

## <a name="action-details"></a>操作详细信息

查询操作提供了可能的操作。 下表描述了操作和相应的输出详细信息与使用操作相关联的必需和可选的输入的字段。

### <a name="filter-array"></a>滤镜数组
以下是该动作，发出出站 HTTP 请求的输入的字段。
A * 意味着它是必填的字段。

|显示名称|属性名称|说明|
|---|---|---|
|从 *|从|要筛选的数组|
|条件 *|在哪里|要计算的每个项的条件|
<br>

### <a name="output-details"></a>输出详细信息

下面是输出的 HTTP 响应的详细信息。

|属性名称|数据类型|说明|
|---|---|---|
|筛选的数组|数组|一个数组，其中包含一个对象，用于每个筛选结果|

## <a name="next-steps"></a>下一步行动

现在，试验平台并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 可以通过看一看我们的[Api 列表](apis-list.md)浏览其他可用的连接器应用程序逻辑中。
