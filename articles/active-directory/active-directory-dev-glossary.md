<properties
   pageTitle="Azure 的 Active Directory 开发术语表 |Microsoft Azure"
   description="常用的 Azure Active Directory 开发概念和功能的术语的列表。"
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory 开发术语表
这篇文章包含一些非常有用的在学习 Azure AD 的应用程序开发时的核心 Azure 活动目录 (AD) 开发概念的定义。

## <a name="access-token"></a>访问令牌
一种类型的[安全令牌](#security-token)颁发的[授权服务器，](#authorization-server)并使用[客户端应用程序](#client-application)才能访问[受保护资源服务器](#resource-server)。 通常在窗体中的[JSON Web 标记 (JWT)][JWT]，标记包含授权向客户端授予[资源所有者](#resource-owner)，对请求的访问级别。 标记包含所有适用[索赔](#claim)有关的主题，使客户端应用程序访问给定的资源时，则会将其用作一种形式的凭据。 这还消除了公开给客户端的凭据的资源所有者的需要。

访问令牌是有时称为"用户 + 应用程序"或"应用程序-仅用于"，具体取决于所表示的凭据。 例如，当客户端应用程序使用:

- 最终用户["授权码"授权授予](#authorization-grant)，首次为委派到客户端的授权访问该资源的资源所有者验证。 在获取访问令牌时，客户端的验证以后。 令牌可以有时被称为更具体地说就是"用户 + App"表示感激，因为它表示这两个用户经过授权的客户端应用程序和应用程序。
- ["客户端凭据"授权授予](#authorization-grant)，客户端提供唯一的身份验证，以便标记可以有时被称为一个"仅应用程序"的标记而资源所有者的身份验证/授权，不正常工作。

请参阅[Azure AD 令牌引用][AAD-Tokens-Claims]了解详情。

## <a name="application-manifest"></a>应用程序清单  
[Azure 的传统门户网站]提供一个功能[AZURE-classic-portal]，其产生的 JSON 表示法作为一种机制，用于更新其关联的[应用程序]所使用的应用程序的标识配置[AAD-Graph-App-Entity]和[ServicePrincipal] [AAD-Graph-Sp-Entity]的实体。 请参阅[了解 Azure 活动目录的应用程序清单][AAD-App-Manifest]了解详情。

## <a name="application-object"></a>应用程序对象  
当您注册/更新在[Azure 的传统门户网站]应用程序[AZURE-classic-portal]，门户创建/更新应用程序对象和相应[服务的主要对象](#service-principal-object)为该租户。 应用程序对象*定义*应用程序的标识配置全局 （跨所有租户在其有权），提供从其及其相应的服务主体对象是用于在运行时 （在一个特定租户） 本地*派生*的模板。

请参见[应用程序和服务主体对象][AAD-App-SP-Objects]的详细信息。

## <a name="application-registration"></a>应用程序注册  
若要允许应用程序相结合，并委派到 Azure AD 身份和访问管理功能，它必须向注册 Azure AD[租户](#tenant)。 Azure 广告与注册应用程序时，您提供标识配置为您的应用程序，从而使其可以与 Azure AD 集成和使用功能，如︰

- 强大的单一登录使用 Azure AD 身份管理和[OpenID 连接]管理[OpenIDConnect]协议实现
- 具有的访问[受保护资源](#resource-server)的[客户端应用程序](#client-application)，通过 Azure AD OAuth 2.0[授权服务器](#authorization-server)实现
- 用于管理客户端访问受保护资源，基于资源所有者授权的[同意框架](#consent)。

请参阅[使用 Azure Active Directory 集成应用程序][AAD-Integrating-Apps]了解详情。

## <a name="authentication"></a>身份验证
行为的具有挑战性的当事方的合法凭据，用于标识和访问控制的安全主体的创建提供依据。 在[OAuth2 授权授予](#authorization-grant)过程为例，当事方进行身份验证填充[资源所有者](#resource-owner)或[客户端应用程序](#client-application)，具体取决于使用的授予的角色。

## <a name="authorization"></a>授权
该法案授予经过身份验证的安全主体权限进行操作。 在 Azure AD 编程模型中有两个主要的用例︰

- 在[OAuth2 授权授予](#authorization-grant)流过程︰ 当[资源所有者](#resource-owner)授予对[客户端应用程序](#client-application)的授权，允许客户端访问资源所有者的资源。
- 在客户端访问资源的过程中︰ 为实现的[资源服务器](#resource-server)，使用[声明](#claim)值出现使访问控制决策取决于其[访问令牌](#access-token)中。

## <a name="authorization-code"></a>授权码
提供给[客户端应用程序](#client-application)通过[授权的终结点](#authorization-endpoint)，作为"授权码"流的一部分之一四个 OAuth2[授权授予](#authorization-grant)，短惊叹"令牌"。 代码返回到客户端应用程序中的一个[资源所有者](#resource-owner)，指示资源所有者已经委托授权访问所请求的资源的身份验证响应。 作为流程的一部分，对于一个[访问令牌](#access-token)以后兑换代码。

## <a name="authorization-endpoint"></a>授权的终结点
其中一个端点执行[授权服务器](#authorization-server)，用于与[资源所有者](#resource-owner)进行交互以 OAuth2 授权期间提供[授权授予](#authorization-grant)授予流。 这取决于使用的授权授予流动，提供实际授予各异，包括[授权代码](#authorization-code)或[安全令牌](#security-token)。

OAuth2 规范的[授权类型的授权]，请参阅[OAuth2-AuthZ-Grant-Types]和[授权终结点][OAuth2-AuthZ-Endpoint]节和[OpenIDConnect 规范][OpenIDConnect-AuthZ-Endpoint]了解详情。

## <a name="authorization-grant"></a>授权授予
一个表示[资源所有者的](#resource-owner)[授权](#authorization)才能访问其授予[客户端应用程序](#client-application)的受保护的资源的凭据。 客户端应用程序可以使用[由 OAuth2 授权框架定义的四个授权类型]之一[OAuth2-AuthZ-Grant-Types]以获得授权，具体取决于客户端的类型要求:"授权代码授予"、"客户端凭据授予"、"隐式授予"、 和"资源所有者密码凭据授予"。 返回到客户端的凭据是一个[访问令牌](#access-token)或[授权码](#authorization-code)（兑换以后访问令牌），具体取决于使用的授权授予的类型。

## <a name="authorization-server"></a>授权服务器
由[OAuth2 授权框架][OAuth2-Role-Def]，负责签发访问的服务器到[客户端](#client-application)令牌之后成功地进行[资源所有者](#resource-owner)身份验证并获得其授权。 [客户端应用程序](#client-application)在运行时通过其[授权](#authorization-endpoint)的授权服务器与交互并根据 OAuth2[令牌](#token-endpoint)的终结点定义[授权授予](#authorization-grant)。

在 Azure 广告应用程序集成，Azure 广告实现 AD Azure 应用程序和 Microsoft 服务的 Api，例如[Microsoft Graph Api]的授权服务器角色[Microsoft-Graph]。

## <a name="claim"></a>报销申请
[安全令牌](#security-token)包含到另一个实体 （例如[资源服务器](#resource-server)） 索赔，提供有关某个实体 （如[客户端应用程序](#client-application)或[资源所有者](#resource-owner)） 的断言。 声明是名称/值对的中继标记的主题 （例如，已[授权服务器](#authorization-server)通过身份验证的安全主体） 有关的事实。 一个给定的令牌中存在这些声明是标记的凭据的取决于几个变量，其中包括用于验证使用者应用程序配置、 等类型的类型。

请参阅[Azure AD 令牌引用][AAD-Tokens-Claims]了解详情。

## <a name="client-application"></a>客户端应用程序  
由[OAuth2 授权框架][OAuth2-Role-Def]，应用程序，使受保护的[资源所有者](#resource-owner)代表的资源请求。 术语"客户端"并不意味着任何特定的硬件实现特征 （例如，是否执行应用程序在服务器、 桌面或其他设备）。  

客户端应用程序请求[授权](#authorization)从一个资源所有者参与[OAuth2 授权授予](#authorization-grant)流，并可能访问 Api 中的数据资源所有者的代表。 [定义两种类型的客户端]授权框架 OAuth2[OAuth2-Client-Types]、"机密"和"公用"、 基于客户端的凭据的保密能力。 应用程序可以实现[web 客户端 （机密）](#web-client)运行在 web 服务器上，安装在设备上或[基于用户代理客户端 （公共）](#user-agent-based-client)设备的浏览器中运行[的本机客户端 （公共）](#native-client) 。

## <a name="consent"></a>同意
[资源所有者](#resource-owner)授予授权给[客户端应用程序](#client-application)访问受保护的资源时，资源所有者代表的特定[权限](#permissions)的过程。 根据客户端请求的权限，管理员或用户将被要求同意分别允许访问他们的组织/个人数据。 注意，在[多租户](#multi-tenant-application)的情况下，应用程序的[服务主体](#service-principal-object)也记录在 consenting 用户的租户。

## <a name="id-token"></a>ID 标记
[OpenID 连接][OpenIDConnect-ID-Token]提供[授权服务器](#authorization-server)[授权的终结点](#authorization-endpoint)，其中包含[索赔](#claim)属于最终用户[资源所有者](#resource-owner)的身份验证[安全令牌](#security-token)。 像一个访问令牌，令牌 ID 也表示为数字签名[JSON Web 标记 (JWT)][JWT]。 与不同的访问令牌，ID 标记声明不用于相关的资源访问的目的和专门访问控制。

请参阅[Azure AD 令牌引用][AAD-Tokens-Claims]了解详情。

## <a name="multi-tenant-application"></a>多租户应用程序
一类的[客户端应用](#client-application)程序，登录，并由用户[同意](#consent)任何 Azure AD[租户](#tenant)，包括承租人以外地方注册的客户端中提供。 与此相反，应用程序注册为单个的租户，将仅允许从与同一组织中设置的用户帐户登录注册应用程序的位置。 [本机客户端](#native-client)应用程序是默认情况下，多租户，而[web 客户端](#web-client)应用程序具有单和多租户之间进行选择的能力。

了解[如何使用多租户应用程序模式的任何 Azure AD 用户提供登录][AAD-Multi-Tenant-Overview]了解详情。

## <a name="native-client"></a>本机客户端
一种[客户端应用程序](#client-application)在设备上本机安装程序类型。 由于在设备上执行的所有代码时，它被认为是由于它无法存储凭据私人/保密"公用"客户端。 [OAuth2 客户端类型和配置文件]，请参阅[OAuth2-Client-Types]了解详情。

## <a name="permissions"></a>权限
[客户端应用程序](#client-application)通过声明权限请求获得对[服务器资源](#resource-server)的访问。 有两种类型︰

- "委派"权限，请求下[基于作用域的](#scopes)访问权限委托签名中[资源所有者](#resource-owner)的授权，在运行时的资源显示为["scp"索赔](#claim)的客户端[访问令牌](#access-token)中。
- "应用程序"权限，请求客户端应用程序的凭据标识下[基于角色的](#roles)访问，被交给在运行时的资源作为客户端的访问令牌中所[声称"的角色"](#claim) 。

他们还曲面过程中[同意](#consent)，给予管理员或资源所有者授予/拒绝对其租户中资源的客户端访问的机会。

在"应用程序"上配置权限请求 / [Azure 的传统门户]的"配置"选项卡上[AZURE-classic-portal]、 （后者需要全局管理员角色中的成员资格）"权限对其他应用程序"，选择所需"授予权限"和"应用程序权限"下。 由于[公共客户端](#client-application)不能维护的凭据，它才可以请求委派的权限，而[机密的客户端](#client-application)的请求委派和应用程序的权限的能力。 客户端的[应用程序对象](#application-object)将声明的权限存储在它的[requiredResourceAccess 属性][AAD-Graph-App-Entity]。

## <a name="resource-owner"></a>资源所有者
由[OAuth2 授权框架][OAuth2-Role-Def]，能够授予对受保护资源的访问权限的实体。 当资源所有者是一个人时，它被称为最终用户。 例如，当一个[客户端应用程序](#client-application)需要通过[Microsoft Graph API]访问用户的邮箱[Microsoft-Graph]，它需要从资源所有者的邮箱的权限。

## <a name="resource-server"></a>服务器资源
由[OAuth2 授权框架][OAuth2-Role-Def]，承载受保护的资源，能够接受和响应服务器保护资源请求由[客户端应用程序](#client-application)提供[访问令牌](#access-token)。 也称为受保护的资源的服务器或应用程序资源。

资源服务器公开的 Api，并强制对[范围](#scopes)和[角色](#roles)，使用 OAuth 2.0 授权框架通过其受保护资源的访问。 示例包括 Azure 广告图形 API 提供对 Azure AD 租户的数据，这和 Office 365 Api 提供对邮件和日历等数据的访问。 这两种也可通过[Microsoft Graph API]访问[Microsoft-Graph]。  

客户端应用程序一样，通过[注册](#application-registration)中提供的应用程序和服务主体对象 Azure AD 租户建立资源应用程序的标识配置。 某些 Microsoft 提供的 Api，如 Azure 广告图形 API，预先注册服务主体资源调配过程中所做所有租户中可用。

## <a name="roles"></a>角色
[作用域](#scopes)，如角色提供[资源服务器](#resource-server)来控制对受保护资源的访问方法。 有两种类型:"用户"角色实现基于角色的访问控制需要访问的资源，而"应用程序"角色实现相同的[客户端应用](#client-application)程序需要访问的用户/组。

角色是资源定义的字符串 (例如"支出审核人"，"只读"、"Directory.ReadWrite.All")、 托管在[Azure 的传统门户网站][AZURE-classic-portal]通过资源的[应用程序清单](#application-manifest)，并存储在资源的[appRoles 属性][AAD-Graph-Sp-Entity]。 Azure 的传统门户网站还用于将用户分配到"用户"角色并配置客户端[应用程序的权限](#permissions)，若要访问"应用程序"角色。

公开的 Azure 广告图形 API 的应用程序角色的详细讨论，请参阅[图形 API 权限范围][AAD-Graph-Perm-Scopes]。 一个逐步实现的示例，请参阅[基于角色的访问控制在云应用程序中使用 Azure AD][Duyshant-Role-Blog]。

## <a name="scopes"></a>作用域
一样[的角色](#roles)，作用域提供[资源服务器](#resource-server)来控制对受保护资源的访问方法。 作用域用来实现[基于作用域的][OAuth2-Access-Token-Scopes]访问控制，有权委派的访问到资源由其所有者的[客户端应用程序](#client-application)。

作用域是资源定义的字符串 （例如"Mail.Read"、"Directory.ReadWrite.All"），托管在[Azure 的传统门户网站][AZURE-classic-portal]通过资源的[应用程序清单](#application-manifest)，并存储在资源的[oauth2Permissions 属性][AAD-Graph-Sp-Entity]。 Azure 的传统门户网站还可用于配置客户端应用程序[委托权限](#permissions)以访问作用域。

最佳做法命名约定，则使用"resource.operation.constraint"的格式。 公开的 Azure 广告图形 API 的作用域的详细讨论，请参阅[图形 API 权限范围][AAD-Graph-Perm-Scopes]。 有关 Office 365 提供服务所公开的范围，请参阅[Office 365 API 权限参考][O365-Perm-Ref]。

## <a name="security-token"></a>安全令牌
已签名的文档，包含索赔，如 OAuth2 令牌或 SAML 2.0 断言。 为[授权授予](#authorization-grant)OAuth2，[访问令牌](#access-token)(OAuth2) 和[ID 令牌](OpenID Connect)类型的安全令牌，这两种实现[JSON Web 标记 (JWT)]为[JWT]。

## <a name="service-principal-object"></a>服务主体对象
当您注册/更新在[Azure 的传统门户网站]应用程序[AZURE-classic-portal]，门户创建/更新[应用程序对象](#application-object)和相应的服务主体对象的租户。 应用程序对象*定义*应用程序的标识配置全局 （跨所有承租人在关联的应用程序已被授予访问），并且从中及其相应的服务的主要对象是用于在运行时 （在一个特定租户） 本地*派生*模板。

请参见[应用程序和服务主体对象][AAD-App-SP-Objects]的详细信息。

## <a name="sign-in"></a>登录
[客户端应用程序](#client-application)启动最终用户身份验证，并将捕获的过程相关的状态，目的是获取一个[安全令牌](#security-token)和范围到该状态的应用程序会话。 声明可以包括的项目，如用户配置文件信息，并从令牌声明派生的信息。

应用程序的登录功能通常用于实现单点登录 (SSO)。 它可能还跟在"注册"功能，为最终用户获得访问权限 （在第一个登录） 的应用程序的入口点。 注册的函数用于收集并保持其他状态特定于用户，并且可能需要[用户同意](#consent)。

## <a name="sign-out"></a>注销
未经身份验证的过程与[客户端应用程序](#client-application)会话期间[登录](#sign-in)相关的最终用户，分离的用户状态

## <a name="tenant"></a>租户
Azure 的广告目录的实例称为 Azure AD 租户。 它提供各种功能，包括︰

- 集成的应用程序注册表服务
- 身份验证的用户帐户和已注册应用程序
- 其余部分需要用来支持各种协议包括 OAuth2 和 SAML，包括[授权终结点](#authorization-endpoint)、[标记](#token-endpoint)与[多租户应用程序](#multi-tenant-application)使用的"普通"的终结点的终结点。

租户也是与 Azure 广告或 Office 365 订阅的订阅，订阅提供身份和访问管理功能的资源调配。 请参阅[如何获取 Azure Active Directory 租户][AAD-How-To-Tenant]有关的各种方法的详细信息可以向租户获得访问权限。 请参阅[如何 Azure 订阅将与 Azure Active Directory] [AAD-How-Subscriptions-Assoc]有关订阅和 Azure AD 租户之间的关系的详细信息。

## <a name="token-endpoint"></a>令牌的终结点
[授权服务器](#authorization-server)以支持 OAuth2[授权授予](#authorization-grant)由实现的终结点之一。 根据授权，它可以使用来获得[访问令牌](#access-token)（和相关"的刷新"标记） 到[客户端](#client-application)或[ID 令牌](#ID-token)使用[OpenID 连接][OpenIDConnect]协议。

## <a name="user-agent-based-client"></a>基于用户代理的客户端
一种[客户端应用](#client-application)程序从 web 服务器下载代码，并执行在用户代理 （例如，web 浏览器），如单页面应用程序 (SPA)。 由于在设备上执行的所有代码时，它被认为是由于它无法存储凭据私人/保密"公用"客户端。 [OAuth2 客户端类型和配置文件]，请参阅[OAuth2-Client-Types]了解详情。

## <a name="user-principal"></a>用户主体
类似于一个服务主体对象用于表示一个应用程序实例的方法，用户主体对象是安全的另一种类型主体，它代表用户。 Azure 广告图形[用户实体][AAD-Graph-User-Entity]定义的架构的用户对象，包括与用户相关的属性，如第一个和最后一个名称、 用户主体名称、 目录角色成员身份等。这提供了 Azure 的广告来建立用户主体在运行时的用户标识配置。 用户主要用来为单一登录，录制[同意](#consent)委派，为了进行访问控制决策等表示身份验证的用户。

## <a name="web-client"></a>web 客户端
一种[客户端应用程序](#client-application)执行的所有代码在 web 服务器上，并且可以作为"机密"的客户端通过服务器上安全地存储它的凭据类型。 [OAuth2 客户端类型和配置文件]，请参阅[OAuth2-Client-Types]了解详情。

## <a name="next-steps"></a>下一步行动
[Azure 广告开发人员指南 》] [AAD-Dev-Guide]是用于所有 Azure 广告开发的门户相关主题，包括[应用程序集成]概述[AAD-How-To-Integrate] [Azure AD 身份验证和支持的身份验证方案]的基本知识和[AAD-Auth-Scenarios]。

请使用以下 Disqus 评论部分提供反馈，帮助我们改进和形状我们的内容。

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
