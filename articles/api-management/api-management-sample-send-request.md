<properties
    pageTitle="在使用 API 管理服务生成的 HTTP 请求"
    description="了解如何使用 API 管理中的请求和响应策略从您的 API 调用外部服务"
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


# <a name="using-external-services-from-the-azure-api-management-service"></a>使用外部服务从 Azure API 管理服务

在 Azure API 管理服务可用的策略可以执行范围广泛的纯粹是基于传入请求，传出响应和基本配置信息的有用工作。 但是，能够与外部服务的 API 管理交互策略开辟更多的机会。

我们以前已经看到我们与[日志记录、 监控和分析的 Azure 事件中心服务](api-management-log-to-eventhub-sample.md)的交互方式。 在这篇文章中我们将展示策略，您可以使用任何外部 HTTP 交互基于服务。 这些策略可用于触发远程事件或检索可用来处理原始请求和响应在某些方面的信息。

## <a name="send-one-way-request"></a>发送一个方法请求
可能是最简单的外部交互是允许某种重要事件通知的外部服务的请求的火灾和忘记的样式。 我们可以使用控制流策略`choose`来检测任何一种，我们感兴趣，然后，如果满足条件，我们可以进行外部 HTTP 请求使用[发送一个方法请求](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest)策略的情况。 这可能是如 Hipchat 或可宽延时间或如 SendGrid 或 MailChimp，邮件 API 消息传递系统的请求或类似于 PagerDuty 的关键支持事件。 所有这些邮件系统具有简单 HTTP Api，我们可以很容易地调用。

### <a name="alerting-with-slack"></a>可宽延时间与报警
下面的示例演示如何将消息发送到可宽延时间的聊天室，HTTP 响应状态代码是否大于或等于 500。 500 范围错误指示我们自己不能解决我们的 API 客户端 API 的问题。 它通常需要某种类型的干预我们。  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

可宽延时间具有入站的 web 挂钩的概念。 在配置入站的 web 挂钩时，可宽延时间生成特殊 URL 可以做简单的 POST 请求，并将邮件传递到可宽延时间的通道。 我们创建的 JSON 主体基于定义可宽延时间的格式。

![可宽延时间 Web 挂钩](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>火灾和佳忘记？
使用请求的火灾和忘记样式时某些权衡。 如果由于某种原因，请求就会失败，则不会报告失败。 在此特定情况下，不保证有辅助故障报告系统并等待响应的额外的性能开销的复杂性。 对于方案时一定要检查响应，则[发送请求](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)策略是更好的选择。

## <a name="send-request"></a>发送请求
`send-request`策略允许使用外部服务来执行复杂的处理函数并返回数据的 API 管理服务，可用于进一步的策略处理。

### <a name="authorizing-reference-tokens"></a>授权的引用标记
API 管理的主要功能保护后端资源。 如果使用您的 API 的授权服务器创建[JWT 令牌](http://jwt.io/)作为它 OAuth2 的流程，像[Azure Active Directory](../active-directory/active-directory-aadconnect.md) ，则可以使用`validate-jwt`策略来验证令牌的有效期。 但是，某些授权服务器创建所谓的[引用标记](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/)，而无需拨打电话回授权服务器无法验证。

### <a name="standardized-introspection"></a>标准化的内省
在过去已没有标准化的方法验证与授权服务器的引用标记。 但是最近建议标准[RFC 7662](https://tools.ietf.org/html/rfc7662)公布由 IETF 定义资源服务器如何验证标记的有效性。

### <a name="extracting-the-token"></a>该标记中提取
第一步是提取授权标头中的标记。 标头值必须设置为`Bearer`授权方案、 一个空格，然后根据[RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1)授权令牌。 遗憾的是一些未授权方案的情况。 分析时，考虑到这，我们分割空间上的标头值，并从返回的字符串的数组中选择最后一个字符串。 这为标头格式不正确的授权提供一种解决方法。

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>验证请求
一旦我们有了授权标记，我们可以验证该令牌的请求。 RFC 7662 调用此过程内省，并且要求您`POST`的内省资源一个 HTML 窗体。 HTML 表单必须至少具有键包含键/值对`token`。 此外必须授权服务器对该请求进行验证以确保恶意客户端不能转到 trawling 的有效标记。

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>检查响应
`response-variable-name`属性用来访问返回的响应。 在此属性中定义的名称可以用作键到`context.Variables`字典访问`IResponse`对象。

我们可以从响应对象检索正文和 RFC 7622 告诉我们响应必须是一个 JSON 对象，并且必须包含至少一个名为的属性`active`，它是一个布尔值。 当`active`为 true，则该标记被视为有效。

### <a name="reporting-failure"></a>报告失败
我们使用`<choose>`策略来检测如果令牌无效，如果是，返回 401 响应。

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

根据[RFC 6750](https://tools.ietf.org/html/rfc6750#section-3)描述如何`bearer`应使用标记，我们也返回`WWW-Authenticate`401 响应标头。 WWW 身份验证用于指示如何构造正确授权的请求的客户端。 由于各种各样的方法可以使用 OAuth2 框架，很难进行通信所需的所有信息。 幸运的是有工作正在开展的旨在帮助[客户了解如何正确授权对资源服务器的请求](http://tools.ietf.org/html/draft-jones-oauth-discovery-00)。

### <a name="final-solution"></a>最终的解决方案
将所有部件都放置在一起，我们得到以下策略︰

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

这是许多方式的示例之一`send-request`策略可用于将有用的外部服务集成到请求和响应流经的 API 管理服务的过程。

## <a name="response-composition"></a>响应组合
`send-request`策略可用于增强主请求到后端系统，因为在前面的示例中，我们看到或可以用作后端调用的完成更换。 使用这种方法我们可以轻松创建复合资源聚合来自多个不同的系统。

### <a name="building-a-dashboard"></a>构建仪表板   
有时您希望能公开信息的存在在多个后端系统中，例如，以推动一个仪表板。 Kpi 来自所有不同后端，但是您不愿意提供直接访问它们，如果无法在单个请求中检索所有的信息就好。 也许一些后端信息需要一些切片并进行分解和少量净化第一次 ！ 可以缓存该复合资源将可用来减少后端负载，如您所知的用户拥有的锤打 f5，才能看到如果及其运行不佳的衡量标准可能会改变一个习惯。    

### <a name="faking-the-resource"></a>欺骗的资源
构建我们面板资源的第一步是配置 API 管理出版商门户中的新操作。 这将是用来配置我们的组合策略来构建我们的动态资源的占位符操作。

![仪表板操作](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>发出请求
一次`dashboard`在创建操作，我们可以配置该操作的具体策略。 

![仪表板操作](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

第一步是提取任何查询参数从传入的请求，以便我们可以将其转发给我们的后端。 在此示例中我们仪表板显示的基于一段时间内的信息因此具有`fromDate`和`toDate`参数。 我们可以使用`set-variable`策略与请求的 URL 中提取信息。

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

一旦我们有了这些信息我们可以向所有的后端系统发出请求。 每个请求构造一个新的 URL 的参数信息并调用其相应的服务器将响应存储在上下文变量。

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

这些请求将执行按顺序排列，并不理想。 在未来版本中我们还将推出新的策略名为`wait`，将启用所有并行执行这些请求。

### <a name="responding"></a>响应

构造复合的响应，我们可以使用[返回响应](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse)策略。 `set-body`元素可以使用表达式来构造一个新`JObject`与属性作为嵌入的组件表示。

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

完成策略如下所示︰

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

在配置中占位符的操作，我们可以配置仪表板资源缓存至少一小时，因为我们了解数据的性质意味着，即使是已过时，它仍然会充分有效地传达给用户有价值的信息的一小时。

## <a name="summary"></a>摘要
Azure 的 API 管理服务提供了灵活的策略，可以有选择地应用于 HTTP 通信量，并使后端服务的组合。 要增强 API 网关与报警功能、 验证、 验证功能或创建新的复合资源，基于多个后端服务， `send-request` ，相关的策略打开一个可能性的世界。

## <a name="watch-a-video-overview-of-these-policies"></a>观看视频这些策略概述
有关[发送请求](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)，[发送一个方法请求](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest)和[返回响应](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse)在这篇文章中介绍的策略的详细信息，请观看下面的视频。

> [AZURE.VIDEO send-request-and-return-response-policies]
