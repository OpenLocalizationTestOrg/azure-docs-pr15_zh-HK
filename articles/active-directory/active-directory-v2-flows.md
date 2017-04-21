<properties
    pageTitle="V2.0 终结点的类型 |Microsoft Azure"
    description="应用程序和方案支持的 Azure AD v2.0 终结点的类型。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>V2.0 终结点的应用程序的类型
V2.0 终结点支持的各种现代应用程序体系结构中，所有这些都基于[OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)和/或[连接 OpenID](active-directory-v2-protocols.md#openid-connect-sign-in-flow)的行业标准协议的身份验证。  此文档简要介绍了您可以生成应用程序的类型，独立的语言或平台的希望。  它将帮助您了解高级别方案之前您[跳转到代码的权限](active-directory-appmodel-v2-overview.md#getting-started)。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

## <a name="the-basics"></a>基础知识
每个使用 v2.0 终结点的应用程序将需要在[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)注册。  应用程序注册过程将收集和将几个值分配给您的应用程序︰

- 唯一地标识您的应用程序的**应用程序 Id**
- **重定向 URI** ，用于直接返回到您的应用程序响应
- 其他几个特定于方案的值。  有关详细信息，了解如何[注册应用程序](active-directory-v2-app-registration.md)。

注册后，该应用程序通信与 Azure 广告通过将请求发送到 Azure Active Directory v2.0 终结点。  我们提供开源框架和库来满足请求，这些请求的详细信息，或者通过手工创建对这些终结点的请求执行身份验证逻辑︰

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web 应用程序
对于 web 应用程序 （.NET、 PHP、 Java、 拼音、 Python、 节点等） 可通过浏览器访问，您可以执行用户使用[OpenID 连接](active-directory-v2-protocols.md#openid-connect-sign-in-flow)登录。  在 OpenID 连接使 web 应用程序会接收到`id_token`，验证用户的身份，并提供了有关索赔表单中的用户信息的安全标记︰

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

您可以了解有关的所有类型的令牌和在[v2.0 令牌引用](active-directory-v2-tokens.md)的应用程序可用的声明。

在 web 服务器应用程序，登录身份验证流采用下列高级别步骤︰

![泳道的 web 应用程序的图像](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Id_token 使用公用签名密钥从 v2.0 终结点接收到的验证足以确保该用户的标识，并设置可用于标识在后续页请求的用户的会话 cookie。

若要查看运行中的这种情况下，试验中我们[入门](active-directory-appmodel-v2-overview.md#getting-started)部分的 web 应用程序签入代码示例之一。

除了简单登录，可能还需要 web 服务器应用程序来访问 REST API，如一些其他 web 服务。  在这种情况下使 web 服务器应用程序可以进行组合 OpenID 连接和 OAuth 2.0 流，使用[OAuth 2.0 授权码流](active-directory-v2-protocols.md#oauth2-authorization-code-flow)。 这种情况是我们[WebApp WebAPI 入门主题](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)中涵盖以下方面。

## <a name="web-apis"></a>Web Api
可以使用 v2.0 终结点来保护 web 服务，如您的应用程序的 rest 风格的 Web API。  而不是 id_tokens 和会话 cookie，Web Api 使用 OAuth 2.0 access_tokens 来保护他们的数据和验证传入的请求。  Web API 的调用方追加了 access_token 中的 HTTP 请求的授权标头︰

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

然后，Web API 可以使用 access_token 来验证 API 调用者的身份并从编码为 access_token 的声明提取有关调用方的信息。  您可以了解有关的所有类型的令牌和在[v2.0 令牌引用](active-directory-v2-tokens.md)的应用程序可用的声明。

Web API 可以让用户选择-在/退出某些功能或数据的电源通过公开又称为[作用域](active-directory-v2-scopes.md)的权限。  用户必须同意的范围种在流动过程中，对于调用应用程序中获取权限范围内的。  询问用户的权限，以及在 Web API 接收的所有 access_tokens 中记录这些权限将负责 v2.0 终结点。  所有 Web API 需要担心是验证它接收每次调用 access_tokens 并执行适当的授权检查。

Web API 可以从所有类型的应用程序，包括 web 服务器应用程序、 桌面和移动应用程序，单个页面的应用程序、 服务器端守护程序和甚至是其他 Web Api 接收 access_tokens。  Web api 身份验证的高级别流程如下所示︰

![Web API 泳道图像](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

要了解有关 authorization_codes、 refresh_tokens 和 access_tokens 的详细的步骤，请阅读有关[OAuth 2.0 协议](active-directory-v2-protocols-oauth-code.md)。

若要了解如何保护 web api 加装 OAuth2 access_tokens 签出在我们的[快速入门部分](active-directory-appmodel-v2-overview.md#getting-started)的 web api 代码示例。


## <a name="mobile-and-native-apps"></a>移动和本机应用程序
安装在设备上，例如移动和桌面应用程序的应用程序经常需要访问后端服务或 Web Api 的数据存储和执行同一用户的各种功能。  这些应用程序可以将登录和授权添加到后端服务使用[OAuth 2.0 授权码流](active-directory-v2-protocols-oauth-code.md)。  

在此流程，应用程序接收授权从 v2.0 终结后用户登录，它代表应用程序的调用后端服务代表当前已登录的用户的权限。  应用程序然后可以交换在后台 OAuth 2.0 access_token 和 refresh_token authoriztion_code。  应用程序可以使用 access_token 来验证对 Web Api 在 HTTP 请求中，并且可以使用 refresh_token 来获取新的 access_tokens 较旧的到期时。

![本机应用程序泳道图像](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>单个页面的应用程序 (javascript)
许多现代的应用程序具有单个页面的应用程序 (SPA) 前端书面主要在 javascript 中，通常使用框架如 AngularJS、 Ember.js、 Durandal 等。 Azure AD v2.0 终结点支持这些应用程序使用[OAuth 2.0 隐式流动](active-directory-v2-protocols-implicit.md)。

在此流程，应用程序接收标记从 v2.0 执行任何后端服务器与服务器交换的情况下直接授权终结点。  这样，所有的验证逻辑和会话处理采取置于完全 javascript 客户端，而无需执行额外的页重定向。

![隐式流泳道图像](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

若要查看运行中的这种情况下，尝试在我们的[入门](active-directory-appmodel-v2-overview.md#getting-started)部分中的单页应用程序代码示例之一。

### <a name="daemonsserver-side-apps"></a>守护程序/服务器端应用程序
包含长时间运行的流程或不存在的用户的情况下运行的应用程序也需要一种方法来访问受保护的资源，例如 Web Api。  这些应用程序可以进行身份验证并获取令牌使用应用程序的标识 （而不是用户的委派的身份） 使用 OAuth 2.0 客户端凭据流。

在此流程，应用程序获取令牌通过直接与交互`/token`终结点︰

![守护进程的应用程序泳道图像](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

若要生成守护程序的应用程序，请参阅我们的[入门](active-directory-appmodel-v2-overview.md#getting-started)部分中的客户端凭据 documeenation 或[此.NET 示例应用程序](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)是指。

## <a name="current-limitations"></a>当前的限制
这些类型的应用程序目前不支持通过 v2.0 终结点，但在规划之中。  [V2.0 限制文章](active-directory-v2-limitations.md)中描述的 2.0 版终结点其他的局限性和限制。

### <a name="chained-web-apis-on-behalf-of"></a>链接的 web Api （上代表的）
许多结构包括一个需要调用另一个下游 Web API，同时受 v2.0 终结点的 Web API。  该方案是在本机客户端具有 Web API 后端，后者又调用 Microsoft 联机服务例如 Office 365 或图形 API 中常见。

可以使用 OAuth 2.0 Jwt 载体凭据授予，否则称为[On-Behalf-Of 流](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)支持此链接的 Web API 方案。  不过，在代表的流量目前未实现 v2.0 终结点中。  若要查看此流原理在普遍适用的 Azure AD 服务，请检查[在 GitHub 上代表的代码示例](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)。
