
<properties
    pageTitle="Azure AD v2.0 OAuth 客户端凭据流 |Microsoft Azure"
    description="构建 web 应用程序使用 Azure 广告实现 OAuth 2.0 的身份验证协议。"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>2.0 版协议的 OAuth 2.0 客户端凭据流

[OAuth 2.0 客户端凭据授予](http://tools.ietf.org/html/rfc6749#section-4.4)，有时也称为"双边 OAuth"可以用于访问宿主的 web 资源使用应用程序的标识。  它通常用于必须在没有最终用户即时 precense 后台运行的服务器到服务器交互。  这些类型的应用程序通常称为**守护程序**或**服务帐户**。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

更常见的三个"三边 OAuth，"在客户端应用程序被授予代表特定用户访问资源的权限。  权限已被**委派**用户应用程序，通常在[同意](active-directory-v2-scopes.md)过程。  但是，客户端凭据流，所有权限都是**直接**与应用程序本身。  当应用程序提供的资源的标记时，该资源实施应用程序本身有权执行某些操作的某些用户，具有授权的不。

## <a name="protocol-diagram"></a>协议关系图
整个客户端凭据流程如下所示 — 下面将详细介绍每个步骤。

![客户端凭据流](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>获得直接的授权 
应用程序通常接收直接授权访问资源的两种方法︰ 通过访问控制列表的资源，或在 Azure 广告中的应用程序权限分配。  有其他几种方式可以选择资源进行授权的客户端，而且资源的每个服务器可以选择最适合其应用程序的方法。  这两种方法在 Azure 的广告是最常见和被推荐用于客户端和要执行的客户端凭据流程的资源。

### <a name="access-control-lists"></a>访问控制列表
给定的资源提供程序可能会强制执行授权检查基于它知道并授予某些特定级别的访问权限的应用程序 Id 的列表。  当资源从 v2.0 终结点收到一个令牌时，它可以解码该标记，并提取从客户端的应用程序 ID`appid`和`iss`索赔。  它可以进行比较，它维护应用程序对某些访问控制列表 (ACL)。  粒度和访问控制列表的方法可以改变极大地从资源到资源。

一个常见的用例此类 Acl 是测试 web 应用程序的流道或 web api。  Web api 可能为其各种客户端中只允许其完全控制权限的一个子集。  但为了 api 上运行端到端的测试，测试客户端将创建一个令牌从 v2.0 终结点，并且将它们发送到 api。  Api 可以然后 ACL 用于完全访问 api 的整个功能测试客户端应用程序 ID。  请注意，是否您在服务上有这样的列表，您应确保不仅验证调用方的`appid`，但另外，验证`iss`令牌的信任也是。

这种类型是授权的常见的守护进程和服务帐户需要访问数据使用者与 Microsoft 的个人帐户的用户所拥有的。  对于由组织拥有的数据，建议您获得应用程序 perimssions 通过必要的授权。

### <a name="application-permissions"></a>应用程序权限
而不是使用 Acl，Api 可以公开一组可被授予对应用程序的**应用程序的权限**。  应用程序权限授予的组织管理员应用程序，只可用于访问由该组织和其员工拥有的数据。  例如，Microsoft Graph 将公开多个应用程序的权限︰

- 所有邮箱中读取邮件
- 读取和写入的所有邮箱中的邮件
- 作为任何用户发送邮件
- 读取目录数据
- [+ 更多](https://graph.microsoft.io)

要获得这些权限在您的应用程序中，您可以执行以下步骤。

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>请求的应用程序注册门户的权限

- 如果还没有的话，请导航到您应用程序[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)，或[创建一个应用程序](active-directory-v2-app-registration.md)中。  您需要确保您的应用程序已创建至少一个应用程序密码。
- 找到**直接应用程序权限**部分，并添加您的应用程序要求的权限。
- 确保**保存**应用程序注册

#### <a name="recommended-sign-the-user-into-your-app"></a>建议︰ 用户登录您的应用程序

一般情况下当构建应用程序，使用应用程序的权限时，应用程序需要具有允许管理员批准的应用程序的权限的页面/视图。  该网页可以是应用程序的注册流程，应用程序的设置的一部分或专用的"连接"流的一部分。  在许多情况下，其意义的应用程序将此页显示"连接"视图仅在用户已登录的工作或学校 Microsoft 帐户后。

用户登录该应用程序使您可以确定用户要求审核的应用程序权限之前对所属组织。  虽然并非绝对必需的它可以帮助您创建组织用户更直观的体验。  签名中的用户，请按照我们[2.0 版协议教程](active-directory-v2-protocols.md)。

#### <a name="request-the-permissions-from-a-directory-admin"></a>从目录管理员请求权限

当您准备请求权限从该公司的管理时，可以将用户重定向到 v2.0**同意终结点的管理**。

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 必填 | 要请求的权限目录租户。  可以提供 guid 或好记的名称格式。  如果不知道用户属于哪个租户，想让他们使用任何租户登录，请使用`common`。 |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| redirect_uri | 必填 | 要为您的应用程序处理的发送的响应的 redirect_uri。  它必须完全匹配您在中注册门户，不同之处在于它必须是 url 编码并且能与其他路径段 redirect_uris 之一。 |
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  使用的状态进行编码之前发生身份验证请求，例如在页面或视图上的应用程序中用户的状态信息。 |

在这种情况下，只有一名租户管理员可以登录以完成该请求 Azure 广告将强制执行。  管理员可能需要批准所有您需要获得为您的应用程序注册门户中的直接应用权限。 

##### <a name="successful-response"></a>成功的响应
如果管理员批准的应用程序的权限，将为成功的响应︰

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 它请求中的 guid 格式的权限授予您的应用程序目录租户。 |
| 状态 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  使用的状态进行编码之前发生身份验证请求，例如在页面或视图上的应用程序中用户的状态信息。 |
| admin_consent | 将设置为`True`。 |


##### <a name="error-response"></a>错误响应
如果管理员不赞成您应用程序的权限，失败的响应将是︰

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员识别错误的根本原因。  |

一旦您已从提供方终结点的应用程序获得到成功的响应，您的应用程序已获得该请求的直接应用权限。  您现在可以移动到请求的所需资源的标记。

## <a name="get-a-token"></a>获取标记
一旦您已经为您的应用程序获得必要的授权，就可以继续与获取的 Api 的访问令牌。  若要获取一个标记，该标记使用客户端凭据授予，发送 POST 请求到`/token`2.0 版终结点︰

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| 作用域 | 必填 | 传入的值`scope`在此请求中的参数应为资源标识符 (应用程序 ID URI) 所需的资源，用`.default`后缀。  因此例如 Microsoft Graph 提供，该值应为`https://graph.microsoft.com/.default`。  此值会通知 v2.0 终结点，所有的直接应用权限已配置为您的应用程序，它应颁发有关的所需资源的那些标记。 |
| client_secret | 必填 | 为您的应用程序的注册门户中生成应用程序密码。 |
| grant_type | 必填 | 必须为`client_credentials`。 | 

#### <a name="successful-response"></a>成功的响应
成功的响应将采用的格式︰

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| access_token | 请求的访问令牌中。 应用程序可以使用此标记通过身份验证的安全资源，例如 web API。 |
| token_type | 表示标记类型值。 Azure 的广告支持的唯一类型`Bearer`。  |
| expires_in | 多长时间访问令牌无效 （以秒为单位）。 |

#### <a name="error-response"></a>错误响应
错误响应中将采用的格式︰

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。  |
| error_codes | 可以帮助诊断过程中的 STS 特定错误代码的列表。  |
| 时间戳 | 该错误发生的时间。 |
| trace_id | 有助于诊断过程中请求的的唯一标识符。  |
| correlation_id | 有助于诊断程序中各组件请求的的唯一标识符。 |

## <a name="use-a-token"></a>使用令牌
现在，您已经获得一个令牌，可以使用该标记将请求发送到资源。  令牌到期后，只需重复请求`/token`终结点，以获取最新的访问令牌。

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>代码示例
要查看的实现 client_credentials 授予使用该管理员同意终结点的应用程序的示例，请参阅[v2.0 守护程序代码示例](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)。
