<properties
   pageTitle="如何将与 Azure Active Directory 集成 |Microsoft Azure"
   description="好处和资源与 Azure Active Directory 集成指南。"
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

# <a name="integrating-with-azure-active-directory"></a>将与 Azure Active Directory 集成

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure 的 Active Directory 提供了企业级身份管理云应用程序的组织。  Azure 广告集成为您的用户提供了简化的注册程序体验，并帮助您符合 IT 策略的应用程序。

## <a name="how-to-integrate"></a>如何集成

有几种方法，应用程序将与 Azure AD 集成。  充分利用尽可能多或尽可能少的这些方案是适合您的应用程序。

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>支持登录到您的应用程序对 Azure 广告

**减少摩擦的登录，并降低支持成本。** 通过使用 Azure 的广告来登录到您的应用程序，您的用户将不会有一个更多名和密码记住。  作为开发人员，您将拥有一个更少密码存储和保护。  无需处理遗忘的密码重置可能就明显减少。  Azure 的广告对某些世界最受欢迎云的应用程序，包括 Office 365 和 Microsoft Azure 电源中的符号。  与数亿从数以百万计的企业用户，很有可能您的用户已经登录到 Azure 的广告。  了解有关[添加支持在 Azure 的广告标志](../active-directory-authentication-scenarios.md)。

**为您的应用程序向上简化符号。**  期间注册了您的应用程序，Azure 的广告可以发送用户的基本信息，以便可以预先填充您的注册表格，也可以完全消除它。  使用类似于那些在社交媒体和移动应用程序中找到的熟悉同意体验通过其 Azure 的广告帐户应用程序，用户可以注册。  任何用户可以注册并登录到应用程序而无需 IT 部门参与与 Azure AD 集成。  了解有关[签名幅 Azure AD 帐户登录应用程序](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)。

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>浏览用户，管理用户供应，并控制对您的应用程序的访问

**浏览目录中的用户。**  使用图形 API 来帮助用户搜索和浏览他们的组织中的其他人时邀请其他人或授予访问权限，而不必键入电子邮件地址。  用户可以浏览使用熟悉的地址簿样式界面，包括查看组织层次结构的详细信息。  了解有关[图形 API](../active-directory-graph-api.md)的详细信息。

**重新使用 Active Directory 组和通讯组列表已管理您的客户。**  Azure 的广告包含您的客户已经被用于电子邮件分发和管理访问权限的组。  通过使用图形 API，重新使用这些组而不是要求您的客户能够创建和管理一组单独的应用程序中的组。  组信息还可以发送到您的应用程序登录令牌。  了解有关[图形 API](../active-directory-graph-api.md)的详细信息。

**使用 Azure 广告控制有权访问您的应用程序。**  管理员和应用程序所有者在 Azure 广告可以向应用程序对特定用户和组分配访问。  通过使用图形 API，可以读取此列表并使用它来控制资源调配和调配资源，消除和访问应用程序中。

**使用 Azure 广告角色基于访问控制。**  管理员和应用程序所有者可以给用户和组定义 Azure AD 中注册应用程序时的角色。  角色信息发送到您的应用程序登录令牌，还可以使用图形 API 读取。  了解有关[使用 Azure 广告进行授权](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx)。

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>有权访问用户配置文件、 日历、 电子邮件、 联系人、 文件和更多

**Azure 的广告是 Office 365 和其他 Microsoft 的业务服务授权服务器。**  如果您登录到您的应用程序或将您当前的用户帐户链接到 Azure AD 用户帐户使用 OAuth 2.0 支持 Azure 的广告支持，您可以请求读和写访问权限的用户配置文件、 日历、 电子邮件、 联系人、 文件和其他信息。  无缝可以写入到用户的日历事件和读取或写入其 OneDrive 文件。  了解有关[访问 Office 365 Api](https://msdn.microsoft.com/office/office365/howto/platform-development-overview)的详细信息。

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>将升级您的应用程序中的 Azure 和 Office 365 的市场

**升级您的应用程序已经在使用 Azure AD 的组织的数以百万计。**  搜索和浏览这些市场的用户已经在使用一个或多个云服务，使它们成为合格的云服务客户。  了解有关升级您的应用程序在[Azure 的市场上](https://azure.microsoft.com/marketplace/partner-program/)。

**当用户注册应用程序时，它将出现在他们的 Azure 广告接入面板和 Office 365 提供应用程序启动程序。**  用户将能够快速、 轻松地返回到您的应用程序以后，改善用户服务。  了解有关[访问面板，请 Azure 的广告](../active-directory-saas-access-panel-introduction.md)。

### <a name="secure-device-to-service-and-service-to-service-communication"></a>设备与服务和服务的服务安全的通讯

**使用 Azure 广告有关的服务和设备的标识管理减少了需要编写的代码，使 IT 管理访问。**  服务和设备可以从 Azure 使用 OAuth 的广告获取令牌，并使用这些标记来访问 web Api。  使用 Azure 广告就可以避免编写复杂的身份验证代码。  由于服务和设备的标识存储在 Azure 广告，IT 可以管理密钥和吊销在一个地方而无需在您的应用程序单独执行此操作。

## <a name="benefits-of-integration"></a>集成的好处

与 Azure AD 集成附带好处，不需要您编写额外的代码。

### <a name="integration-with-enterprise-identity-management"></a>与企业标识管理的集成

**可帮助您遵守 IT 策略的应用程序。**  组织将其企业标识管理系统集成使用 Azure 的广告，因此，当一个人离开一个组织，他们将自动无法访问的应用程序，而无需执行额外的步骤的 IT。  IT 可以管理谁可以访问您的应用程序和确定哪些访问策略所需的示例多因素身份验证-减少您需要编写代码以遵守复杂的公司政策。  Azure 广告为管理员提供的登录到您的应用程序如此详细的审核日志 IT 可以跟踪使用情况。

**Azure 的广告将 Active Directory 扩展到云，以便您的应用程序可以与 AD 集成。**  世界各地的许多组织使用活动目录作为其主体的登录和身份管理系统，并要求他们的应用程序来处理的广告。  将与 Azure AD 集成与 Active Directory 集成您的应用程序。

### <a name="advanced-security-features"></a>高级的安全功能

**多因素身份验证。**  Azure 的广告提供了本机的多因素身份验证。  IT 管理员可以要求多因素身份验证才能访问您的应用程序，这样您不需要您自己的代码这种支持。  了解有关[多因素身份验证](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)的详细信息。

**在检测中的反常号。**  Azure AD 处理超过十亿号接一天，同时使用机器学习算法来检测可疑活动，并通知给 IT 管理员可能出现的问题。  通过支持 Azure 广告登录，您的应用程序获取这种保护的好处。 了解更多有关[查看 Azure Active Directory 访问报告](../active-directory-view-access-usage-reports.md)。

**条件的访问。**  管理员可以要求除了多因素身份验证之前用户可以登录到您的应用程序满足特定条件。  可以设置的条件包括客户端设备、 指定的组中的成员身份和用于访问设备的状态的 IP 地址范围。  了解更多关于[Azure Active Directory 条件访问](../active-directory-conditional-access.md)。

### <a name="easy-development"></a>更轻松的开发

**行业标准协议。**  Microsoft 致力于支持行业标准。  Azure 的广告支持的 SAML 2.0，OpenID 连接 1.0，OAuth 2.0 和 1.2 WS 联合身份验证的身份验证协议。  Graph API 是 OData 4.0 兼容。  如果应用程序已经为联合签名支持 SAML 2.0 或 OpenID 连接 1.0 协议，可以直接添加支持 Azure 的广告。  了解更多有关[支持的 Azure AD 身份验证协议](../active-directory-authentication-protocols.md)。

**开放源代码库。**  Microsoft 提供的流行语言和平台，再到加快开发完全受支持的开源库。  源代码 Apache 2.0 的许可，您可以自由地分叉和参与项目。  了解更多关于[Azure AD 身份验证库](../active-directory-authentication-libraries.md)。

### <a name="worldwide-presence-and-high-availability"></a>全球存在和高可用性

**Azure 的广告在世界各地的数据中心部署和管理和全天候监视。**  Azure 广告是 Microsoft Azure 和 Office 365 的身份管理系统，和部署在世界各地的 28 数据中心。  保证目录数据复制到至少三个数据中心。  全局负载平衡器确保最近的包含其数据，Azure AD 副本的用户访问权限，如果检测到问题时自动重新路由到其他数据中心的请求。

## <a name="next-steps"></a>下一步行动

[开始编写代码](../active-directory-developers-guide.md#getting-started)。

[注册用户中使用 Azure 的广告](../active-directory-authentication-scenarios.md)
