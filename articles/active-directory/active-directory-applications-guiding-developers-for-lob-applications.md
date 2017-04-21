<properties
    pageTitle="Azure 的 AD 和应用程序︰ 指导开发 |Microsoft Azure"
    description="这篇文章面向 IT 专业人员，为与 Active Directory 集成 Azure 应用程序提供指导。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure 的 AD 和应用程序︰ 开发业务线应用程序

本指南概述了开发业务线 (LoB) 应用程序的 Azure 活动目录 (AD)。目标的读者是活动目录/Office 365 的全局管理员。

## <a name="overview"></a>概述

构建与 Azure AD 集成的应用程序为用户提供在您的组织单一登录与 Office 365。 让在 Azure 的广告让您控制应用程序的身份验证策略的应用程序。 要了解更多有关有条件的访问以及如何保护应用程序使用多因素身份验证 (MFA)，请参阅[配置访问规则](active-directory-conditional-access-azuread-connected-apps.md)。

注册应用程序以使用 Azure 活动目录。 注册应用程序意味着开发人员可以使用 Azure 广告对用户进行身份验证并请求访问用户资源，例如电子邮件、 日历和文档。

目录 （而不是客人） 的任何成员都可以注册应用程序，又称为*创建应用程序对象*。

注册应用程序允许任何用户执行下列操作︰

- Azure 广告认识到其应用程序获取标识
- 获取一个或多个秘密/键应用程序可用于对 AD 进行自身身份验证
- 品牌在 Azure 使用自定义名称、 徽标、 等门户应用程序。
- 将 Azure 广告授权功能应用到他们的应用程序，其中包括︰
  - 基于角色的访问控制 (RBAC)
  - Azure 作为 oAuth 授权服务器的 Active Directory （安全由应用程序公开的 API）

- 声明所需的权限运行的应用程序需要像预期的那样，包括:-应用程序的权限 （仅适用于全局管理员）。 例如︰ 另一个 Azure AD 应用程序或角色成员资格中相对于 Azure 资源、 资源组或订阅-委派权限 （任何用户） 的角色成员资格。 例如︰ Azure 的广告，签入和读取配置文件


> [AZURE.NOTE]默认情况下，任何成员可以注册应用程序。 若要了解如何注册到特定成员的应用程序的权限，请参阅[如何将应用程序添加到 Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance)。

下面是全局管理员，您需要执行操作来帮助开发人员使他们的应用程序可供生产︰

- 配置访问规则 (访问策略/MFA)
- 配置应用程序需要用户分配并分配用户
- 取消默认用户同意的情况下体验

## <a name="configure-access-rules"></a>配置访问规则

配置每个应用程序访问 SaaS 应用程序的规则。 例如，可以要求 MFA 或仅允许受信任的网络上的用户访问。 文档[配置访问规则](active-directory-conditional-access-azuread-connected-apps.md)中有对此详细信息。

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>配置应用程序需要用户分配并分配用户

默认情况下，用户可以访问应用程序，而不需要分配。 但是，如果应用程序公开的角色或者您想要显示在用户访问权面板上的应用程序，您应该要求用户分配。

[要求用户分配](active-directory-applications-guiding-developers-requiring-user-assignment.md)

如果您是 Azure AD 津贴或企业移动套件 (EMS) 的订阅服务器，我们强烈建议使用组。 将组分配到该应用程序允许您委派组的所有者进行访问的权限管理。 可以创建组，也可以在您的组织创建的组中使用您的组管理功能要求责任方。

[将用户分配到应用程序](active-directory-applications-guiding-developers-assigning-users.md)  
[将组分配到应用程序](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>禁止用户同意的情况下

默认情况下，每个用户所经历的同意的情况下体验可登录。 同意的情况下体验，让用户向应用程序授予权限可能对于不熟悉做出此类决定的用户带来不便。

对于您信任的应用程序，可以通过同意代表您的组织的应用程序来简化用户体验。

有关用户同意和同意的情况下体验 Azure 中的详细信息，请参阅[使用 Azure Active Directory 集成应用程序](active-directory-integrating-applications.md)。

##<a name="related-articles"></a>相关的文章

- [启用对 Azure AD 应用程序代理服务器的内部应用程序的安全远程访问](active-directory-application-proxy-get-started.md)
- [Azure 的条件访问 SaaS 应用程序的预览](active-directory-conditional-access-azuread-connected-apps.md)
- [管理对 Azure AD 的应用程序的访问](active-directory-managing-access-to-apps.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
