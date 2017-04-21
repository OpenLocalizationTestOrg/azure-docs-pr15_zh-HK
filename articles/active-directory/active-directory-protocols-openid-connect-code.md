<properties
    pageTitle="Azure AD.NET 协议概述 |Microsoft Azure"
    description="本文介绍了如何使用 HTTP 消息来访问 web 应用程序和 web Api 使用 Azure Active Directory 和 OpenID 连接您组织中的授权。"
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>对 web 应用程序使用 OpenID 连接和 Azure Active Directory 授权访问

[OpenID 连接](http://openid.net/specs/openid-connect-core-1_0.html)是基于 OAuth 2.0 协议的简单标识图层。 OAuth 2.0 定义机制来获取并使用**访问令牌**来访问受保护的资源，但并不定义标准的方法，以提供标识信息。 OpenID 连接实现身份验证扩展 OAuth 2.0 授权过程中，提供有关在窗体中的最终用户信息`id_token`的验证用户的身份，以及提供有关用户的基本配置文件信息。

OpenID 连接是我们建议，如果您正在生成的 web 应用程序驻留在服务器上并可通过浏览器访问。

## <a name="authentication-flow-using-openid-connect"></a>使用 OpenID 连接的身份验证流程

最基本的登录流程包含以下步骤 — 下面将详细介绍其中每台。

![OpenId 连接身份验证流程](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>发送登录请求

当 web 应用程序需要对用户进行身份验证时，则必须将用户定向到`/authorize`终结点。 此请求是类似于第一个引线的[OAuth 2.0 授权代码流](active-directory-protocols-oauth-code.md)，有几个重要的差别︰

- 该请求必须包含范围`openid`在`scope`参数。
- `response_type`参数必须包含`id_token`。
- 该请求必须包含`nonce`参数。

因此，示例请求将如下所示︰

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 必填 | `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为租户标识符，例如`8eaef023-2b34-4da1-9baa-8bc8c9d6a490`或`contoso.onmicrosoft.com`或`common`为独立于租户的标记 |
| client_id | 必填 | 应用程序 Id 分配给您的应用程序，当您进行注册 Azure 的广告。 您可以发现这在 Azure 经典门户。 单击在**Active Directory**目录应用程序上单击和，然后单击**配置** |
| response_type | 必填 | 必须包括`id_token`的 OpenID 连接登录。  它还可能包括其他 response_types，如`code`。 |
| 作用域 | 必填 | 以空格分隔的作用域的列表。  OpenID 连接时，它必须包含范围`openid`，这进而又转化为同意用户界面中的"登录"权限。  您还可能会请求同意该请求中包括其他范围。 |
| 现时 | 必填 | 请求，生成的应用程序，将包含在结果中包括的值`id_token`为声明。  应用程序然后可以验证此值可减少令牌重放攻击。  值通常是随机的且唯一的字符串或可用于标识请求的来源的 GUID。  |
| redirect_uri | 建议 | 您的应用程序，可以发送和接收应用程序的身份验证响应 redirect_uri。  它必须完全匹配您在中注册门户，不同之处在于它必须是 url 编码的 redirect_uris 之一。 |
| response_mode | 建议 | 指定用于将得到授权发送回您的应用程序的方法。  受支持的值是`form_post`为*HTTP 窗体过帐*或`fragment`的*URL 片段*。  建议对于 web 应用程序使用`response_mode=form_post`以确保最安全令牌传输到您的应用程序。  
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  随机生成唯一值通常用于[防止跨站点请求伪造攻击](http://tools.ietf.org/html/rfc6749#section-10.12)。  状态还用于对应用程序中用户的状态信息进行编码之前发生身份验证请求，例如在页面或视图上。 |
| 提示 | 可选 | 指示用户交互所需的类型。  这次只有效值为登录，无和同意。  `prompt=login`将强制用户输入其凭据在该请求时，绝对值单点登录。  `prompt=none`相反的它将确保不向用户显示任何任何交互式提示符。  如果不能通过单点登录自动完成请求，则终结点将返回错误。  `prompt=consent`登录用户，要求用户向应用程序授予权限之后，将触发 OAuth 同意对话框。 |
| login_hint | 可选 | 可以用于预填充用户名/电子邮件地址字段的登录页面的用户，如果您知道他们的用户名提前。  通常应用程序将使用此参数，重新进行身份验证期间，已有从以前登录使用中提取用户名`preferred_username`说。 |


在这种情况下，将要求用户输入其凭据并完成身份验证。

### <a name="sample-response"></a>响应示例

示例响应，用户已通过身份验证后，可以如下所示︰

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| id_token | `id_token`应用程序请求。 您可以使用`id_token`来验证用户的身份，并开始与用户的会话。  |
| 状态 | 此外将令牌响应中返回的请求中包含一个值。 随机生成唯一值通常用于[防止跨站点请求伪造攻击](http://tools.ietf.org/html/rfc6749#section-10.12)。  状态还用于对应用程序中用户的状态信息进行编码之前发生身份验证请求，例如在页面或视图上。 |

### <a name="error-response"></a>错误响应
此外可能被错误响应发送到`redirect_uri`以便应用程序可以正确处理它们︰

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
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

## <a name="validate-the-idtoken"></a>验证 id_token

只接收`id_token`不足以进行身份验证的用户;您必须验证签名并确认在索赔`id_token`每个应用程序的要求。 Azure AD 终结点使用 JSON Web 标记 (JWTs) 和公钥加密签名令牌并验证它们有效。

您可以选择验证`id_token`在客户端代码，但通常的做法是发送`id_token`向后端服务器并执行验证存在。 一旦您已经验证的签名`id_token`，有几个索赔需要验证。

您可能还希望验证其他索赔，具体取决于您的方案。 一些常见的验证包括︰

- 确保用户组织已注册应用程序。
- 确保用户具有适当的授权权限
- 出现确保身份验证的某些优点，如多因素身份验证。

一旦完全验证`id_token`，可以开始与用户的会话，并使用在索赔`id_token`以获取有关您的应用程序中的用户信息。 此信息可用于显示、 记录、 授权等。标记类型和索赔有关的详细信息，请阅读[支持令牌和声明类型](active-directory-token-and-claims.md)。

## <a name="send-a-sign-out-request"></a>发送请求注销

当您想要对用户在该应用程序进行签名时，它不足以清除应用程序的 cookie 或以其他方式结束与用户的会话。  此外必须重定向到用户`end_session_endpoint`出的标志。  如果无法做到这一点，用户将能够重新进行身份验证为您的应用程序而无需输入其凭据，因为它们将具有有效单个登录会话使用 Azure 广告的终结点。

您可以只是将用户重定向到`end_session_endpoint`OpenID 连接元数据文档中列出︰

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | 建议 | 用户成功注销后应被重定向到的 URL。  如果未包括，一般邮件将显示用户。  |

## <a name="token-acquisition"></a>令牌的购置

许多 web 应用程序需要进行签名，用户不仅能访问 web 服务代表使用 OAuth 该用户。 这种情况下结合 OpenID 连接用户身份验证时同时获得`authorization_code`，可用于获取`access_tokens`使用 OAuth 授权代码流。

## <a name="get-access-tokens"></a>获取访问令牌

要获取的访问令牌，您需要修改登录请求从上面︰

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

通过在请求中包括的权限范围，并使用`response_type=code+id_token`，`authorize`端点将确保用户已同意中指示的权限`scope`查询参数，并返回您的应用程序授权代码的访问令牌交换。

### <a name="successful-response"></a>成功的响应

成功的响应使用`response_mode=form_post`如下所示︰

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| id_token | `id_token`应用程序请求。 您可以使用`id_token`来验证用户的身份，并开始与用户的会话。 |
| 代码 | 应用程序请求的授权。 应用程序可以使用授权代码请求的目标资源的访问令牌。 Authorization_codes 是非常短期，他们通常在大约 10 分钟后过期。 |
| 状态 | 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |

### <a name="error-response"></a>错误响应

此外可能被错误响应发送到`redirect_uri`以便应用程序可以正确处理它们︰

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。  |

有关可能的错误代码和其建议客户端操作的说明，请参阅[授权终结点错误的错误代码](#error-codes-for-authorization-endpoint-errors)。

一旦您已经看到了一种授权， `code` ， `id_token`，可以在登录用户和代表他们获取访问令牌。  签名中的用户，您必须验证`id_token`完全按上文所述。 若要获取访问令牌，可以按照我们的[OAuth 协议文档](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token)中描述的步骤。
