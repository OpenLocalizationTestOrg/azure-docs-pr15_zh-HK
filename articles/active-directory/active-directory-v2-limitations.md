<properties
    pageTitle="v2.0 终结点限制和限制 |Microsoft Azure"
    description="限制和限制使用 Azure AD v2.0 终结点的列表。"
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

# <a name="should-i-use-the-v20-endpoint"></a>使用 v2.0 终结点吗？

在生成时将与 Azure Active Directory 集成的应用程序，您将需要决定 v2.0 终结点和身份验证协议是否满足您的需要。  原始的 Azure AD 终结点仍然完全支持它在某些方面，比 2.0 版的详细功能丰富。  但是，2.0 版终结点[带来显著的好处](active-directory-v2-compare.md)可能会诱使您可以使用新的编程模型的开发人员。

在此时间点，我们建议使用 v2.0 终结点如下所示︰

- 如果您想要在您的应用程序中支持 Microsoft 的个人帐户，则应使用 v2.0 终结点。  但是，请务必了解列在本文中，尤其是那些关于专门用来工作和学校科目的局限性。
- 如果您的应用程序仅需要支持工作和学校的科目，应使用[原来的 Azure AD 终结点](active-directory-developers-guide.md)。

随着时间的推移将增长 v2.0 终结点，以消除这里列出的限制，以便您只需要使用 v2.0 终结点。  同时，本文旨在帮助您确定您是要 v2.0 终结点。  我们将继续以反映 v2.0 终结点的当前状态，因此请查看后要重新评估您的需求与 2.0 版功能更新本文。

如果您有不使用 v2.0 端点的 Azure 广告与对现有应用程序，则无需从头开始。  将来，我们将提供您启用现有的 Azure 广告应用程序，使用 v2.0 终结点的一种方式。

## <a name="restrictions-on-apps"></a>对应用程序的限制
以下类型的应用程序当前不支持 v2.0 终结点。  有关受支持的应用程序类型的说明，请参阅[这篇文章](active-directory-v2-flows.md)。

##### <a name="standalone-web-apis"></a>独立 Web Api
使用 v2.0 的终结点，必须[生成一个 Web API，受到使用 OAuth 2.0](active-directory-v2-flows.md#web-apis)的功能。  但是，Web API 将只能接收标记的应用程序共享相同的应用程序 id。  不支持生成网站不同的应用程序 Id 的客户端访问的 API。  该客户端将不能申请或获得对您的 web API 权限。

如何构建一个 Web API，接受来自客户机具有相同的应用程序 Id 的标记，请参阅[入门指南](active-directory-appmodel-v2-overview.md#getting-started)中的 2.0 版终结点 Web API 示例。

##### <a name="web-api-on-behalf-of-flow"></a>Web API 上代表的流
许多结构包括一个需要调用另一个下游 Web API，同时受 v2.0 终结点的 Web API。  该方案是在本机客户端具有 Web API 后端，后者又调用 Microsoft 在线服务或其他内置的自定义 web API 支持 Azure 广告中常见的。

可以使用 OAuth 2.0 Jwt 载体凭据授予，否则称为 On-Behalf-Of 流支持此方案。  但是，在代表的流是目前不支持 v2.0 终结点。  若要查看此流原理在普遍适用的 Azure AD 服务，请检查[在 GitHub 上代表的代码示例](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)。

## <a name="restrictions-on-app-registrations"></a>限制对应用程序登记
在此时间点，想要集成使用 2.0 版的终结点的所有应用程序必须在[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)中创建新的应用程序注册。  任何现有的 Azure 广告或 Microsoft 客户应用程序将不兼容使用 2.0 版的终结点，也不会在任何新的应用程序注册门户除了门户中注册应用程序。  我们计划提供一种方法，来促使用作 2.0 版应用程序的现有应用程序。 这一次尽管没有迁移路径到 2.0 版的终结点的应用程序。

同样，应用程序注册新的应用程序注册门户中对原始 Azure AD 身份验证终结点无效。  但是，可以使用应用程序应用程序注册门户中创建与 Microsoft 帐户身份验证终结点，成功集成`https://login.live.com`。

此外，在[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)上创建应用程序登记都有以下几点︰

- 不支持**主页**属性也称为**登录 URL** 。  主页，而这些应用程序将不显示 Office MyApps 面板中。
- 每个应用程序 Id 此时允许只有两个应用程序的机密信息。
- 只能查看和管理单个开发人员帐户的应用程序注册。  它不能多个开发人员之间共享。
- 有几个格式的 redirect_uri 允许的限制。  请参阅下的一节有关更多详细信息。

## <a name="restrictions-on-redirect-uris"></a>重定向 Uri 的限制
在新的应用程序注册门户中注册的应用程序是当前限制为一组有限的 redirect_uri 值。  与方案开始必须为 web 应用程序和服务 redirect_uri `https`，和 redirect_uri 的所有值必须都共享一个 DNS 域。  例如，不能注册了 redirect_uris 一个 web 应用程序︰

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

注册系统对 redirect_uri 您要添加的 DNS 名称与现有 redirect_uri 的整个 DNS 名称进行比较。 要添加的 DNS 名称的请求将会失败，如果满足下列条件之一︰  

- 如果新的 redirect_uri 的整个 DNS 名称与现有 redirect_uri 的 DNS 名称不匹配
- 如果新的 redirect_uri 的整个 DNS 名称不是一个现有 redirect_uri 的子域

例如，如果应用程序当前具有 redirect_uri:

`https://login.contoso.com`

然后就可以添加︰

`https://login.contoso.com/new`

这与完全匹配的 DNS 名称，或︰

`https://new.login.contoso.com`

这是 login.contoso.com 的 DNS 子域。  如果您希望应用程序具有登录 east.contoso.com 和登录-west.contoso.com 为 redirect_uris，则您必须按顺序添加下面的 redirect_uris:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

可以添加后两者，因为它们的第一个 redirect_uri，子域 contoso.com。 未来版本中将删除此限制。

若要了解如何在新的应用程序注册门户中注册应用程序，请参阅[该文章](active-directory-v2-app-registration.md)。

## <a name="restrictions-on-services--apis"></a>有关服务和 Api 的限制
V2.0 端点当前支持登录注册新的应用程序注册门户，提供在任何应用程序属于[支持的身份验证流程](active-directory-v2-flows.md)列表。  但是，这些应用程序将只能获得非常有限的一组资源的 OAuth 2.0 的访问令牌。  V2.0 终结点只会颁发的 access_tokens:

- 应用程序请求令牌。  应用程序可以获取 access_token 本身，如果逻辑应用程序由若干个不同组件或层。  若要查看运行中的这种情况下，检查出我们[快速入门](active-directory-appmodel-v2-overview.md#getting-started)教程。
- Outlook 邮件、 日历和联系人 REST Api，它们都是位于 https://outlook.office.com。  要了解如何编写访问这些 Api 的应用程序，请参阅这些[办公室快速入门](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)教程。
- Microsoft Graph Api 中。  若要了解有关 Microsoft Graph 和可用的所有数据，请访问[https://graph.microsoft.io](https://graph.microsoft.io)。

这次不支持的任何其他服务。  将支持您自己的自定义生成的 Web Api 和服务以及将来，添加更多 Microsoft 联机服务。

## <a name="restrictions-on-libraries--sdks"></a>库和 Sdk 的限制
这一次，库 v2.0 终结点的支持却非常有限。  如果您想要在成品应用程序中使用 v2.0 终结点，您有下列选项︰

- 如果您正在构建一个 web 应用程序，可以安全地使用我们通常可用的服务器端中间件执行中的符号和标记验证。  ASP.NET 和我们 NodeJS 护照插件，其中包括 OWIN 打开 ID 连接中间件。  我们[开始](active-directory-appmodel-v2-overview.md#getting-started)部分也提供了代码示例使用我们中间件。
- 其他平台和本机和移动应用程序，您还可以将集成使用 2.0 版的终结点直接发送和接收应用程序代码中的协议消息的。  OpenID 连接和 OAuth 协议[明确记载](active-directory-v2-protocols.md)可帮助您执行此类集成 2.0 版。
- 最后，您可以使用开源打开连接 ID 和 OAuth 库集成使用 2.0 版的终结点。  V2.0 协议应符合许多开源协议库无需进行重大更改。  这种库的可用性是变化的每个语言和平台上，并[打开 ID 连接](http://openid.net/connect/)和[OAuth 2.0](http://oauth.net/2/)网站维护一份流行的实现。 打开源客户端库和使用 v2.0 终结点测试过的样本的列表和详细信息，请参阅[Azure 活动目录 (AD) 2.0 版和身份验证库](active-directory-v2-libraries.md)。

我们还发布了[Microsoft 身份验证库 (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)的初始预览.net 只。  现在欢迎可试出此库在.NET 客户端和服务器应用程序，但作为预览库都不将伴随着正式提供质量支持。

## <a name="restrictions-on-protocols"></a>协议的限制
V2.0 终结点仅支持打开 ID 连接和 OAuth 2.0。  但是，并非所有特性和功能的每个协议都已都并入 v2.0 终结点，包括︰

- OpenID 连接`end_session_endpoint`，它允许应用程序结束使用 2.0 版的终结点用户的会话。
- id_tokens 颁发的 2.0 版终结点仅包含成对用户的标识符。  这意味着，两个不同的应用程序都将收到相同的用户 Id 不同。  请注意，通过查询 Microsoft Graph`/me`终结点，您将能够获得 correlatable ID，可以使用跨应用程序的用户。
- id_tokens 颁发的 2.0 版终结点不包含`email`这一次，声称用户，即使您获取来自用户的权限以查看其电子邮件。
- OpenID 用户信息连接终结点。 这一次的用户信息终结点未实现 v2.0 终结点上。  但是，您可能会收到该终结点上的所有用户配置文件数据都可从 Microsoft Graph`/me`终结点。
- 角色和组声明。  到目前为止，2.0 版终结点不支持颁发的角色或组索赔中的 id_tokens。

为了更好地理解在 v2.0 终结点支持的协议功能的范围，阅读我们的[OpenID 连接和 OAuth 2.0 协议参考](active-directory-v2-protocols.md)。

## <a name="restrictions-for-work--school-accounts"></a>限制工作和学校的科目
有几个功能专门针对 Microsoft 2.0 版终结点尚不支持的组织中的业务用户。

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>基于设备的条件访问、 本机和移动应用程序和 Microsoft Graph
V2.0 终结点还不支持移动和本机应用程序，例如在 iOS 或 Android 上运行的本机应用程序的身份验证设备。  这可能会阻止应用程序本机 Microsoft Graph 中调用的特定组织。  管理员可以设置一个基于设备的条件访问策略的应用程序时，设备的身份验证是必需的。  V2.0 终结点，为基于设备的条件访问最可能的情况是在 Microsoft Graph 中，如 Outlook API 资源上设置策略的管理员。  如果管理员将此策略设置和本机应用程序请求指向 Microsoft Graph 的标记，则请求最终将失败，因为尚不支持设备的身份验证。  Web 应用程序请求标记为 Microsoft Graph 中，但是，当设备基于策略的配置支持。  在 web 应用程序方案设备身份验证通过用户的 web 浏览器执行。

作为开发人员，您很可能可以没有控制策略设置 Microsoft Graph 资源，或甚至知道当它发生时。  如果您要构建工作和学校的用户应用程序，应使用[原始的 Azure AD 端点](active-directory-developers-guide.md)，直到 v2.0 终结点支持的身份验证设备。  基于设备的条件访问有关的详细信息，请检查出[这篇文章](active-directory-conditional-access.md#device-based-conditional-access)。

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>对于联合承租人 Windows 集成身份验证
如果您已经在 Windows 应用程序中使用 ADAL （和原始的 Azure AD 端点），您可能已经利用所谓的 SAML 断言权限授予。  此授权允许的联盟 Azure AD 用户承租人进行自动身份验证其内部部署 Active Directory 实例而无需输入其凭据。  SAML 断言的权限授予当前不支持在 v2.0 终结点上。
