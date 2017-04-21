<properties 
    pageTitle="为逻辑应用程序创建一个 API" 
    description="创建一个自定义的 API 与逻辑应用程序一起使用" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>创建一个自定义的 API 与逻辑应用程序一起使用

如果您想要扩展的逻辑应用程序平台，有很多方法可以调用的 Api 或不可用的我们许多优秀的连接线的系统。  创建 API 应用程序可以从应用程序的逻辑流中调用这些方法之一。

## <a name="helpful-tools"></a>有用的工具

为更好地配合逻辑应用程序 Api，我们建议生成您的 API 的详细介绍支持的操作和参数的[swagger](http://swagger.io)文档。  有 （如[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle))，将自动生成 swagger 为您的多个库。  此外可以使用[TRex](https://github.com/nihaue/TRex)有助于批注 swagger 与逻辑 （显示名称、 属性类型等） 的应用程序很好地工作。  构建用于逻辑的应用程序的 API 应用程序的一些示例，请务必签出我们[GitHub 资料库](http://github.com/logicappsio)或[博客](http://aka.ms/logicappsblog)。

## <a name="actions"></a>操作

逻辑应用程序的基本操作是将接受 HTTP 请求并返回响应 (通常 200) 控制器。  但也有不同的模式可以遵循扩展操作根据您的需要。

默认情况下，逻辑应用程序引擎将超时请求后 1 分钟。  但是，您可以您的 API 执行操作的时间很长，并有等待完成后，按照下面详细说明的异步或 webhook 模式的引擎。

对于标准操作，只需在您的 swagger 通过公开的 API 编写 HTTP 请求方法。  您可以看到我们[GitHub 存储库](https://github.com/logicappsio)中的逻辑应用程序所使用的 API 应用程序的示例。  以下是完成自定义连接线与通用模式的方法。

### <a name="long-running-actions---async-pattern"></a>长时间运行操作的异步模式

当运行长步骤或任务时，您需要做的第一件事是确保引擎知道没有超时。 您还需要它如何知道时您就完成了任务，与引擎通信，最后，您需要返回到引擎相关数据，以便它可以继续执行工作流。 您可以通过 API 完成，按下面的顺序。 这些步骤是从自定义的 API 的看点︰

1. 在收到请求后，立即返回响应 （之前已完成的工作）。 此响应将是`202 ACCEPTED`响应，让知道你数据，该引擎接受有效负载，并立即处理。 202 响应应该包含以下标头︰ 
 * `location`标题 （必填）︰ 这是一个绝对路径的 URL 逻辑应用程序可以使用它来检查作业的状态。
 * `retry-after`(可选的将默认为 20 个操作)。 这是该引擎轮询检查状态的位置标题 URL 之前应等待的秒数。

2. 当检查作业状态时，请执行以下检查︰ 
 * 如果工作已完成︰ 返回`200 OK`响应，响应负载。
 * 如果作业仍在处理︰ 返回另一个`202 ACCEPTED`响应，具有相同标题作为初始响应

此模式允许您运行您自定义的 API 的线程内的超长任务但保持活动连接处于活动状态的逻辑应用程序引擎以便它不会超时或继续之前完成工作。 添加到应用程序逻辑，时，一定要注意您不需要做任何事情在继续轮询并检查状态逻辑应用程序定义中。 一旦引擎可以看到有一个有效的位置标头的 202 接受响应，它将接受了异步模式并继续直到返回非 202 轮询位置标头。

您可以看到这种模式在 GitHub 的样本[在此处](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook 操作

在您的工作流，您可以有逻辑应用程序暂停并等待"回调"，若要继续。  此回调的 HTTP POST 的形式出现。  若要实现此模式，您需要提供您的控制器上的两个端点︰ 订阅和取消订阅。

在订阅，逻辑应用程序将创建并已准备好作为 HTTP POST 中注册回调 URL 可以存储您的 API 和回调。  任何内容或接头将传递到逻辑应用程序，可以用于工作流的其余部分中。  一旦达到该步骤的逻辑应用程序引擎将执行调用订阅点。

如果运行已被取消，逻辑应用程序引擎将使到取消订阅的终结点的调用。  根据需要您的 API 可以再取消注册回调 URL。

当前逻辑应用程序设计器不支持发现通过 swagger，webhook 端点，因此使用这种类型的操作必须添加"Webhook"操作，并指定 URL、 标题和正文中您的请求。  您可以使用`@listCallbackUrl()`任意根据需要回调 URL 中传递这些字段中的工作流功能。

您可以看到在 GitHub 的 webhook 模式的示例[这里](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>触发器

除了操作外，您可以为触发器逻辑应用程序让您自定义的 API 就。  有两种模式，您可以按照下面来触发逻辑应用程序︰

### <a name="polling-triggers"></a>轮询触发器

轮询触发器操作非常类似上面的长时间运行的异步操作。  逻辑应用程序引擎将调用触发器终结点过后一段时间 （取决于 1 分钟标准为特优，15 秒和 1 小时免费的 SKU）。

如果没有数据可用，该触发器返回`202 ACCEPTED`响应，与`location`和`retry-after`头。  但是，对于触发器建议`location`标头中包含的查询参数`triggerState`。  这是您的 API，知道最后一次的逻辑应用程序触发时一些标识符。  如果数据可用，该触发器返回`200 OK`与内容的有效负载的响应。  此操作将启动逻辑应用程序。

例如，如果我被轮询来查看文件是否可用，您可以构建轮询触发器可以执行以下任务︰

* 如果没有 triggerState 收到请求 API 将返回`202 ACCEPTED`与`location`标头，包含当前时间的 triggerState 和`retry-after`为 15。
* 如果已使用 triggerState 收到的请求︰
 * 检查 triggerState 的日期时间后是否添加的任何文件。 
  * 如果没有 1 个文件，则返回`200 OK`响应内容的有效负载，递增到我返回，并设置该文件的日期时间 triggerState`retry-after`到 15。
  * 如果有多个文件，我可以在一次返回 1 `200 OK`，增加我在 triggerState`location`标头，并设置`retry-after`为 0。  这将让引擎知道没有更多数据可用，它将立即请求它在`location`指定的标头。
  * 如果没有可用的文件，返回`202 ACCEPTED`响应，然后将`location`triggerState 相同。  设置`retry-after`到 15。

您可以看到在 GitHub 轮询触发器示例[这里](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook 触发器

Webhook 触发器操作非常类似上面的 Webhook 操作。  只要添加并保存 webhook 触发器逻辑应用程序引擎将调用订阅终结点。  您的 API 可以注册的 webhook URL 并调用通过 HTTP POST 数据时可用。  有效负载的内容和标题将被传递到逻辑应用程序运行。

如果 webhook 触发器是否曾被删除 (逻辑应用完全，或只是 webhook 触发器)，则引擎将打电话到取消订阅 URL 位置您的 API 可以取消注册回调 URL 并根据需要停止任何进程。

当前逻辑应用程序设计器不支持发现通过 swagger，webhook 触发器，所以要使用这种类型的操作必须添加"Webhook"触发器，并指定 URL、 标题和正文中您的请求。  您可以使用`@listCallbackUrl()`任意根据需要回调 URL 中传递这些字段中的工作流功能。

您可以看到在 GitHub webhook 触发器示例[这里](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)