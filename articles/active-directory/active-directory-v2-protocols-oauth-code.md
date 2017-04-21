

<properties
    pageTitle="Azure AD v2.0 OAuth 授权代码流 |Microsoft Azure"
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>v2.0 协议-OAuth 2.0 授权码流

OAuth 2.0 授权代码授予可访问受保护的资源，如 web Api 的设备安装的应用程序中。  使用 OAuth 2.0 应用程序模型 2.0 版的实施，可以添加登录并访问到您的移动和桌面应用程序的 API。  本指南是独立于语言，并介绍了如何发送和接收 HTTP 消息而无需使用任何我们的开源库。

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

OAuth 2.0 授权代码流[OAuth 2.0 规范的 4.1 节](http://tools.ietf.org/html/rfc6749)中所述。  它用于在大多数的应用程序类型，包括[web 应用程序](active-directory-v2-flows.md#web-apps)和[本地安装的应用程序](active-directory-v2-flows.md#mobile-and-native-apps)执行身份验证和授权。  它使应用程序安全地获得 access_tokens 用于访问使用 v2.0 端点保护的资源。  

## <a name="protocol-diagram"></a>协议关系图
从较高层面，本机/移动应用程序的整个身份验证流看上去有点像这样︰

![OAuth 授权码流](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>请求授权码
以客户端定向到用户的授权代码流开始`/authorize`终结点。  在此请求中，客户端指示需要从用户获得的权限︰

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] 单击下面的链接以执行此请求 ！ 登录后，您的浏览器应被重定向到`https://localhost/myapp/`与`code`的地址栏中。
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 必填 | `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为`common`， `organizations`， `consumers`，和租户标识符。  有关详细信息，请参阅[协议基础](active-directory-v2-protocols.md#endpoints)。 |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| response_type | 必填 | 必须包括`code`授权代码流。 |
| redirect_uri | 建议 | 您的应用程序，可以发送和接收应用程序的身份验证响应 redirect_uri。  它必须完全匹配您在中注册门户，不同之处在于它必须是 url 编码的 redirect_uris 之一。  对于本机和移动应用程序，应使用默认值的`https://login.microsoftonline.com/common/oauth2/nativeclient`。 |
| 作用域 | 必填 | 空格分隔的列表，您希望用户同意的[范围](active-directory-v2-scopes.md)。  |
| response_mode | 建议 | 指定用于将生成的令牌后发送给您的应用程序的方法。  可以是`query`或`form_post`。  |
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  随机生成唯一值通常用于[防止跨站点请求伪造攻击](http://tools.ietf.org/html/rfc6749#section-10.12)。  状态还用于对应用程序中用户的状态信息进行编码之前发生身份验证请求，例如在页面或视图上。 |
| 提示 | 可选 | 指示用户交互所需的类型。  这次只有效值为登录，无和同意。  `prompt=login`将强制用户输入其凭据在该请求时，绝对值单点登录。  `prompt=none`相反的它将确保不向用户显示任何任何交互式提示符。  如果不能通过单点登录自动完成请求，2.0 版终结点将返回错误。  `prompt=consent`登录用户，要求用户向应用程序授予权限之后，将触发 OAuth 同意对话框。 |
| login_hint | 可选 | 可以用于预填充用户名/电子邮件地址字段的登录页面的用户，如果您知道他们的用户名提前。  通常应用程序将使用此参数，重新进行身份验证期间，已有从以前登录使用中提取用户名`preferred_username`说。 |
| domain_hint | 可选 | 可以是一种`consumers`或`organizations`。  如果包含，则将跳过基于电子邮件的搜索过程该用户经历在 2.0 版登录页面，从而导致更为流畅的用户体验。  通常应用程序将使用此参数时重新进行身份验证，通过提取`tid`从以前的签入。  如果`tid`声称值是`9188040d-6c67-4c5b-b112-36a304b66dad`，您应该使用`domain_hint=consumers`。  否则，请使用`domain_hint=organizations`。 |

在这种情况下，将要求用户输入其凭据并完成身份验证。  V2.0 终结点还将确保用户已同意中指示的权限`scope`查询参数。  如果用户不具有这些权限的任何许可的它将要求用户同意所需的权限。  [此处提供的权限，同意的情况下和多租户应用程序](active-directory-v2-scopes.md)的详细信息。

一旦用户进行身份验证并授予同意时，2.0 版终结点将返回以指示您的应用程序的响应`redirect_uri`，使用指定的方法在`response_mode`参数。

#### <a name="successful-response"></a>成功的响应
成功的响应使用`response_mode=query`如下所示︰

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 代码 | 应用程序请求的授权。 应用程序可以使用授权代码请求的目标资源的访问令牌。  Authorization_codes 是非常短期，他们通常在大约 10 分钟后过期。 |
| 状态 | 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |

#### <a name="error-response"></a>错误响应
此外可能被错误响应发送到`redirect_uri`以便应用程序可以正确处理它们︰

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>授权的终结点错误的错误代码

下表描述了各种可以在返回的错误代码`error`参数的错误响应。

| 错误代码 | 说明 | 客户端操作 |
|------------|-------------|---------------|
| invalid_request | 协议错误，如缺少必需的参数。 | 修复并重新提交请求。 这是一个开发初始测试时，通常发现错误。|
| unauthorized_client | 不允许客户端应用程序要请求授权码。 | 这通常发生在客户端应用程序在 Azure AD 中没有注册或不会将其添加到用户的 Azure AD 租户。 应用程序可以通过提示用户安装应用程序并将其添加到 Azure AD 的指令。 |
| access_denied | 资源所有者拒绝同意的情况下 | 客户端应用程序可能会通知用户，除非用户同意，否则，它就无法继续进行。 |
| unsupported_response_type | 授权服务器不支持请求的响应类型。 | 修复并重新提交请求。 这是一个开发初始测试时，通常发现错误。|
|server_error | 服务器遇到一个意外的错误。 | 重试请求。 这些错误可能会导致临时的条件。 客户端应用程序可以向用户解释由于临时错误，延迟了其响应。 |
| temporarily_unavailable | 临时服务器不太忙，无法处理请求。 | 重试请求。 客户端应用程序可以向用户解释由于临时情况，延迟了其响应。 |
| invalid_resource |目标资源无效，因为它不存在，Azure 广告找不到它，或者配置不正确。| 这表明该资源，是否它存在，尚未配置在租户。 应用程序可以通过提示用户安装应用程序并将其添加到 Azure AD 的指令。 |

## <a name="request-an-access-token"></a>请求访问令牌
既然已经获得授权，并已被授予用户的权限，可以兑换`code`的`access_token`对所需资源，通过发送`POST`请求的`/token`终结点︰

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] 尝试执行此请求把邮递员弄 ！ (别忘了替换`code`)    [![把邮递员弄在运行](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------------- |
| 租户 | 必填 | `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为`common`， `organizations`， `consumers`，和租户标识符。  有关详细信息，请参阅[协议基础](active-directory-v2-protocols.md#endpoints)。 |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| grant_type | 必填 | 必须为`authorization_code`的授权代码流。 |
| 作用域 | 必填 | 以空格分隔的作用域的列表。  在此段中请求的范围必须等同于或在第一个腿中请求的范围的子集。  如果此请求中指定的范围跨越多个资源服务器，2.0 版终结点将返回的第一个作用域中指定的资源的标记。  有关范围的详细说明，请参阅[权限、 同意的情况下和范围](active-directory-v2-scopes.md)。  |
| 代码 | 必填 | 中流的第一个腿获得授权。   |
| redirect_uri | 必填 | 相同的 redirect_uri 值，用于获得授权。 |
| client_secret | 所需的 web 应用程序 | 为您的应用程序的应用程序注册门户中创建应用程序密码。  它不应使用在本机应用程序中，因为 client_secrets 不能可靠地存储在设备上。  它是 web 应用程序和 web Api，可以将 client_secret 安全地存储在服务器端所必需的。 |

#### <a name="successful-response"></a>成功的响应
成功的令牌响应类似于︰

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| access_token | 请求的访问令牌中。 应用程序可以使用此标记通过身份验证的安全资源，例如 web API。 |
| token_type | 表示标记类型值。 Azure 的广告支持的唯一类型是持有者  |
| expires_in | 多长时间访问令牌无效 （以秒为单位）。 |
| 作用域 | Access_token 无效的范围。 |
| refresh_token |  OAuth 2.0 刷新令牌。 应用程序可以使用此标记当前存取令牌到期后获得更多的访问令牌。  Refresh_tokens 将长期存在，并可用于较长时间保留对资源的访问。  有关详细信息，请参阅[v2.0 令牌引用](active-directory-v2-tokens.md)。  |
| id_token | 无符号 JSON Web 标记 (JWT)。 应用程序可以 base64Url 解码部分的请求有关的信息的用户的登录此标记。 应用程序可以缓存值，并显示它们，但它不应依赖它们用于任何授权或安全边界。  有关 id_tokens 的详细信息请参阅[v2.0 端点令牌引用](active-directory-v2-tokens.md)。 |

#### <a name="error-response"></a>错误响应
错误响应类似于︰

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

#### <a name="error-codes-for-token-endpoint-errors"></a>令牌的终结点错误的错误代码

| 错误代码 | 说明 | 客户端操作 |
|------------|-------------|---------------|
| invalid_request | 协议错误，如缺少必需的参数。 | 修复并重新提交请求 |
| invalid_grant | 授权码无效或已过期。 | 尝试到新请求`/authorize`终结点 |
| unauthorized_client | 经过身份验证的客户端未授权使用此授权授予类型。 | 这通常发生在客户端应用程序在 Azure AD 中没有注册或不会将其添加到用户的 Azure AD 租户。 应用程序可以通过提示用户安装应用程序并将其添加到 Azure AD 的指令。 |
| invalid_client | 客户端身份验证失败。 | 客户端凭据无效。 要解决问题，应用程序管理员更新凭据。 |
| unsupported_grant_type | 授权服务器不支持授权授权类型。 | 更改请求中的授权类型。 这种类型的错误应该只在开发过程中发生，在初始测试过程中检测到。 |
| invalid_resource | 目标资源无效，因为它不存在，Azure 广告找不到它，或者配置不正确。 | 这表明该资源，是否它存在，尚未配置在租户。 应用程序可以通过提示用户安装应用程序并将其添加到 Azure AD 的指令。 |
| interaction_required | 该请求需要用户交互。 例如，则需要额外的身份验证步骤。 | 重试请求具有相同的资源。 |
| temporarily_unavailable | 临时服务器不太忙，无法处理请求。 | 重试请求。 客户端应用程序可以向用户解释由于临时情况，延迟了其响应。|

## <a name="use-the-access-token"></a>使用访问令牌
现在，您已经成功获得`access_token`，可以通过包括在 Web api 请求中使用标记`Authorization`标头︰

> [AZURE.TIP] 执行此请求把邮递员弄 ！ (替换`Authorization`头第一)    [![把邮递员弄在运行](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>刷新的存取令牌
Access_tokens 很短的惊叹，以及过期继续访问资源后，必须刷新它们。  可以做到通过提交另一个`POST`请求的`/token`终结点，这次提供`refresh_token`而不是`code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] 尝试执行此请求把邮递员弄 ！ (别忘了替换`refresh_token`)    [![把邮递员弄在运行](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | -------- |
| 租户 | 必填 | `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为`common`， `organizations`， `consumers`，和租户标识符。  有关详细信息，请参阅[协议基础](active-directory-v2-protocols.md#endpoints)。 |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| grant_type | 必填 | 必须是`refresh_token`对此链路的授权代码流。 |
| 作用域 | 必填 | 以空格分隔的作用域的列表。  在此段中请求的范围必须等同于或请求的原始授权请求腿的作用域的子集。  如果此请求中指定的范围跨越多个资源服务器，2.0 版终结点将返回的第一个作用域中指定的资源的标记。  有关范围的详细说明，请参阅[权限、 同意的情况下和范围](active-directory-v2-scopes.md)。  |
| refresh_token | 必填 | 流程的第二个腿中获得 refresh_token。   |
| redirect_uri | 必填 | 相同的 redirect_uri 值，用于获得授权。 |
| client_secret | 所需的 web 应用程序 | 为您的应用程序的应用程序注册门户中创建应用程序密码。  它不应使用在本机应用程序中，因为 client_secrets 不能可靠地存储在设备上。  它是 web 应用程序和 web Api，可以将 client_secret 安全地存储在服务器端所必需的。 |

#### <a name="successful-response"></a>成功的响应
成功的令牌响应类似于︰

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| access_token | 请求的访问令牌中。 应用程序可以使用此标记通过身份验证的安全资源，例如 web API。 |
| token_type | 表示标记类型值。 Azure 的广告支持的唯一类型是持有者  |
| expires_in | 多长时间访问令牌无效 （以秒为单位）。 |
| 作用域 | Access_token 无效的范围。 |
| refresh_token |  新的 OAuth 2.0 刷新令牌。 使用此新收购的刷新令牌以确保刷新标记保持尽可能长时间有效，则应当替换旧的刷新标记。  |
| id_token | 无符号 JSON Web 标记 (JWT)。 应用程序可以 base64Url 解码部分的请求有关的信息的用户的登录此标记。 应用程序可以缓存值，并显示它们，但它不应依赖它们用于任何授权或安全边界。  有关 id_tokens 的详细信息请参阅[v2.0 端点令牌引用](active-directory-v2-tokens.md)。 |

#### <a name="error-response"></a>错误响应
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

有关错误代码的建议客户端操作的说明，请参阅[标记终结点错误的错误代码](#error-codes-for-token-endpoint-errors)。
