<properties
    pageTitle="在 Azure API 管理中的自定义缓存"
    description="了解如何通过键在 Azure API 管理缓存项"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="custom-caching-in-azure-api-management"></a>在 Azure API 管理中的自定义缓存
Azure 的 API 管理服务具有内置支持[HTTP 响应缓存](api-management-howto-cache.md)键使用资源的 URL。 可以通过使用请求标头中修改键`vary-by`属性。 这可用于缓存整个 HTTP 响应 （亦即表示），但有时是很有用的只是缓存的表示形式的一部分。 新的[缓存查找值](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey)和[缓存存储值](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey)策略提供了用于存储和检索任意个策略定义内的数据的能力。 这种能力也将值添加到以前引入的[发送请求](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)策略因为您现在可以缓存来自外部服务的响应。

## <a name="architecture"></a>体系结构  
API 管理服务使用共享的每个租户的数据缓存，这样您还可以访问相同的多个单位达扩展缓存的数据。 但是，使用较多的地区部署时有独立缓存在每个区域内。 因此，务必要将缓存作为数据存储，即一些信息的唯一来源。 如果说过，而且以后决定利用多区域部署，旅行的用户的客户可能会向缓存的数据无法访问。

## <a name="fragment-caching"></a>片段缓存
有某些情况下，返回的响应中包含将耗费大量资源，以确定和尚未保持最新的合理的一段时间的数据部分。 举一个例子，考虑建立由航空公司提供相关航班预订、 飞行状态等信息服务。如果用户是航空公司点计划的成员，他们将还可以与它们的当前状态和累积的里程信息。 此用户相关的信息可能存储在不同的系统中，但它可能需要将其包括在关于飞行状态和预留空间返回的响应。 这可以使用称为片段缓存的过程。 主要表示可从使用某种类型的标记来指明在何处插入与用户相关的信息的源服务器。 

请考虑下面的 JSON 响应从后端 API。


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

和辅助资源的`/userprofile/{userid}`，如下所示，

     { "username" : "Bob Smith", "Status" : "Gold" }

为了确定要包括适当的用户信息，我们需要弄清楚谁是最终用户。 这种机制是依赖于实现。 作为一个示例，我使用`Subject`声称的`JWT`令牌。 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

我们将存储在此`enduserid`在上下文变量中以供将来使用的值。 下一步是确定是否前一个请求已检索到的用户信息并存储在缓存中。 为此，我们使用`cache-lookup-value`策略。

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

如果键值，则没有相对应的缓存中没有项`userprofile`将创建上下文变量。 查找使用的成功，我们检查`choose`控制流策略。

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


如果`userprofile`环境变量不存在，则我们要作的 HTTP 请求来检索它。

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

我们使用`enduserid`来构造用户配置文件资源的 URL。 一旦我们有了响应，我们可以从响应的正文中拉出，并将其存储回上下文变量。

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

为了避免我们无需再次进行此 HTTP 请求同一用户发出另一个请求时，我们可以在缓存中存储的用户配置文件。

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

我们将值存储在缓存中使用我们最初试图检索与之完全相同的密钥。 我们选择要存储的值的持续时间应具体取决于更改的信息和如何容错的用户往往是个过时的信息。 

务必要认识到从缓存中检索出的进程，网络请求仍是和可能仍然可以添加数十毫秒到请求。 好处是确定用户配置文件信息时间显著长于，由于需要进行数据库查询或聚合来自多个后端的信息时。

在此过程中的最后一步是与我们的用户配置文件信息更新返回的响应。

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

我选择使用引号作为标记的一部分，以便响应替换不会发生，即使是仍然有效的 JSON。 这主要是要使调试变得更容易。

一旦所有这些步骤中组合在一起，最终结果将是如下所示的下一个策略。

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

此缓存方法主要用于 web 站点在 HTML 由服务器端以使其可以呈现为单个页。 但是，它也可以适用于客户端无法对客户端执行操作的 Api 侧 HTTP 缓存或并不想将该责任放在客户端上。

片段缓存此同一种也可以使用缓存服务器 Redis 后端 web 服务器上，但是，如果使用 API 管理服务来执行这项任务时缓存的片段来自主要的响应比的不同后端。

## <a name="transparent-versioning"></a>透明的版本控制
它是多个不同的实现版本的 API 来支持任何一次的常见做法。 这或许是为了支持不同的环境中，如开发、 测试、 生产等，也可能是要支持早期版本的 API 来提供 API 的使用者将迁移到较新版本的时间。 

一种方法而不需要更改从 Url 的客户端开发人员处理这`/v1/customers`到`/v2/customers`是他们目前要使用并调用适当的后端 URL 的 api 版本存储在使用者的配置文件数据。 为了确定要为特定客户机调用正确的后端 URL，则需要查询一些配置数据。 通过缓存此配置数据，我们可以尽量减少执行此查找的性能损失。

第一步是确定用来配置所需的版本的标识符。 在此示例中，选择要关联到产品订购项的版本。 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

我们再做一个高速缓存查找以是否我们已检索到所需的客户端版本，请参阅。

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

然后我们检查是否我们找不到它在缓存中。

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

如果我们并不知道，然后我们继续并检索它。

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

从响应中提取响应正文文本。

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

将其存储在缓存以备将来使用。

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

和最后更新后端 URL 来选择所需的服务的客户端版本。

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

完全策略如下所示。

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


启用 API 使用者透明地控制由客户端无需更新和重新部署客户端访问后端版本是简洁的解决方案，解决了很多的 API 版本控制问题。

## <a name="tenant-isolation"></a>租户隔离

大规模的多租户部署中一些公司在不同的后端硬件部署上创建承租人的单独的组。 这将大大减少由后端硬件问题影响的客户数。 它还启用了新的软件版本，要分阶段推出。 理想情况下该后端体系结构应该是透明的 API 的使用者。 这可以实现透明的版本控制以类似方式，因为它基于操作使用每个 API 键的配置状态的后端 URL 相同的技术。  

而不是返回的首选的版本的每个订购项的 API，就会返回一个租户与分配的硬件组的标识符。 该标识符可用于构造适当的后端 URL。

## <a name="summary"></a>摘要
自由地使用 Azure API 管理缓存，用于存储任何类型的数据使高效访问配置数据可能会影响处理入站的请求的方式。 它还可以用于存储可补充从后端 API 返回的响应的数据片段。

## <a name="next-steps"></a>下一步行动
请给我们一个反馈 Disqus 线程中此主题如果有这些策略已经启用，为您的其他方案或方案是否要实现但不是觉得当前可能存在。
