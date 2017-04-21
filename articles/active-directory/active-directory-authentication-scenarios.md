
<properties
   pageTitle="Azure AD 身份验证方案 |Microsoft Azure"
   description="五种最常见的身份验证方案的 Azure 活动目录 (AAD) 的概述"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Azure AD 身份验证方案

Azure 活动目录 (AD Azure) 作为服务，支持业界标准协议，如 OAuth 2.0 和 OpenID 连接，以及开放源代码库，用于不同的平台，以帮助您快速编写代码来提供标识，从而简化了开发人员的身份验证。 本文档将帮助您了解各种方案 Azure 的广告支持，并将向您展示如何开始。 它分为以下部分︰

- [在 Azure AD 身份验证基础知识](#basics-of-authentication-in-azure-ad)

- [在 Azure AD 安全令牌中的声明](#claims-in-azure-ad-security-tokens)

- [在 Azure AD 中注册应用程序的基础知识](#basics-of-registering-an-application-in-azure-ad)

- [应用程序类型和方案](#application-types-and-scenarios)

  - [为 Web 应用程序的 web 浏览器](#web-browser-to-web-application)

  - [单页应用程序 (SPA)](#single-page-application-spa)

  - [本机应用程序与 Web API](#native-application-to-web-api)

  - [Web 应用程序与 Web API](#web-application-to-web-api)

  - [守护程序或服务器应用程序与 Web API](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>在 Azure AD 身份验证基础知识

如果您熟悉在 Azure AD 身份验证的基本概念，请阅读本节。 否则，您可以跳到[应用程序类型和方案](#application-types-and-scenarios)。

让我们来看其中标识是所需的最基本方案︰ 在 web 浏览器中的用户需要向 web 应用程序进行身份验证。 在[Web 浏览器的 Web 应用程序](#web-browser-to-web-application)部分中，更详细地介绍了这种情况下，但它是来演示 Azure 广告的能力，并形成原理方案的概念的有用起点。 请考虑下面的关系图对于此方案︰

![登录到 web 应用程序的概述](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

记住上面图，下面是您需要了解其各个组件︰

- Azure 的广告都是身份标识提供程序，负责验证身份的用户和应用程序存在于组织的目录并最终颁发安全令牌的那些用户和应用程序的身份验证成功时。


- 想要外包到 Azure AD 身份验证的应用程序必须在 Azure 的广告，它注册并唯一地标识应用程序的目录中进行注册。


- 开发人员可以使用开源 Azure AD 身份验证库进行身份验证容易处理协议的详细信息。 [Azure 活动目录身份验证库](active-directory-authentication-libraries.md)的详细信息，请参见


• 用户已经过身份验证之后，应用程序必须验证该用户的安全令牌，以确保预期的当事方的身份验证成功。 开发人员可以使用所提供的身份验证库处理从 Azure 的广告，包括 JSON Web 标记 (JWT) 或 SAML 2.0 任何标记的验证。 如果要手动执行验证，请参阅[JWT 标记处理程序](https://msdn.microsoft.com/library/dn205065.aspx)文档。


> [AZURE.IMPORTANT] Azure AD 使用公钥加密签名令牌并验证它们有效。 请参阅[重要信息关于签名密钥变换图像在 Azure 的广告](active-directory-signing-key-rollover.md)必须在您的应用程序，以确保它有必要的逻辑的详细信息为始终更新使用新的密钥。


• 由 OAuth 2.0，OpenID 连接，如所使用的身份验证协议来确定请求和身份验证过程的响应流 WS 联合身份验证或者 SAML 2.0。 在[Azure 活动目录的身份验证协议](active-directory-authentication-protocols.md)的主题以及在以下各节将详细讨论这些协议。

> [AZURE.NOTE] Azure 的广告支持 OAuth 2.0，OpenID 连接标准，使得大量使用的持有者标记，包括持有者标记表示为 JWTs。 持有者标记是一个轻量的安全，以授予对受保护资源的"载体"访问权限。 在此意义上，"载体"是任何一方可以提供标记。 尽管当事方必须首先进行身份验证使用 Azure 广告接收的持有者标记，如果没有采取所需的步骤来保护在传输和存储的标记，它可以截获并由意外的当事方。 某些安全令牌有阻止未经授权的方使用其内置的机制，而持有者标记并不一定这一机制必须在传输层安全 (HTTPS) 等安全通道传输。 如果持有者标记明文传输的手册中攻击可通过恶意方获取令牌并将其用于对受保护资源的未经授权的访问。 存储或缓存的持有者标记以供以后使用时，将应用相同的安全性原则。 始终确保您的应用程序传输和安全的方式存储载体的标记。 更多载体令牌的安全考虑，请参阅[RFC 6750 第 5 节](http://tools.ietf.org/html/rfc6750)。


既然有基础知识的概述，阅读理解资源调配的工作原理在 Azure 的广告中，以下各节，常用到 Azure 的广告支持。


## <a name="claims-in-azure-ad-security-tokens"></a>在 Azure AD 安全令牌中的声明

通过 Azure 广告所颁发的安全令牌包含声明或断言已经过身份验证的主题有关的信息。 各种任务，应用程序可以使用这些声明。 例如，它们可用来验证该令牌，识别使用者的目录租户、 显示用户信息、 确定主题的授权等等。 任何给定的安全令牌中的声明所依赖的凭据进行身份验证的用户和应用程序配置类型的标记的类型。 下表中提供了每种类型的索赔发射的 Azure AD 的简短说明。 有关详细信息，请参阅[支持令牌和声明类型](active-directory-token-and-claims.md)。


| 报销申请 | 说明 |
|-------|-------------|
| 应用程序 ID | 标识的应用程序正在使用该标记。
| 访问群体 | 标识标记用于收件人资源。 |
| 应用程序身份验证上下文类引用 | 指示客户端的身份验证 （公共客户端与机密客户端） 的方式。 |
| 即时的身份验证 | 记录的日期和时间时进行身份验证。 |
| 身份验证方法 | 指示该标记的主题已通过身份验证 （密码、 证书等）。 |
| 第一名 | 在 Azure AD 设置提供给定的用户的名称。 |
| 组 | 包含对象 Id Azure AD 用户组的成员。 |
| 标识提供程序 | 记录标识提供程序进行身份验证的令牌的主题。 |
| 在颁发 | 记录的颁发令牌，通常用于标记的新鲜的时间。 |
| 颁发者 | 标识发出的令牌，以及 Azure AD 租户 STS。 |
| 姓氏 | 在 Azure AD 中为组提供用户的姓。 |
| 名称 | 提供人类可读值标识标记的主题。 |
| 对象 Id | 在 Azure AD 中包含主题的不可变的唯一标识符。 |
| 角色 | 包含已被授予用户的 Azure AD 应用程序角色的友好名称。 |
| 作用域 | 指示客户端应用程序授予的权限。 |
| 主题 | 指示哪些标记断言信息的主体。 |
| 租户 Id | 包含的目录组织颁发令牌的不可变、 唯一的标识符。 |
| 令牌生存期 | 定义内的一个标记是有效的时间间隔。 |
| 用户主要名称 | 包含主题的用户主体的名称。 |
| 版本 | 包含该标记的版本编号。 |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>在 Azure AD 中注册应用程序的基础知识

Outsources 对 Azure AD 身份验证的任何应用程序必须在目录中进行注册。 此步骤涉及到有关您的应用程序，包括有位置，将答复发送身份验证之后，URI 来标识您的应用程序，以及其他的 URL 的 URL 告诉 Azure 的广告。 此信息是必需的几个主要原因︰

- Azure 广告需要坐标来处理登录或交换令牌时与应用程序进行通信。 其中包括以下︰

  - 应用程序 ID 的 URI: 应用程序的标识符。 此值发送到 Azure AD 身份验证期间，指示哪个应用程序调用者的标记。 此外，此值包含在标记，以便应用程序知道它的预定的目标。


  - 回复 URL 和重定向 URI: web API 或 web 应用程序中，回复 URL 是 Azure 广告将向其发送身份验证响应，如果身份验证成功，其中包括一个标记的位置。 对于本机应用程序，重定向 URI 是到 Azure 的广告将重定向 OAuth 2.0 请求中的用户代理的唯一标识符。


  - 客户机 ID︰ 由 Azure 广告当注册应用程序生成的应用程序 ID。 请求授权码或令牌时, 的客户端 ID 和密钥发送到 Azure AD 身份验证过程。


  - 密钥︰ 一起发送的客户端 ID 进行身份验证时对 Azure AD 调用 web API 密钥。


- Azure 广告需要确保应用程序具有所需的权限来访问您的目录数据，您的组织中的其他应用程序，等等。

当您理解有两种类别的应用程序可以开发并集成在一起 Azure 广告，供应会更加清晰︰

- 单个客户端应用程序︰ 一个租户应用程序将在一个组织中使用。 这些是通常的业务线 (LoB) 应用程序由企业开发人员编写的。 单个租户应用程序只需要在一个目录中，用户访问，因此，它只需要设置一个目录中。 这些应用程序通常被注册在组织中的开发者。


- 多租户应用程序︰ 多租户应用程序旨在用于在许多组织中，不只是一个组织。 这些是由独立软件供应商 (ISV) 编写的一般软件作为-服务 (SaaS) 应用程序。 多租户应用程序需要设置，它们将被使用，它要求用户或管理员，同意注册它们每个目录中。 此许可过程启动时应用程序已经在目录中注册和图形 API 或可能是其他 web API 被授予访问权。 当用户或管理员从其他组织注册使用该应用程序时，它们将显示一个对话框，其中显示应用程序要求的权限。 用户或管理员可以再同意该应用程序，其中规定的数据，使应用程序可以访问和最后在其目录中注册应用程序。 有关详细信息，请参阅[概述同意框架](active-directory-integrating-applications.md#overview-of-the-consent-framework)。

开发一个多租户应用程序，而不是单个租户应用程序时，会出现一些额外的注意事项。 例如，如果您要向您的应用程序提供多个目录中的用户，您需要一种机制来确定哪个租户他们进入。 单个客户端应用程序只需要而在 Azure 广告标识所有目录中的特定用户所需要的多租户应用程序在用户目录中查找。 若要完成此任务，Azure AD 提供其中任何多租户应用程序可以直接登录请求，而不是特定于租户的终结点的通用身份验证终结点。 此终结点处于所有目录 https://login.microsoftonline.com/common Azure 的广告，而特定于租户的终结点可以是 https://login.microsoftonline.com/contoso.onmicrosoft.com。 常见的终结点是尤其重要，因为您需要必要的逻辑以处理登录、 注销和标记验证过程的多个承租人在开发应用程序时要考虑的。

如果您当前正在开发一个租户应用程序，但所做的许多公司，就可以轻松地更改到该应用程序并在 Azure 的广告其配置使其多租户能力。 此外，Azure AD 使用相同的签名密钥的所有目录中的所有标记是否提供了单个租户或多租户应用程序中的身份验证。

在本文档中列出的每个方案包括一个子部分描述其供应的要求。 有关设置 Azure 广告中的应用程序和单个和多租户应用程序之间的差异的详细信息，请参阅详细信息的[Azure Active Directory 集成应用程序](active-directory-integrating-applications.md)。 继续阅读了解 Azure AD 中常见的应用方案。

## <a name="application-types-and-scenarios"></a>应用程序类型和方案

可以使用各种语言和平台开发每本文档中介绍的方案。 他们是坚强后盾完整代码示例，它们可在我们的[示例代码指南](active-directory-code-samples.md)，或直接从相应[Github 的示例存储库](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory)。 此外，如果您的应用程序需要的特定段或段的端到端方案的在大多数情况下该功能可以添加独立。 例如，如果调用 web API 的本机应用程序，可以轻松地添加的 web 应用程序还会调用 web API。 下图说明了这些方案和应用程序类型，以及如何可以添加不同的组件︰

![应用程序类型和方案](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

这些是 Azure 的广告支持的五个主要应用方案︰

- [为 Web 应用程序的 web 浏览器](#web-browser-to-web-application)︰ 用户需要登录到 web 应用程序安全通过 Azure 的广告。

- [单页面应用程序 (SPA)](#single-page-application-spa): 用户需要登录到单页面应用程序保护的 Azure 的广告。

- [本机应用程序与 Web API](#native-application-to-web-api)︰ 电话、 平板电脑或 PC 运行本机应用程序需要进行身份验证的用户可以从 web API，从而保护由 Azure 的广告资源。

- [Web 应用程序与 Web API](#web-application-to-web-api): web 应用程序需要从 web API 通过 Azure AD 安全获得资源。

- [守护程序或服务器应用程序与 Web API](#daemon-or-server-application-to-web-api): 守护程序的应用程序或服务器应用程序与 web 用户界面需要从 web API 通过 Azure AD 安全获得资源。

### <a name="web-browser-to-web-application"></a>为 Web 应用程序的 web 浏览器

本部分介绍的应用程序对 web 浏览器访问 web 应用程序中的用户进行身份验证。 在这种情况下，web 应用程序指示用户的浏览器来使它们登录到 Azure 的广告。 Azure AD 返回登录的响应通过用户的浏览器，其中包含有关该用户的安全令牌中的声明。 这种情况下支持使用 WS 联合身份验证、 SAML 2.0 和 OpenID 连接协议登录。


#### <a name="diagram"></a>关系图
![浏览器的 web 应用程序的身份验证流](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>协议的流的说明


1. 当用户访问应用程序，需要在每次登录时，他们在 Azure AD 中重定向通过对身份验证终结点的登录请求。


2. 用户登录页上签署。


3. 如果身份验证成功，Azure 广告创建身份验证令牌，并返回到 Azure 管理门户中已配置的应用程序的答复 URL 登录的响应。 对于生产应用程序，此回复 URL 应为 HTTPS。 返回的标记包含有关用户和 Azure 广告的索赔所需的应用程序来验证该令牌。


4. 应用程序验证该令牌通过为 Azure AD 联合元数据文档在使用公用签名密钥和颁发者信息。 应用程序验证该令牌之后，Azure 的广告会与用户启动一个新会话。 此会话，则允许用户访问该应用程序，直到它过期。


#### <a name="code-samples"></a>代码示例


请参见代码示例 Web 浏览器对 Web 应用程序方案。 并且，频繁地检查--所有的时间，我们添加新的示例。 [为 Web 应用程序的 web 浏览器](active-directory-code-samples.md#web-browser-to-web-application)。


#### <a name="registering"></a>注册


- 单租户︰ 如果只是为您的组织构建应用程序，它必须注册在贵公司的目录中使用 Azure 管理门户。


- 多租户︰ 如果您要构建的应用程序可由您的组织之外的用户，它必须在公司目录中注册，但还必须在将使用该应用程序的每个组织的目录中注册。 若要使您的应用程序在其目录中可用，您可以为您的客户，使他们能够表示同意在您的应用程序包括注册过程。 当他们注册应用程序时，他们将会看到一个对话框，其中显示了该应用程序需要，权限，然后选择同意。 这取决于所需的权限，其他组织中的管理员可能需要给予同意的情况。 当用户或管理员同意时，在其目录中注册应用程序。 有关详细信息，请参阅[使用 Azure Active Directory 集成应用程序](active-directory-integrating-applications.md)。


#### <a name="token-expiration"></a>令牌到期

用户的会话过期 Azure 广告发出的令牌的生存期过期时。 您的应用程序，可以缩短此时间段内，如果需要，例如签出用户基于非活动时间段。 当会话到期时，将提示用户重新登录。





### <a name="single-page-application-spa"></a>单页应用程序 (SPA)

本部分介绍单页面应用程序，使用 Azure 的 AD 和 OAuth 2.0 隐式授权授予安全 API 返回结束其站点的身份验证。 通常结构是单页面应用程序以在浏览器和 Web API 后端服务器上运行，并实现应用程序的业务逻辑运行 JavaScript 表示层 （前端）。 若要了解更多有关隐式授权授予，并帮助您确定它是否适合您的应用程序方案，请参见[了解 Azure Active Directory 中的 OAuth2 隐式授予流](active-directory-dev-understanding-oauth2-implicit-grant.md)。

在这种情况下，当用户登录时，JavaScript 前结束使用[活动目录身份验证库的 JavaScript (ADAL。JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev)和隐式授权，授权从 Azure 的广告获取 ID 令牌 (id_token)。 该标记将被缓存，客户端将其附加到该请求的持有者标记为进行后端，使用 OWIN 中间件保护其 Web API 调用时。 
#### <a name="diagram"></a>关系图

![单页面应用程序关系图](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>协议的流的说明

1. 用户导航到 web 应用程序。


2. 该应用程序返回到浏览器的 JavaScript 前端 （表示层）。


3. 用户启动登录，例如通过单击登录链接。 浏览器发送 GET 请求 ID 标记的 Azure 广告授权端点。 此请求中包括的客户端 ID 和回复 URL 中的查询参数。


4. Azure AD 验证回复 URL 对 Azure 管理门户中配置注册回复 URL。


5. 用户登录页上签署。


6. 如果身份验证成功，Azure 广告创建 ID 标记，并将其作为 URL 片段 （#） 返回到应用程序的答复 URL。 对于生产应用程序，此回复 URL 应为 HTTPS。 返回的标记包含有关用户和 Azure 广告的索赔所需的应用程序来验证该令牌。


7. 运行在浏览器的 JavaScript 客户端代码提取响应用于保护对应用程序的 web API 返回结束标记。


8. 浏览器将调用应用程序的 web API 返回结尾的访问令牌在身份验证头。

#### <a name="code-samples"></a>代码示例


对于单页面应用程序 (SPA) 方案的代码示例，请参阅。 一定要经常检查--所有的时间，我们添加新的示例。 [单页应用程序 (SPA)](active-directory-code-samples.md#single-page-application-spa)。


#### <a name="registering"></a>注册


- 单租户︰ 如果只是为您的组织构建应用程序，它必须注册在贵公司的目录中使用 Azure 管理门户。


- 多租户︰ 如果您要构建的应用程序可由您的组织之外的用户，它必须在公司目录中注册，但还必须在将使用该应用程序的每个组织的目录中注册。 若要使您的应用程序在其目录中可用，您可以为您的客户，使他们能够表示同意在您的应用程序包括注册过程。 当他们注册应用程序时，他们将会看到一个对话框，其中显示了该应用程序需要，权限，然后选择同意。 这取决于所需的权限，其他组织中的管理员可能需要给予同意的情况。 当用户或管理员同意时，在其目录中注册应用程序。 有关详细信息，请参阅[使用 Azure Active Directory 集成应用程序](active-directory-integrating-applications.md)。

注册后的应用程序，它必须配置为使用 OAuth 2.0 隐式授权协议。 默认情况下，该协议已禁用的应用程序。 若要使您的应用程序的 OAuth2 隐式授权协议，从 Azure 管理门户下载其应用程序清单，"oauth2AllowImplicitFlow"值设置为 true，然后将清单后上载到门户网站。 有关详细说明，请参阅[启用 OAuth 2.0 隐式授予的单页面应用程序](active-directory-integrating-applications.md)。


#### <a name="token-expiration"></a>令牌到期

当您使用 ADAL.js 管理使用 Azure AD 身份验证时，您将受益于多种功能，可促进刷新到期的令牌，以及可能由应用程序调用其他 web API 资源获取令牌。 当用户成功通过验证使用 Azure 广告时，浏览器和 Azure 的广告之间的用户建立受 cookie 的会话。 值得注意的是，Azure 广告与用户之间，并在用户和服务器上运行的 web 应用程序之间不存在会话。 令牌到期后，ADAL.js 将使用此会话自动获取另一个标记。 这是通过使用隐藏的 iFrame 来发送和接收该请求使用 OAuth 隐式授权协议。 ADAL.js 还可以使用此相同的机制以静默方式获取访问令牌从 Azure AD 为其他 web API 资源的应用程序调用，只要这些资源支持跨来源的资源共享 (CORS) 中注册用户的目录和任何所需的同意过程登录用户给出。


### <a name="native-application-to-web-api"></a>本机应用程序与 Web API


本部分介绍的本机应用程序代表用户 web API 调用。 这种情况下会生成 OAuth 2.0 授权代码授予类型与公共客户端， [OAuth 2.0 规范](http://tools.ietf.org/html/rfc6749)的 4.1 节中所述。 本机应用程序通过使用 OAuth 2.0 协议获取用户的访问令牌。 该访问令牌然后被发送到 web API，它授予用户的权限，并返回所需的资源请求中。

#### <a name="diagram"></a>关系图

![本机应用程序 Web API 图](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>对于本机 api 的应用程序的身份验证流

#### <a name="description-of-protocol-flow"></a>协议的流的说明


如果您正在使用 AD 身份验证库，大部分协议细节如下所述，处理浏览器弹出窗口、 令牌缓存和刷新令牌的处理等。

1. 使用浏览器弹出，本机应用程序发出请求时到授权的终结点在 Azure 的广告。 此请求中包括的客户机 ID 和重定向 URI 的本机应用程序中管理门户和 web API 的应用程序 ID URI 所示。 如果用户没有已经登录，系统会提示用户再次登录


2. Azure 广告对用户进行身份验证。 如果它是多租户应用程序并同意的情况下，则需要使用该应用程序，用户将需要同意如果它们尚未这样做。 之后授予的同意并在身份验证成功时，Azure 广告发出授权代码响应返回给客户端应用程序的重定向 URI。


3. 当 Azure AD 问题回重定向 URI 授权代码响应时，客户端应用程序停止浏览器交互，并从响应中提取的授权码。 使用此授权码，客户端应用程序发送一个请求到 Azure AD 令牌的终结点所包括的授权码，详细介绍了有关客户端应用程序 （客户机 ID 和重定向 URI） 和所需的资源 （应用程序 ID 的 URI 的 web API）。


4. 通过 Azure AD 验证授权代码和客户端应用程序和 web API 的信息。 成功地验证，Azure AD 返回两个标记︰ JWT 访问令牌和一个 JWT 刷新令牌。 此外，Azure AD 返回基本用户有关的信息，例如显示名称和租户 id。


5. 通过 HTTPS，客户端应用程序使用返回的 JWT 访问令牌请求授权标头中的"载体"称号的 JWT 字符串添加到站点 API。 Web API 验证该 JWT 令牌，然后如果验证成功，则返回所需的资源。


6. 当访问令牌到期时，客户端应用程序将收到一个错误，指示该用户需要再次进行身份验证。 如果应用程序有一个有效的刷新令牌，它可用于而不会提示用户再次登录获取新的访问令牌。 如果刷新令牌到期，应用程序将需要以交互方式重新验证用户。


> [AZURE.NOTE] 颁发的 Azure AD 刷新令牌可以用于访问多个资源。 例如，如果必须有权限调用两个 web Api 客户端应用程序，刷新令牌可以用于获取访问令牌到其他 web API 也。


#### <a name="code-samples"></a>代码示例


代码示例的 Web API 方案的本机应用程序，请参见 并且，频繁地检查--所有的时间，我们添加新的示例。 [本机应用程序与 Web API](active-directory-code-samples.md#native-application-to-web-api)。


#### <a name="registering"></a>注册


- 一个租户︰ 无论是本机应用程序和 web API 必须注册在相同的目录在 Azure 的广告。 可以配置 web API 来公开一组权限，用于限制对其资源的本机应用程序的访问。 然后，客户端应用程序从 Azure 管理门户中"权限与其他应用程序"下拉菜单中选择所需的权限。


- 多租户︰ 首先，原来的应用程序只注册开发人员或发布服务器的目录中。 第二，本机应用程序被配置为表示可以正常工作所需的权限。 当用户或管理员目标目录中的应用程序，使其可用于其组织中给予同意的情况下，此列表中的所需权限所示对话框。 某些应用程序要求组织中的任何用户可以同意的只是用户级权限。 其他应用程序需要管理员级别权限，不能同意在该组织中的用户。 只有目录管理员可以同意的情况下要求此级别的权限的应用程序。 当用户或管理员同意时，在其目录中注册 web API。 有关详细信息，请参阅[使用 Azure Active Directory 集成应用程序](active-directory-integrating-applications.md)。


#### <a name="token-expiration"></a>令牌到期


当本机应用程序使用其授权代码得到 JWT 访问令牌时，它还接收 JWT 刷新令牌。 当访问令牌到期时，刷新令牌可以用于重新进行用户身份验证而不要求用户重新登录。 然后，使用此刷新令牌进行身份验证的用户，这将导致新的访问令牌和刷新令牌。





### <a name="web-application-to-web-api"></a>Web 应用程序与 Web API


本部分描述的 web 应用程序需要从 web API 获取资源。 在此方案中，有两个 web 应用程序可以使用身份验证和 web API 调用的身份类型︰ 应用程序标识或委派的用户标识。

*应用程序标识︰*这种情况下使用 OAuth 2.0 客户端凭据授予对应用程序进行身份验证和访问 web API。 当使用应用程序标识，web API 才能检测到该 web 应用程序调用它，作为 web API 没有收到任何有关用户的信息。 如果应用程序收到的与用户有关的信息，它将被发送通过应用程序协议，和它没有签名的 Azure 的广告。 Web API 信任 web 应用程序的用户进行身份验证。 由于这个原因，这种模式被称为受信任的子系统。

*委派的用户标识︰*这种情况下，可以通过两种方式︰ OpenID 连接和机密的客户端使用 OAuth 2.0 授权代码授予。 Web 应用程序获取证明 web API 用户成功通过身份验证对 web 应用程序和 web 应用程序无法获取 web API 调用委派的用户标识为该用户的访问令牌。 在对 web API，它授予用户的权限，并返回所需的资源的请求中发送该访问令牌。

#### <a name="diagram"></a>关系图

![Web API 图中的 web 应用程序](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>协议的流的说明

下面的流程中讨论的应用程序标识和委派的用户标识类型。 它们之间的关键区别是委派的用户标识必须首先获得授权码之前用户可以登录并访问 web API。

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>使用 OAuth 2.0 客户端的应用程序标识凭据授予

1. 用户登录到 web 应用程序中的 Azure AD （请参见上面[的 Web 应用程序的 Web 浏览器](#web-browser-to-web-application)）。


2. Web 应用程序需要获取一个访问令牌，以便它可以通过身份验证的 web API 并检索所需的资源。 它到 Azure AD 标记终结点，提供凭据、 客户端 ID 和 web API 的应用程序 ID URI 发出请求。


3. Azure 广告对应用程序进行身份验证并返回用于调用 web API JWT 访问令牌。


4. 通过 HTTPS，web 应用程序使用返回的 JWT 访问令牌请求授权标头中的"载体"称号的 JWT 字符串添加到站点 API。 Web API 验证该 JWT 令牌，然后如果验证成功，则返回所需的资源。

##### <a name="delegated-user-identity-with-openid-connect"></a>使用 OpenID 委派的用户身份连接

1. 用户已登录到 web 应用程序使用 Azure AD （请参见上面的[Web 浏览器的 Web 应用程序](#web-browser-to-web-application)一节）。 如果 web 应用程序的用户已不尚未同意允许 web 应用程序代表其调用 web API，用户将需要同意。 将显示应用程序的权限要求，并如果任何这些管理员级别的权限，普通用户在目录中的将不能同意。 此许可过程仅适用于多租户应用程序，不单一租户应用程序，该应用程序将已具有所需的权限。 当用户登录时，web 应用程序收到信息的用户，以及授权码 ID 标记。


2. 使用 Azure 广告发出的授权码，web 应用程序发送一个请求到 Azure AD 令牌的终结点所包括的授权码，详细介绍了有关客户端应用程序 （客户机 ID 和重定向 URI） 和所需的资源 （应用程序 ID 的 URI 的 web API）。


3. 通过 Azure 的广告会验证授权码和 web 应用程序和 web API 的信息。 成功地验证，Azure AD 返回两个标记︰ JWT 访问令牌和一个 JWT 刷新令牌。


4. 通过 HTTPS，web 应用程序使用返回的 JWT 访问令牌请求授权标头中的"载体"称号的 JWT 字符串添加到站点 API。 Web API 验证该 JWT 令牌，然后如果验证成功，则返回所需的资源。

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>OAuth 2.0 授权代码授予委派的用户标识

1. 用户已登录到 web 应用程序，其身份验证机制是独立于 Azure 的广告。


2. Web 应用程序要求授权代码获取一个访问令牌，以便 Azure 广告授权的终结点，提供客户机 ID 为发出请求通过浏览器和 web 应用程序在身份验证成功之后的重定向 URI。 用户登录到 Azure 的广告。


3. 如果 web 应用程序的用户已不尚未同意允许 web 应用程序代表其调用 web API，用户将需要同意。 将显示应用程序的权限要求，并如果任何这些管理员级别的权限，普通用户在目录中的将不能同意。 此许可过程仅适用于多租户应用程序，不单一租户应用程序，该应用程序将已具有所需的权限。


4. 用户有异议后，web 应用程序会收到需要它来获取一个访问令牌的授权码。


5. 使用 Azure 广告发出的授权码，web 应用程序发送一个请求到 Azure AD 令牌的终结点所包括的授权码，详细介绍了有关客户端应用程序 （客户机 ID 和重定向 URI） 和所需的资源 （应用程序 ID 的 URI 的 web API）。


6. 通过 Azure 的广告会验证授权码和 web 应用程序和 web API 的信息。 成功地验证，Azure AD 返回两个标记︰ JWT 访问令牌和一个 JWT 刷新令牌。


7. 通过 HTTPS，web 应用程序使用返回的 JWT 访问令牌请求授权标头中的"载体"称号的 JWT 字符串添加到站点 API。 Web API 验证该 JWT 令牌，然后如果验证成功，则返回所需的资源。

#### <a name="code-samples"></a>代码示例

代码示例 Web 应用程序的 Web API 方案，请参见 并且，频繁地检查--所有的时间，我们添加新的示例。 Web[应用程序与 Web API](active-directory-code-samples.md#web-application-to-web-api)。


#### <a name="registering"></a>注册

- 单个租户︰ 对于应用程序标识和委派的用户标识的情况下，web 应用程序和 web API 必须注册相同的目录中在 Azure 的广告。 可以配置 web API 来公开一组权限，用于限制对其资源的 web 应用程序的访问权限。 如果正在使用委派的用户标识类型，web 应用程序需要从 Azure 管理门户中"权限与其他应用程序"下拉菜单中选择所需的权限。 如果正在使用的应用程序标识类型，则不需要此步骤。


- 多租户︰ 首先，web 应用程序被配置为表示可以正常工作所需的权限。 当用户或管理员目标目录中的应用程序，使其可用于其组织中给予同意的情况下，此列表中的所需权限所示对话框。 某些应用程序要求组织中的任何用户可以同意的只是用户级权限。 其他应用程序需要管理员级别权限，不能同意在该组织中的用户。 只有目录管理员可以同意的情况下要求此级别的权限的应用程序。 当用户或管理员同意时，web 应用程序和 web API 两者中登记其目录。

#### <a name="token-expiration"></a>令牌到期

当 web 应用程序使用其授权代码得到 JWT 访问令牌时，它还接收 JWT 刷新令牌。 当访问令牌到期时，刷新令牌可以用于重新进行用户身份验证而不要求用户重新登录。 然后，使用此刷新令牌进行身份验证的用户，这将导致新的访问令牌和刷新令牌。


### <a name="daemon-or-server-application-to-web-api"></a>守护程序或服务器应用程序与 Web API


本部分介绍的守护程序或服务器的应用程序需要从 web API 获取资源。 有两种适用于本节的子方案︰ 守护程序需要调用 web API，基于 OAuth 2.0 客户端凭据授予类型;和服务器应用程序 （如 web API) 程序需要调用 web API，基于 OAuth 2.0 On-Behalf-Of 规范草案。

方案的一个守护程序的应用程序需要调用 web API，时一定要了解几个事情。 首先，用户交互是不可能的后台应用程序，这就要求应用程序具有自己的身份。 守护程序应用程序示例是一个批处理作业或在后台中运行的操作系统服务。 这种类型的应用程序通过使用其应用程序标识和展示其客户机 ID、 应用程序和凭据 （密码或证书），请求访问令牌 ID 对 Azure AD 的 URI。 身份验证成功后, 该守护进程从 Azure 的广告，然后用于调用 web API 接收一个访问令牌。

方案的服务器应用程序需要调用 web API 时，使用一个示例很有帮助。 想象一下，在本机应用程序，用户身份验证，此本机应用程序需要调用 web API。 Azure 广告发出调用 web API JWT 访问令牌。 如果 web API 需要调用另一个下游 web API，它可用于在代表的流委托的用户标识和身份验证到第二层 web API。

#### <a name="diagram"></a>关系图

![守护程序或服务器应用程序与 Web API 图](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>协议的流的说明

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>使用 OAuth 2.0 客户端的应用程序标识凭据授予

1. 首先，服务器应用程序需要与为其本身，而无需任何人员交互，如交互登录对话框的 Azure AD 身份验证。 它到 Azure AD 标记终结点，提供凭据、 客户端 ID 和应用程序 ID URI 发出请求。


2. Azure 广告对应用程序进行身份验证并返回用于调用 web API JWT 访问令牌。


3. 通过 HTTPS，web 应用程序使用返回的 JWT 访问令牌请求授权标头中的"载体"称号的 JWT 字符串添加到站点 API。 Web API 验证该 JWT 令牌，然后如果验证成功，则返回所需的资源。


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>委派的用户标识使用 OAuth 2.0 规范上代表的草案

下面讨论的流程假定用户已经过身份验证的其他应用程序 （如本机应用程序），并且其用户标识已用于获取到第一层 web API 访问令牌。

1. 本机应用程序发送到第一层 web API 的访问令牌。


2. 第一层 web API 发送一个请求到 Azure AD 标记终结点，提供其客户机 ID 和凭据，以及用户的访问令牌。 此外，将该请求发送的 on_behalf_of 参数，指示 web API 请求新的令牌来代表原始用户下游 web API 调用。


3. Azure AD 验证第一层 web API 具有第二层 web API 访问权限并验证该请求，则返回 JWT 访问令牌，JWT 刷新到第一层 web API 的标记。


4. 通过 HTTPS，第一层 web API 然后调用第二层 web API 通过附加在请求授权标头中的标记的字符串。 第一层 web API 可以继续调用第二层 web API，只要是有效的访问令牌并刷新令牌。

#### <a name="code-samples"></a>代码示例

守护程序或服务器应用程序的 Web API 方案请参见代码示例。 并且，频繁地检查--所有的时间，我们添加新的示例。 [服务器或后台程序到 Web API 的应用程序](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>注册

- 单个租户︰ 对于应用程序标识和委派的用户标识的情况下，守护程序或服务器应用程序必须注册相同的目录中在 Azure 的广告。 可以配置 web API 来公开一组权限，用来限制守护程序或对其资源的服务器的访问。 如果正在使用委派的用户标识类型，服务器应用程序需要从 Azure 管理门户中"权限与其他应用程序"下拉菜单中选择所需的权限。 如果正在使用的应用程序标识类型，则不需要此步骤。


- 多租户︰ 首先，守护程序或服务器配置应用程序以指示可以正常工作所需的权限。 当用户或管理员目标目录中的应用程序，使其可用于其组织中给予同意的情况下，此列表中的所需权限所示对话框。 某些应用程序要求组织中的任何用户可以同意的只是用户级权限。 其他应用程序需要管理员级别权限，不能同意在该组织中的用户。 只有目录管理员可以同意的情况下要求此级别的权限的应用程序。 当用户或管理员同意时，这两个 web Api 在其目录中注册。

#### <a name="token-expiration"></a>令牌到期

当第一个应用程序使用其授权代码得到 JWT 访问令牌时，它还接收 JWT 刷新令牌。 当访问令牌到期时，可以使用刷新令牌重新进行用户身份验证而不提示输入凭据。 然后，使用此刷新令牌进行身份验证的用户，这将导致新的访问令牌和刷新令牌。

## <a name="see-also"></a>请参见

[Azure 的 Active Directory 开发人员指南 》](active-directory-developers-guide.md)

[Azure 的 Active Directory 代码示例](active-directory-code-samples.md)

[在 Azure AD 签名密钥翻转的重要信息](active-directory-signing-key-rollover.md)

[OAuth 2.0 Azure 做广告](https://msdn.microsoft.com/library/azure/dn645545.aspx)
