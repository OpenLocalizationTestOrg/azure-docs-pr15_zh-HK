 <properties
   pageTitle="Azure AD 标记引用 |Microsoft Azure"
   description="为了解和评估的声明中发出的 Azure 活动目录 (AAD) 的 SAML 2.0 和 JSON Web 标记 (JWT) 标记指南"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD 令牌引用

Azure 活动目录 (AD Azure) 发出几种类型的身份验证的每个流的处理过程中的安全令牌。 本文档描述的格式、 安全特征和每种类型的令牌的内容。

## <a name="types-of-tokens"></a>类型的标记

Azure 的广告支持的[OAuth 2.0 授权协议](active-directory-protocols-oauth-code.md)，从而使使用的 access_tokens 和 refresh_tokens。  它还支持身份验证和登录通过[OpenID 连接](active-directory-protocols-openid-connect-code.md)，其中介绍了第三种类型的令牌，id_token。  每个这些标记表示为"持有者标记"。

持有者标记是一个轻量的安全，以授予对受保护资源的"载体"访问权限。 在此意义上，"载体"是任何一方可以提供标记。 尽管使用 Azure AD 身份验证所需接收的持有者标记，必须采取步骤来安全令牌，以防止意外一方截获。 持有者标记没有内置的机制来防止未经授权的方使用它们，因为它们必须传输在传输层安全 (HTTPS) 等安全通道。 如果持有者标记明文传输的一个男士接攻击可获取令牌，并获得对受保护资源的未授权的访问。 存储或缓存的持有者标记以供以后使用时，将应用相同的安全性原则。 始终确保您的应用程序传输和安全的方式存储载体的标记。 更多载体令牌的安全考虑，请参阅[RFC 6750 第 5 节](http://tools.ietf.org/html/rfc6750)。

许多通过 Azure 广告所颁发的令牌实现为 JSON Web 标记或 JWTs。  JWT 是结构紧凑、 URL 安全两个当事方之间传输信息的方法。  JWTs 中包含的信息被称为"声明"或断言的信息载体和标记的主题有关。  JWTs 中的声明的编码和传输序列化 JSON 对象。  因为颁发的 Azure AD JWTs 已经签署，但未加密，可以出于调试目的轻松地检查 JWT 的内容。  有几种工具可用于执行此操作，如[jwt.calebb.net](http://jwt.calebb.net)。 JWTs 的详细信息，您可以参考[JWT 规范](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)。

## <a name="idtokens"></a>Id_tokens

Id_tokens 是一种执行使用[OpenID 连接](active-directory-protocols-openid-connect-code.md)进行身份验证时，您的应用程序接收的登录的安全令牌。  它们表示为[JWTs](#types-of-tokens)，并且包含用于登录到您的应用程序的用户的声明。  根据您的习惯-用于显示帐户信息，或在应用程序中进行访问控制决策通常使用它们，可以使用 id_token 中的索赔。

Id_tokens 是签名，但这一次没有加密。  当您的应用程序收到 id_token 时，则它必须[验证签名](#validating-tokens)以证明真实性的标记并验证令牌中的几个声明以证明其有效性。  由应用程序验证的声明有所不同根据方案的要求，但您的应用程序必须在每个方案中执行一些[常见声明验证](#validating-tokens)。

Id_tokens 索赔以及示例 id_token 请参阅下一节的信息。  请注意，id_tokens 中的声明不会以任何特定的顺序返回。  此外，新的报销申请可以及时引入到任意点处的 id_tokens-您的应用程序不应该中断，因为引入新的报销申请。  下面的列表包含您的应用程序能够可靠地解释在撰写本文时的声明。  如有必要，更多的细节可以找[OpenID 连接规范](http://openid.net/specs/openid-connect-core-1_0.html)中。

#### <a name="sample-idtoken"></a>Id_token 示例

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] 实践中，请尝试通过将它粘贴到[calebb.net](http://jwt.calebb.net)检查示例 id_token 中的声明。

#### <a name="claims-in-idtokens"></a>在 id_tokens 中的索赔

| JWT 索赔 | 名称 | 说明 |
|-----------|------|-------------|
| `appid`| 应用程序 ID | 标识的应用程序正在使用该标记来访问资源。 应用程序可以为本身或代表用户进行操作。 应用程序 ID 通常表示应用程序对象，但它也可以在 Azure AD 表示服务的主要对象。 <br><br> **JWT 值的示例**︰ <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| 访问群体 | 预期的接收方的令牌。 观众值正确，并且拒绝任何面向不同的受众的令牌，必须验证接收标记的应用程序。 <br><br> **SAML 值的示例**︰ <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **JWT 值的示例**︰ <br> `"aud":"https://contoso.com"` |
| `appidacr`| 应用程序身份验证上下文类引用 | 指示客户端已通过身份验证。 公用客户端，值为 0。 如果客户机 ID 和客户端密钥，则值为 1。 <br><br> **JWT 值的示例**︰ <br> `"appidacr": "0"`|
| `acr`| 身份验证上下文类引用 | 指示如何使用者进行了身份验证，而不是客户端应用程序身份验证上下文类引用声明中。 值为"0"表示最终用户身份验证不符合 ISO/IEC 29115 的要求。 <br><br> **JWT 值的示例**︰ <br> `"acr": "0"`|
| | 即时的身份验证 | 记录的日期和时间时进行身份验证。 <br><br> **SAML 值的示例**︰ <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| 身份验证方法 | 标识标记的主题已通过身份验证。 <br><br> **SAML 值的示例**︰ <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **JWT 值的示例**︰`“amr”: ["pwd"]` |
| `given_name`| 第一名 | 提供第一或用户的名称，"给定"Azure AD 用户对象上设置。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `"given_name": "Frank"` |
| `groups`| 组 | 提供代表使用者的组成员身份的对象 Id。 这些值唯一 （请参见对象 ID），并可安全地用于管理访问权限，如强制实施访问资源的授权。 组声明中包含的组被配置在每个应用程序的基础，通过"groupMembershipClaims"属性的应用程序清单上。 空值将排除所有组，只有活动目录安全组成员身份，值为"全部"将包括安全组和 Office 365 通讯组列表将包含的值为"SecurityGroup"。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | 标识提供程序 | 记录标识提供程序进行身份验证的令牌的主题。 除非在颁发者比其他租户的用户帐户，则此值为颁发者声明的值相同。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | 存储颁发的令牌的时间。 它通常用于测量标记常用常新。 <br><br> **SAML 值的示例**︰ <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **JWT 值的示例**︰ <br> `"iat": 1390234181` |
| `iss` | 颁发者 | 标识安全令牌服务 (STS) 的构造和返回的标记。 在 Azure AD 返回的标记，该颁发者是 sts.windows.net。 颁发者声明值中的 GUID 是 Azure 的广告目录的租户 ID。 租户 ID 是不可变的和可靠的目录标识符。 <br><br> **SAML 值的示例**︰ <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **JWT 值的示例**︰ <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | 姓氏 | 在 Azure AD 用户对象中定义提供最后一个名称、 姓氏或系列的用户的名称。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `"family_name": "Miller"` |
| `unique_name`| 名称 | 提供人类可读值标识标记的主题。 此值不能保证一个租户中是唯一的并且设计为仅用于显示目的。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | 对象 ID | 在 Azure AD 中包含对象的唯一的标识符。 此值是不可变的无法重新分配或重新使用。 使用对象 ID 来标识对 Azure AD 查询中的对象。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | 角色 | 表示所有的应用程序角色的主题已被授予包括直接和间接通过组成员资格和可用于强制执行基于角色的访问控制。 应用程序角色定义在每个应用程序的基础上通过`appRoles`属性的应用程序清单。 `value`每个应用程序角色的属性是在角色声明中显示的值。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `“roles”: ["Admin", … ]` |
| `scp` | 作用域 | 指示客户端应用程序授予的模拟权限。 默认的权限是`user_impersonation`。 受保护的资源的所有者可以在 Azure AD 中注册附加值。 <br><br> **JWT 值的示例**︰ <br> `"scp": "user_impersonation"`|
| `sub` |主题| 标识哪些标记断言的信息，如应用程序的用户的主体。 此值是不可变的并且不能重新分配或重新使用，因此可以使用它来安全地执行授权检查。 因为主题始终存在于标记的 Azure 的广告问题，我们建议在一般用途授权系统中使用此值。 <br> `SubjectConfirmation`不是索赔。 它描述了如何验证标记的主题。 `Bearer`指示主题被确认通过其拥有的该标记。 <br><br> **SAML 值的示例**︰ <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **JWT 值的示例**︰ <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | 租户 ID | 不可变、 非可重复使用的标识符，它标识目录组织颁发令牌。 可以使用此值来访问特定于租户的目录在多租户应用程序中的资源。 例如，可以使用此值来识别租户对图形 API 的调用中。 <br><br> **SAML 值的示例**︰ <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **JWT 值的示例**︰ <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|令牌生存期 | 定义内的一个标记是有效的时间间隔。 验证该令牌的服务应验证当前日期位于令牌生存期，其他则应拒绝该标记。 服务可能允许最多五分钟令牌生存期范围之外的时钟时间 （"时间上不一致"） 中的任何差额之间 Azure 广告和服务。 <br><br> **SAML 值的示例**︰ <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **JWT 值的示例**︰ <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| 用户主要名称 | 存储用户的用户主体名称。<br><br> **JWT 值的示例**︰ <br> `"upn": frankm@contoso.com`|
| `ver`| 版本 | 存储的标记的版本编号。 <br><br> **JWT 值的示例**︰ <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>访问令牌

在此时间点才可由 Microsoft 服务的访问令牌。  您的应用程序应该不需要执行任何验证或检查访问令牌的所有当前支持的方案。  为完全不透明，可以将访问令牌-它们都是字符串，您的应用程序可以在 HTTP 请求中传递给 Microsoft。

当您请求一个访问令牌时，Azure 的广告也会返回有关您的应用程序消耗的访问令牌的某些元数据。  此信息包括访问令牌和它的有效范围的到期时间。  这将允许您的应用程序执行而无需打开分析智能缓存访问令牌本身的访问令牌。

## <a name="refresh-tokens"></a>刷新的标记

刷新令牌的安全令牌，您的应用程序可以使用它来获取新的访问令牌 OAuth 2.0 流中。  它允许您的应用程序以实现代表用户长期对资源的访问权限，而无需用户交互。

刷新令牌是多的资源，这意味着可能会接收到请求标记为一个资源，但兑换到完全不同的资源的访问令牌。 若要指定多个资源，请将`resource`为目标的资源请求中的参数。

刷新标记会为您的应用程序完全不透明。 它们长期存在，但是您的应用程序不应写入需要刷新令牌将持续为任意时间段内。  可以在任何时刻的原因有多种失效刷新令牌。  您知道刷新令牌是否有效的应用程序的唯一方法是尝试通过令牌请求到 Azure AD 标记终结点将兑换成现金。

兑换新的访问令牌的刷新标记时，将标记的响应中收到新的刷新令牌。  您应保存新颁发的刷新令牌，替换请求中使用。  这将保证您刷新标记保持尽可能长时间有效。

## <a name="validating-tokens"></a>验证标记

在此时间点，仅标记验证您的客户端应用程序需要执行验证 id_tokens。  为了验证 id_token，id_token 的签名和 id_token 中的声明，应验证您的应用程序。

我们提供库和代码示例，演示如何轻松地处理令牌验证，如果您想要了解基础的过程。  还有几个第三方开放源代码库可用于 JWT 验证，几乎每一种平台和语言有关的至少一个选项。 有关 Azure AD 身份验证库和代码示例的详细信息，请参阅[Azure AD 身份验证库](active-directory-authentication-libraries.md)。

#### <a name="validating-the-signature"></a>验证签名

JWT 包含三个部分，以分隔`.`字符。  第一段被称为**头**，第二个作为**主体**和作为**签名**的第三个。  特征码段可用于验证 id_token 的真实性，以便它可以被您的应用程序信任。

Id_Tokens 使用业界标准的非对称加密算法，例如 RSA 256 色进行了签名。 Id_token 标头包含用于签名令牌的密钥和加密方法的信息︰

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

`alg`声明指示用于签名令牌，而算法`x5t`声明表示用于对令牌进行签名的特定公钥。

在任一给定时间点，Azure AD 可能签署使用一组特定的公共 / 专用密钥对中的任何一个 id_token。 Azure AD 旋转一组可能的密钥定期，因此应该编写您的应用程序来自动处理这些关键的变化。  检查到 Azure 广告所使用的公钥的密钥更新的合理频率是每 24 小时。

您可以获取签名验证签名通过使用 OpenID 连接位于元数据文档所需的关键数据︰

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] 尝试在浏览器中的此 URL ！

此元数据文档是信息的一个 JSON 对象，包含许多有用，如执行连接 OpenID 身份验证所需的各种端点的位置。  

它还包括`jwks_uri`，这样的公用密钥用于签名的标记集的位置。  JSON 文档位于`jwks_uri`包含所有在该特定时刻使用公钥信息。  您的应用程序可以使用`kid`JWT 标题选择此文档中的公共密钥已用于签署特定标记中声明。  然后，它可以执行签名验证使用正确的公钥和指定的算法。

执行签名验证已超出了本文档的范围-有很多开放源代码库可用来帮助您执行此操作，如有必要。

#### <a name="validating-the-claims"></a>验证声明

当您的应用程序接收在用户登录时的 id_token 时，它还应在 id_token 执行对索赔的一些检查。  这些包括但不是限于︰

  - **观众**声明-验证 id_token 旨在提供给您的应用程序。
  - **不前**和**到期时间**索赔-验证 id_token 尚未过期。
  - **颁发者**声明的确认，令牌确实通过 Azure 广告颁发给您的应用程序。
  - **现时**的缓解令牌重放攻击。
  - 和更多...

有关声明验证您的应用程序应该执行的完整列表，请参阅[连接 OpenID 技术指标](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation)。

这些索赔的预期值的详细信息包含在[id_token 一节](#id-tokens)一节所述。

## <a name="sample-tokens"></a>示例标记

此部分显示的 SAML 和 JWT Azure AD 返回的标记的样本。 这些示例使您可以查看上下文中的声明。
SAML 令牌

这是一个典型的 SAML 令牌的一个示例。

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT 令牌的用户模拟

这是一个典型 JSON web 标记 (JWT) 授权代码授予流中使用的示例。
除了索赔，标记**版**和**appidacr**，该值指示客户端已通过身份验证的身份验证上下文类引用中包含版本号。 公用客户端，值为 0。 如果使用的客户端 ID 或者客户端密码，值为 1。

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>相关的内容
- 请参阅 Azure 广告图[策略操作](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations)和[策略实体](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity)，更多地了解管理令牌的生存期策略通过 Azure 广告图形 API。
- 详细信息和示例管理策略通过 PowerShell 的 cmdlet，包括示例，请参阅[Azure AD 中的可配置令牌生存期](active-directory-configurable-token-lifetimes.md)。 
