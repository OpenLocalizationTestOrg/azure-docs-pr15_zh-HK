<properties
   pageTitle="Azure Active Directory 开发人员指南 》 |Microsoft Azure"
   description="这篇文章的 Azure Active Directory 提供全面面向开发人员的资源指南。"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Azure Active Directory 开发人员指南 》

## <a name="overview"></a>概述
作为为服务 (IDMaaS) 平台标识管理，Azure 活动目录 (AD) 为开发人员提供了有效的方法来集成到其应用程序的标识管理。 下列文章提供有关实施和 Azure 广告的主要功能概述。 我们建议您阅读顺序，或如果您已经准备好要深入了解跳转到[入门](#getting-started)。


1. [Azure 广告集成的好处](./develop/active-directory-how-to-integrate.md)︰ 发现为什么与 Azure AD 集成提供安全登录和授权的最佳解决方案。

1. [Azure AD 身份验证方案](active-directory-authentication-scenarios.md)︰ 利用 Azure 的广告来提供登录到应用程序中的简化身份验证。

1. [集成与 Azure AD 的应用程序](active-directory-integrating-applications.md)︰ 了解如何添加、 更新和删除应用程序，从 Azure 的广告，和关于商标使用准则用于集成的应用程序。

1. [Azure 广告图形 API](active-directory-graph-api.md)︰ 使用 Azure 广告图形 API 以编程方式通过 REST API 端点来访问 Azure 的广告。 Azure 广告图形 API 也是可以通过[Microsoft Graph](https://graph.microsoft.io/)访问的。 Microsoft Graph 提供了一个统一的 API，可以访问多个 Microsoft 云服务的 Api，通过一个 REST API，终结点，并使用一个访问令牌。

1. [Azure AD 身份验证库](active-directory-authentication-libraries.md)︰ 轻松地验证用户以获得针对.NET，JavaScript，使用 Azure AD 身份验证库的访问令牌目标 C、 Android，等等。


## <a name="getting-started"></a>入门教程

这些教程针对多个平台，可以帮助您快速开始使用 Azure Active Directory 进行开发。 作为先决条件，您必须[获取 Azure Active Directory 租户](active-directory-howto-tenant.md)。

### <a name="mobile-and-pc-application-quick-start-guides"></a>移动设备和 PC 应用程序快速启动指南

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Windows 世界](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Windows 世界](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[直接使用 OAuth 2.0 集成](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Web 应用程序的快速启动指南

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![Javascript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID 连接](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[Javascript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[集成与 OpenID 的直接连接](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Web API 快速入门指南

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>查询目录快速入门指南

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph API](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>操作方法

这些文章介绍如何使用 Azure Active Directory 执行特定任务︰

- [获取 Azure AD 租户](active-directory-howto-tenant.md)
- [登录任何 Azure AD 用户使用的多租户应用程序模式](active-directory-devhowto-multi-tenant-overview.md)
- 启用跨应用程序 SSO [Android](active-directory-sso-android.md)和[iOS](active-directory-sso-ios.md)设备上，使用 ADAL，
- [使应用程序认证的 AppSource Azure 的广告](active-directory-devhowto-appsource-certified.md)
- [列出在 Azure AD 应用程序库应用程序](active-directory-app-gallery-listing.md)
- [提交针对 Office 365 提供对卖方的仪表板的 web 应用程序](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [了解 Azure 活动目录的应用程序清单](active-directory-application-manifest.md)
- [了解客户端应用程序中的签入和应用程序购买按钮商标使用准则](active-directory-branding-guidelines.md)
- [预览︰ 如何构建应用程序签名同时个人和工作或学校的帐户登录的用户](active-directory-appmodel-v2-overview.md)
- [预览︰ 如何构建应用程序的注册和登录使用者](../active-directory-b2c/active-directory-b2c-overview.md)
- [预览︰ 在 Azure AD 配置令牌生存期](active-directory-configurable-token-lifetimes.md)使用 PowerShell。 配置通过 Azure 广告图形 API，请参阅[策略操作](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations)和[策略实体](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity)的详细信息。

## <a name="reference"></a>引用

这些文章的其余部分和身份验证库的 Api、 协议、 错误、 代码示例和终结点提供基础参考。  

###  <a name="support"></a>支持
- [标记的问题](http://stackoverflow.com/questions/tagged/azure-active-directory)︰ 找到 Azure Active Directory 解决方案堆栈溢出通过搜索标记[azure 活动目录](http://stackoverflow.com/questions/tagged/azure-active-directory)和[adal](http://stackoverflow.com/questions/tagged/adal)。
- 请参阅有关的一些相关的应用程序开发和集成的常用术语定义[Azure 广告开发术语表](active-directory-dev-glossary.md)。

### <a name="code"></a>代码

- [Azure Active Directory 的开源库](http://github.com/AzureAD)︰ 查找库的源的最简单方法是使用我们的[存储库列表](active-directory-authentication-libraries.md)。

- [Azure Active Directory 示例](https://github.com/azure-samples?query=active-directory)︰ 导航示例的列表的最简单方法是使用[索引的代码样本](active-directory-code-samples.md)。

- [活动目录身份验证库 (ADAL) 为使.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)的参考文档是[最新的主要版本](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)和[上一个主要版本](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)可用。

### <a name="graph-api"></a>Graph API

- [Graph API 参考](https://msdn.microsoft.com/library/azure/hh974476.aspx)︰ Azure 活动目录图形 API 的其他引用。 [查看交互式图形 API 参考经验](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)。

- [Graph API 权限范围](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)︰ OAuth 2.0 用于控制应用程序目录中的数据承租人拥有的访问权限的权限范围。

### <a name="authentication-and-authorization-protocols"></a>身份验证和授权协议

- [在 Azure 广告签名密钥变换图像](active-directory-signing-key-rollover.md)︰ 了解 Azure 广告的签名密钥变换节奏和如何进行更新的键最常用的应用程序方案。

- [OAuth 2.0 协议︰ 使用授权代码授予](active-directory-protocols-oauth-code.md)︰ 可以使用 OAuth 2.0 协议的授权代码授权，授权访问 Web 应用程序和 Web Api 在您 Azure Active Directory 租户。

- [OAuth 2.0 协议︰ 了解隐式授予](active-directory-dev-understanding-oauth2-implicit-grant.md)︰ 了解更多有关隐式授权，授权，以及是否是最适合您的应用程序。

- [OAuth 2.0 协议︰ 服务调用使用客户端凭据的服务](active-directory-protocols-oauth-service-to-service.md): OAuth 2.0 客户端凭据授予允许 web 服务 （机密客户端） 使用自己的凭据进行身份验证时调用另一个 web 服务，而不是模拟用户。 在这种情况下，客户端通常是中间层 web 服务、 后台程序服务或网站。

- [OpenID 连接 1.0 协议︰ 登录和身份验证](active-directory-protocols-openid-connect-code.md): OpenID 连接 1.0 协议扩展 OAuth 2.0 用作身份验证协议。 客户端应用程序可以接收 id_token 来管理签入过程中，或增加授权代码流接收 id_token 和授权代码。

- [SAML 2.0 协议引用](active-directory-saml-protocol-reference.md)︰ SAML 2.0 协议使应用程序能够为用户提供单一登录体验。

- [WS 联合身份验证 1.2 协议](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html)︰ Azure 活动目录支持根据 Web 服务联盟版本 1.2 规范的 WS 联合身份验证 1.2。 有关联合元数据文档的详细信息，请参阅[联合元数据](active-directory-federation-metadata.md)。

- [受支持的标记和声明类型](active-directory-token-and-claims.md)︰ 您可以使用本指南可以了解和评估在 SAML 2.0 和 JSON Web 标记 (JWT) 令牌中的声明。

## <a name="videos"></a>视频

### <a name="build"></a>生成

这些概述演示文稿上开发的应用程序通过使用 Azure Active Directory 功能直接处理工程团队的扬声器。 演示文稿涵盖基本主题，其中包括 IDMaaS、 身份验证、 联合，身份验证和单一登录。

- [联合和未来方向的 Microsoft 身份︰ 状态](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure 的 Active Directory︰ 标识管理作为现代应用程序的服务](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [开发使用 Azure Active Directory 的现代 web 应用程序](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [开发使用 Azure Active Directory 的现代本机应用程序](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure 星期五
[Azure 星期五](https://azure.microsoft.com/documentation/videos/azure-friday/)是定期星期五与各种 Azure 主题专家会见了 1:1 的视频系列，致力于为您带来短 （10 – 15 分钟）。  服务筛选功能用于在页面上查看所有 Azure Active Directory 视频。

- [Azure 标识 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure 标识 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure 标识 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>社会

- [活动目录团队博客](http://blogs.technet.com/b/ad/)︰ Azure Active Directory 的世界的最新进展。

- [Azure 活动目录图表团队博客](http://blogs.msdn.com/b/aadgraphteam)︰ 特定于图形 API 的 Azure Active Directory 信息。

- [云的身份](http://www.cloudidentity.net)︰ 标识管理作为一种服务，从主体 Azure 活动目录下午的思路。  

- [在 Twitter 上的 azure Active Directory](https://twitter.com/azuread): Azure Active Directory 在 140 个字符或更少的通知。

## <a name="windows-server-on-premises-development"></a>Windows 服务器内部开发
有关使用 Windows 服务器和活动目录联合身份验证服务 (ADF) 开发的指南，请参阅︰

- [AD FS 方案对于开发人员来说](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers)︰ 概述 AD FS 组件和它的工作原理，使用受支持的身份验证/授权方案的详细信息。
- [AD FS 演练](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development)︰ 浏览文章列表，实现相关的身份验证/授权流程提供的分步说明。
