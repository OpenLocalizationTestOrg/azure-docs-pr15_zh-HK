<properties
    pageTitle="逻辑应用程序中添加一个延迟 |Microsoft Azure"
    description="介绍了延迟和延迟的操作，以及如何使用 Azure 的逻辑应用程序中使用它们之前。"
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
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>入门的延迟和延迟的操作直到

通过延迟和"延迟-直到"操作，您可以完成工作流的方案。

例如，您可以︰

- 等到工作日要通过电子邮件发送的状态更新。
- 直到 HTTP 调用有时间来恢复和检索结果之前完成延迟该工作流。

若要开始使用延迟操作逻辑应用程序中，请参阅[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="use-the-delay-actions"></a>使用延迟操作

对策是由逻辑应用程序中定义的工作流执行的操作。 [了解有关操作的详细](connectors-overview.md)信息。

下面是如何使用延迟步骤逻辑应用程序中的一个示例序列︰

1. 之后添加一个触发器，单击要添加操作的**新步骤**。
2. 搜索**延迟**启动延迟操作。 在此示例中，我们将选择**延迟**。

    ![延迟的操作](./media/connectors-native-delay/using-action-1.png)

3. 完成所有操作的属性来配置延迟。

    ![延迟配置](./media/connectors-native-delay/using-action-2.png)

4. 单击**保存**以发布并激活逻辑应用程序。


## <a name="action-details"></a>操作详细信息

定期触发器具有以下可配置的属性。

### <a name="delay-action"></a>延迟的操作

此操作会延迟为一个特定的时间间隔运行。
A * 意味着它是必填的字段。

|显示名称|属性名称|说明|
|---|---|---|
|计数 *|计数|延迟的时间单位数|
|设备 *|单位|时间单位︰ `Second`， `Minute`， `Hour`，或`Day`|
<br>

### <a name="delay-until-action"></a>延迟的操作直到

此操作延迟到一个指定的日期/时间运行。
A * 意味着它是必填的字段。

|显示名称|属性名称|说明|
|---|---|---|
|年份 *|时间戳|要延迟 （格林威治标准时间） 之前的年份|
|月 *|时间戳|月前 （格林威治标准时间） 延迟|
|一天 *|时间戳|一天前 （格林威治标准时间） 延迟|
<br>


## <a name="next-steps"></a>下一步行动

现在，试验平台并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 可以通过看一看我们的[Api 列表](apis-list.md)浏览其他可用的连接器应用程序逻辑中。
