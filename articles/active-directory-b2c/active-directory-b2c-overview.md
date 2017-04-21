<properties
    pageTitle="Azure 的 Active Directory B2C︰ 概述 |Microsoft Azure"
    description="使用活动目录 B2C 的 Azure 开发面向消费者的应用程序"
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
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure 的活动目录 B2C︰ 注册并登录您的应用程序的使用者

Azure 的活动目录 B2C 是综合云标识管理解决方案，为您的面向客户的 web 和移动应用程序。 它是高度可用的全局服务，能够扩展到拥有数以亿计的消费者身份。 在企业级安全平台上构建，您的应用程序，您的业务和保护使用者保持 Azure 活动目录 B2C。

在过去，想要注册并登录到其应用程序的使用者应用程序开发人员就会编写自己的代码。 然后他们将使用内部数据库或系统中存储用户名和密码。 Azure 的活动目录 B2C 为开发人员提供了更好的方法，将使用者的身份管理集成到其应用程序的帮助下一个安全的、 基于标准的平台和一套丰富的可扩展的策略。 当您使用 Azure 活动目录 B2C 时，使用者可以注册您的应用程序通过使用其现有的社会帐户，Facebook、 Google、 Amazon （LinkedIn） 或创建新的凭据 （电子邮件地址和密码，或用户名和密码）;我们所说的后一种"本地帐户。"

## <a name="get-started"></a>入门

若要构建的应用程序接受使用者注册和登录，您将向应用程序注册 Azure 活动目录 B2C 的第一需要租户。 通过使用[创建 Azure AD B2C 租户](active-directory-b2c-get-started.md)中列出的步骤获取您自己的组织。

通过选择要发送的协议消息直接使用[OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow)或[打开 ID 连接](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow)，或通过使用我们的库来为您完成此工作，您可以编写对 Azure 活动目录 B2C 服务应用程序。 在下表中选择您最喜爱的平台并开始工作。

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>新增功能

在此处检查通常以了解将来对 Azure 活动目录 B2C 的更改。 我们将还 tweet 有关任何更新使用@AzureAD。

- 了解有关我们的[可扩展的策略框架](active-directory-b2c-reference-policies.md)和策略，您可以创建和使用应用程序中的类型。
- 添加书签的次要服务问题、 更新、 状态和缓解通知我们[服务的博客](https://blogs.msdn.microsoft.com/azureadb2c/)。 继续监视[Azure 状态仪表板](https://azure.microsoft.com/status/)也。
- 当前[服务限制、 限制和约束](active-directory-b2c-limitations.md)。
- 最后，[代码示例](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c)使用 Azure AD B2C 和 ASP.NET 核心。

## <a name="how-to-articles"></a>操作方法文章

了解如何使用 Azure 活动目录 B2C 的特定功能︰

- 面向消费者的应用程序中配置使用[Facebook](active-directory-b2c-setup-fb-app.md)、 [Google +](active-directory-b2c-setup-goog-app.md)、 [Microsoft 帐户](active-directory-b2c-setup-msa-app.md)、 [Amazon](active-directory-b2c-setup-amzn-app.md)和[LinkedIn](active-directory-b2c-setup-li-app.md)帐户。
- [使用自定义特性，以收集有关使用者的信息](active-directory-b2c-reference-custom-attr.md)。
- [面向消费者的应用程序中启用 Azure 的多因素验证](active-directory-b2c-reference-mfa.md)。
- [设置自助服务密码重置您的使用者](active-directory-b2c-reference-sspr.md)。
- [自定义注册，登录，外观和其他面向使用者的页面](active-directory-b2c-reference-ui-customization.md)所提供的 Azure 活动目录 B2C。
- 在 Azure 活动目录 B2C 租户[使用 Azure 活动目录图形 API 以编程方式创建、 读取、 更新和删除使用者](active-directory-b2c-devquickstarts-graph-dotnet.md)。

## <a name="next-steps"></a>下一步行动

这些链接将用于探索中深度的服务︰

- 请参阅[Azure 活动目录 B2C 定价信息](https://azure.microsoft.com/pricing/details/active-directory-b2c/)。
- 通过使用[azure 活动目录](http://stackoverflow.com/questions/tagged/azure-active-directory)获取有关堆栈溢出的帮助或[adal](http://stackoverflow.com/questions/tagged/adal)标记。
- 通过[用户的声音](https://feedback.azure.com/forums/169401-azure-active-directory/)给我们您的想法，我们想要听 ！ 使用短语"AzureADB2C:"您的帖子，以便我们可以找到它的标题中。
- 查看[Azure AD B2C 协议参考](active-directory-b2c-reference-protocols.md)。
- 查看[Azure AD B2C 令牌引用](active-directory-b2c-reference-tokens.md)。
- 阅读[Azure Active Directory B2C 常见问题解答](active-directory-b2c-faqs.md)。
- [支持活动目录 B2C 的 Azure 的请求的文件](active-directory-b2c-support.md)。

## <a name="get-security-updates-for-our-products"></a>我们的产品以获得安全更新

我们鼓励您能够访问[此页](https://technet.microsoft.com/security/dd252948)并订阅安全通报警告出现安全事件时的通知。
