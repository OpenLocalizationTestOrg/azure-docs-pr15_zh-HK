<properties
    pageTitle="Azure 的活动目录 B2C︰ 限制和限制 |Microsoft Azure"
    description="限制和 Azure 活动目录 B2C 的限制条件的列表"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure 的活动目录 B2C︰ 限制和限制

有几种功能和 Azure 活动目录 (AD Azure) B2C 的尚不支持的功能。 将从长远看，解决许多这些已知的问题和限制，但如果您要构建使用 Azure AD B2C 的面向消费者的应用程序应该了解它们。

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>在 Azure AD B2C 承租人的创建过程中的问题

如果在[Azure AD B2C 租户的创建](active-directory-b2c-get-started.md)过程中遇到问题，请参阅[创建 Azure AD 租户或 Azure AD B2C 租户 — 问题和解决方法](active-directory-b2c-support-create-directory.md)指南。

请注意，存在一些已知问题删除现有 B2C 租户和重新使用相同的域名创建它时。 您必须使用不同的域名创建 B2C 租户。

## <a name="note-about-b2c-tenant-quotas"></a>请注意有关 B2C 租户配额

默认情况下，在 B2C 租户的用户数是超过 50000 个用户。 如果您需要引发 B2C 租户的配额，您应联系支持。

## <a name="branding-issues-on-verification-email"></a>品牌上验证电子邮件的问题

默认验证电子邮件包含 Microsoft 的品牌。 我们将在以后将其删除。 现在，您可以通过删除它[公司的品牌特征](../active-directory/active-directory-add-company-branding.md)。

## <a name="restrictions-on-applications"></a>在应用程序上的限制

在 Azure AD B2C 当前不支持以下类型的应用程序。 有关受支持的应用程序类型的说明，请参阅[Azure AD B2C︰ 类型的应用程序](active-directory-b2c-apps.md)。

### <a name="single-page-applications-javascript"></a>单个页面的应用程序 (JavaScript)

许多现代的应用程序具有单页面应用程序 (SPA) 前端主要是用 JavaScript 编写，通常使用一个 SPA 框架，如 AngularJS、 Ember.js、 Durandal 等。这个流量尚不可用在 Azure AD B2C。

### <a name="daemons--server-side-applications"></a>守护程序 / 服务器端应用程序

包含长时间运行的进程或用户如果没有运行的应用程序也需要一种方法来访问受保护的资源，例如 Web Api。 这些应用程序可以进行身份验证，并通过使用应用程序的标识 （而不是使用者的代理的身份） [OAuth 2.0 客户端凭据流](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow)中获取令牌。 这个流量尚无在 Azure AD B2C，所以现在，应用程序之后，可以获取令牌只出现一个交互式使用者签入流。

### <a name="standalone-web-apis"></a>独立 Web Api

在 Azure AD B2C，您可以[构建使用 OAuth 2.0 令牌受到 Web API](active-directory-b2c-apps.md#web-apis)的功能。 但是，Web API 将只能接收标记从客户端共享相同的应用程序 id。 不支持生成一个 Web API，从几个不同的客户端访问。

### <a name="web-api-chains-on-behalf-of"></a>Web API 链 （上代表的）

许多结构包括需要调用另一个下游 Web API，同时受 Azure AD B2C 网站 API。 该方案是在本机客户端后端 Web API，后者又调用类似 Azure 广告图形 API 的 Microsoft 联机服务中常见的。

此链接的 Web API 方案可以支持使用 OAuth 2.0 Jwt 载体凭据授予，否则称为亮代表的流。 但是，在代表的流量目前未实现在 Azure AD B2C。

## <a name="restriction-on-libraries-and-sdks"></a>对库和 Sdk 的限制

这一次是非常有限的一套工作 Azure AD B2C 的 Microsoft 支持库。 我们都支持基于.NET web 应用程序和服务，以及 NodeJS web 应用程序和服务。  我们还提供了称为 MSAL，可用于在 Windows 和其他.NET 应用程序中的 Azure AD B2C 预览.NET 客户端库。

我们目前没有支持其他语言或平台，包括 iOS 和 Android 的库。  如果您想要生成比上面提到的那些不同的平台上，我们建议使用开源的 SDK，指我们的[OAuth 2.0 和 OpenID 连接协议引用](active-directory-b2c-reference-protocols.md)根据需要。  Azure AD B2C 实现 OAuth 和 OpenID 连接，这样就有可能要用于集成通用 OAuth 或 OpenID 连接库。

我们的 iOS 和 Android 快速入门教程使用我们测试的开放源代码库与 Azure AD B2C 的兼容性。  所有我们的快速启动教程中有我们的[入门](active-directory-b2c-overview.md#getting-started)部分。

## <a name="restriction-on-protocols"></a>对协议的限制

Azure AD B2C 支持 OpenID 连接和 OAuth 2.0。 但是，并非所有特性和功能的每个协议已经都实现了。 为更好地了解在 Azure AD B2C，阅读我们的[OpenID 连接并 OAuth 2.0 协议引用](active-directory-b2c-reference-protocols.md)支持的协议功能的范围。 SAML 和 WS 进协议支持不可用。

## <a name="restriction-on-tokens"></a>限制标记

许多 Azure AD B2C 所颁发的令牌实现为 JSON Web 标记或 JWTs。 但是，并不是所有 JWTs （即所谓"索赔"） 中包含的信息都是相当应该或缺少。 例如，"sub"和"preferred_username"的索赔。  作为值、 格式或含义的索赔变化随着时间的推移，现有策略的标记将不会影响-您可以依靠它们在生产应用程序中的值。  随着值的变化，我们将为您提供为每个您的策略中配置这些更改的机会。  要更好地了解当前发出 Azure AD B2C 服务的标记，阅读我们的[令牌引用](active-directory-b2c-reference-tokens.md)。

## <a name="restriction-on-nested-groups"></a>对嵌套组的限制

在 Azure AD B2C 承租人不支持嵌套的组成员身份。 我们不打算添加此功能。

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>在 Azure 广告图形 API 上的差异查询功能上的限制

在 Azure AD B2C 承租人不支持[Azure 广告图形 API 上的差异的查询功能](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query)。 这是我们长期的规划图上。

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>在 Azure 的传统门户网站上的用户管理的问题

B2C 的功能都可以在 Azure 的门户网站上访问。 但是，可以使用 Azure 的传统门户网站访问其他租户功能，包括用户管理。 目前有几个在 Azure 的传统门户网站上的用户管理 （**用户**选项卡） 的已知问题︰

- 为本地帐户的用户 （即，使用者注册电子邮件地址和密码，或用户名和密码），**用户名**字段中不对应的登录标识符 （电子邮件地址或用户名） 期间签约使用。 这是因为在 Azure 的传统门户网站上显示的字段是实际用户主体名称 (UPN)，未在 B2C 方案中使用。 若要查看本地帐户的登录标识符，查找[图形资源管理器](https://graphexplorer.cloudapp.net/)中的用户对象。 您会发现同样的问题与社会帐户用户 （即，使用者注册与 Facebook、 Google +，等等），但在这种情况下，没有任何的登录标识符来跟他讲话。

    ![本地帐户的 UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- 对于本地帐户用户，您将无法编辑任何字段并将更改保存在**配置文件**选项卡上。

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>管理启动密码重置在 Azure 的传统门户网站上的问题

如果您重置本地帐户基于 Azure 传统门户网站 （**用户**选项卡上的**重置密码**命令） 上使用者的密码，该使用者将不能更改他或她的下一次登录，密码，如果您使用一个符号或登录策略，并将锁定您的应用程序。 作为一种替代方法，使用[Azure 广告图形 API](active-directory-b2c-devquickstarts-graph-dotnet.md) （无密码过期） 的使用者的密码重置或使用登录策略而不是一个符号或登录策略。

## <a name="issues-with-creating-a-custom-attribute"></a>创建自定义属性的问题

不在 B2C 租户中立即创建[Azure 门户上添加自定义属性](active-directory-b2c-reference-custom-attr.md)。 您需要在 B2C 租户中得到创建，并通过图形 API 可用中至少一个为该策略中使用的自定义特性。

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>验证域在 Azure 的传统门户网站上的问题

当前不能验证域[Azure 的传统门户网站](https://manage.windowsazure.com/)上的成功。

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>在 Safari 浏览器上的 MFA 策略与登录问题

间歇性地在 Safari 浏览器的 HTTP 400 （错误请求） 错误 （与开启的 MFA) 签入策略失败的请求。 这是由于 Safari 的低 cookie 的大小限制。 有几个针对此问题的替代方法︰

- 使用"注册或登录策略"而不是"签入策略"。
- 减少**应用程序声明**您的策略中所请求的数量。
