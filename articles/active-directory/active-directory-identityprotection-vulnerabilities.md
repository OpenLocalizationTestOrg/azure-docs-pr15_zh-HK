<properties
    pageTitle="安全漏洞检测到 Azure 活动目录标识保护 |Microsoft Azure"
    description="检测到的 Azure 活动目录身份保护的安全漏洞的概述。"
    services="active-directory"
    keywords="azure 的活动目录身份保护，云应用程序发现，管理应用程序、 安全性、 风险、 风险程度、 漏洞、 安全策略"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>通过 Azure 活动目录标识保护检测到的漏洞 

安全漏洞的攻击者可以利用您环境中的弱点。 我们建议您解决这些漏洞来提高您的组织的安全状况，防止攻击者利用这些漏洞。
<br><br>
![安全漏洞](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

以下各节将向您提供身份保护所报告的安全漏洞的概述。

## <a name="multi-factor-authentication-registration-not-configured"></a>未配置的多因素身份验证注册 

此漏洞可以帮助您控制您的组织中部署的 Azure 多因素身份验证。 

Azure 的多因素身份验证提供第二层的安全用户身份验证。 它有助于保护数据和应用程序的访问权限同时满足一个简单的登录过程的用户要求。 它提供了强身份验证通过一系列简单的验证选项 — — 电话、 短信或移动应用程序通知或验证代码和第三方誓言令牌。

我们建议，要求 Azure 多因素身份验证的用户登录。 多因素身份验证可通过身份保护的基于风险的条件访问策略中扮演着关键角色。

有关详细信息，请参阅[Azure 多因素身份验证是什么？](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>非托管的云应用程序

此漏洞可帮助您确定您的组织中的非托管的云应用程序。
 
在现代企业中，IT 部门往往没有意识到组织中的用户使用以完成其工作的所有云应用程序。 很容易看出为什么管理员必须顾虑到公司数据、 可能的数据泄漏和其他安全威胁的未经授权的访问。 

我们建议您的组织部署发现非托管的云应用程序，以及管理这些应用程序使用 Azure Active Directory 的云应用程序发现。

有关详细信息，请参阅[查找非托管的云与云应用程序查询的应用程序](active-directory-cloudappdiscovery-whatis.md)。



##<a name="security-alerts-from-privileged-identity-management"></a>从特权的身份管理的安全警报

此漏洞可帮助您发现并解决您组织中的特权身份有关的警报。  

若要使用户能够执行特权操作，组织需要在 Azure 广告，授予用户临时的或永久的特权的访问 Azure 或 Office 365 提供资源或其他 SaaS 应用程序。 这些特权的用户会增加您的组织的攻击面。 此漏洞可帮助您识别用户使用不必要的特权的访问，并采取适当的措施，以减少或消除它们带来的风险。 

我们建议您的组织使用 Azure AD 特权身份管理来管理、 控制，并监视特权身份和他们对 Azure AD 中的资源的访问，以及如 Office 365 或 Microsoft Intune 其他 Microsoft 在线服务。

有关更多详细信息，请参阅[Azure AD 特权身份管理](active-directory-privileged-identity-management-configure.md)。 



## <a name="see-also"></a>请参见

 - [Azure 的 Active Directory 身份保护](active-directory-identityprotection.md)
