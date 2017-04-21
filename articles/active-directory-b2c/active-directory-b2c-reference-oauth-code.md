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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure 的活动目录 B2C: OAuth 2.0 授权代码流

OAuth 2.0 授权代码授予可访问受保护的资源，如 web Api 的设备安装的应用程序中。 通过使用 OAuth 2.0 的 Azure 活动目录 (AD Azure) B2C 实现，您可以添加移动和桌面应用程序的注册、 登录、 和其他身份管理任务。 本指南是独立的语言。 它描述了如何发送和接收 HTTP 消息而无需使用任何我们的开源库。

<!-- TODO: Need link to libraries -->

[OAuth 2.0 规范的 4.1 节](http://tools.ietf.org/html/rfc6749)描述了 OAuth 2.0 授权代码流。 可用于在大多数的应用程序类型，包括[web 应用程序](active-directory-b2c-apps.md#web-apps)和[本地安装的应用程序](active-directory-b2c-apps.md#mobile-and-native-apps)执行身份验证和授权。 它使应用程序安全地获得**access_tokens**，它可以用于访问资源的[授权服务器](active-directory-b2c-reference-protocols.md#the-basics)都不会得到。

本指南将重点 OAuth 2.0 授权代码流 —**公共客户端**的特殊口味。 公用客户端是不能信任，可以安全地保护密码的任何客户端应用程序。 这包括移动应用程序、 桌面应用程序，以及几乎任何应用程序在设备上运行，并需要获得 access_tokens。 如果您想要添加到 web 应用程序使用 Azure AD B2C 的标识管理，您应该使用[OpenID 连接](active-directory-b2c-reference-oidc.md)而不是不是 OAuth 2.0。

Azure AD B2C 扩展 OAuth 2.0 流动做更多比简单身份验证和授权的标准。 它引入了[**策略参数**](active-directory-b2c-reference-policies.md)，它使您能够使用 OAuth 2.0 将用户体验添加到您的应用程序，如注册，登录，和配置文件管理。 下面我们将介绍如何使用 OAuth 2.0 和策略来实现这些体验的每个本机应用程序中。 我们还将介绍如何访问 web Api 获取 access_tokens。

下面的示例 HTTP 请求将使用我们的示例的 B2C 目录、 **fabrikamb2c.onmicrosoft.com**，以及我们的示例应用程序和策略。 您可以自由地使用这些值，请出自己的请求，或者可以替换为您自己。
了解如何[获得自己的 B2C 目录、 应用程序和策略](#use-your-own-b2c-directory)。

## <a name="1-get-an-authorization-code"></a>1.获得授权码
以客户端定向到用户的授权代码流开始`/authorize`终结点。 这是流的交互部分用户实际采取措施。 在此请求中，客户端指示中，用户获得所需的权限`scope`参数和策略在执行`p`参数。 三个示例附带以下 （换行以提高可读性），每一个使用不同的策略。

#### <a name="use-a-sign-in-policy"></a>使用签入策略

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>使用注册策略

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>使用编辑配置文件策略

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| 参数 | 必填？ | 说明 |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | 必填 | [Azure 门户](https://portal.azure.com)分配给您的应用程序的应用程序 ID。 |
| response_type | 必填 | 响应类型，该类型必须包含`code`授权代码流。 |
| redirect_uri | 必填 | 您的应用程序，可以发送和接收应用程序的身份验证响应 redirect_uri。 它必须完全匹配您注册在门户中，redirect_uris 的一个不同之处在于，它必须是 URL 编码。 |
| 作用域 | 必填 | 以空格分隔的作用域的列表。 单个作用域值表示到 Azure AD 这两个正在请求的权限。 使用客户机 ID 范围指示您的应用程序需要**访问令牌**可以根据自己的服务或 web API 使用，由相同的客户端 id。  `offline_access`范围表明，您的应用程序将需要**refresh_token** ，长期存在对资源的访问。  您还可以使用`openid`请求**id_token** Azure AD B2C 的范围。 |
| response_mode | 建议 | 应使用将得到授权发送回您的应用程序的方法。 它可以是一个查询、 form_post 或片断。
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。 它可以是您希望的任何内容的字符串。 随机生成唯一值通常用于防止跨站点请求伪造攻击。 状态还用于发生身份验证请求，如页上或正在执行的策略之前对应用程序中用户的状态信息进行编码。 |
| p | 必填 | 将执行该策略。 它是策略的在 B2C 目录中创建的名称。 策略名称值，您应使用"b2c\_1\_"。 了解有关[扩展策略框架](active-directory-b2c-reference-policies.md)中的策略。 |
| 提示 | 可选 | 用户交互所需的类型。 目前唯一有效的值为登录，强制用户输入其凭据在该请求。 单一登录才会生效。 |

在这种情况下，用户可能需要完成该策略的工作流。 这可能包括用户输入他们的用户名和密码，登录社交身份，注册了目录或其他任意数量的步骤，具体取决于如何定义策略。

Azure AD 用户完成策略后，将返回以指示您的应用程序的响应`redirect_uri`，通过使用指定的方法在`response_mode`参数。 响应将是完全相同的每个上述的情况下，独立执行的政策。

成功的响应，使用`response_mode=query`如下所示︰

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 代码 | 应用程序请求的授权。 应用程序可以使用授权代码请求的目标资源为 access_token。 Authorization_codes 是很短期的。 通常情况下，在到期后大约 10 分钟。 |
| 状态 | 上表中的完整说明，请参阅。 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |

此外可能被错误响应发送到`redirect_uri`，以便应用程序可以正确处理它们︰

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可以用于分类的各种错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员来标识身份验证错误的根本原因。 |
| 状态 | 请参阅本部分中的第一个表格的完整说明。 如果请求中包含一个状态参数，相同的值应显示在响应中。 应用程序应确认请求和响应中的状态值的完全相同。 |


## <a name="2-get-a-token"></a>2.获取标记
现在，您已获得授权，可以兑换`code`通过发送所需的资源的标记为`POST`请求的`/token`终结点。 在 Azure AD B2C，您可以请求的标记的唯一资源是应用程序自己的后端 web API。 用于请求给自己一个令牌的约定是将您的应用程序的客户端 ID 用作范围︰

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| 参数 | 必填？ | 说明 |
| ----------------------- | ------------------------------- | --------------------- |
| p | 必填 | 获得授权码时使用的策略。 不能在此请求中使用不同的策略。 请注意您将此参数添加到中的*查询字符串*，不发布正文。 |
| client_id | 必填 | [Azure 门户](https://portal.azure.com)分配给您的应用程序的应用程序 ID。 |
| grant_type | 必填 | 授权，必须是类型`authorization_code`授权代码流。 |
| 作用域 | 推荐 | 以空格分隔的作用域的列表。 单个作用域值表示到 Azure AD 这两个正在请求的权限。 使用客户机 ID 范围指示您的应用程序需要**访问令牌**可以根据自己的服务或 web API 使用，由相同的客户端 id。  `offline_access`范围表明，您的应用程序将需要**refresh_token** ，长期存在对资源的访问。  您还可以使用`openid`请求**id_token** Azure AD B2C 的范围。 |
| 代码 | 必填 | 中流的第一个腿获得授权。 |
| redirect_uri | 必填 | 授权的接收位置的应用程序 redirect_uri。 |

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
| access_token | 您请求的签名的 JSON Web 标记 (JWT) 标记。 |
| 作用域 | 该标记是有效的它可以用来缓存以供以后使用的标记的范围。 |
| expires_in | 该标记是有效 （以秒为单位） 的时间长度。 |
| refresh_token | OAuth 2.0 refresh_token。 应用程序可以使用此标记当前令牌到期后获得额外的标记。 Refresh_tokens 是多长时间，然后可以用于较长时间保留对资源的访问。 有关详细信息，请参阅[B2C 令牌引用](active-directory-b2c-reference-tokens.md)。 |

错误响应类似于︰

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- |
| 错误 | 错误代码字符串，可以用于分类的各种错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员来标识身份验证错误的根本原因。 |

## <a name="3-use-the-token"></a>3.使用该标记
现在，您已经成功获得`access_token`，可以在请求中使用标记到您的后端 web Api 包括在`Authorization`标头︰

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4.刷新令牌
访问和 Id 标记是短期的。 他们期满继续能够访问资源后，您必须刷新它们。 可以做到通过提交另一个`POST`请求的`/token`终结点。 这次提供`refresh_token`而不是`code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| 参数 | 必填？ | 说明 |
| ----------------------- | ------------------------------- | -------- |
| p | 必填 | 被用来获得原始的 refresh_token 策略。 不能在此请求中使用不同的策略。 请注意您将此参数添加到中的*查询字符串*，不发布正文。 |
| client_id | 建议 | [Azure 门户](https://portal.azure.com)分配给您的应用程序的应用程序 ID。 |
| grant_type | 必填 | 授权，必须是类型`refresh_token`对此链路的授权代码流。 |
| 作用域 | 建议 | 以空格分隔的作用域的列表。 单个作用域值表示到 Azure AD 这两个正在请求的权限。 使用客户机 ID 范围指示您的应用程序需要**访问令牌**可以根据自己的服务或 web API 使用，由相同的客户端 id。  `offline_access`范围表明，您的应用程序将需要**refresh_token** ，长期存在对资源的访问。  您还可以使用`openid`请求**id_token** Azure AD B2C 的范围。 |
| redirect_uri | 可选 | 授权的接收位置的应用程序 redirect_uri。 |
| refresh_token | 必填 | 流程的第二个腿中获得的原始 refresh_token。 |

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
| access_token | 您请求的签名的 JSON Web 标记 (JWT) 标记。 |
| 作用域 | 该标记是有效的它可以用来缓存以供以后使用的标记的范围。 |
| expires_in | 该标记是有效 （以秒为单位） 的时间长度。 |
| refresh_token | OAuth 2.0 refresh_token。 应用程序可以使用此标记当前令牌到期后获得额外的标记。 Refresh_tokens 是多长时间，然后可以用于较长时间保留对资源的访问。 有关详细信息，请参阅[B2C 令牌引用](active-directory-b2c-reference-tokens.md)。 |

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

## <a name="use-your-own-b2c-directory"></a>使用您自己的 B2C 目录

如果您想亲自尝试这些请求，必须首先执行以下三个步骤，然后替换为您自己的示例值的上方︰

- [创建一个 B2C 的目录](active-directory-b2c-get-started.md)和使用您的请求中的目录的名称。
- [创建应用程序](active-directory-b2c-app-registration.md)，以获得应用程序 ID 和 redirect_uri。 想要包括在您的应用程序中的**本机客户端**。
- [创建您的策略](active-directory-b2c-reference-policies.md)，以获得您的策略名称。
