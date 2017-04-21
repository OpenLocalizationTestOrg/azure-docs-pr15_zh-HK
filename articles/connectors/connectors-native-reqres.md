<properties
    pageTitle="使用的请求和响应的操作 |Microsoft Azure"
    description="请求和响应触发器和操作在 Azure 的逻辑应用程序概述"
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

# <a name="get-started-with-the-request-and-response-components"></a>开始的请求和响应组件

与逻辑应用程序中的请求和响应组件，您可以实时对事件作出响应。

例如，您可以︰

- 从内部数据库中通过逻辑应用程序响应的 HTTP 请求的数据。
- 触发逻辑应用程序从外部 webhook 事件。
- 调用请求和响应在一个操作中从另一个逻辑应用程序逻辑的应用的程序。

若要开始使用逻辑应用程序中的请求和响应的操作，请参阅[创建逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="use-the-http-request-trigger"></a>使用 HTTP 请求触发

触发器是可以用于启动的工作流逻辑应用程序中定义的事件。 [了解更多有关触发器](connectors-overview.md)的信息。

这里是如何设置的逻辑应用程序设计器中的 HTTP 请求示例序列。

1. 逻辑应用程序中添加**接收到请求-当 HTTP 请求**触发。 请求正文，可以选择 （通过使用[JSONSchema.net](http://jsonschema.net)类工具） 提供一个 JSON 架构。 这允许设计器在 HTTP 请求中生成属性的标记。
2. 添加另一个操作，以便您可以保存逻辑应用程序。
3. 保存后的逻辑的应用程序，可以从请求卡获取 HTTP 请求的 URL。
4. （您可以使用像[把邮递员弄](https://www.getpostman.com/)的工具） 的 url HTTP POST 触发逻辑应用程序。

>[AZURE.NOTE] 如果没有定义响应操作，`202 ACCEPTED`响应立即返回到调用方。 响应操作可用于自定义响应。

![响应触发器](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>使用 HTTP 响应操作

由 HTTP 请求触发工作流中使用它时，HTTP 响应操作才有效。 如果没有定义响应操作，`202 ACCEPTED`响应立即返回到调用方。  您可以在工作流中的任何一步添加响应操作。 逻辑应用程序仅保留传入请求打开一分钟的响应时间。  后一分钟，如果从工作流发送没有响应 （和响应操作定义中存在）`504 GATEWAY TIMEOUT`返回给调用方。

下面介绍了如何添加 HTTP 响应操作︰

1. 选择**新步骤**按钮。
2. 选择**添加操作**。
3. 在操作搜索框中，键入列表的响应操作的**响应**。

    ![选择响应操作](./media/connectors-native-reqres/using-action-1.png)

4. 添加任何所需的 HTTP 响应消息的参数。

    ![完成响应的操作](./media/connectors-native-reqres/using-action-2.png)

5. 单击工具栏上要保存，左上角和逻辑应用程序会进行保存和发布 （激活）。

## <a name="request-trigger"></a>请求触发器

以下是此连接器支持的触发器的详细信息。 没有一个单一的请求触发器。

|触发器|说明|
|---|---|
|请求|当接收到 HTTP 请求时，发生|

## <a name="response-action"></a>响应操作

以下是该连接器所支持的操作的详细信息。 没有伴随请求触发器时可以只使用单个响应操作。

|操作|说明|
|---|---|
|响应|返回关联的 HTTP 请求的响应|

### <a name="trigger-and-action-details"></a>触发器和操作详细信息

下表描述了用于触发器和操作，输入的字段和相应的输出详细信息。

#### <a name="request-trigger"></a>请求触发器
以下是传入的 HTTP 请求中的触发器的输入的字段。

|显示名称|属性名称|说明|
|---|---|---|
|JSON 架构|架构|HTTP 请求正文的 JSON 架构|
<br>

**输出详细信息**

下面是输出详细信息的请求。

|属性名称|数据类型|说明|
|---|---|---|
|邮件头|对象|请求标头|
|正文|对象|请求对象|

#### <a name="response-action"></a>响应操作

以下是 HTTP 响应操作的输入的字段。 A * 意味着它是必填的字段。

|显示名称|属性名称|说明|
|---|---|---|
|状态代码 *|状态代码|HTTP 状态代码|
|邮件头|邮件头|任何响应标头包含一个 JSON 对象|
|正文|正文|响应正文|

## <a name="next-steps"></a>下一步行动

现在，试验平台并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 可以通过看一看我们的[Api 列表](apis-list.md)浏览其他可用的连接器应用程序逻辑中。
