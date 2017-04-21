<properties
    pageTitle="在应用程序逻辑中添加定期触发器 |Microsoft Azure"
    description="定期触发器，以及如何使用 Azure 的逻辑应用程序的概述。"
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

# <a name="get-started-with-the-recurrence-trigger"></a>开始使用定期触发器

通过使用定期触发器，可以在云中创建功能强大的工作流。

例如，您可以︰

- 计划每天运行的 SQL 存储过程的工作流。
- 关于某些 hashtag 上周的电子邮件的所有 tweets 的摘要。

若要开始使用定期触发器逻辑应用程序中，请参阅[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="use-a-recurrence-trigger"></a>使用定期触发器

触发器是可以用于启动的工作流逻辑应用程序中定义的事件。 [了解更多有关触发器](connectors-overview.md)的信息。

下面是如何设置的逻辑应用程序中重复执行触发器示例序列︰

1. 将**定期**触发器添加逻辑应用程序中的第一步。
2. 在参数中重复出现的间隔填充。

逻辑应用程序现在开始在每个时间间隔后的运行。

![HTTP 触发器](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>触发器的详细信息

定期触发器具有以下属性，您可以进行配置。

它在指定的时间间隔后触发逻辑应用程序。
A * 意味着它是必填的字段。

|显示名称|属性名称|说明|
|---|---|---|
|频率 *|频率|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|间隔 *|时间间隔|定期对给定频率的间隔。|
|时区|时区|如果开始时间的 UTC 偏移量而提供，则将使用此时区。|
|开始时间|开始时间|在[ISO 8601 格式](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)开始时间。|
<br>


## <a name="next-steps"></a>下一步行动

现在，试验平台并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 可以通过看一看我们的[Api 列表](apis-list.md)浏览其他可用的连接器应用程序逻辑中。
