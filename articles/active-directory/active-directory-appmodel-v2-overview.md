<properties
    pageTitle="v2.0 终结点概述 |Microsoft Azure"
    description="构建与 Microsoft 帐户和 Azure Active Directory 登录应用程序简介。"
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
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>登录 Microsoft 帐户和 Azure AD 用户在单个应用程序

在过去，应用程序开发人员希望支持 Microsoft 帐户和 Azure Active Directory 所需与两个单独的系统集成。  我们现在推出了一个新的身份验证 API 版本，您可以使用两种类型的帐户使用 Azure 的广告系统的用户提供登录。  此混合身份验证系统被称为**v2.0 终结点**。  使用 v2.0 的终结点，一个简单的集成允许您更跨越数以百万计的个人和工作/学校帐户的用户的访问群体。

使用 v2.0 终结点的应用程序还可以使用 REST Api 从[Microsoft Graph](https://graph.microsoft.io)和[Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)使用任何类型的帐户。

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>入门教程
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

从下面的列表来构建应用程序，使用我们的开放源代码库和框架中选择您喜欢的平台。  或者，您可以使用 OAuth 2.0 和 OpenID 连接协议文档来发送和接收协议消息直接不使用身份验证库。

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>新增功能
概念的信息将有助于了解是什么和什么不是可能使用 2.0 版的终结点。

- 了解[您可以构建使用 2.0 版的终结点的应用程序的类型](active-directory-v2-flows.md)。
- 理解的[局限性、 限制和约束](active-directory-v2-limitations.md)使用 2.0 版的终结点。
- 我们最近已添加[管理员限制作用域](active-directory-v2-scopes.md)，并且[OAuth2 客户端凭据授予](active-directory-v2-protocols-oauth-client-creds.md)的支持。  试验一下 ！

## <a name="reference"></a>引用
这些链接将用于探索中深度的平台︰

- 生成 2016:[开始使用 Microsoft 身份︰ 中用于您的应用程序的企业级号](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- 获取有关堆栈溢出使用[azure 活动目录](http://stackoverflow.com/questions/tagged/azure-active-directory)的帮助或[adal](http://stackoverflow.com/questions/tagged/adal)标记。
- [v2.0 协议引用](active-directory-v2-protocols.md)
- [v2.0 令牌引用](active-directory-v2-tokens.md)
- [v2.0 库参考](active-directory-v2-libraries.md)
- [V2.0 终结点中的同意和作用域](active-directory-v2-scopes.md)
- [Microsoft 的应用程序注册门户](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST API 参考](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Microsoft Graph](https://graph.microsoft.io)