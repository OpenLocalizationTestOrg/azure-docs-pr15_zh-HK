<properties
    pageTitle="Azure AD v2.0 协议 |Microsoft Azure"
    description="Azure AD v2.0 终结点支持的协议参考。"
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

# <a name="v20-protocols---oauth-20--openid-connect"></a>2.0 版协议的 OAuth 2.0 和 OpenID 连接

V2.0 终结点可用于标识为-服务使用行业标准协议，OpenID 连接和 OAuth 2.0 Azure 的广告。  符合标准的服务时，可能会有这些协议的任意两个实现之间的细微差别。  如果您选择通过直接发送编写代码和处理 HTTP 请求或使用第三方开放源库，而不是使用我们的开源库之一，此处的信息将非常有用。
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

## <a name="the-basics"></a>基础知识
在几乎所有 OAuth 和 OpenID 连接流，有四个当事方参与交换︰

![OAuth 2.0 角色](../media/active-directory-v2-flows/protocols_roles.png)

- **授权服务器**是 v2.0 终结点。  它负责确保该用户的标识、 授予和撤消对资源的访问和颁发令牌。  它也称为身份提供程序-它安全地处理任何与用户的信息，他们的访问和在流中的当事方之间的信任关系。
- **资源所有者**通常是最终用户。  它是当事方拥有此数据，并允许第三方来访问该数据或资源的能力。
- **OAuth 客户**是您的应用程序，由其应用程序 id 标识。  通常是当事方的最终用户进行交互，并从授权服务器请求令牌。  客户端必须通过资源所有者授予资源的访问权。
- **服务器资源**是资源或数据驻留的位置。  它信任授权服务器进行安全身份验证和授权 OAuth 客户端，并使用载体 access_tokens，以确保可以授予对资源的访问。


## <a name="app-registration"></a>应用程序注册
每个应用程序，它使用 v2.0 端点需要交互使用 OAuth 或 OpenID 连接之前在[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)注册。  应用程序注册过程将收集和将几个值分配给您的应用程序︰

- 唯一地标识您的应用程序的**应用程序 Id**
- **重定向 URI**或**包标识符**可以用来直接返回到您的应用程序响应
- 其他几个特定于方案的值。

有关详细信息，了解如何[注册应用程序](active-directory-v2-app-registration.md)。

## <a name="endpoints"></a>终结点
注册后，应用程序可以与进行通讯 Azure 广告通过将请求发送到 v2.0 终结点︰

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

其中`{tenant}`可采用不同的四个值之一︰

| 值 | 说明 |
| ----------------------- | ------------------------------- |
| `common` | 从 Azure Active Directory 以登录到该应用程序允许使用 Microsoft 的个人帐户和工作/学校帐户的用户。 |
| `organizations` | 仅允许用户与工作/学校科目从 Azure Active Directory 以登录到该应用程序。 |
| `consumers` | 仅允许用户使用个人 Microsoft 帐户 (MSA) 登录到该应用程序。 |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`或`contoso.onmicrosoft.com` | 仅允许用户与工作/学校帐户从一个特定的 Azure Active Directory 租户，登录到应用程序。  可以使用 Azure AD 租户好记的域名，或者租户的 guid 标识符。  |

如何与这些终结点进行交互的详细信息，请选择下面的特定应用程序类型。

## <a name="tokens"></a>标记
V2.0 实现 OAuth 2.0 和 OpenID 连接使持有者标记，包括持有者标记表示为 JWTs 的广泛使用。 持有者标记是一个轻量的安全，以授予对受保护资源的"载体"访问权限。 在此意义上，"载体"是任何一方可以提供标记。 尽管当事方必须首先进行身份验证使用 Azure 广告接收的持有者标记，如果没有采取所需的步骤来保护在传输和存储的标记，它可以截获并由意外的当事方。 某些安全令牌有阻止未经授权的方使用其内置的机制，而持有者标记并不一定这一机制必须在传输层安全 (HTTPS) 等安全通道传输。 如果持有者标记明文传输的手册中攻击可通过恶意方获取令牌并将其用于对受保护资源的未经授权的访问。 存储或缓存的持有者标记以供以后使用时，将应用相同的安全性原则。 始终确保您的应用程序传输和安全的方式存储载体的标记。 更多载体令牌的安全考虑，请参阅[RFC 6750 第 5 节](http://tools.ietf.org/html/rfc6750)。

不同类型的令牌使用 v2.0 终结点中的更多详细信息将出现在[v2.0 端点令牌引用](active-directory-v2-tokens.md)。

## <a name="protocols"></a>协议

如果您已经准备好要查看一些示例请求，开始使用一种下面教程。  每个对应于特定的身份验证方案。  如果您需要帮助确定哪个连接正确的数据流，签出[您可以构建使用 2.0 版的应用程序的类型](active-directory-v2-flows.md)。

- [建立移动和使用 OAuth 2.0 的本机应用程序](active-directory-v2-protocols-oauth-code.md)
- [生成 Web 应用程序 id 为打开连接](active-directory-v2-protocols-oidc.md)
- [构建使用 OAuth 2.0 隐式流的单页应用程序](active-directory-v2-protocols-implicit.md)
- [生成守护程序或数据 OAuth 2.0 客户端凭据与服务器端进程流](active-directory-v2-protocols-oauth-client-creds.md)
- 获取标记在 Web API 中具有 OAuth 2.0 代表流 （即将提供）

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
