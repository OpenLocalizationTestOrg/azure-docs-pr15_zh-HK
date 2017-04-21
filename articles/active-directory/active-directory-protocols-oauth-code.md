<properties
    pageTitle="Azure AD.NET 协议概述 |Microsoft Azure"
    description="本文介绍了如何使用 HTTP 消息进行授权访问 web 应用程序和 web Api 在您使用 Azure Active Directory 和 OAuth 2.0 的租户。"
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


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>对 web 应用程序使用 OAuth 2.0 和 Azure Active Directory 授权访问

Azure 活动目录 (AD Azure) 使用 OAuth 2.0，使您能够将 web 应用程序和 web Api 在 Azure AD 租户访问权限授予。 本指南是独立的语言，并介绍了如何发送和接收 HTTP 消息而无需使用任何我们的开源库。

[OAuth 2.0 规范的 4.1 节](https://tools.ietf.org/html/rfc6749#section-4.1)描述了 OAuth 2.0 授权代码流。 它用于在大多数的应用程序类型，包括 web 应用程序和本地安装的应用程序执行身份验证和授权。

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 授权流程

从较高层面，整个授权流程的应用程序看上去有点像这样︰

![OAuth 授权码流](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>请求授权码

以客户端定向到用户的授权代码流开始`/authorize`终结点。 在此请求中，客户端指示需要从用户获得的权限。 您的应用程序中的页在 Azure 经典门户中，底部抽屉中的**查看终结点**按钮可以 OAuth 2.0 终结点。

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 必填 | `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为租户标识符，例如`8eaef023-2b34-4da1-9baa-8bc8c9d6a490`或`contoso.onmicrosoft.com`或`common`为独立于租户的标记 |
| client_id | 必填 | 应用程序 Id 分配给您的应用程序，当您进行注册 Azure 的广告。 您可以发现这在 Azure 管理门户。 单击在**Active Directory**目录应用程序上单击和，然后单击**配置** |
| response_type | 必填 | 必须包括`code`授权代码流。 |
| redirect_uri | 建议 | 您的应用程序，可以发送和接收应用程序的身份验证响应 redirect_uri。  它必须完全匹配您在中注册门户，不同之处在于它必须是 url 编码的 redirect_uris 之一。  对于本机和移动应用程序，应使用默认值的`urn:ietf:wg:oauth:2.0:oob`。 |
| response_mode | 建议 | 指定用于将生成的令牌后发送给您的应用程序的方法。  可以是`query`或`form_post`。  |
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。 随机生成唯一值通常用于[防止跨站点请求伪造攻击](http://tools.ietf.org/html/rfc6749#section-10.12)。  状态还用于对应用程序中用户的状态信息进行编码之前发生身份验证请求，例如在页面或视图上。 |
| 资源 | 可选 | 应用程序 ID URI 的 web API （受保护的资源）。 若要查找 web API，应用程序 ID URI Azure 管理门户中，单击**Active Directory**目录单击应用程序和，然后单击**配置**。 |
| 提示 | 可选 |  指示用户交互所需的类型。<p> 有效值包括︰ <p> *登录*︰ 应提示用户重新进行身份验证。 <p> *同意*︰ 用户同意的情况下已被授予，但需要更新。 应提示用户以同意。 <p> *admin_consent*︰ 应提示管理员同意代表他们的组织中的所有用户 |
| login_hint | 可选 | 可以用于预填充用户名/电子邮件地址字段的登录页面的用户，如果您知道他们的用户名提前。  通常应用程序将使用此参数，重新进行身份验证期间，已有从以前登录使用中提取用户名`preferred_username`说。 |
| domain_hint | 可选 | 提供有关租户或域的用户应使用登录提示。 Domain_hint 的值是为租户已注册的域。 如果租户联盟到内部部署目录，AAD 重定向到指定的租户联合身份验证服务器。 |

> [AZURE.NOTE] 如果用户是组织的一部分，组织管理员可以同意或拒绝用户的代表，或允许用户同意。 用户提供的选项只有在管理员允许时，才同意。

在这种情况下，将要求用户输入其凭据，并同意在指示的权限`scope`查询参数。 一旦用户进行身份验证并授予同意时，Azure 广告将响应发送给您的应用程序在`redirect_uri`您的请求中的地址。

### <a name="successful-response"></a>成功的响应

成功的响应可能如下所示︰

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| 参数 | 说明 |
| -----------------------| --------------- |
| admin_consent | 如果管理员同意同意请求提示，则值为 True。|
| 代码 | 应用程序请求授权码。 应用程序可以使用授权代码请求的目标资源的访问令牌。 |
| session_state | 标识当前用户会话的唯一值。 此值是一个 GUID，但应被视为不透明值，而无需检查传递。 |
| 状态 | 如果请求中包含一个状态参数，相同的值应显示在响应中。 它是很好的做法要验证之前使用响应状态请求和响应中的值相同的应用程序。 这有助于防止客户端检测[跨站点请求伪造 (: CSRF) 攻击](https://tools.ietf.org/html/rfc6749#section-10.12)。  

### <a name="error-response"></a>错误响应

此外可能被错误响应发送到`redirect_uri`，以便应用程序可以正确处理它们。

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| 参数 | 说明 |
|-----------|-------------|
| 错误 | 第 5.2 节的[OAuth 2.0 授权框架](http://tools.ietf.org/html/rfc6749)中定义的错误代码值。 下表描述了 Azure AD 返回的错误代码。 |
| error_description | 错误的更详细的说明。 此消息不是为最终用户友好。 |
| 状态 | 州值是随机生成非重复使用值是在请求中发送，以防止跨站点请求伪造 (: CSRF) 攻击的响应中返回。 |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>授权码用于请求访问令牌

既然已经获得授权码，且已被授予用户的权限，您可以通过发送 POST 请求到兑换对所需资源的访问标记的代码`/token`终结点︰

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------------- |
| 租户 | 必填 |  `{tenant}`请求的路径中的值可用于控制哪些用户可以登录到该应用程序。  允许的值为租户标识符，例如`8eaef023-2b34-4da1-9baa-8bc8c9d6a490`或`contoso.onmicrosoft.com`或`common`为独立于租户的标记 |
| client_id | 必填 | 应用程序 Id 分配给您的应用程序，当您进行注册 Azure 的广告。 您可以发现这在 Azure 经典门户。 单击在**Active Directory**目录应用程序上单击和，然后单击**配置** |
| grant_type | 必填 | 必须为`authorization_code`的授权代码流。 |
| 代码 | 必填 | `authorization_code`在上一节中获得   |
| redirect_uri | 必填 | 相同`redirect_uri`值，用来获取`authorization_code`。 |
| client_secret | 所需的 web 应用程序 | 为您的应用程序的应用程序注册门户中创建应用程序密码。  它不应使用在本机应用程序中，因为 client_secrets 不能可靠地存储在设备上。  它是必需的 web 应用程序和 web Api，可以存储`client_secret`安全地在服务器端。 |
| 资源 | 如果授权代码请求，其他可选中指定，必需的 | 应用程序 ID URI 的 web API （受保护的资源）。
若要在 Azure 管理门户中查找应用程序 ID URI， **Active Directory**单击目录，单击应用程序中，和，然后单击**配置**。

### <a name="successful-response"></a>成功的响应

Azure AD 返回到成功的响应时的访问令牌。 为了尽可能减少从客户端应用程序和它们相关联的延迟的网络调用，客户端应用程序应 OAuth 2.0 响应中指定的令牌生存期缓存的访问令牌。 要确定令牌的生存期，可以使用两种`expires_in`或`expires_on`参数的值。

如果 web API 资源返回`invalid_token`错误代码，这可能表明该资源已确定标记为已过期。 如果客户端和资源时钟时间不同 （也叫"时间倾斜"），该资源可以考虑令牌过期之前从客户端缓存中清除该标记。 如果发生这种情况，清除从高速缓存中，该标记，即使是仍在计算生存期。

成功的响应可能如下所示︰

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| access_token | 请求的访问令牌中。 应用程序可以使用此标记通过身份验证的安全资源，例如 web API。 |
| token_type | 表示标记类型值。 Azure 的广告支持的唯一类型为载体。 持有者标记有关的详细信息，请参阅[OAuth2.0 授权框架︰ 持有者标记用法 (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | 多长时间访问令牌无效 （以秒为单位）。 |
| expires_on | 当访问令牌过期时间。 该日期表示为秒数从 1970年-01-01T0:0:0Z UTC 直到过期时间。 此值用于确定缓存的令牌的生存期。 |
| 资源 | 应用程序 ID URI 的 web API （受保护的资源）。|
| 作用域 | 模拟客户端应用程序授予的权限。 默认的权限是`user_impersonation`。 受保护的资源的所有者可以在 Azure AD 中注册附加值。|
| refresh_token |  OAuth 2.0 刷新令牌。 应用程序可以使用此标记当前存取令牌到期后获得更多的访问令牌。  刷新令牌寿命较长，并可较长时间保留对资源的访问。 |
| id_token | 无符号 JSON Web 标记 (JWT)。 应用程序可以 base64Url 解码部分的请求有关的信息的用户的登录此标记。 应用程序可以缓存值，并显示它们，但它不应依赖它们用于任何授权或安全边界。 |

### <a name="jwt-token-claims"></a>JWT 标记声明
JWT 令牌的值在`id_token`参数可以解码为以下声明︰

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

`id_token`参数包括以下声明类型。 有关 JSON web 标记的详细信息，请参阅[JWT IETF 草案规范](http://go.microsoft.com/fwlink/?LinkId=392344)。 标记类型和索赔有关的详细信息，请阅读[支持令牌和报销申请类型](active-directory-token-and-claims.md)

| 声明类型 | 说明 |
|------------|-------------|
| aud | 令牌的观众。 令牌颁发给客户端应用程序，当观众为`client_id`的客户端。
| exp | 到期时间。 当令牌过期时间。 有效的标记，为当前的日期/时间必须小于或等于`exp`值。 从 1970 年 1 月 1 日的秒数表示的时间 (1970年-01-01T0:0:0Z) 直到颁发令牌的时间的 UTC。 |
| family_name | 用户的姓氏或姓。 应用程序可以显示此值。 |
| given_name | 用户的名字。 应用程序可以显示此值。 |
| iat | 发出次。 JWT 发出时的时间。 从 1970 年 1 月 1 日的秒数表示的时间 (1970年-01-01T0:0:0Z) 直到颁发令牌的时间的 UTC。 |
| iss | 令牌颁发者标识 |
| nbf | 不早时间。 该令牌生效时间。 有效的标记，为当前的日期/时间必须大于或等于 Nbf 值。 从 1970 年 1 月 1 日的秒数表示的时间 (1970年-01-01T0:0:0Z) 直到颁发令牌的时间的 UTC。 |
| oid | 在 Azure 广告中的用户对象的对象标识符 (ID)。 |
| 子 | 标记的主题标识符。 这是标记描述的用户的永久和不可变的标识符。 使用此值在缓存逻辑。 |
| tid | 租户 Azure 广告组织颁发令牌的标识符 (ID)。 |
| unique_name | 可以向用户显示程序的唯一标识符。 这通常是用户主体名称 (UPN)。 |
| upn | 用户的用户主体名称。 |
| 版 | 版本。 JWT 标记，通常是 1.0 的版本。 |

### <a name="error-response"></a>错误响应

令牌颁发终结点错误是 HTTP 错误代码，因为在客户机直接调用令牌发放终结点。 除了 HTTP 状态代码，Azure AD 令牌发放终结点还会返回描述该错误的对象的 JSON 文档。

示例错误响应可能如下所示︰

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。  |
| error_codes | 可以帮助诊断过程中的 STS 特定错误代码的列表。 |
| 时间戳 | 该错误发生的时间。 |
| trace_id | 有助于诊断过程中请求的的唯一标识符。  |
| correlation_id | 有助于诊断程序中各组件请求的的唯一标识符。|

#### <a name="http-status-codes"></a>HTTP 状态代码

下表列出了令牌发放终结点返回的 HTTP 状态代码。 在某些情况下，错误代码是不足以描述响应，但万一遇到错误，您将需要分析伴随的 JSON 文档并检查其错误代码。

| HTTP 代码 | 说明 |
|-----------|-------------|
| 400       | 默认 HTTP 代码。 在大多数情况下使用，并且通常是格式错误的请求。 修复并重新提交请求。 |
| 401       | 身份验证失败。 例如，请求缺少 client_secret 参数。|
| 403       | 授权失败。 例如，用户没有访问该资源的权限。 |
| 500       | 在服务中发生内部错误。 重试请求。 |

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

## <a name="use-the-access-token-to-access-the-resource"></a>使用访问令牌来访问资源

现在，您已经成功获得`access_token`，可以通过包括在 Web api 请求中使用标记`Authorization`头。 [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt)规范说明了如何使用 HTTP 请求中的持有者令牌来访问受保护的资源。

### <a name="sample-request"></a>示例请求

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>错误响应

实现 RFC 6750 问题 HTTP 状态代码的受保护的资源。 如果请求不包含身份验证凭据，或缺少该标记，该响应包括`WWW-Authenticate`头。 当请求失败时，资源服务器进行响应的 HTTP 状态代码和错误代码。

下面是客户端请求不包括持有者标记成功响应的示例︰

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>错误的参数

| 参数 | 说明 |
|-----------|-------------|
| authorization_uri | 授权服务器 URI （物理端点）。 此值还用作查找键，从发现端点获取有关服务器的详细信息。 <p><p> 客户端必须是受信任的授权服务器进行验证。 Azure AD 的受保护资源，足以验证 URL 开始用 https://login.windows.net 或 Azure 的广告支持的另一个主机名。 特定于租户的资源应始终返回特定于租户的授权的 URI。 |
| 错误 | 第 5.2 节的[OAuth 2.0 授权框架](http://tools.ietf.org/html/rfc6749)中定义的错误代码值。|
| error_description | 错误的更详细的说明。 此消息不是为最终用户友好。|
| resource_id | 返回资源的唯一标识符。 客户端应用程序可以使用此标识符的值与`resource`请求的资源的标记时的参数。 <p><p> 对于客户端应用程序来验证此值非常重要，否则，恶意服务可能能够促使以**提升的权限**的攻击 <p><p> 防止攻击的推荐的策略是以验证`resource_id`正在访问相匹配的 web API 的 URL 基。 例如，如果正在访问 https://service.contoso.com/data，`resource_id`可以是 htttps://service.contoso.com/。 客户端应用程序必须拒绝`resource_id`，没有开头的基 URL 除非有可靠的替代方法，来验证 id。 |

#### <a name="bearer-scheme-error-codes"></a>持有者方案错误代码

RFC 6750 规范定义了使用响应中使用 WWW 身份验证标头和持有者方案的资源的下列错误。

| HTTP 状态代码 | 错误代码 | 说明 | 客户端操作 |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | 该请求不是格式良好的。 例如，它可能缺少一个参数或两次使用相同的参数。 | 修复该错误，然后重试该请求。 这种类型的错误应该只在开发过程中发生，在初始测试中被检测到。 |
| 401 | invalid_token   | 访问令牌是缺失、 无效或已被吊销。 Error_description 参数的值提供了更多详细信息。 |  从授权服务器申请新的令牌。 如果新的令牌失败，发生意外的错误。 之后随机延迟到用户，然后重试发送一条错误消息。 |
| 403 | insufficient_scope | 访问令牌不包含访问该资源所需的模拟权限。 | 将新的授权请求发送到授权的终结点。 如果响应包含 scope 参数，对资源请求中使用的作用域值。 |
| 403 | insufficient_access | 该标记的使用者没有访问该资源所需的权限。 | 提示用户使用不同的帐户或请求到指定的资源的权限。 |

## <a name="refreshing-the-access-tokens"></a>刷新的访问令牌

访问令牌的生存期较短和过期继续访问资源后，必须刷新。 您可以刷新`access_token`通过提交另一`POST`请求的`/token`终结点，但该时间提供`refresh_token`而不是`code`。

刷新令牌是否有指定的生存期。 通常情况下，刷新令牌的生存期不是相对较长。 但是，在某些情况下，刷新令牌过期、 吊销证书，或没有足够的权限执行所需操作。 您的应用程序需要希望和处理正确的令牌发放终结点返回错误。

当您收到有刷新令牌错误的响应时，放弃当前的刷新令牌并请求新的授权码或存取令牌。 特别是，当使用刷新令牌中的授权代码授予流动，如果您收到的响应与`interaction_required`或`invalid_grant`错误代码，放弃刷新令牌并请求新的授权码。

样本与**特定于租户的**终结点 （您可以使用**常见**的端点） 请求获取一个新的访问令牌使用刷新标记如下所示︰

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| 参数 | 说明 |
|-----------|-------------|
| access_token | 请求新的访问令牌。|
| expires_in   | 以秒为单位的令牌的剩余寿命。 典型值为 3600 （1 小时）。 |
| expires_on   | 过期日期和时间的标记。 该日期表示为秒数从 1970年-01-01T0:0:0Z UTC 直到过期时间。 |
| refresh_token | 可以用于请求新的访问令牌过期此响应中的一个时新 OAuth 2.0 refresh_token。 |
| 资源     | 标识受保护的资源的访问令牌可以用来访问。 |
| 作用域        | 模拟的本机客户端应用程序授予的权限。 默认的权限是**user_impersonation**。 目标资源的所有者，可以注册 Azure AD 中的替代值。 |
| token_type   | 标记的类型。 唯一受支持的值是**载体**。 |

### <a name="successful-response"></a>成功的响应

成功的令牌响应类似于︰

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>错误响应

示例错误响应可能如下所示︰

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。  |
| error_codes | 可以帮助诊断过程中的 STS 特定错误代码的列表。 |
| 时间戳 | 该错误发生的时间。 |
| trace_id | 有助于诊断过程中请求的的唯一标识符。  |
| correlation_id | 有助于诊断程序中各组件请求的的唯一标识符。|

有关错误代码的建议客户端操作的说明，请参阅[标记终结点错误的错误代码](#error-codes-for-token-endpoint-errors)。
