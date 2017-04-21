<properties
    pageTitle="Azure 的 Active Directory B2C |Microsoft Azure"
    description="在 Azure 活动目录 B2C 颁发的令牌类型。"
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


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C︰ 令牌引用

处理每个[身份验证流程](active-directory-b2c-apps.md)，azure 的活动目录 (AD Azure) B2C 发出几种类型的安全令牌。 本文档描述的格式、 安全特征和每种类型的令牌的内容。

## <a name="types-of-tokens"></a>类型的标记

Azure AD B2C 支持[OAuth 2.0 授权协议](active-directory-b2c-reference-protocols.md)，从而使访问令牌和刷新令牌的使用。 它还支持身份验证和登录通过[OpenID 连接](active-directory-b2c-reference-protocols.md)，其中介绍了第三种类型的令牌︰ ID 标记。 每个这些标记表示为载体令牌。

持有者标记是一个轻量的安全，以授予对受保护资源的"载体"访问权限。 载体是任意一方都可以显示该标记。 Azure 的广告必须首先验证身份一方，它可以接收的持有者标记之前。 但是，如果没有采取所需的步骤来保护在传输和存储的标记，它可以截获和意外的当事方使用。 某些安全令牌有内置机制来防止未经授权的方使用它们，但持有者标记并没有这种机制。 他们必须在安全通道中，如传输层安全 (HTTPS) 传输。

如果持有者令牌传输安全通道之外，恶意方可以使用拦截的攻击以获取令牌并使用它来获取对受保护资源的未授权的访问。 载体令牌存储或缓存以备以后使用时，将应用相同的安全性原则。 始终确保您的应用程序传输和安全的方式存储载体的标记。

持有者标记上的附加安全注意事项，请参阅[RFC 6750 第 5 节](http://tools.ietf.org/html/rfc6750)。

许多 Azure AD B2C 颁发的令牌实现为 JSON web 标记 (JWTs)。 JWT 是结构紧凑、 URL 安全两个当事方之间传输信息的方法。 JWTs 包含称为索赔的信息。 这些是信息的断言载体和主体的标记。 在 JWTs 的声明是 JSON 编码和传输序列化的对象。 颁发的 Azure AD B2C JWTs 签署但未加密，因为您可以轻松地检查 JWT 进行调试的内容。 可以使用几种工具，可以执行此操作，包括[calebb.net](http://calebb.net)。 有关 JWTs 的详细信息，请参阅[JWT 规范](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)。

### <a name="id-tokens"></a>ID 标记

一个 ID 的标记是一种从 Azure AD B2C 接收您的应用程序的安全令牌`authorize`和`token`的终结点。 ID 标记表示为[JWTs](#types-of-tokens)，并且它们包含可用于标识您的应用程序中的用户的声明。 当从获取 ID 标记`authorize`终结点，它们通常用来登录到 web 应用程序的用户。 当从获取 ID 标记`token`终结点，也可以采用发送 HTTP 请求中的同一个应用程序或服务的两个组件之间进行通信的过程。 随心所欲，您可以使用声明中的 ID 标记。 它们通常用于显示帐户信息或使应用程序中的访问控制决策。  

登录 ID 标记，但这一次没有加密。 当您的应用程序或 API 接收 ID 标记时，则它必须[验证签名](#token-validation)以证明令牌可信。 您的应用程序或 API 还必须验证令牌中的一些索赔证明是有效的。 根据方案的要求，由应用程序验证的声明可以各不相同，但在每种情况下，您的应用程序必须执行一些[常见声明验证](#token-validation)。

#### <a name="sample-id-token"></a>示例标识标记
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>访问令牌

访问标记也是一种从 Azure AD B2C 接收您的应用程序的安全令牌`authorize`和`token`的终结点。 访问令牌还表现为[JWTs](#types-of-tokens)，并且它们包含可用于确定用户在您的 web 服务和 Api 的索赔。

访问令牌签名，但它们不是这次的加密和非常类似于 id 标记。  提供对 web 服务和 Api 的访问以及进行识别和对这些服务中的用户进行身份验证，应使用访问令牌。  但是，它们不提供这些服务的授权任何断言。  也就是说，`scp`在访问令牌中的声明并不限制或否则表示该标记的主题已被授予访问权限。

当您的 API 接收一个访问令牌时，它必须[验证签名](#token-validation)以证明令牌可信。 您的 API 还必须验证令牌中的一些索赔证明是有效的。 根据方案的要求，由应用程序验证的声明可以各不相同，但在每种情况下，您的应用程序必须执行一些[常见声明验证](#token-validation)。

### <a name="claims-in-id--access-tokens"></a>标识和访问令牌中的声明

当您使用 Azure AD B2C 时，可以精细地控制您标记的内容。 您可以配置[策略](active-directory-b2c-reference-policies.md)以发送您的应用程序需要对其操作的索赔中的某些用户数据集。 这些声明可以包括标准属性，例如用户的`displayName`， `emailAddress`。 它们还可以包括[自定义用户属性](active-directory-b2c-reference-custom-attr.md)，您可以定义在 B2C 目录中。 每个标识和访问您收到的标记将包含一组特定的与安全相关的索赔。 您的应用程序可以使用这些声明来安全地进行身份验证的用户和请求。

请注意，在 ID 令牌中的声明不会以任何特定的顺序返回。 此外，新的索赔在任何时候在 ID 令牌中被引入。 您的应用程序不应该中断，因为引入新的报销申请。 这里是您期望中的标识和访问令牌颁发的 Azure AD B2C 存在的声明。 任何其他索赔将由策略决定。 实践中，请尝试通过将它粘贴到[calebb.net](http://calebb.net)检查示例 ID 令牌中的声明。 [OpenID 连接规范](http://openid.net/specs/openid-connect-core-1_0.html)中找不到更多详细信息。

| 名称 | 报销申请 | 示例值 | 说明 |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| 访问群体 | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | 观众声明标识标记的预期的收件人。 对 Azure AD B2C，观众是您的应用程序的应用程序 ID，因为分配给您的应用程序应用程序注册门户中。 您的应用程序应验证此值，并拒绝该标记，如果它不匹配。 |
| 颁发者 | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | 此声明标识安全令牌服务 (STS) 的构造和返回的标记。 它还标识用户已通过身份验证的 Azure 的广告目录。 您的应用程序应验证颁发者声明以确保令牌来自 v2.0 终结点。 |
| 在颁发 | `iat` | `1438535543` | 此声明是在令牌颁发，所表示的日期时间的时间。 |
| 过期时间 | `exp` | `1438539443` | 索赔是在标记变得无效，而过期时间表示日期时间中。 您的应用程序应使用此声明来验证令牌的生存期的有效性。  |
| 不前 | `nbf` | `1438535543` | 此报销申请时的标记变为有效，表示的日期时间。 这通常是已颁发令牌的时间相同。 您的应用程序应使用此声明来验证令牌的生存期的有效性。  |
| 版本 | `ver` | `1.0` | 这是由 Azure 广告定义 ID 标记的版本。 |
| 哈希代码 | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | 仅当令牌颁发以及 OAuth 2.0 授权码时 ID 令牌中包括代码哈希。 可以使用代码哈希来验证授权码的真实性。 有关如何执行此验证，请参阅[OpenID 连接规范](http://openid.net/specs/openid-connect-core-1_0.html)有关更多详细信息。 |
| 访问令牌希 | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | 仅当令牌颁发以及 OAuth 2.0 访问令牌 ID 令牌中包括访问令牌希。 访问令牌哈希可用于验证访问令牌的真实性。 有关如何执行此验证，请参阅[OpenID 连接规范](http://openid.net/specs/openid-connect-core-1_0.html)有关更多详细信息。 |
| 现时 | `nonce` | `12345` | 现时是用来缓解令牌重放攻击的策略。 您的应用程序可以通过一个授权请求中指定为 nonce`nonce`查询参数。 您在请求中提供的值将保持不变在激发`nonce`声称的 ID 标记。 这将允许您的应用程序验证的值，它指定在请求时，它将应用程序的会话与给定的 ID 标记相关联的值。 您的应用程序应该执行此验证 ID 标记的验证过程。 |
| 主题 | `sub` | `Not supported currently. Use oid claim.` | 这是有关哪些标记断言的信息，如应用程序的用户的主体。 此值是不可变的无法重新分配或重新使用。 它可以用来执行授权检查安全，例如，当使用标记来访问资源。 但是，主题声明尚未实现在 Azure AD B2C。 您应该配置您要包括的对象 ID 的策略`oid`声称，它的值用于标识用户，而不是使用授权的主题声明。 |
| 身份验证上下文类引用 | `acr` | `b2c_1_sign_in` | 这是用于获取 ID 令牌策略的名称。  |
| 身份验证时间 | `auth_time` | `1438535543` | 此声明是用户最后一次输入的凭据，表示日期时间的时间。 |


### <a name="refresh-tokens"></a>刷新的标记

刷新标记是您的应用程序可以使用它来获取新的 ID 标记和访问令牌 OAuth 2.0 流中的安全令牌。 它们提供您的应用程序代表用户长期对资源的访问权限而不需要与用户进行交互。

若要接收刷新令牌令牌的响应，您的应用程序必须请求`offline_acesss`范围。 若要了解有关`offline_access`范围， [Azure AD B2C 协议参考](active-directory-b2c-reference-protocols.md)。

刷新的标记，并将始终保持，为您的应用程序完全不透明。 他们通过 Azure 广告发布和可检查和解释只的 Azure 的广告。 它们是长期存在，但不是应该在特定时间内将持续刷新令牌的假定条件写入您的应用程序。 在任意时刻的原因有多种，刷新令牌可以失效。 您知道刷新令牌是否有效的应用程序的唯一方法是尝试通过令牌请求到 Azure AD 将兑换成现金。

当您兑换新的令牌的刷新标记 (和您的应用程序是否已被授予`offline_access`范围内)，您将收到新的刷新令牌，令牌的响应。 您应保存新颁发的刷新令牌。 它应取代先前请求中使用的刷新令牌。 这有助于确保您刷新标记保持尽可能长时间有效。

## <a name="token-validation"></a>标记验证

若要验证一个令牌，您的应用程序应该检查的签名和声明的标记。

为验证 JWTs，具体取决于您的首选语言提供了很多开放源代码库。 我们建议您熟悉这些选项而不是实现您自己的验证逻辑。 本指南中的信息可以帮助您了解如何正确使用这些库。

### <a name="validate-the-signature"></a>验证签名
JWT 包含三个，由`.`字符。 第一段是**头**，第二是**正文**，和第三个是**签名**。 特征码段可用于验证令牌的真实性，以便它可以被您的应用程序信任。

Azure AD B2C 令牌签名通过使用业界标准的非对称加密算法，例如 RSA 256 色。 标头的标记包含用于签名令牌的密钥和加密方法的信息︰

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

`alg`声明表示用于对令牌进行签名的算法。 `kid`声明表示用于对令牌进行签名的特定公钥。

在任何给定时间，Azure 的广告可能会使用一组特定的公共 / 专用密钥对中的任何一个令牌签名。 因此应该编写您的应用程序来自动处理这些密钥更改定期，azure AD 旋转一组可能的密钥。 检查到 Azure 广告所使用的公钥的密钥更新的合理频率是每 24 小时。

Azure AD B2C 的 OpenID 连接的元数据终结点。 这允许应用程序获取信息在运行时的 Azure AD B2C。 此信息包括终结点、 标记内容和令牌签名密钥。 B2C 目录包含每个策略的 JSON 元数据文档。 例如，元数据文档的`b2c_1_sign_in`中的策略`fabrikamb2c.onmicrosoft.com`位于︰

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`是用来对用户进行身份验证的 B2C 目录和`b2c_1_sign_in`是用来获取令牌策略。 若要确定哪个策略用于签署一个令牌 （以及在哪里提取元数据），有两种选择。 首先，该策略名称包括在`acr`令牌中的声明。 您可以分析出 JWT 索赔由 base64 解码正文和反序列化生成的 JSON 字符串。 `acr`索赔将会用来颁发令牌策略的名称。  您选择编码中的值的策略就是`state`参数时发出请求，然后解码以确定使用哪种策略。 任何一种方法是有效的。

元数据文档是信息的一个 JSON 对象，包含几条有用。 其中包括执行连接 OpenID 身份验证所需的终结点的位置。 它们还包括`jwks_uri`，这样的公用密钥集的位置，用来签名的标记。 位置这里提供的但最好是通过使用元数据文档和分析出动态游标位置`jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

位于此 URL 的 JSON 文档包含所有在某个特定时刻，使用公钥信息。 您的应用程序可以使用`kid`声称在 JWT 头用于签署特定标记的 JSON 文档中选择的公共密钥。 然后，它可以通过使用正确的公钥和指定的算法执行签名验证。

如何执行签名验证的说明已超出了本文的范围。 很多开放源代码库都可以与此帮助您，如果您需要它。

### <a name="validate-the-claims"></a>验证声明
当您的应用程序或 API 接收 ID 标记时，还应执行索赔针对的几种检查中的 ID 标记。 这些包括但不是限于︰

- **观众**声明︰ 这将验证 ID 标记旨在提供给您的应用程序。
- **不早**和**到期时间**索赔︰ 这些验证 ID 标记尚未过期。
- **颁发者**声明︰ 这将验证令牌是否由 Azure AD 颁发到您的应用程序。
- **现时**︰ 这是一个战略，令牌重放攻击缓解。

验证您的应用程序应该执行的完整列表，请参阅[连接 OpenID 技术指标](https://openid.net)。 这些索赔的预期值的详细信息都包含在前面[标记节](#types-of-tokens)。  

## <a name="token-lifetimes"></a>令牌生存期

提供了以下的令牌生存期的进一步了解。 开发和调试应用程序时，它们可以帮助您。 请注意，您的应用程序不应写入期望任何这些生存期中保持不变。 他们可以并将更改。  您可以阅读更多有关自定义的令牌生存期 Azure AD B2C 在[这里](active-directory-b2c-token-session-sso.md)。

| 标记 | 生命周期 | 说明 |
| ----------------------- | ------------------------------- | ------------ |
| ID 标记 | 一小时 | ID 的标记的通常有效期均为一小时。 您的 web 应用程序可以使用这一寿命维护其自己的会话与用户 （推荐）。 您还可以选择不同的会话生存期。 如果您的应用程序需要获取新的 ID 标记，它只需要对 Azure 广告进行新的登录请求。 如果用户具有有效的浏览器会话使用 Azure 的广告，该用户可能不需要再次输入凭据。 |
| 刷新的标记 | 最多 14 天 | 一个刷新令牌无效，最多 14 天。 但是，刷新令牌可能因多种原因随时会无效。 您的应用程序应该继续使用刷新令牌，直到请求就会失败，或者您的应用程序刷新标记替换为一个新尝试。  此外刷新令牌会如果在 90 天后用户上次输入的凭据无效。 |
| 授权码 | 五分钟 | 授权码都有意很短。 他们应该兑现立即访问令牌、 ID 标记或刷新令牌在到达时。 |
