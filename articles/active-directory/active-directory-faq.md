<properties
    pageTitle="Azure 的 Active Directory 常见问题 |Microsoft Azure"
    description="为配合问题的解答提供了访问 Azure 和 Azure Active Directory 的 azure 活动目录常见问题解答密码管理和应用程序访问。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Azure 的 Active Directory 常见问题解答

Azure 的 Active Directory 是作为服务 (IDaaS) 的解决方案跨越的身份和访问管理安全所有方面的全面身份。


有关详细信息，请参阅[Azure Active Directory 是什么？](active-directory-whatis.md)。



## <a name="accessing-azure-and-azure-active-directory"></a>访问 Azure 和 Azure Active Directory


**问︰ 为何会收到"没有订阅找到"尝试访问在 Azure 传统门户网站 (https://manage.windowsazure.com) 的 Azure AD 时？**

**A:**访问 Azure 的经典门户要求每个用户订阅了 Azure 上具有权限。 如果您有支付的 Office 365 或 Azure 广告定位到[http://aka.ms/accessAAD](http://aka.ms/accessAAD)为一次性激活步骤，否则您将需要激活完整[Azure 试用](https://azure.microsoft.com/pricing/free-trial/)或付费的订阅。 

有关详细信息，请参阅︰

- [如何与 Azure Active Directory Azure 订阅](active-directory-how-subscriptions-associated-directory.md)

- [为 Azure 中订购 Office 365 管理目录](active-directory-manage-o365-subscription.md)

---

**问︰ 什么是 Azure 广告，之间的关系 Office 365 和 Azure？**

**A:**Azure 的 Active Directory 提供了到所有 Microsoft 在线服务的公共身份和访问功能。 无论您正在使用 Office 365、 Microsoft Azure，Intune 或其他人，已经在使用 Azure 的广告来启用登录和访问所有这些服务的管理。 

事实上，所有您已启用 Microsoft 在线服务的用户被定义为一个或多个 Azure 的广告实例中的用户帐户。 您可以启用这些帐户免费 Azure 的广告功能，如云应用程序的访问。
 
此外，Azure 广告支付服务 (例如︰ Azure AD 基本的特优、 EMS，等等) 补充其他在线服务，如 Office 365 和 Microsoft Azure 与综合的企业范围的管理和安全性解决方案。


---



## <a name="getting-started-with-hybrid-azure-ad"></a>要开始使用混合 Azure 的广告


**问︰ 如何将我的内部目录连接到 Azure 的广告？**

**A:**您可以连接到 Azure 广告使用**Azure AD 连接**的内部目录。 

有关更多详细信息，请参阅[您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。


---

**问︰ 怎样我内部目录和我的云应用程序之间设置 SSO？**

**A:**只需您的内部目录和 Azure 广告之间设置 SSO。 只要 Azure 的广告可以通过访问您的云应用程序，该服务将自动驱动器您正确使用他们内部的凭据进行身份验证的用户。

从内部实现 SSO 可以轻松实现与联合身份验证解决方案如 ADFS 或通过配置密码哈希同步。 您可以轻松地部署这两个选项使用 Azure AD 连接配置向导。
  

有关更多详细信息，请参阅[您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
  

---

**问︰ Azure Active Directory 自助服务门户为用户提供了在我的组织？**

**A:**是的 Azure Active Directory 提供了[Azure 广告接入面板](http://myapps.microsoft.com)为用户自助服务和应用程序访问。 如果您是 Office 365 客户，您可以找到许多相同的功能在 Office 365 门户。 

有关详细信息，请参阅[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。 



---

**问︰ Azure 广告可以帮助我管理我的内部部署基础结构？**

**A:**是的是的。 Azure 广告高级版为您提供**连接的运行状况**。 Azure AD 连接健康可以帮助您监视并深入内部身份基础结构和同步服务。  

有关详细信息，请参阅[监视您在内部标识基础结构和同步服务在云中](active-directory-aadconnect-health.md)。  

---

## <a name="password-management"></a>密码管理

**问︰ 是否可以使用回写的 Azure AD 密码而无需密码同步？（亦即想用密码写回使用 Azure AD SSPR 但我不希望我的密码存储在 cloud? 中）**

**A:**不需要同步到 Azure 广告广告密码以便启用写回。 在联合环境中，要对用户进行身份验证的内部目录依赖于 Azure AD SSO。 这种情况下不需要在 Azure AD 中跟踪的内部密码。

---

**问︰ 如何长时间写回本地 AD 的密码？**

**A:**在实时运行密码回写。 

有关详细信息，请参阅[密码管理入门](active-directory-passwords-getting-started.md) 


---

**问︰ 是否可以使用由管理员管理的密码与密码回写？**

**A:**是的如果您有启用写回的密码，由管理员执行的密码操作写回到您的内部环境。  

有关密码的详细解答相关的问题，请参阅[密码管理常见问题](active-directory-passwords-faq.md)。

---

## <a name="application-access"></a>应用程序访问


**问︰ 在哪里可以找到 Azure 广告与预先集成的应用程序的列表及其功能？**

**A:**Azure 的广告已经从 Microsoft、 应用程序服务提供商或合作伙伴超过 2600年预先集成的应用程序。 所有预先集成的应用程序支持 SSO。 SSO 可以使用您组织的凭据来访问您的应用程序。 某些应用程序还支持自动化的资源调配和消除资源调配

预集成的应用程序的完整列表，请参阅[活动目录市场](https://azure.microsoft.com/marketplace/active-directory/)。


---

**问︰ 要是我需要该应用程序不是 Azure 广告市场？**

**A:**使用 Azure AD 特优，您可以添加和配置所需的任何应用程序。 这取决于应用程序的功能和您的首选项，您可以配置 SSO 和自动化的资源调配。  

有关详细信息，请参阅︰

- [在 Azure Active Directory 应用库中的应用程序的单一登录配置](active-directory-saas-custom-apps.md)
- [如何使用 SCIM 以使用户和应用程序从 Azure 的 Active Directory 组的自动资源调配](active-directory-scim-provisioning.md) 


---

**问︰ 如何用户登录到应用程序中使用 Azure Active Directory？**
 
**A:**Azure 的 Active directory 提供了几种方法，用户可以查看和访问他们的应用程序，如︰

- Azure 广告接入面板

- Office 365 提供应用程序启动器

- 直接登录到联合应用程序

- 联合的、 基于密码的深层链接或现有应用程序

有关详细信息，请参阅[部署 Azure AD 集成到用户的应用程序](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)。


---

**问︰ 什么是 Azure Active Directory 的不同方式使身份验证和单一登录应用程序？**
 
**A:**Azure 的 Active Directory 进行身份验证和授权如 SAML 2.0、 连接 OpenID，OAuth 2.0 和 WS 联合身份验证支持许多标准化的协议。 Azure 的广告还支持保险存储的密码，自动登录功能仅支持基于表单的身份验证的应用程序。  

有关详细信息，请参阅︰

- [Azure AD 身份验证方案](active-directory-authentication-scenarios.md)

- [活动目录的身份验证协议](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [如何 does 单一登录使用 Azure Active Directory 工作吗？](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**问︰ 是否可以添加在运行内部部署的应用程序？**

**A:**Azure AD 应用程序代理为您提供轻松、 安全访问您选择的内部部署 web 应用程序。 以相同的方式访问您在 Azure Active Directory 的 SaaS 应用程序时，您可以访问这些应用程序。 没有需要 VPN 或更改您的网络基础结构。  

有关更多详细信息，请参阅[如何提供对内部应用程序的安全远程访问](active-directory-application-proxy-get-started.md)。


--- 

**问︰ 如何为用户访问特定的应用程序要求 MFA？**

**A:**Azure AD 条件访问，您可以分配每个应用程序的唯一访问策略。 在您的策略，您可以要求 MFA，在所有的时间，或用户没有连接到本地网络。  

有关详细信息，请参阅[Office 365 和其他应用程序连接到 Azure Active Directory 的保护权](active-directory-conditional-access.md)。


---

**问︰ 什么是自动化用户对于 SaaS 应用程序资源调配？**

**A:**Azure 的活动目录允许您自动执行创建、 维护和删除很多流行的云 (SaaS) 应用程序中的用户标识。 

有关详细信息，请参阅[自动化用户供应和 Deprovisioning 到 Azure Active Directory 的 SaaS 应用程序](active-directory-saas-app-provisioning.md)

---



