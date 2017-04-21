<properties
    pageTitle="Azure 的 Active Directory B2C |Microsoft Azure"
    description="如何构建应用程序直接通过使用 Azure 活动目录 B2C 支持的协议。"
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

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C︰ 身份验证协议

Azure 的活动目录 (AD Azure) B2C 提供标识作为您的应用程序的服务支持两种行业标准协议︰ OpenID 连接和 OAuth 2.0。 服务符合标准，但这些协议的任意两个实现可能有细微的差别。  如果您编写代码，通过直接发送和处理 HTTP 请求，而不是通过使用一个开放源码库，本指南中的信息将对您有用。 我们建议您研究一下每个特定协议的细节之前，阅读此页。 但是，如果您已经熟悉 Azure AD B2C，可以直接到[协议参考指南](#protocols)。

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>基础知识
每个应用程序使用 Azure AD B2C 需要在 B2C 在[Azure 的门户网站](https://portal.azure.com)目录中注册。 应用程序注册过程收集和将几个值分配给您的应用程序︰

- 唯一地标识您的应用程序**的应用程序 ID** 。
- **重定向 URI**或**包标识符**可以用来直接返回到您的应用程序响应。
- 其他几个特定于方案的值。 有关详细信息，了解[如何注册您的应用程序](active-directory-b2c-app-registration.md)。

注册您的应用程序后，它与通信 Azure 广告通过将请求发送到 v2.0 终结点︰

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

几乎所有 OAuth，OpenID 连接流，在四个当事方都将参与交换︰

![OAuth 2.0 角色](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- **授权服务器**是 Azure AD v2.0 终结点。 它安全地处理与用户信息和访问相关的任何内容。 它还可以处理流程中的当事方之间的信任关系。 它是负责验证用户的身份、 授予和撤消对资源的访问并颁发令牌。 它也称为身份提供程序。
- **资源所有者**通常是最终用户。 它是当事方所拥有的数据，并具有允许第三方访问该数据或资源的能力。
- **OAuth 客户**机应用程序。 它标识的应用程序 id。 它通常是当事方最终用户与之交互。 它还请求来自授权服务器的令牌。 资源所有者必须授予访问资源的客户端权限。
- **服务器资源**是资源或数据驻留的位置。 它信任授权服务器进行安全身份验证和授权 OAuth 客户端。 它还使用载体的访问令牌来确保可以授予对资源的访问。

## <a name="policies"></a>策略
可以说，Azure AD B2C 策略是服务的最重要功能。 Azure AD B2C 通过引入策略扩展标准的 OAuth 2.0 和 OpenID 连接协议。 这些允许 Azure AD B2C 执行不仅仅简单身份验证和授权。 策略充分描述使用者身份体验，包括注册、 登录和编辑配置文件。 在管理用户界面中，可以定义策略。 他们可以执行的 HTTP 身份验证请求中使用的特殊的查询参数。 策略不是标准功能的 OAuth 2.0 和 OpenID 连接，因此您应该花时间来理解它们。 有关详细信息，请参阅[Azure AD B2C 策略参考指南](active-directory-b2c-reference-policies.md)。

## <a name="tokens"></a>标记
Azure AD B2C 实现 OAuth 2.0 和 OpenID 连接使持有者标记，包括持有者标记表示为 JSON web 标记 (JWTs) 的广泛使用。 持有者标记是一个轻量的安全，以授予对受保护资源的"载体"访问权限。 载体是任意一方都可以显示该标记。 Azure 的广告必须首先验证身份一方，它可以接收的持有者标记之前。 但是，如果没有采取所需的步骤来保护在传输和存储的标记，它可以截获和意外的当事方使用。

某些安全令牌具有内置的机制，可防止未经授权的方使用它们，但持有者标记并没有这种机制。 他们必须在安全通道中，如传输层安全 (HTTPS) 传输。 如果持有者令牌传输安全通道之外，恶意方可以使用拦截的攻击以获取令牌并使用它来获取对受保护资源的未授权的访问。 载体令牌存储或缓存以备以后使用时，将应用相同的安全性原则。 始终确保您的应用程序传输和安全的方式存储载体的标记。

其他载体令牌的安全注意事项，请参阅[RFC 6750 第 5 节](http://tools.ietf.org/html/rfc6750)。

在 Azure AD B2C 中使用的标记的不同类型的更多详细信息位于[Azure AD 令牌引用](active-directory-b2c-reference-tokens.md)。

## <a name="protocols"></a>协议

时您就可以查看一些示例请求，您可以开始下面的教程之一。 每个对应于特定的身份验证方案。 如果您需要的帮助确定最适合您的流量，检出[可以通过使用 Azure AD B2C 生成应用程序的类型](active-directory-b2c-apps.md)。

- [通过使用 OAuth 2.0 构建移动和本机应用程序](active-directory-b2c-reference-oauth-code.md)
- [通过使用 OpenID 连接构建 web 应用程序](active-directory-b2c-reference-oidc.md)
