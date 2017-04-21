<properties
    pageTitle="Azure AD v2.0 令牌引用 |Microsoft Azure"
    description="标记和索赔发出 v2.0 终结点的类型"
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

# <a name="v20-token-reference"></a>v2.0 令牌引用

V2.0 终结点会发出几种类型的[身份验证流](active-directory-v2-flows.md)的每个处理过程中的安全令牌。 本文档描述的格式、 安全特征和每种类型的令牌的内容。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

## <a name="types-of-tokens"></a>类型的标记

V2.0 终结点支持的[OAuth 2.0 授权协议](active-directory-v2-protocols.md)，从而使使用的 access_tokens 和 refresh_tokens。  它还支持身份验证和登录通过[OpenID 连接](active-directory-v2-protocols.md#openid-connect-sign-in-flow)，其中介绍了第三种类型的令牌，id_token。  每个这些标记表示为"持有者标记"。

持有者标记是一个轻量的安全，以授予对受保护资源的"载体"访问权限。 在此意义上，"载体"是任何一方可以提供标记。 尽管当事方必须首先进行身份验证使用 Azure 广告接收的持有者标记，如果没有采取所需的步骤来保护在传输和存储的标记，它可以截获并由意外的当事方。 某些安全令牌有阻止未经授权的方使用其内置的机制，而持有者标记并不一定这一机制必须在传输层安全 (HTTPS) 等安全通道传输。 如果持有者标记明文传输的手册中攻击可通过恶意方获取令牌并将其用于对受保护资源的未经授权的访问。 存储或缓存的持有者标记以供以后使用时，将应用相同的安全性原则。 始终确保您的应用程序传输和安全的方式存储载体的标记。 更多载体令牌的安全考虑，请参阅[RFC 6750 第 5 节](http://tools.ietf.org/html/rfc6750)。

许多 v2.0 终结点所颁发的令牌实现为 Json Web 标记或 JWTs。  JWT 是结构紧凑、 URL 安全两个当事方之间传输信息的方法。  JWTs 中包含的信息被称为"声明"或断言的信息载体和标记的主题有关。  JWTs 中的声明的编码和传输序列化 JSON 对象。  因为颁发 v2.0 终结点的 JWTs 已经签署，但未加密，可以出于调试目的轻松地检查 JWT 的内容。 JWTs 的详细信息，您可以参考[JWT 规范](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)。

## <a name="idtokens"></a>Id_tokens

Id_tokens 是一种执行使用[OpenID 连接](active-directory-v2-protocols.md#openid-connect-sign-in-flow)进行身份验证时，您的应用程序接收的登录的安全令牌。  它们表示为[JWTs](#types-of-tokens)，并且包含用于登录到您的应用程序的用户的声明。  根据您的习惯-用于显示帐户信息，或在应用程序中进行访问控制决策通常使用它们，可以使用 id_token 中的索赔。  V2.0 终结点只会发出一种类型的 id_token，有一套一致的索赔与已登录的用户的类型无关。  这是说的格式和内容的 id_tokens 将是相同的两个人的 Microsoft 帐户用户和工作或学校的科目。

Id_tokens 是签名，但这一次没有加密。  当您的应用程序收到 id_token 时，则它必须[验证签名](#validating-tokens)以证明真实性的标记并验证令牌中的几个声明以证明其有效性。  由应用程序验证的声明有所不同根据方案的要求，但您的应用程序必须在每个方案中执行一些[常见声明验证](#validating-tokens)。

在 id_tokens 中索赔的详细信息如下，以及示例 id_token。  请注意，id_tokens 中的声明不会以任何特定的顺序返回。  此外，新的报销申请可以及时引入到任意点处的 id_tokens-您的应用程序不应该中断，因为引入新的报销申请。  下面的列表包括您的应用程序能够可靠地解释在撰写本文时的声明。  如有必要，更多的细节可以找[OpenID 连接规范](http://openid.net/specs/openid-connect-core-1_0.html)中。

#### <a name="sample-idtoken"></a>Id_token 示例

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] 实践中，请尝试通过将它粘贴到[calebb.net](https://calebb.net)检查示例 id_token 中的声明。

#### <a name="claims-in-idtokens"></a>在 id_tokens 中的索赔
| 名称 | 报销申请 | 示例值 | 说明 |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| 访问群体 | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | 标识目标的收件人的令牌。  在 id_tokens，观众是应用程序的应用程序 Id，因为分配给您的应用程序应用程序注册门户中。  您的应用程序应验证此值，并拒绝该标记，如果它不匹配。 |
| 颁发者 | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | 标识安全令牌服务 (STS) 构造并返回该标记，以及用户已通过身份验证的 Azure AD 租户。  您的应用程序应验证颁发者声明以确保令牌来自 v2.0 终结点。  它应使用声明的 guid 部分来限制承租人允许登录到该应用程序集。  指示用户的 guid 是消费者用户帐户是 microsoft `9188040d-6c67-4c5b-b112-36a304b66dad`。 |
| 在颁发 | `iat` | `1452285331` | 日期时间表示的已颁发令牌的时间。 |
| 过期时间 | `exp` | `1452289231` | 在标记变得无效，表示日期时间。  您的应用程序应使用此声明来验证令牌的生存期的有效性。  |
| 不前 | `nbf` | `1452285331` |  在标记变得有效，表示日期时间。 它通常是颁发时间相同。  您的应用程序应使用此声明来验证令牌的生存期的有效性。  |
| 版本 | `ver` | `2.0` | Id_token，通过 Azure 广告定义的版本。  V2.0 终结点，值将为`2.0`。 |
| 租户 Id | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | 表示 Azure AD 的 guid 租户的用户是从。  工作和学校科目的 guid 将是组织的用户所属的永恒的租户 ID。  对于个人帐户的值将为`9188040d-6c67-4c5b-b112-36a304b66dad`。  `profile`范围需接收此声明。 |
| 哈希代码 | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | 哈希代码包含在 id_tokens id_token 发出旁边 OAuth 2.0 授权码时只。  它可以用来验证授权码的真实性。  有关执行此验证，请参阅[OpenID 连接规范](http://openid.net/specs/openid-connect-core-1_0.html)有关更多详细信息。 |
| 访问令牌希 | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | 访问令牌希包含在 id_tokens id_token 发出旁边 OAuth 2.0 访问令牌时只。  它可以用于验证访问令牌的真实性。  有关执行此验证，请参阅[OpenID 连接规范](http://openid.net/specs/openid-connect-core-1_0.html)有关更多详细信息。 |
| 现时 | `nonce` | `12345` | 现时是缓解令牌重放攻击的策略。  您的应用程序可以通过一个授权请求中指定为 nonce`nonce`查询参数。  您在请求中提供的值将激发在 id_token`nonce`声明，不做任何修改。  这将允许您的应用程序验证的值，它指定在请求时，它将应用程序的会话与给定 id_token 相关联的值。  您的应用程序应该执行此验证 id_token 验证过程。 |
| 名称 | `name` | `Babe Ruth` | 名称声明提供人类可读值标识标记的主题。 此值不一定是唯一的是可变的并且设计为仅用于显示目的。  `profile`范围需接收此声明。 |
| 电子邮件 | `email` | `thegreatbambino@nyy.onmicrosoft.com` | 如果存在与该用户帐户，主电子邮件地址。  其值是可变的随着时间的推移，给定用户可能会更改。  `email`范围需接收此声明。 |
| 首选的用户名 | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | 用来表示在 v2.0 终结点用户主用户名。  它可能是电子邮件地址、 电话号码或泛型用户名而不指定格式。  其值是可变的随着时间的推移，给定用户可能会更改。  `profile`范围需接收此声明。 |
| 主题 | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | 有关哪些标记断言的信息，例如用户的应用程序的主体。 此值是不可变的并且不能重新分配或重新使用，因此可以使用它来执行授权检查安全，例如，当使用标记来访问资源。 因为主题始终存在于标记的 Azure 的广告问题，我们建议在一般用途授权系统中使用此值。 |
| 对象 Id | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | 在 Azure 的广告系统中的工作或学校帐户 Id 的对象。  Microsoft 的个人帐户不会再发出此声明。  `profile`范围需接收此声明。 |


## <a name="access-tokens"></a>访问令牌

在此时间点才可由 Microsoft 服务颁发的 2.0 版端点的访问令牌。  您的应用程序应该不需要执行任何验证或检查访问令牌的所有当前支持的方案。  为完全不透明，可以将访问令牌-它们都是字符串，您的应用程序可以在 HTTP 请求中传递给 Microsoft。

在不久的将来，2.0 版终结点将引入您的应用程序以接收从其他客户端访问令牌的能力。  届时，将与您的应用程序需要执行访问令牌验证和其他类似的任务的信息更新此信息。

当您请求一个访问令牌从 v2.0 终结点时，v2.0 端点也会返回有关您的应用程序消耗的访问令牌的某些元数据。  此信息包括访问令牌和它的有效范围的到期时间。  这将允许您的应用程序执行而无需打开分析智能缓存访问令牌本身的访问令牌。

## <a name="refresh-tokens"></a>刷新的标记

刷新令牌将您的应用程序可以使用它来获取新的安全性令牌访问令牌 OAuth 2.0 流中的。  它允许您的应用程序以实现代表用户长期对资源的访问权限，而无需用户交互。

刷新令牌是多的资源。  也就是说在标记为一个资源请求期间收到刷新令牌可以兑换到完全不同的资源的访问令牌。

若要接收刷新令牌的响应中，您的应用程序必须请求和授予`offline_acesss`范围。   若要了解有关`offline_access`范围，请参阅[此处的同意和范围文章](active-directory-v2-scopes.md)。

刷新的标记，并将始终保持，为您的应用程序完全不透明。  他们颁发的 Azure AD v2.0 终结点可以只是检查并解释 v2.0 终结点。  它们长期存在，但是您的应用程序不应写入需要刷新令牌将持续为任意时间段内。  可以在任何时刻的原因有多种失效刷新令牌。  您知道刷新令牌是否有效的应用程序的唯一方法是尝试通过令牌请求到 v2.0 终结点将兑换成现金。

当您兑换新的访问令牌的刷新标记 (如果您的应用程序被授予`offline_access`范围内)，您将收到新的刷新令牌，令牌的响应中。  您应保存新颁发的刷新令牌，替换请求中使用。  这将保证您刷新标记保持尽可能长时间有效。

## <a name="validating-tokens"></a>验证标记

在此时间点，仅标记验证您的应用程序需要执行验证 id_tokens。  为了验证 id_token，id_token 的签名和 id_token 中的声明，应验证您的应用程序。

<!-- TODO: Link -->
库和代码示例，演示如何轻松地处理令牌验证-我们提供给那些想要了解基础的过程只需提供以下信息。  还有几个第三方开放源代码库可用于 JWT 验证-几乎每个平台与语言有至少一个选项。

#### <a name="validating-the-signature"></a>验证签名
JWT 包含三个部分，以分隔`.`字符。  第一段被称为**头**，第二个作为**主体**和作为**签名**的第三个。  特征码段可用于验证 id_token 的真实性，以便它可以被您的应用程序信任。

Id_Tokens 使用业界标准的非对称加密算法，例如 RSA 256 色进行了签名。 Id_token 标头包含用于签名令牌的密钥和加密方法的信息︰

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

`alg`声明指示用于签名令牌，而算法`kid`声明表示用于对令牌进行签名的特定公钥。

在任一给定时间点，2.0 版终结点可能对签名使用一组特定的公共 / 专用密钥对中的任何一个 id_token。  V2.0 端点旋转一组可能的密钥定期，因此应该编写您的应用程序来自动处理这些关键的变化。  合理的频率检查到 v2.0 终结点使用的公钥的密钥更新是大约 24 小时。

您可以获取签名验证签名通过使用 OpenID 连接位于元数据文档所需的关键数据︰

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] 尝试在浏览器中的此 URL ！

此元数据文档是信息的一个 JSON 对象，包含许多有用，如执行连接 OpenID 身份验证所需的各种端点的位置。  

它还包括`jwks_uri`，这样的公用密钥用于签名的标记集的位置。  JSON 文档位于`jwks_uri`包含所有在该特定时刻使用公钥信息。  您的应用程序可以使用`kid`JWT 标题选择此文档中的公共密钥已用于签署特定标记中声明。  然后，它可以执行签名验证使用正确的公钥和指定的算法。

执行签名验证已超出了本文档的范围-有很多开放源代码库可用来帮助您执行此操作，如有必要。

#### <a name="validating-the-claims"></a>验证声明
当您的应用程序接收在用户登录时的 id_token 时，它还应在 id_token 执行对索赔的一些检查。  这些包括但不是限于︰

- **观众**声明-验证 id_token 旨在提供给您的应用程序。
- **不前**和**到期时间**索赔-验证 id_token 尚未过期。
- **颁发者**声明-验证令牌确实由 v2.0 终结点颁发给您的应用程序。
- **现时**的作为令牌重放攻击缓解。
- 和更多...

有关声明验证您的应用程序应该执行的完整列表，请参阅[连接 OpenID 技术指标](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation)。

这些索赔的预期值的详细信息包含在[id_token 节](#id_tokens)上面。


## <a name="token-lifetimes"></a>令牌生存期

下面的令牌生存期被提供纯粹的了解，有助于在开发和调试应用程序。  您的应用程序不应写入期望任何这些生存期中保持不变-它们可以并将更改在任何给定点的时间。

| 标记 | 生命周期 | 说明 |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens （工作或学校科目） | 1 小时 | Id_Tokens 的通常有效期均为一小时。  您的 web 应用程序可以使用此相同的生存期与用户 （推荐），维护各自的会话，或选择完全不同的会话生存期。  如果您的应用程序需要获取新的 id_token，它只需要制作一个新登录请求到 2.0 版授权终结点。  如果用户具有有效的浏览器会话使用 2.0 版的终结点时，他们可能不需要重新输入其凭据。 |
| Id_Tokens （个人客户） | 24 小时 | Id_Tokens 对于个人帐户的通常有效期为 24 小时。  您的 web 应用程序可以使用此相同的生存期与用户 （推荐），维护各自的会话，或选择完全不同的会话生存期。  如果您的应用程序需要获取新的 id_token，它只需要制作一个新登录请求到 2.0 版授权终结点。  如果用户具有有效的浏览器会话使用 2.0 版的终结点时，他们可能不需要重新输入其凭据。 |
| 访问令牌 （工作或学校科目） | 1 小时 | 在令牌响应指定标记的元数据的一部分。 |
| 访问令牌 （个人客户） | 1 小时 | 在令牌响应指定标记的元数据的一部分。  代表个人帐户颁发的 Access_tokens 可能会配置不同的寿命，但是一小时通常是这种情况。 |
| 刷新令牌 （工作或学校帐户） | 最多 14 天 | 一个刷新令牌无效，最多 14 天。  但是，使您的应用程序应继续尝试，直到失败，或者您的应用程序替换为新的刷新令牌使用刷新令牌，刷新令牌可能会因多种原因，任何时候无效。  将成为刷新令牌无效，因为用户输入其凭据后 90 天。 |
| 刷新 （个人客户） 的标记 | 最多一年 | 一个刷新令牌无效，最多为 1 年。  但是，使您的应用程序应继续尝试，直到它无法正常使用刷新令牌，刷新令牌可能会因多种原因，任何时候无效。 |
| 授权码 （工作或学校科目） | 10 分钟 | 授权码故意短生存期，并且应立即兑换 access_tokens 和 refresh_tokens 在到达时。 |
| 授权码 （个人客户） | 5 分钟 | 授权码故意短生存期，并且应立即兑换 access_tokens 和 refresh_tokens 在到达时。  发布个人帐户代表的授权码，还有一次性使用。 |
