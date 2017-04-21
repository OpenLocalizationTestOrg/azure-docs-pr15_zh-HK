<properties 
    pageTitle="应用程序服务 API 应用触发器 |Microsoft Azure" 
    description="如何实现在 Azure 应用程序服务 API 应用程序中的触发器" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure 应用程序服务 API 的应用程序的触发器

>[AZURE.NOTE] 本版文章适用于 API 的应用程序 2014年-12-01-预览架构版本。


## <a name="overview"></a>概述

这篇文章解释了如何实现 API 的应用程序的触发器和从逻辑应用程序中使用它们。

本主题中的代码段的所有复制[FileWatcher API 应用程序代码示例](http://go.microsoft.com/fwlink/?LinkId=534802)。 

请注意，您将需要下载以下 nuget 程序包生成并运行此文章中的代码︰ [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/)。

## <a name="what-are-api-app-triggers"></a>什么是 API 的应用程序的触发器？

它是一个常见情形 API 使应用程序若要激发事件，以便客户端 API 应用程序可采取适当的措施，对事件作出响应。 支持这种情况下的基于 REST API 机制调用 API 的应用程序触发。 

例如，假设客户端代码使用[使用 Twitter，连接器 API 的应用程序](../app-service-logic/app-service-logic-connector-twitter.md)和您的代码需要执行某项操作基于新的 tweets，包含特定的词。 在这种情况下，您可能设置轮询或推触发器来满足这一需要。

## <a name="poll-trigger-versus-push-trigger"></a>轮询与推触发器的触发器

目前，支持两种类型的触发器︰

- 轮询触发器 — 客户端轮询 API 的应用程序具有已触发的事件的通知 
- 事件触发时，推触发器 — 客户端通知通过 API 的应用程序 

### <a name="poll-trigger"></a>轮询触发器

轮询触发器作为常规的 REST API 实现的并且期望其客户端 （例如逻辑的应用程序） 来轮询才能得到通知。 虽然客户端可能会保持状态，轮询触发器本身是无状态的。 

有关请求和响应数据包的以下信息说明了轮询触发器合同的某些关键方面︰

- 请求
    - HTTP 方法︰ 获取
    - 参数
        - triggerState-此可选参数允许客户端指定其状态以便轮询触发器可以正确地决定是否或不返回通知基于指定的状态。
        - API 的特定参数
- 响应
    - 状态代码**200** -请求有效，并且没有来自该触发器的通知。 通知的内容都将响应正文。 在响应中的"重试之后"标头指示必须使用后面的请求调用中检索其他通知数据。
    - 状态代码**202** -请求是有效的但从触发器没有新通知。
    - 状态代码**4xx** -请求无效是的。 客户端不应重试请求。
    - 状态代码**5xx** -请求导致了内部服务器错误和/或临时问题。 客户端应重试请求。

下面的代码段是如何实现轮询触发器的一个示例。

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

若要测试此轮询触发器，请执行以下步骤︰

1. 将部署身份验证设置为**匿名的公共**API 的应用程序。
2. 调用**触摸**操作来打开文件。 下图显示了示例请求通过把邮递员弄。
   ![通过把邮递员弄调用触摸操作](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. **TriggerState**参数设置为在步骤 2 之前的时间戳与调用轮询触发器。 下图显示该示例请求通过把邮递员弄。
   ![调用轮询触发器通过把邮递员弄](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>推触发器

推触发器作为向特定事件触发时通知已注册的客户端进行推送安装通知正则的 REST API 来实现。

有关请求和响应数据包的以下信息说明了推触发器合同的某些关键方面。

- 请求
    - HTTP 方法︰ 放置
    - 参数
        - triggerId︰ 所需的不透明表示推触发器的登记 （例如 GUID) 字符串。
        - callbackUrl︰ 所需的触发事件时调用的回调 URL。 该调用是简单的 POST HTTP 调用。
        - API 的特定参数
- 响应
    - 状态代码**200** -注册成功的客户端的请求。
    - 状态代码**4xx** -请求无效是的。 客户端不应重试请求。
    - 状态代码**5xx** -请求导致了内部服务器错误和/或临时问题。 客户端应重试请求。
- 回叫
    - HTTP 方法︰ 开机自检
    - 请求正文︰ 通知内容。

下面的代码段是如何实现推触发器的一个示例︰

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

若要测试此轮询触发器，请执行以下步骤︰

1. 将部署身份验证设置为**匿名的公共**API 的应用程序。
2. 浏览到要创建充当回调 URL RequestBin [http://requestb.in/](http://requestb.in/) 。
3. 调用使用 GUID，则为**triggerId**和**callbackUrl**为 RequestBin URL 推触发器。
   ![调用推触发器通过把邮递员弄](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. 调用**触摸**操作来打开文件。 下图显示了示例请求通过把邮递员弄。
   ![通过把邮递员弄调用触摸操作](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. 请检查以确认推触发器回调被调用属性输出 RequestBin。
   ![调用轮询触发器通过把邮递员弄](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>描述在 API 定义的触发器

之后执行触发器并将您的 API 应用程序部署到 Azure，导航到 Azure 预览门户中**API 定义**刀片式服务器，您将看到触发器会自动识别在 UI 中，它由 Swagger 2.0 API 定义的 API 的应用程序。

![API 定义刀片式服务器](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

如果单击**下载 Swagger**按钮，打开 JSON 文件，您将看到类似下面的结果︰

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

扩展属性**x 毫秒-schedular 触发器**是触发器的 API 定义所述的方式，如果您请求的 API 定义通过网关时自动添加 API 应用程序网关通过向下列条件之一的请求。 （还可以此属性手动。）

- 轮询触发器
    - 如果 HTTP 方法**获得**。
    - 如果将**operationId**属性包含的字符串**触发器**。
    - 如果**参数**属性包含参数的**名称**属性设置为**triggerState**。
- 推触发器
    - 如果 HTTP 方法是**放**。
    - 如果将**operationId**属性包含的字符串**触发器**。
    - 如果**参数**属性包含参数的**名称**属性设置为**triggerId**。

## <a name="use-api-app-triggers-in-logic-apps"></a>在应用程序逻辑中使用 API 应用触发器

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>列出和配置 API 应用触发器逻辑应用程序设计器中

如果 API 应用程序所在的资源组中创建逻辑应用程序时，可以只通过单击它时将其添加到设计器的画布。 下面的图像阐释了这一点︰

![逻辑应用程序设计器中的触发器](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![逻辑应用程序设计器中配置轮询触发器](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![逻辑应用程序设计器中配置推触发器](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>优化用于逻辑的应用程序的 API 应用触发器

触发器添加到 API 的应用程序之后，有几个方法可以改善逻辑应用程序中使用该 API 的应用程序时的体验。

例如，轮询触发器的**triggerState**参数应设置为以下表达式逻辑应用程序中。 此表达式应该评估上次逻辑应用程序中，从触发器调用并返回该值。  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

注︰ 用上述表达式中的函数的说明，请参阅上[的逻辑应用程序工作流定义语言](https://msdn.microsoft.com/library/azure/dn948512.aspx)的文档。

逻辑应用程序用户将需要提供的**triggerState**参数使用触发器时上面表达式。 很可能有预设逻辑应用程序设计器通过扩展属性**x ms 计划建议**此值。  **X ms 可见度**扩展属性可以设置为*内部*的值，以便参数本身不会显示在设计器。  下面的代码段阐释了。

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

对于推触发器， **triggerId**参数必须唯一标识逻辑应用程序。 建议的最佳做法是将此属性设置为该工作流的名称，通过使用下面的表达式︰

    @workflow().name

其 API 定义中使用的**x ms 计划推荐**和**x ms 可见度**的扩展属性，API 的应用程序可以将传达给逻辑应用程序设计器会自动设置此表达式的用户。

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>在 API 定义中添加扩展属性

附加的元数据的信息，如扩展属性**x ms 计划程序建议**和**可见性 x-ms** -可以添加 API 定义两种方式之一︰ 静态或动态。

对于静态元数据，可以直接编辑项目中的*/metadata/apiDefinition.swagger.json*文件，并手动添加属性。

对于使用动态元数据 API 应用程序，您可以编辑要添加一个操作筛选器，可以添加这些扩展的 SwaggerConfig.cs 文件。

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


以下是如何实现此类便于动态元数据方案的一个示例。

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
