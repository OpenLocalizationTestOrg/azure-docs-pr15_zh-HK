<properties
    pageTitle="共享帐户使用 Azure 广告 | Microsoft Azure"
    description="介绍了 Azure Active Directory 如何使组织能够安全地共享内部部署的应用程序和云服务使用者帐户。"
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>与 Azure 广告共享帐户

## <a name="overview"></a>概述
有时，企业需要为多个用户使用单一的用户名和密码。 这通常发生在两种情况︰

- 在访问每个用户需要的唯一的登录和密码的应用程序时，是否内部部署的应用程序或使用者云服务 （例如公司的社交媒体帐户）。
- 当创建多用户环境。 您可能必须一单个的本地帐户，具有更高的特权，可以用于执行核心安装、 管理和恢复活动 （例如 Office 365 的本地"全局管理员"帐户） 或超级用户帐户在队伍中。

传统上，这些帐户将共享分发给适当的人员的凭据 （用户名/密码） 或将它们存储在多个受信任的代理可以访问它们的共享位置。

传统的共享模式有几个缺点︰

- 新的应用程序访问，从而要求分发给每个人都需要访问权限的凭据。
- 每个共享的应用程序可能需要自己唯一的一组共享凭据，要求用户需要记住多个凭据集。 当用户需要记住多个凭据时，会增大风险，他们会求助于危险的做法。 （如写下密码）。
- 您不能判断谁有权访问应用程序。
- 不知道谁可以*访问*应用程序。
- 当您需要删除对应用程序的访问时，您必须更新凭据，然后重新将其分发给所有需要访问该应用程序。

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory 帐户共享

Azure 的广告提供了一种新方法使用共享的帐户，以消除这些缺点。

Azure AD 管理员配置的应用程序，用户可以通过使用访问权限面板并选择单个登录最佳的类型访问适合于该应用程序。 属于这些类型，*基于密码的单点登录*，可以 Azure 广告，应用程序的登录过程中充当一种"经纪人"。

用户登录一次使用其组织的帐户。 这是他们经常使用以访问其桌面或电子邮件的同一个帐户。 他们可以发现和访问分配给他们的那些应用程序。 共享帐户，此应用程序的列表可以包含任意数量的共享凭据。 最终用户不需要记住或记下他们可能使用的各种科目。

共享的帐户不仅增加监督并提高可用性，它们还增强了安全性。 有权使用的凭据的用户看不到共享的密码，但而获得权限使用的密码，这些都是身份验证流的一部分。 此外，某些密码 SSO 应用程序，可以选择具有 Azure 广告定期变换 （更新） 使用大型、 复杂的密码，增加帐户安全的密码。 管理员可以授予或废除对应用程序的访问，也知道谁有权访问该帐户和谁在过去访问。

Azure 的广告支持共享的帐户的任何企业移动套件 (EMS)，津贴或基本的授权的用户，跨所有类型的单个的密码登录应用程序。 您可以共享数以千计的预集成的应用程序，应用程序库中的任何帐户，可以添加自己的密码进行身份验证的应用程序使用[自定义的 SSO 应用程序](active-directory-sso-integrate-saas-apps.md)。

启用帐户共享的 azure AD 功能包括︰

- [密码单一登录](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- 密码单一登录代理
- [组分配](active-directory-accessmanagement-self-service-group-management.md)
- 自定义密码的应用程序
- [应用程序使用情况仪表板/报告](active-directory-passwords-get-insights.md)
- 最终用户访问门户网站
- [应用程序代理](active-directory-application-proxy-get-started.md)
- [Active Directory 市场](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>共享帐户
要使用 Azure 广告共用帐户，您将需要︰

- 添加应用程序[的应用程序库](https://azure.microsoft.com/marketplace/active-directory/)或[自定义应用程序](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- 配置应用程序的单一登录 (SSO) 的密码
- 使用[基于分配的组](active-directory-accessmanagement-group-saasapps.md)，然后选择输入共享的凭据选项
- 可选︰ 在某些应用程序，例如 Facebook、 Twitter 或 LinkedIn，您可以启用[Azure 广告自动的密码滚动](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)的选项

此外可以使您共享的帐户更安全的多因素身份验证 (MFA) （了解更多有关[使用 Azure 广告的固定应用程序](../multi-factor-authentication/multi-factor-authentication-get-started.md)） 和您可以委派管理谁有权使用[Azure AD 自助式](active-directory-accessmanagement-self-service-group-management.md)组管理的应用程序的能力。

## <a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [保护应用程序具有访问权限的条件](active-directory-conditional-access.md)
- [自助服务组管理/SSAA](active-directory-accessmanagement-self-service-group-management.md)
