<properties
    pageTitle="监视您的 Api 使用 Azure API 管理、 事件集线器和 Runscope"
    description="示例应用程序演示通过连接 Azure API 管理，Azure 事件集线器，Runscope 用于 HTTP 日志和监控日志到 eventhub 策略"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>监视您的 Api 使用 Azure API 管理、 事件集线器和 Runscope

[API 管理服务](api-management-key-concepts.md)提供了许多功能，以提高处理的 HTTP 请求发送到 HTTP API。 但是，请求和响应的存在是暂时的。 发出请求时，您的后端 API 的 API 管理服务中的流向。 您的 API 处理请求和响应通过流向 API 使用者。 API 管理服务 Api，可用于显示有关的一些重要统计数据中保留发布门户的仪表板，但场外的详细信息，都已不存在。

通过 API 管理服务中使用[日志到 eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [策略](api-management-howto-policies.md)可以从请求和响应在[Azure 事件中心](../event-hubs/event-hubs-what-is-event-hubs.md)发送的任何详细信息。 有各种原因为什么您可能想要从 HTTP 消息被发送到您的 Api 生成事件。 一些示例包括更新、 使用情况分析、 异常报警和第三方集成的审核跟踪。   

本文演示如何捕获整个 HTTP 请求和响应消息发送到事件中心，然后中继邮件给第三方服务提供 HTTP 日志和监控服务。

## <a name="why-send-from-api-management-service"></a>为什么从 API 管理服务发送？
它是可以编写 HTTP 中间件可以插入 HTTP API 框架捕获 HTTP 请求和响应，并送入日志和监控系统。 这种方法的缺点是 HTTP 中间件需要集成到后端的 API，并且必须匹配的 api 的平台。 如果有多个 Api，每个必须部署中间件。 经常有的原因为什么不能更新后端的 Api。

使用 Azure API 管理服务集成与日志记录基础架构提供集中化和独立于平台的解决方案。 它也是可扩展的部分由于 Azure API 管理的[地区复制](api-management-howto-deploy-multi-region.md)功能。

## <a name="why-send-to-an-azure-event-hub"></a>为什么发送到 Azure 事件集线器？
有理由问，为什么要创建特定于 Azure 事件集线器的策略？ 有许多不同的地方，我想记录我的请求。 为什么不只是请求将直接发送到最终的目标？  这是一个选项。 但是，在从 API 管理服务的日志记录请求时，有必要考虑如何记录消息会影响 API 的性能。 通过增加可用实例的系统组件或通过利用地区复制，可以处理逐渐增加负载。 但是，短流量高峰可能导致请求被大大延迟，如果请求日志基础结构启动负载情况下速度减慢。

Azure 事件集线器到进入巨大的数据量的设计，产能比的 HTTP 请求数大多数 Api 过程处理得更多的事件。 事件中心作为一种复杂缓冲区之间您的 API 管理服务和基础结构将存储和处理的消息。 这将确保您的 API 性能不会由于日志基础结构。  

一旦有了数据被传递给事件中心保持不变，并将等待事件中心使用者对其进行处理。 事件中心不关心它的处理方式，它只是关心并确保成功传递消息。     

事件集线器有多个使用者组流事件的能力。 这使事件完全不同的系统进行处理。 这使支持不放中的 API 管理服务 API 请求的处理添加延迟为一个事件生成所需要的多种集成方案。

## <a name="a-policy-to-send-applicationhttp-messages"></a>要发送的 http 应用程序/消息的策略
事件中心接受作为一个简单的字符串的事件数据。 该字符串的内容是完全取决于您的。 能打包的 HTTP 请求并将其发送到事件集线器我们需要设置格式的字符串的请求或响应的信息。 在这样的情况下的现有格式是否我们可以重复使用，然后我们可能不必编写自己的代码分析。 最初我认为使用[HAR](http://www.softwareishard.com/blog/har-12-spec/)发送 HTTP 请求和响应。 但是，这种格式非常适合存储在基于 JSON 格式的 HTTP 请求的序列。 它含有大量的必需元素添加不必要的复杂性，将 HTTP 消息传递通信的方案。  

另一种方法是使用`application/http`媒体类型如[RFC 7230](http://tools.ietf.org/html/rfc7230)于 HTTP 规范中所述。 此媒体类型使用完全相同的格式用于实际发送 HTTP 消息，在线路上，但整个邮件可以放在另一个 HTTP 请求的正文中。 在我们的例子中我们只需将用作正文消息要发送到事件的集线器。 为方便起见，没有分析这种格式并将其转换为本机[Microsoft ASP.NET Web API 2.2 客户端](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)库中存在一个分析器`HttpRequestMessage`和`HttpResponseMessage`的对象。

为了能够创建我们需要充分利用 C# 此邮件基于[策略表达式](https://msdn.microsoft.com/library/azure/dn910913.aspx)在 Azure API 管理。 下面是将 HTTP 请求消息发送到 Azure 事件集线器的策略。

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>策略声明
那里一些特定的事情值得一提的是关于此策略表达式。 日志对 eventhub 策略有一个称为记录器 id，它是指已创建 API 管理服务中的记录器名称的属性。 [如何将事件记录到 Azure 事件集线器在 Azure API 管理](api-management-howto-log-event-hubs.md)文档中找不到如何设置事件中心记录器中的 API 管理服务的详细信息。 第二个属性是可选参数，指示事件集线器的分区来存储中的邮件。 事件集线器使用分区启用可扩展性，并且需要最少两个。 在分区内只能保证消息按顺序的传递。 如果我们不会指示事件在将该消息放哪个分区中的集线器，它将使用循环调度算法以分散负载。 但是，可能会造成一些我们不按顺序处理消息。  

### <a name="partitions"></a>分区
为确保我们的邮件将被传送到使用者按顺序和利用分区负载分发功能，我选择将 HTTP 请求消息发送到一个分区和 HTTP 响应消息的第二个分区。 这将确保即使负载分布，我们可以保证所有请求将都使用顺序并将会按顺序都耗尽所有响应。 可能会响应要消耗前相应的请求，但因为这不是个问题，因为我们有关联的不同机制请求响应，我们知道请求始终排在响应之前。

### <a name="http-payloads"></a>HTTP 的有效负载
构建后`requestLine`我们检查是否应请求正文会被截断。 请求正文被截断为只有 1024年。 这可能增加，但是个别事件中心消息，限制为 256 KB，因此很有可能一些 HTTP 消息正文将不适合在单个邮件中。 当执行日志记录和分析大量的信息可从只是 HTTP 请求行和标题。 此外，许多 API 请求只返回小团体再也丢失的信息通过截断大主体的价值减少的传输、 处理和存储成本，以保持所有正文内容相比非常小。 最后要注意有关处理主体是我们需要传递`true`as<string>（） 方法因为我们正在阅读的主体内容，但也是需要后端 API 能够阅读正文。 如果为 true，则此方法传递我们导致主体可缓冲，以便它可以读取第二次。 这是一定要知道如果您有一个 API，并不上传大型文件或使用长轮询。 在这些情况下将是最好避免在所有阅读正文。   

### <a name="http-headers"></a>HTTP 标头
HTTP 标头可以只转移转移到以简单的键/值对形式的邮件格式。 我们已选择出某些安全敏感字段，若要避免不必要地泄漏凭据信息最小化。 它不太可能，API 密钥和其他凭据将用于分析目的。 如果我们想要对用户和特定的产品进行分析他们使用的然后我们可以得到，从`context`对象并将其添加到邮件。     
### <a name="message-metadata"></a>消息的元数据
在生成要发送到事件中心的完整消息时，第一行是不真正属于`application/http`消息。 第一行是组成该消息是否请求或响应消息，消息 id 用于将请求与响应关联起来的附加元数据。 消息 id 创建通过使用另一策略如下所示︰

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

我们可以创建请求消息，直到返回，然后只需发送的请求和响应在一个消息响应变量中存储的。 但是，通过分别发送请求和响应，并使用消息 id 关联两个，我们接到有点更大的灵活性的消息大小，能够充分利用多个分区，同时维护邮件顺序和请求将显示在我们记录仪表板快。 此外可能有一些方案有效响应永远不会被发送到事件中心，可能是由于在 API 管理服务中，一个致命的请求错误，但我们仍将具有请求的记录。

要发送的响应的 HTTP 消息的策略看起来非常类似于请求，因此完成策略配置如下所示︰

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

`set-variable`策略创建一个值，可由两个访问`log-to-eventhub`中的策略`<inbound>`部分和`<outbound>`部分。  

## <a name="receiving-events-from-event-hubs"></a>从事件集线器接收事件
使用[AMQP 协议](http://www.amqp.org/)接收从 Azure 事件中心的事件。 Microsoft 服务总线团队做客户端库可以轻松的使用事件。 支持两种不同的方法、 一个正在*直接使用者*和其他使用`EventProcessorHost`类。 [事件集线器编程指南](../event-hubs/event-hubs-programming-guide.md)中找不到这两种方法的示例。 短版本不同的是，`Direct Consumer`为您提供完整的控制和`EventProcessorHost`为您但作出某些假设处理这些事件的方式执行一些琐碎工作。  

### <a name="eventprocessorhost"></a>EventProcessorHost
在此示例中，我们将使用`EventProcessorHost`为简单起见，但是它可能不具体到本方案的最佳选择。 `EventProcessorHost`执行硬工作确保您不必担心线程特定的事件处理器类中的问题。 不过，在我们的方案中，我们只需将消息转换为另一种格式，将其沿传递给另一个服务使用异步方法。 必要更新共享的状态，因此没有风险的线程处理问题。 大多数情况下，对`EventProcessorHost`可能是最佳选择，这当然是更容易的选项。     

### <a name="ieventprocessor"></a>IEventProcessor
在使用时的核心概念`EventProcessorHost`是创建实现的`IEventProcessor`接口包含的方法`ProcessEventAsync`。 该方法的实质如下所示︰

  异步任务 IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

EventData 对象的列表传递给该方法，我们循环访问该列表。 个字节的每一种方法分析为 HttpMessage 对象，该对象将被传递给 IHttpMessageProcessor 的实例。

### <a name="httpmessage"></a>HttpMessage
`HttpMessage`实例都包含三个部分的数据︰

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

`HttpMessage`实例包含`MessageId`，我们可以将 HTTP 请求连接到相应的 HTTP 响应和一个布尔值，如果该对象包含一个 HttpRequestMessage 和 HttpResponseMessage 的实例标识的 GUID。 从 HTTP 类中使用内置的`System.Net.Http`，我就能够利用`application/http`分析中包含的代码`System.Net.Http.Formatting`。  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
`HttpMessage`实例随后即被转发给实现`IHttpMessageProcessor`即接口创建使分离接收和从 Azure 事件中心事件的解释和实际处理的事情。


## <a name="forwarding-the-http-message"></a>发送的 HTTP 消息
对于此示例，我决定一定会非常有趣，可以将 HTTP 请求通过推送到[Runscope](http://www.runscope.com)。 Runscope 是一种基于云服务，从事 HTTP 调试、 日志记录和监视。 他们有自由层，因此很容易尝试，它使我们看到中实时流过我们的 API 管理服务的 HTTP 请求。

`IHttpMessageProcessor`实现如下所示，

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

我以前能够充分利用可以方便地对推[为 Runscope 的现有客户端库](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha)的`HttpRequestMessage`，`HttpResponseMessage`实例上移到他们的服务。 为了访问 Runscope API 需要一个帐户和一个 API 键。 [创建的应用程序访问 Runscope api](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api)截屏视频中找不到用于获取 API 密钥的说明。

## <a name="complete-sample"></a>完整示例
[源代码](https://github.com/darrelmiller/ApimEventProcessor)和测试有关的示例是在 Github 上。 您将需要[API 管理服务](api-management-get-started.md)、[连接的事件中心](api-management-howto-log-event-hubs.md)和一个[存储帐户](../storage/storage-create-storage-account.md)来运行该示例，为自己。   

该示例是刚才的简单控制台应用程序侦听的事件从事件中心将转换到`HttpRequestMessage`，`HttpResponseMessage`对象，然后将它们转发到 Runscope API。

在下面的动画图像，您可以看到正在在开发人员门户中，控制台应用程序显示邮件的接收、 处理和转发请求和响应显示在 Runscope 通信检查器 api 发出的请求。

![请求被转发到 Runscope 的演示](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>摘要
Azure 的 API 管理服务提供了捕获您的 Api 与旅行的 HTTP 通信的理想场所。 Azure 事件集线器是一种高度可扩展、 低成本的解决方案，用于捕获该通讯并送入辅助处理系统上的日志记录，监控和其他复杂的分析。 连接到第三方通讯监控系统 Runscope 为几十个简短的代码非常简单，就像。

## <a name="next-steps"></a>下一步行动
-   了解更多有关 Azure 事件集线器
    -   [开始使用 Azure 事件集线器](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [接收邮件 EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [事件集线器编程指南](../event-hubs/event-hubs-programming-guide.md)
-   了解更多关于 API 管理和事件集线器集成
    -   [如何将事件记录到 Azure API 管理 Azure 事件集线器](api-management-howto-log-event-hubs.md)
    -   [记录器实体引用](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [日志对 eventhub 策略引用](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    