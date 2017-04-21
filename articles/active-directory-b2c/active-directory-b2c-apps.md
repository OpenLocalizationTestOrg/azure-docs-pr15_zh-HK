<properties
    pageTitle="Azure AD B2C |Microsoft Azure"
    description="您可以在 Azure 活动目录 B2C 中生成应用程序的类型。"
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure 的活动目录 B2C︰ 类型的应用程序

Azure 的活动目录 (AD Azure) B2C 的各种现代应用程序体系结构支持身份验证。 所有这些都基于[OAuth 2.0](active-directory-b2c-reference-protocols.md)或[连接 OpenID](active-directory-b2c-reference-protocols.md)的行业标准协议。 本文简要介绍了可生成的应用程序的类型，独立的语言或平台的希望。 它还有助于您理解之前您[开始构建应用程序](active-directory-b2c-overview.md#getting-started)的高级方案。

## <a name="the-basics"></a>基础知识
每个应用程序使用 Azure AD B2C 必须在[Azure 门户](https://portal.azure.com/)通过[B2C 目录](active-directory-b2c-get-started.md)中注册。 应用程序注册过程收集和将几个值分配给您的应用程序︰

- 唯一地标识您的应用程序**的应用程序 ID** 。
- 可用于指导返回到您的应用程序响应**重定向 URI** 。
- 任何其他特定于方案的值。 有关详细信息，了解如何[注册应用程序](active-directory-b2c-app-registration.md)。

注册应用程序后，它与通信 Azure 广告通过将请求发送到 Azure AD v2.0 终结点︰

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

每个请求发送到 Azure AD B2C 指定**策略**。 由策略控制 Azure 的广告的行为。 您可以使用这些终结点创建一整套高度自定义的用户体验。 通用的策略包括注册、 登录、 并配置文件编辑策略。 如果您还不熟悉策略，应在继续操作之前了解 Azure AD B2C[可扩展策略框架](active-directory-b2c-reference-policies.md)。

每个应用程序的交互使用 v2.0 终结点如下所示类似的高级模式︰

1. 该应用程序将定向到 2.0 版的终结点用户执行[策略](active-directory-b2c-reference-policies.md)。
2. 在用户完成根据策略定义的策略。
4. 该应用程序从 v2.0 终结点接收某些类型的安全令牌。
5. 该应用程序将使用安全令牌来访问受保护的信息或受保护的资源。
6. 资源服务器验证安全令牌，以验证可以授予访问权限。
7. 该应用程序会定期刷新安全令牌。

<!-- TODO: Need a page for libraries to link to -->
这些步骤可以不同于您正在构建的应用程序的类型上有细微的差别。 开放源代码库可以为您的地址详细信息。

## <a name="web-apps"></a>Web 应用程序
对于 web 应用程序 （包括.NET、 PHP、 Java、 拼音、 Python 和 Node.js） 承载的服务器上并可通过浏览器访问，Azure AD B2C 支持[OpenID 连接](active-directory-b2c-reference-protocols.md)的所有用户体验。 这包括登录、 注册、 和配置文件管理。 在 Azure AD B2C 的 OpenID 连接实现，您的 web 应用程序启动这些用户体验到 Azure AD 发出身份验证请求。 该请求的结果是`id_token`。 此安全令牌表示用户的标识。 它还提供了有关索赔表单中的用户信息︰

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

了解更多有关类型的令牌和在[B2C 标记引用](active-directory-b2c-reference-tokens.md)的应用程序可用的声明。

在 web 应用程序中，每个[策略](active-directory-b2c-reference-policies.md)的执行采用这些主要步骤︰

![泳道的 web 应用程序的图像](./media/active-directory-b2c-apps/webapp.png)

验证`id_token`通过从 Azure AD 接收公用签名密钥足以用来验证用户的身份。 这还将设置可用于标识在后续页请求的用户的会话 cookie。

若要查看运行中的这种情况下，可以尝试中[入门部分](active-directory-b2c-overview.md#getting-started)我们的 web 应用程序登录的代码示例。

除了促进简单的签名中，web 服务器应用程序可能还需要访问后端 web 服务。 在这种情况下，web 应用程序可以执行略有不同的[OpenID 连接流](active-directory-b2c-reference-oidc.md)和使用授权代码获取令牌和刷新令牌。 这种情况下会在以下[Web Api 部分](#web-apis)描述。

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Web Api
可以使用 Azure AD B2C 来保护 web 服务，如您的应用程序的 rest 风格 web API。 Web Api 可以使用 OAuth 2.0 来保护其数据，通过验证传入的 HTTP 请求使用标记。 Web API 的调用方将追加的 HTTP 请求的授权标头中的标记︰

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API 然后可以使用此标记，来验证 API 调用者的身份并在标记中编码的索赔从提取有关调用方的信息。 了解更多有关类型的令牌和在[Azure AD B2C 标记引用](active-directory-b2c-reference-tokens.md)的应用程序可用的声明。

> [AZURE.NOTE]
    Azure AD B2C 目前支持仅 web 自己已知的客户端访问的 Api。 例如，您完成的应用程序可能包括 iOS 应用程序，Android 应用程序和后端 web API。 完全支持这种体系结构。 允许合作伙伴客户端，如另一个的 iOS 应用程序访问同一 web API 当前不受支持。 所有完整的应用程序的组件必须共享一个应用程序 id。

Web API 可以从多种类型的客户端，包括 web 应用程序、 桌面和移动应用程序、 单个页面的应用程序、 服务器端守护程序和其他 web Api 接收标记。 这里是 web API 调用一个 web 应用程序的完整流程的示例︰

![Web 应用程序 Web API 泳道图像](./media/active-directory-b2c-apps/webapi.png)

若要了解更多有关的授权码，刷新令牌和获取标记的步骤，了解[OAuth 2.0 协议](active-directory-b2c-reference-oauth-code.md)。

若要了解如何保护 web API 使用 Azure AD B2C，签出 web API 教程中我们的[入门一节](active-directory-b2c-overview.md#getting-started)。

## <a name="mobile-and-native-apps"></a>移动和本机应用程序
安装的设备，如移动和桌面应用程序，应用程序经常需要访问后端服务或代表用户的 web Api。 使用 Azure AD B2C 和[OAuth 2.0 授权代码流](active-directory-b2c-reference-oauth-code.md)，可以将自定义的标识管理经验添加到本机应用程序和安全地调用后端服务。  

在此流程，应用程序执行[策略](active-directory-b2c-reference-policies.md)并接收`authorization_code`从 Azure 广告后用户完成策略。 `authorization_code`表示要调用后端服务代表当前登录的用户的应用程序的权限。 应用程序然后可以交换`authorization_code`在后台中为`id_token`， `refresh_token`。  应用程序可以使用`id_token`向后端 web API 中的 HTTP 请求进行身份验证。 它也可以使用`refresh_token`以获取一个新`id_token`旧过期时。

> [AZURE.NOTE]
    Azure AD B2C 目前支持仅用于访问应用程序自己的后端 web 服务的令牌。 例如，您完成的应用程序可能包括 iOS 应用程序，Android 应用程序和后端 web API。 完全支持这种体系结构。 目前不支持允许您使用 OAuth 2.0 的访问令牌来访问合作伙伴 web API 的 iOS 应用程序。 所有完整的应用程序的组件必须共享一个应用程序 id。

![本机应用程序泳道图像](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>当前的限制
Azure AD B2C 当前不支持以下类型的应用程序，但它们在 roadmapy。 其他限制和 Azure AD B2C 与相关的限制所述[限制和限制](active-directory-b2c-limitations.md)。

### <a name="single-page-apps-javascript"></a>单个页面的应用程序 (JavaScript)
许多现代的应用程序具有主要用 JavaScript 编写的单页面应用程序前端。 他们通常使用一个框架，如 AngularJS、 Ember.js 或 Durandal。 通常可用 Azure 广告服务支持这些应用程序通过使用 OAuth 2.0 隐式流。 但是，这个流量尚无在 Azure AD B2C。

### <a name="daemonsserver-side-apps"></a>守护程序/服务器端应用程序
包含长时间运行的进程或用户如果没有运行的应用程序也需要一种方法来访问受保护的资源，如 web Api。 这些应用程序可以进行身份验证并获取令牌通过使用应用程序的标识 （而不是用户的委派的身份） 以及使用 OAuth 2.0 客户端凭据流。

Azure AD B2C 目前不支持此流。 这些应用程序可以获取令牌仅在非交互式用户流发生之后。

### <a name="web-api-chains-on-behalf-of-flow"></a>Web API 链接 （在代表的流）
许多结构包括一个站点需要调用另一个下游 web API，其中两个都不会得到 Azure AD B2C 的 API。 该方案是在本机客户端有一个后端的 Web API 中常见。 然后，它调用类似 Azure 广告图形 API 的 Microsoft 在线服务。

此链接的 web API 的情况下可以使用 OAuth 2.0 JWT 载体凭据授予，也称为上代表的流的支持。  不过，在代表的流量目前未实现在 Azure AD B2C。
