<properties
    pageTitle="Azure 的 Active Directory B2C |Microsoft Azure"
    description="通过使用 OpenID 连接的身份验证协议的 Azure Active Directory 实现构建 web 应用程序。"
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure 的活动目录 B2C: Web 登录使用 OpenID 连接

OpenID 连接是基于 OAuth 2.0 中，可以用于安全地登录到 web 应用程序的用户的身份验证协议。  通过使用 OpenID 连接的 Azure 活动目录 (AD Azure) B2C 实现，可以外包注册、 登录、 和其他身份管理体验 Azure 广告您的 web 应用程序中。 本指南将向您介绍如何做到这一点与语言无关的方式。 它将描述如何发送和接收 HTTP 消息而无需使用任何我们的开源库。

[OpenID 连接](http://openid.net/specs/openid-connect-core-1_0.html)扩展作为*身份验证*协议使用 OAuth 2.0*授权*协议。 这允许您通过使用 OAuth 执行单一登录。 它引入了这一概念的`id_token`，这是一个允许客户端验证用户的身份，并获得用户的基本配置文件信息的安全令牌。

因为它扩展 OAuth 2.0，它还使应用程序安全地获得**access_tokens**。 您可以使用 access_tokens 访问的资源进行保护的[授权服务器](active-directory-b2c-reference-protocols.md#the-basics)。 我们建议 OpenID 连接如果您正在构建的 web 应用程序驻留在服务器上并可通过浏览器访问。 如果您想要使用 Azure AD B2C 移动或桌面应用程序添加标识管理，您应该使用[OAuth 2.0](active-directory-b2c-reference-oauth-code.md) ，而不是 OpenID 连接。

Azure AD B2C 扩展标准的 OpenID 连接协议，做更多比简单身份验证和授权。 它引入了[**策略参数**](active-directory-b2c-reference-policies.md)，它使您能够使用 OpenID 连接添加到您的应用程序，用户体验，如注册，登录，和配置文件管理。 下面我们将介绍如何使用 OpenID 连接和策略来实现这些体验的每个 web 应用程序中。 我们还会向您如何获取关于访问 web Api 的 access_tokens。

下面的示例 HTTP 请求将使用我们的示例的 B2C 目录、 **fabrikamb2c.onmicrosoft.com**，以及我们的示例应用程序**https://aadb2cplayground.azurewebsites.net**和策略。 您可以自由地尝试请求自己使用这些值，或者可以替换为您自己。
了解如何[获得自己的 B2C 租户、 应用程序和策略](#use-your-own-b2c-directory)。

## <a name="send-authentication-requests"></a>发送身份验证请求
当您的 web 应用程序需要对用户进行身份验证和执行策略时，它可以将用户引导到`/authorize`终结点。 这是用户实际采取行动，根据策略的交互流的部分。

在此请求中，客户端指示中，用户获得所需的权限`scope`参数和策略在执行`p`参数。 三个示例附带以下 （换行以提高可读性），每一个使用不同的策略。 为了解现况，为每个请求的工作原理，请尝试粘贴到浏览器的请求然后运行它。

#### <a name="use-a-sign-in-policy"></a>使用签入策略

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>使用注册策略

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>使用编辑配置文件策略

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| 参数 | 必填？ | 说明 |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | 必填 | [Azure 门户](https://portal.azure.com/)分配给您的应用程序的应用程序 ID。 |
| response_type | 必填 | 响应类型，该类型必须包含`id_token`的 OpenID 连接。 如果您的 web 应用程序还需要标记用于调用 web API，您可以使用`code+id_token`，因为这里我们做。 |
| redirect_uri | 建议 | 您的应用程序，可以发送和接收应用程序的身份验证响应 redirect_uri。 它必须完全匹配您注册在门户中，redirect_uris 的一个不同之处在于，它必须是 URL 编码。 |
| 作用域 | 必填 | 以空格分隔的作用域的列表。 单个作用域值表示到 Azure AD 这两个正在请求的权限。 `openid`范围表示权限登录用户并获得有关在窗体中的**id_tokens** （多对此放在文章的后面） 的用户数据。 `offline_access`范围是对于 web 应用程序是可选的。 这表明，您的应用程序将需要**refresh_token** ，长期存在对资源的访问。 |
| response_mode | 建议 | 应使用将得到授权发送回您的应用程序的方法。 它可以是一个查询、 form_post 或片断。  为了获得最佳安全性建议 'form_post'。 |
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。 它可以是您希望的任何内容的字符串。 随机生成唯一值通常用于防止跨站点请求伪造攻击。 状态还用于编码之前发生身份验证请求，例如，页上的应用程序中用户的状态信息。 |
| 现时 | 必填 | 应为声明产生 id_token （由应用程序生成） 请求中包含一个值。 应用程序然后可以验证此值可减少令牌重放攻击。 值通常是随机且唯一的字符串，用于标识请求的来源。 |
| p | 必填 | 将执行该策略。 它是策略的在 B2C 租户中创建的名称。 策略名称值，您应使用"b2c\_1\_"。 了解有关[扩展策略框架](active-directory-b2c-reference-policies.md)中的策略。 |
| 提示 | 可选 | 用户交互所需的类型。 目前唯一有效的值为登录，强制用户输入其凭据在该请求。 单一登录才会生效。 |

在这种情况下，用户可能需要完成该策略的工作流。 这可能包括用户输入他们的用户名和密码，登录社交身份，注册了目录或其他任意数量的步骤，具体取决于如何定义策略。

Azure AD 用户完成策略后，将返回以指示您的应用程序的响应`redirect_uri`，使用中指定的方法`response_mode`参数。 响应将是完全相同的每个上述的情况下，独立执行的政策。

成功的响应使用`response_mode=fragment`将如下所示︰

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| id_token | 应用程序请求的 id_token。 您可以使用 id_token 来验证用户的身份，并开始与用户的会话。 Id_tokens 和其内容的更多详细信息都包含在[Azure AD B2C 令牌引用](active-directory-b2c-reference-tokens.md)。 |
| 代码 | 应用程序请求，如果您使用授权`response_type=code+id_token`。 应用程序可以使用授权代码请求的目标资源为 access_token。 Authorization_codes 是很短期的。 通常情况下，在到期后大约 10 分钟。 |
| 状态 | 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |

此外可以将错误响应发送到`redirect_uri`，以便应用程序可以正确处理它们︰

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。 |
| 状态 | 上表中的完整说明，请参阅。 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |


## <a name="validate-the-idtoken"></a>验证 id_token
只接收 id_token 不足以进行身份验证的用户，您必须验证 id_token 的签名和验证是否根据您的应用程序要求的令牌中的声明。 Azure AD B2C 使用[JSON Web 标记 (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)和公钥加密签名令牌并验证它们有效。

有很多开放源代码库所提供的验证 JWTs，具体取决于您的语言首选项。 我们建议探索这些选项，而不是实现您自己的验证逻辑。 此处的信息将有助于找出如何正确使用这些库。

Azure AD B2C 的 OpenID 连接的元数据终结点，从而允许应用程序获取信息在运行时的 Azure AD B2C。 此信息包括终结点、 标记内容和令牌签名密钥。 没有 B2C 租户中每个策略的 JSON 元数据文档。 例如对于元数据文档`b2c_1_sign_in`中的策略`fabrikamb2c.onmicrosoft.com`位于︰

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

此配置文档的属性之一是`jwks_uri`，其值是相同的策略︰

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

在为了确定签名 id_token 中使用哪种策略 （以及在何处可以获取的元数据的） 中，有两种选择。 首先，该策略名称包括在`acr`id_token 中声称。 如何分析来自 id_token 声明上的信息，请参阅[Azure AD B2C 令牌引用](active-directory-b2c-reference-tokens.md)。 您选择编码中的值的策略就是`state`参数时发出请求，然后解码以确定使用哪种策略。 任何一种方法是完全有效的。

所得到的 OpenID 连接的元数据终结点中的元数据文档后，您可以使用 RSA 256 公用密钥 （位于该终结点上） 的 id_token 的签名验证。 可能有多个键的时间列出任何给定时刻该终结点上，每个标识的`kid`。 Id_token 标头还包含`kid`声称，这表示其中这些密钥用于对 id_token 进行。 请参阅有关详细信息，包括有关[验证令牌](active-directory-b2c-reference-tokens.md#validating-tokens)和[重要信息关于签名密钥翻转](active-directory-b2c-reference-tokens.md#validating-tokens)部分[Azure AD B2C 令牌引用](active-directory-b2c-reference-tokens.md)。
<!--TODO: Improve the information on this-->

已验证 id_token 的签名后，有几个索赔所需来验证，例如︰

- 您应验证`nonce`声称禁止令牌重放攻击。 其值应登录请求中指定的内容。
- 您应验证`aud`声称，以确保您的应用程序发出 id_token。 其值应为您的应用程序的应用程序 ID。
- 您应验证`iat`和`exp`声称以确保 id_token 尚未过期。

还有几个更多的验证应执行， [OpenID 连接核心规范](http://openid.net/specs/openid-connect-core-1_0.html)中详细介绍。  您还可以验证其他索赔项目，具体取决于您的方案。 一些常见的验证包括︰

- 确保用户组织已注册应用程序。
- 确保用户具有适当的授权权限。
- 确保身份验证的某些优点都有发生，例如，Azure 多因素身份验证。

在 id_token 中索赔的详细信息，请参阅[Azure AD B2C 令牌引用](active-directory-b2c-reference-tokens.md)。

完全验证 id_token 后，可以开始与用户的会话，并在 id_token 中使用这些声明来获取有关您的应用程序中的用户信息。 此信息可用于显示、 记录、 授权等。

## <a name="get-a-token"></a>获取标记
如果您的 web 应用程序需要做所有的就是执行策略，则可以跳过下面的几节。 这些部分仅适用于 web 应用程序需要进行身份验证对 web API 调用，同样会受到 Azure AD B2C。

可以兑换获得授权 (使用`response_type=code+id_token`) 通过发送所需的资源的标记为`POST`请求的`/token`终结点。 目前，您可以请求的标记的唯一资源是应用程序自己的后端 web API。 请求为您自己标记的约定是将您的应用程序的客户端 ID 用作范围︰

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| 参数 | 必填？ | 说明 |
| ----------------------- | ------------------------------- | --------------------- |
| p | 必填 | 获得授权码时使用的策略。 不能在此请求中使用不同的策略。 请注意您将此参数添加到中的**查询字符串**，不发布正文。 |
| client_id | 必填 | [Azure 门户](https://portal.azure.com/)分配给您的应用程序的应用程序 ID。 |
| grant_type | 必填 | 授权，必须是类型`authorization_code`授权代码流。 |
| 作用域 | 建议 | 以空格分隔的作用域的列表。 单个作用域值表示到 Azure AD 这两个正在请求的权限。 `openid`范围指示登录用户并获得有关在窗体中的**id_tokens**的用户数据的权限。 它可以用于获取对您的应用程序自己的后端 web API，这由与客户端相同的应用程序 ID 的标记。 `offline_access`范围表明，您的应用程序将需要**refresh_token** ，长期存在对资源的访问。 |
| 代码 | 必填 | 中流的第一个腿获得授权。 |
| redirect_uri | 必填 | 授权的接收位置的应用程序 redirect_uri。 |
| client_secret | 必填 | 在[Azure 门户](https://portal.azure.com/)中生成应用程序密码。 此应用程序的机密是重要的安全项目。 您应该在您的服务器上安全地存储它。 您还应采取措施将该客户端密码定期。 |

成功的令牌响应类似于︰

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| not_before | 该标记是有效的在日期时间时间。 |
| token_type | 标记类型值。 Azure 的广告支持的唯一类型为载体。 |
| access_token | 您请求的签名的 JWT 标记。 |
| 作用域 | 该标记是有效的它可以用来缓存以供以后使用的标记的范围。 |
| expires_in | Access_token 是有效 （以秒为单位） 的时间长度。 |
| refresh_token | OAuth 2.0 refresh_token。 应用程序可以使用此标记当前令牌到期后获得额外的标记。  Refresh_tokens 是多长时间，然后可以用于较长时间保留对资源的访问。 有关详细信息，请参阅[B2C 令牌引用](active-directory-b2c-reference-tokens.md)。 请注意，您必须使用范围`offline_access`中的授权和为了接收 refresh_token 令牌请求。 |

错误响应类似于︰

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。 |

## <a name="use-the-token"></a>使用该标记
现在，您已经成功获得`access_token`，可以在请求中使用标记到您的后端 web Api 包括在`Authorization`标头︰

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>刷新该标记
Id_tokens 是短期的。 他们期满继续能够访问资源后，您必须刷新它们。 可以做到通过提交另一个`POST`请求的`/token`终结点。 这一次，提供`refresh_token`而不是`code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| 参数 | 必填 | 说明 |
| ----------------------- | ------------------------------- | -------- |
| p | 必填 | 被用来获得原始的 refresh_token 策略。 不能在此请求中使用不同的策略。 请注意您将此参数添加到中的**查询字符串**，不发布正文。 |
| client_id | 必填 | [Azure 门户](https://portal.azure.com/)分配给您的应用程序的应用程序 ID。 |
| grant_type | 必填 | 授权，必须是类型`refresh_token`对此链路的授权代码流。 |
| 作用域 | 建议 | 以空格分隔的作用域的列表。 单个作用域值表示到 Azure AD 这两个正在请求的权限。 `openid`范围指示登录用户并获得有关在窗体中的**id_tokens**的用户数据的权限。 它可以用于获取对您的应用程序自己的后端 web API，这由与客户端相同的应用程序 ID 的标记。 `offline_access`范围表明，您的应用程序将需要**refresh_token** ，长期存在对资源的访问。 |
| redirect_uri | 建议 | 授权的接收位置的应用程序 redirect_uri。 |
| refresh_token | 必填 | 流程的第二个腿中获得的原始 refresh_token。 请注意，您必须使用范围`offline_access`中的授权和为了接收 refresh_token 令牌请求。 |
| client_secret | 必填 | 在[Azure 门户](https://portal.azure.com/)中生成应用程序密码。 此应用程序的机密是重要的安全项目。 您应该在您的服务器上安全地存储它。 您还应采取措施将该客户端密码定期。 |

成功的令牌响应类似于︰

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| not_before | 该标记是有效的在日期时间时间。 |
| token_type | 标记类型值。 Azure 的广告支持的唯一类型为载体。 |
| access_token | 您请求的签名的 JWT 标记。 |
| 作用域 | 该标记是有效的它可以用来缓存以供以后使用的标记的范围。 |
| expires_in | Access_token 是有效 （以秒为单位） 的时间长度。 |
| refresh_token | OAuth 2.0 refresh_token。 应用程序可以使用此标记当前令牌到期后获得额外的标记。  Refresh_tokens 是多长时间，然后可以用于较长时间保留对资源的访问。 有关详细信息，请参阅[B2C 令牌引用](active-directory-b2c-reference-tokens.md)。 |

错误响应类似于︰

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员确定身份验证错误的根本原因。 |


## <a name="send-a-sign-out-request"></a>请求退出

当您想要注册用户在该应用程序时，它不足以清除应用程序的 cookie 或以其他方式结束与用户的会话。 您还必须将用户指向 Azure 广告注销。 如果您不能执行此操作，用户可以重新进行身份验证为您的应用程序，而无需重新输入其凭据。 这是因为它们将具有有效单个登录会话使用 Azure 的广告。

您可以只是将用户重定向到`end_session_endpoint`列出同一 OpenID 连接元数据文档中前面"的验证 id_token"一节中所述︰

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| 参数 | 必填？ | 说明 |
| ----------------------- | ------------------------------- | ------------ |
| p | 必填 | 您想要使用登录应用程序用户的策略。 |
| post_logout_redirect_uri | 建议 | 用户应该被重定向到 URL 后成功退出。 如果不包含，用户将通过 Azure AD B2C 显示通用的消息。  |

> [AZURE.NOTE]
    而定向至`end_session_endpoint`将清除某些用户的单个登录状态与 Azure AD B2C，它不会对用户在该用户的社会身份提供程序 (IDP) 会话进行签名。 如果用户选择的同一 IDP 后续登录的过程中，他们将重新进行身份验证，而无需输入其凭据。 如果用户想要退出 B2C 应用程序，它并不意味着他们要完全退出他们的 Facebook 帐户。 但是，如果是本地帐户，该用户的会话将结束正常。

## <a name="use-your-own-b2c-tenant"></a>使用您自己的 B2C 租户

如果您想亲自尝试这些请求，必须首先执行以下三个步骤，然后替换为您自己的示例值的上方︰

- [创建一个 B2C 租户](active-directory-b2c-get-started.md)和使用您的请求中的组织的名称。
- [创建应用程序](active-directory-b2c-app-registration.md)，以获得应用程序 ID 和 redirect_uri。 想要包括**的 web 应用程序/web api**在您的应用程序，还可以选择创建**应用程序密码**。
- [创建您的策略](active-directory-b2c-reference-policies.md)，以获得您的策略名称。
