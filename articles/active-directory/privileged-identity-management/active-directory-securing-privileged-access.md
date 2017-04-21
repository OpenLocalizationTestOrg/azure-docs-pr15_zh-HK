<properties
    pageTitle="在 Azure AD 中保护特权的访问 |Microsoft Azure"
    description="解释在 Azure，Azure Active Directory 和 Microsoft Online Services 保护特权的访问方法的主题。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>在 Azure AD 中保护特权的访问

保护的特权的访问是重要的第一步，来帮助保护在现代组织中的业务资产。 特权的帐户是管理和管理 IT 系统。 网络攻击者针对这些帐户才能访问组织的数据和系统。 为了保护安全访问权限，应将隔离的帐户和系统风险的暴露给恶意用户。

更多的用户也开始通过云服务获得授权访问权限。 这可以包括 Office365 的全局管理员、 Azure 预订管理员和在虚拟机中或在 SaaS 应用程序具有管理访问权限的用户。

Microsoft 建议您按照此路线图上[保护特权访问](https://technet.microsoft.com/library/mt631194.aspx)。

对于使用 Azure Active Directory 管理 Azure，Office 365 或其他 Microsoft 服务和应用程序访问客户，无论管理和身份验证通过 Active Directory 或 Azure Active Directory 中的用户帐户都适用这些原则。 以下各节提供在 Azure 的广告功能，以支持保护特权的访问的详细信息。

## <a name="multi-factor-authentication"></a>多因素身份验证

若要提高管理员的身份验证的安全性，您应该要求多因素身份验证 (MFA) 之前授予访问权限。 MFA 是种方法来验证您的身份，需要使用多个用户名和密码。 提供第二层的用户登录和交易的安全。

Azure 多因素身份验证可帮助保护访问数据和应用程序同时还可以满足一个简单的登录过程的用户要求。 它提供了通过一系列简单的验证选项，包括电话、 短信、 移动应用程序的通知，或验证代码和第三方誓言令牌的强身份验证。

Azure 多因素身份验证的工作原理的概述，请参阅下面的视频。

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

有关更多详细信息，请参阅[Office 365 和 Azure 的 MFA MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/)。

## <a name="time-bound-privileges"></a>时间限制权限

某些组织可能会发现它们在高度特权的角色中有太多的用户。 用户可能已添加到的角色的特定活动，如注册一项服务，但不经常以后使用这些权限。

降低的特权的曝光时间，提高看不到它们的使用，限制用户仅从事实时 (JIT)，只是他们的特权，当用户需要执行某项任务时。 对于 Azure Active Directory 和 Microsoft Online Services，您可以使用[Azure AD 特权身份管理 (PIM)](http://aka.ms/AzurePIM)。


![PIM 仪表板][2]


## <a name="attack-detection"></a>攻击检测

[Azure 活动目录身份信息保护](../active-directory-identityprotection.md)提供了一个整合的视图到风险事件和潜在的安全漏洞影响您的组织的身份。 基于风险事件，身份保护计算用户风险级别为每个用户，使您能够配置基于风险的策略，以自动保护您的组织的身份。 这些策略，以及其他 Azure Active Directory 和 EMS，所提供的条件访问控制可以自动阻止用户或提供包括密码重置和多因素身份验证实施的建议。

![Azure AD 身份保护][3]

## <a name="conditional-access"></a>条件接收

有条件的访问控制，Azure Active Directory 检查进行用户身份验证才允许访问应用程序时，您选择的特定条件。 一旦满足了这些条件，是经过身份验证的用户，并允许应用程序访问。


![设置与 MFA 的条件访问规则][4]


## <a name="related-articles"></a>相关的文章

- 启用[Azure 的多因素身份验证](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
- 启用[Azure AD 特权身份管理](../active-directory-privileged-identity-management-configure.md)
- 启用[Azure 广告标识保护](../active-directory-identityprotection.md)
- 启用[条件访问控制](../active-directory-conditional-access.md)


构建一个完整的安全路线图的详细信息，请参阅[Microsoft 企业架构师的云安全](http://aka.ms/securecustomer)文档的"客户责任和指南"部分。 利用 Microsoft 服务来协助这些主题的详细信息，请与您的 Microsoft 代表联系或访问我们的[网络安全解决方案页面](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)。

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
