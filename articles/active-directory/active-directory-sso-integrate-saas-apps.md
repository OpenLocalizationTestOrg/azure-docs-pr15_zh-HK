<properties
    pageTitle="SaaS 应用程序集成 Azure Active Directory 单一登录 | Microsoft Azure"
    description="启用单一登录身份验证和用户供应 Azure Active Directory 中的 SaaS 应用程序的集中式访问权限管理。 概述如何将 Azure Active Directory 集成到 SaaS 应用程序。"
    services="active-directory"
      keywords="将 Azure 广告与 SaaS 应用程序集成"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>SaaS 应用程序集成 Azure Active Directory 单一登录  

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-enterprise-apps-manage-sso.md)
- [Azure 的传统门户网站](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

首先要将您的组织的应用程序的单一登录设置，您将使用现有的目录在 Azure 活动目录 (AD Azure)。 您可以使用您获得通过 Microsoft Azure，Office 365 或 Windows Intune Azure 的广告目录。 如果您有两个或多个这些，请参阅[管理 Azure 的广告目录](active-directory-administer.md)来确定要使用哪一种。

## <a name="authentication"></a>身份验证

对于应用程序，支持 SAML 2.0 中，WS 联合身份验证或 OpenID 连接协议，Azure Active Directory 使用签名证书来建立信任关系。 有关详细信息，请参阅[管理联盟单一登录证书](active-directory-sso-certs.md)。

对于应用程序支持仅 HTML 基于表单的登录，Azure Active Directory 使用了密码保险存储来建立信任关系。 这使您的组织中的用户能够自动登录到 SaaS 应用程序通过 Azure 广告使用的 SaaS 应用程序的用户帐户信息。 Azure 的广告收集并安全地存储用户帐户信息和相关的密码。 有关详细信息，请参阅[基于密码的单一登录](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)。

## <a name="authorization"></a>授权

配置的帐户使用户获得授权之后他们已经通过单一登录身份验证应用程序的使用。 手动执行，或者在某些情况下，您可以添加和删除用户信息从 SaaS 应用程序基于 Azure Active Directory 中所做的更改，可以完成用户供应。 自动化资源调配使用现有 Azure AD 连接器的详细信息，请参阅[自动资源调配和消除资源调配的 SaaS 应用程序的用户](active-directory-saas-app-provisioning.md)。

否则为您可以手动将用户信息添加到应用程序中，或使用其他资源调配解决方案提供了市场上的。

## <a name="access"></a>访问

Azure 的广告提供了可自定义多种应用程序部署到您的组织中的最终用户。 不锁定任何特定部署或访问解决方案。 您可以使用[的最适合您需要的解决方案](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)。

## <a name="additional-considerations-for-applications-already-in-use"></a>其他注意事项的应用程序已在使用中

单一登录上设置为您的组织已经使用的应用程序是不同的进程中创建新帐户，新应用程序的过程。 有几个基本步骤包括︰ 映射到 Azure 广告标识应用程序中的用户标识和了解用户将会遇到登录到应用程序中集成之后。

> [AZURE.NOTE] 若要为现有的应用程序设置了 SSO，需要有两个 Azure AD 中的全局管理员权限和 SaaS 应用程序。

### <a name="mapping-user-accounts"></a>映射用户帐户

用户的标识通常有可能是电子邮件地址或用户主体名称 (UPN) 的唯一标识符。 您将需要链接 （图） 到它们各自的每个用户的应用程序标识 Azure 的广告标识。 有两种方法来实现此目的具体方式取决于您的应用程序身份验证要求。

有关映射与 Azure 广告标识的应用程序标识的详细信息，请参阅[发出 SAML 令牌中的自定义声明](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx)和[自定义属性映射的资源调配](active-directory-saas-customizing-attribute-mappings.md)。

### <a name="understanding-the-users-log-in-experience"></a>了解用户的登录体验

您正在使用的应用程序集成了 SSO，时，一定要认识到用户体验将会受到影响。 对于所有应用程序，用户将开始使用 Azure AD 凭据登录。 它也可能是他们必须使用不同的门户来访问应用程序。

对于某些应用程序的 SSO 可以在应用程序的登录界面，但是对于其他应用程序，用户将需要经过中央门户网站 （[我的应用程序](http://myapps.microsoft.com)或[Office365](http://portal.office.com/myapps)） 登录等。 了解有关不同类型的 SSO 和[什么是应用程序访问权限和单一登录使用 Azure Active Directory](active-directory-appssoaccess-whatis.md)中的用户体验。

另一个有价值的资源是*Suppressing 用户同意*在[Guiding 开发](active-directory-applications-guiding-developers-for-lob-applications.md)项目。

## <a name="next-steps"></a>下一步行动


对于 SaaS 应用程序的应用程序库中查找，Azure 广告提供了大量[有关如何集成 SaaS 应用程序的教程](active-directory-saas-tutorial-list.md)。

如果应用程序不是在应用程序库中，则可以[将其添加到 Azure AD 应用程序库中作为自定义应用程序](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)。

还有所有从开始在 Azure.com 库中，这些问题的更多详细信息[应用程序访问权限和单一登录使用 Azure Active Directory。](active-directory-appssoaccess-whatis.md)。

## <a name="see-also"></a>请参见

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
