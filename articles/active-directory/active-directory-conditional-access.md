<Properties
    pageTitle="Azure Active Directory 条件访问 |Microsoft Azure"  
    description="使用 Azure Active Directory 中条件访问控制用于访问应用程序进行身份验证时检查有特定的条件。"  
    services="active-directory"
    keywords="对应用程序、 使用 Azure AD 的条件访问、 安全地访问公司资源，条件访问策略的条件访问"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>在 Azure Active Directory 中的条件访问   

在 Azure 活动目录 (AD Azure) 条件访问控制功能提供了有助于保护资源中的云和内部的简单方法。 像多因素身份验证可帮助防止风险的条件访问策略被盗和 phished 凭据。 其他条件访问策略可帮助确保您组织的数据安全。 例如，除了要求凭据，您可能有一个策略像 Microsoft Intune 可以访问您的组织的敏感的服务的移动设备管理系统中登记的唯一设备。


## <a name="prerequisites"></a>系统必备组件

Azure AD 条件访问是[Azure 活动目录津贴](http://www.microsoft.com/identity)的功能。 每个用户都可以访问的应用程序有应用的条件访问策略必须 Azure 广告增值许可证。 您可以了解有关许可的要求，在[未经授权的用户报告](https://aka.ms/utc5ix)。


## <a name="how-is-conditional-access-control-enforced"></a>条件的访问控制是如何实施的？  

到位的条件访问控制，请使用 Azure 广告检查特定条件为用户访问应用程序设置。 访问要求都满足之后，用户已经过身份验证，并且可以访问该应用程序。  

![条件访问概述](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>条件

这些是可以包含条件访问策略中的条件︰

- **组成员身份**。 控件基于组成员身份的用户的访问。

- **位置**。 使用触发器多因素身份验证，用户的位置和使用块控件，当用户不在受信任的网络上。

- **设备平台**。 设备平台，如 iOS、 Android、 Windows Mobile 或 Windows，用作应用策略的条件。

- **已启用设备的**。 设备的状态，无论启用还是禁用，验证设备策略评估过程。 如果您禁用目录中的丢失或被盗设备，它不再能够满足策略要求。

- **登录和用户的风险**。 可以使用[Azure AD 身份保护](active-directory-identityprotection.md)的条件访问风险策略。 条件接收风险策略帮助您组织高级保护基于风险事件和特殊符号中的活动。


## <a name="controls"></a>控件

这些都是控件，可用于实施条件访问策略︰

- **多因素身份验证**。 您可以要求强身份验证通过多因素身份验证。 您可以使用多因素身份验证，使用 Azure 多因素身份验证也可通过使用内部多因素身份验证提供程序，结合 Active Directory 联合身份验证服务 (AD FS)。 使用多因素身份验证可帮助防止未经授权的用户可能获取了访问权限的有效用户的凭据访问资源。

- **块**。 您可以应用类似用户位置，以阻止用户访问的情况。 例如，当用户不在受信任的网络上可以阻止访问。

- **兼容设备**。 您可以在设备级别设置条件的访问策略。 这样只是加入域的计算机或移动设备的移动设备管理应用程序中注册可以访问您的组织的资源，您可能设置策略。 例如，可以使用 Intune 来检查设备法规遵从性，然后报告到 Azure 广告对于强制用户试图访问应用程序时。 有关如何使用 Intune 来保护应用程序和数据的详细指导，请参阅[保护应用程序和数据使用 Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune)。 您还可以使用 Intune 强制丢失或被盗设备的数据保护。 有关详细信息，请参阅[帮助保护您的数据与使用 Microsoft Intune 完整或选择性擦除](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)。

## <a name="applications"></a>应用程序

在应用程序级别的条件访问策略，可以强制。 在云或内部设置应用程序和服务的访问级别。 该策略会直接应用于网站或服务。 实施策略的浏览器，和应用程序访问该服务的访问。


## <a name="device-based-conditional-access"></a>基于设备的条件访问

从设备的注册 Azure 的广告，并符合特定条件，可以对应用程序限制访问。 基于设备的条件访问防止用户试图访问的资源的组织的资源︰

- 未知的或非托管设备。
- 设备不能满足安全策略，您的组织设置。

您可以设置策略根据这些要求︰

- **加入域的设备**。 设置限制访问的设备连接到内部部署 Active Directory 域，和，也都注册 Azure 的广告策略。 此策略适用于 Windows 台式机、 笔记本电脑和平板电脑企业。
有关如何设置使用 Azure AD 域内设备的自动注册的详细信息，请参阅[设置自动注册的 Windows Azure Active Directory 域内的设备](active-directory-conditional-access-automatic-device-registration-setup.md)。

- **兼容设备**。 设置策略以限制访问标记**符合**管理系统目录中的设备。 此策略可确保只有符合安全策略，如实施设备上的文件加密的设备允许访问。 可以使用此策略来限制访问从以下设备︰

    - **Windows 加入域的设备**。 管理通过系统中心配置管理器 （在当前分支） 部署在混合配置。
    - **Windows 10 移动工作或者个人的设备**。 管理通过 Intune 或受支持的第三方移动设备管理系统。
    - **iOS 和 Android 设备**。 由 Intune 进行管理。


访问受保护的基于设备的应用程序的用户，证书颁发机构策略必须符合此策略要求的设备中访问应用程序。 如果试图访问被拒绝不符合策略要求的设备上。

有关如何在 Azure AD 配置基于设备的证书颁发机构策略的信息，请参阅[设置 Azure Active Directory 连接应用程序的基于设备的条件访问策略](active-directory-conditional-access-policy-connected-applications.md)。

## <a name="resources"></a>资源

请参阅以下资源类别和文章，以了解更多有关设置为您的组织的条件访问。


### <a name="multi-factor-authentication-and-location-policies"></a>多因素身份验证和位置策略

- [入门条件对 Azure AD 连接应用程序基于组、 位置和多因素身份验证策略](active-directory-conditional-access-azuread-connected-apps.md)
- [受支持的应用程序](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>基于设备的条件访问

- [对 Azure Active Directory 连接的应用程序设置的访问控制基于设备的条件访问策略](active-directory-conditional-access-policy-connected-applications.md)
- [设置自动注册的 Windows Azure Active Directory 域内的设备](active-directory-conditional-access-automatic-device-registration-setup.md)
- [对 Azure Active Directory 访问问题进行故障诊断](active-directory-conditional-access-device-remediation.md)
- [帮助保护通过使用 Microsoft Intune 丢失或被盗设备上的数据](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>保护资源根据签到风险

-   [Azure AD 身份保护](active-directory-identityprotection.md)

### <a name="next-steps"></a>下一步行动

- [条件接收常见问题解答](active-directory-conditional-faqs.md)
- [技术参考](active-directory-conditional-access-technical-reference.md)
