<properties
    pageTitle="Azure AD v2.0 隐式流 |Microsoft Azure"
    description="构建 web 应用程序用于单个页面的应用程序使用隐式流 Azure AD v2.0 实现。"
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 协议-SPAs 使用隐式流
使用 2.0 版的终结点，您可以到单页应用程序使用来自 Microsoft 的个人和工作/学校帐户登录用户。  单个页面和其他 JavaScript 应用程序主要在中运行浏览器表面几个有趣的挑战谈到身份验证︰

- 这些应用程序的安全特征都明显不同于传统的基于服务器的 web 应用程序。
- 许多授权服务器和标识提供程序不支持 CORS 请求。
- 整页浏览器重定向从应用程序变得尤其影响用户体验。

这些应用程序 (认为︰ AngularJS、 Ember.js、 React.js，等等) Azure 的广告支持的 OAuth 2.0 隐式授权流程。  [OAuth 2.0 规范](http://tools.ietf.org/html/rfc6749#section-4.2)描述了隐式流。  其主要优点是它允许应用程序获取令牌从 Azure 的广告而不执行后端服务器凭据交换。  这允许应用程序在用户登录、 维护会话，并获取标记为在客户端的所有其他 web Api 的 JavaScript 代码。  有使用隐式流-特别是在[客户端](http://tools.ietf.org/html/rfc6749#section-10.3)和[用户模拟](http://tools.ietf.org/html/rfc6749#section-10.3)时，需要考虑的几个重要的安全注意事项。

如果您想要使用的隐式流和 Azure 广告于 JavaScript 应用程序添加身份验证，我们建议使用我们开源 JavaScript 库， [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js)。  有几个 AngularJS 教程[这里](active-directory-appmodel-v2-overview.md#getting-started)来帮助您入门。  

但是，如果您不愿意使用单页应用程序中的库和自己发送的协议消息，请按照下面的常规步骤。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。
    
## <a name="protocol-diagram"></a>协议关系图
整个流程中的隐式符号如下所示 — 下面将详细介绍每个步骤。

![OpenId 连接泳道](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>发送登录请求

最初登录到您的应用程序的用户，您可以发送一个[OpenID 连接](active-directory-v2-protocols-oidc.md)授权请求，并得到`id_token`从 v2.0 终结点︰

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] 单击下面的链接以执行此请求 ！ 登录后，您的浏览器应被重定向到`https://localhost/myapp/`与`id_token`的地址栏中。
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 必填 | `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为`common`， `organizations`， `consumers`，和租户标识符。  有关详细信息，请参阅[协议基础](active-directory-v2-protocols.md#endpoints)。 |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| response_type | 必填 | 必须包括`id_token`的 OpenID 连接登录。  它还可能包括 response_type `token`。 使用`token`在此处将允许您的应用程序的授权终结点从立即收到一个访问令牌，而无需进行第二个请求到授权的终结点。  如果您使用`token`response_type，`scope`参数必须包含作用域指示哪些资源发出的标记。 |
| redirect_uri | 建议 | 您的应用程序，可以发送和接收应用程序的身份验证响应 redirect_uri。  它必须完全匹配您在中注册门户，不同之处在于它必须是 url 编码的 redirect_uris 之一。 |
| 作用域 | 必填 | 以空格分隔的作用域的列表。  OpenID 连接时，它必须包含范围`openid`，这进而又转化为同意用户界面中的"登录"权限。  （可选） 您可能还想要包括`email`或`profile`的访问其他用户的数据的[范围](active-directory-v2-scopes.md)。  在此请求同意的情况下对各种资源的请求，可能还包括其他范围。  |
| response_mode | 建议 | 指定用于将生成的令牌后发送给您的应用程序的方法。  应该是`fragment`隐式流。  |
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  随机生成唯一值通常用于[防止跨站点请求伪造攻击](http://tools.ietf.org/html/rfc6749#section-10.12)。  状态还用于对应用程序中用户的状态信息进行编码之前发生身份验证请求，例如在页面或视图上。 |
| 现时 | 必填 | 生成应用程序，将包含在作为索赔得到的 id_token 的申请中包含一个值。  应用程序然后可以验证此值可减少令牌重放攻击。  值通常是随机且唯一的字符串，用于标识请求的来源。  |
| 提示 | 可选 | 指示用户交互所需的类型。  这次只有效值为登录，无和同意。  `prompt=login`将强制用户输入其凭据在该请求时，绝对值单点登录。  `prompt=none`相反的它将确保不向用户显示任何任何交互式提示符。  如果不能通过单点登录自动完成请求，2.0 版终结点将返回错误。  `prompt=consent`登录用户，要求用户向应用程序授予权限之后，将触发 OAuth 同意对话框。 |
| login_hint | 可选 | 可以用于预填充用户名/电子邮件地址字段的登录页面的用户，如果您知道他们的用户名提前。  通常应用程序将使用此参数，重新进行身份验证期间，已有从以前登录使用中提取用户名`preferred_username`说。 |
| domain_hint | 可选 | 可以是一种`consumers`或`organizations`。  如果包含，则将跳过基于电子邮件的搜索过程该用户经历在 2.0 版登录页面，从而导致更为流畅的用户体验。  通常应用程序将使用此参数时重新进行身份验证，通过提取`tid`id_token 从声明。  如果`tid`声称值是`9188040d-6c67-4c5b-b112-36a304b66dad`，您应该使用`domain_hint=consumers`。  否则，请使用`domain_hint=organizations`。 |

在这种情况下，将要求用户输入其凭据并完成身份验证。  V2.0 终结点还将确保用户已同意中指示的权限`scope`查询参数。  如果用户不具有这些权限的任何许可的它将要求用户同意所需的权限。  [此处提供的权限，同意的情况下和多租户应用程序](active-directory-v2-scopes.md)的详细信息。

一旦用户进行身份验证并授予同意时，2.0 版终结点将返回以指示您的应用程序的响应`redirect_uri`，使用指定的方法在`response_mode`参数。

#### <a name="successful-response"></a>成功的响应

成功的响应使用`response_mode=fragment`和`response_type=id_token+token`外观如下，与可读性的换行所示︰

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| access_token | 包括的 if`response_type`包括`token`。 应用程序请求，在这种情况下 Microsoft Graph 的访问令牌。  访问令牌不能解码或否则检查，可以被视为一个不透明的字符串。 |
| token_type | 包括的 if`response_type`包括`token`。  将始终为`Bearer`。 |
| expires_in | 包括的 if`response_type`包括`token`。  指示该标记是有效的对于缓存目的的秒数。 |
| 作用域 | 包括的 if`response_type`包括`token`。  表明 access_token 将对其有效范围。 |
| id_token | 应用程序请求的 id_token。 您可以使用 id_token 来验证用户的身份，并开始与用户的会话。  Id_tokens 和其内容的更多详细信息包含在[v2.0 端点令牌引用](active-directory-v2-tokens.md)。  |
| 状态 | 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |


#### <a name="error-response"></a>错误响应
此外可能被错误响应发送到`redirect_uri`以便应用程序可以正确处理它们︰

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。  |

## <a name="validate-the-idtoken"></a>验证 id_token
只接收 id_token 不足以进行身份验证的用户;必须验证 id_token 的签名，并验证根据您的应用程序要求的令牌中的声明。  V2.0 终结点使用[JSON Web 标记 (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)和公钥加密签名令牌并验证它们有效。

您可以选择验证`id_token`在客户端代码，但通常的做法是发送`id_token`向后端服务器并执行验证存在。  一旦您已验证 id_token 的签名，有几个索赔需要验证。  请参阅有关详细信息，包括[验证令牌](active-directory-v2-tokens.md#validating-tokens)和[重要信息关于签名密钥翻转](active-directory-v2-tokens.md#validating-tokens) [v2.0 令牌引用](active-directory-v2-tokens.md)。  我们建议利用库的分析和验证令牌-没有至少一个可用的大多数语言和平台。
<!--TODO: Improve the information on this-->

您可能还希望验证其他索赔，具体取决于您的方案。  一些常见的验证包括︰

- 确保用户组织已注册应用程序。
- 确保用户具有适当的授权权限
- 出现确保身份验证的某些优点，如多因素身份验证。

在 id_token 中索赔的详细信息，请参阅[v2.0 端点令牌引用](active-directory-v2-tokens.md)。

一旦完全具有验证 id_token，可以开始与用户的会话，并在 id_token 中使用这些声明来获取有关您的应用程序中的用户信息。  此信息可用于显示、 记录、 授权等。

## <a name="get-access-tokens"></a>获取访问令牌

现在，用户已经登录您单个页面的应用程序，可以获取调用 web 安全的 Azure 的广告，如[Microsoft Graph](https://graph.microsoft.io)Api 访问令牌。  即使您已经收到一个令牌使用`token`response_type，您可以使用此方法来获取而无需再次登录的用户重定向到其他资源的标记。

在正常的 OpenID 连接/OAuth 流程，这样做通过请求到 2.0 版`/token`终结点。  但是，2.0 版终结点不支持 CORS 请求，以便进行 AJAX 调用来获取和刷新标记不足问题。  相反，可以使用隐式流中隐藏的 iframe 可获得其他 web Api 的新标记︰ 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] 请尝试复制并粘贴到浏览器选项卡上的请求下 ！ (别忘了替换`domain_hint`，`login_hint`与您的用户的正确值的值)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 必填 | `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为`common`， `organizations`， `consumers`，和租户标识符。  有关详细信息，请参阅[协议基础](active-directory-v2-protocols.md#endpoints)。 |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| response_type | 必填 | 必须包括`id_token`的 OpenID 连接登录。  它还可能包括其他 response_types，如`code`。 |
| redirect_uri | 建议 | 您的应用程序，可以发送和接收应用程序的身份验证响应 redirect_uri。  它必须完全匹配您在中注册门户，不同之处在于它必须是 url 编码的 redirect_uris 之一。 |
| 作用域 | 必填 | 以空格分隔的作用域的列表。  获取标记包含您感兴趣的资源需要的所有[作用域](active-directory-v2-scopes.md)。  |
| response_mode | 建议 | 指定用于将生成的令牌后发送给您的应用程序的方法。  可以是一种`query`， `form_post`，或`fragment`。  |
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  随机生成唯一值通常用于防止跨站点请求伪造攻击。  状态还用于对应用程序中用户的状态信息进行编码之前发生身份验证请求，例如在页面或视图上。 |
| 现时 | 必填 | 生成应用程序，将包含在作为索赔得到的 id_token 的申请中包含一个值。  应用程序然后可以验证此值可减少令牌重放攻击。  值通常是随机且唯一的字符串，用于标识请求的来源。  |
| 提示 | 必填 | 刷新和隐藏的 iframe 中获取令牌，您应该使用`prompt=none`以确保 iframe 未挂起在 2.0 版登录页面，并立即返回。 |
| login_hint | 必填 | 刷新和隐藏的 iframe 中获取令牌，必须在此提示包括用户的用户名，以区分多个会话，则用户可能必须在指定的时刻及时。 您可以从以前登录使用中提取用户名`preferred_username`说。 |
| domain_hint | 必填 | 可以是一种`consumers`或`organizations`。  刷新和隐藏的 iframe 中获取令牌，必须在请求中包含 domain_hint。  应提取`tid`声称从先前登录的来确定要使用哪个值 id_token。  如果`tid`声称值是`9188040d-6c67-4c5b-b112-36a304b66dad`，您应该使用`domain_hint=consumers`。  否则，请使用`domain_hint=organizations`。 |

为感谢`prompt=none`参数，此请求将是成功或失败立即并返回到您的应用程序。  成功的响应将发送到您的应用程序在指定`redirect_uri`，使用指定的方法在`response_mode`参数。

#### <a name="successful-response"></a>成功的响应
成功的响应使用`response_mode=fragment`如下所示︰

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| access_token | 应用程序请求令牌。 |
| token_type | 将始终为`Bearer`。 |
| 状态 | 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |
| expires_in | 多长时间访问令牌无效 （以秒为单位）。 |
| 作用域 | 访问令牌是有效的范围。 |

#### <a name="error-response"></a>错误响应
此外可能被错误响应发送到`redirect_uri`以便应用程序可以正确处理它们。  对于`prompt=none`，将预期的错误︰

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。  |

如果 iframe 请求中收到此错误，用户必须以交互方式登录检索新的令牌。  您可以选择来处理这种情况下，以任何方式对您的应用程序有意义。

## <a name="refreshing-tokens"></a>正在刷新令牌

同时`id_token`s 和`access_token`s 将过期后短时间内，因此您的应用程序必须准备刷新这些定期标记。  若要刷新两种类型的令牌，您可以执行从使用同一个隐藏的 iframe 请求`prompt=none`参数来控制 Azure 广告的行为。  如果您希望收到一个新`id_token`，请务必使用`response_type=id_token`， `scope=openid`，以及一个`nonce`参数。


## <a name="send-a-sign-out-request"></a>发送请求注销

OpenIdConnect `end_session_endpoint` 2.0 版端点当前不支持。 这意味着您的应用程序无法将请求发送到 v2.0 终结点来结束用户会话并清除 v2.0 终结点设置的 cookie。
登录用户，您的应用程序只需结束自己与用户的会话并使用 v2.0 终结点完好保留用户的会话。  下一次用户尝试登录，他们将看到"选择帐户"页，列出其主动已登录的帐户。
在该页面中，用户可以选择退出结束会话使用 2.0 版的终结点的任何帐户。

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->