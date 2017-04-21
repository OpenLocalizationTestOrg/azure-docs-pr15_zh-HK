<properties
    pageTitle="Office 365 提供服务的条件访问设备策略 |Microsoft Azure"
    description="基于设备的状态的详细信息来控制访问 Office 365 提供服务。 信息工作者 (Iw) 从他们的个人设备需要访问 Office 365 提供服务，如交换和 SharePoint Online 工作或学校，而其 IT 管理员想要 secure.IT 管理员可以设置条件访问设备策略，以保护公司的资源，而同时允许 Iw 上兼容的设备来访问这些服务的访问。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Office 365 提供服务的条件访问设备策略

"条件访问"一词，有很多条件与其关联等多因素身份验证的用户进行身份验证设备，符合标准的设备等。本主题主要是基于设备的条件，来控制访问 Office 365 提供服务上 focusses。 信息工作者 (Iw) 从他们的个人设备需要访问 Office 365 提供服务，如交换和 SharePoint Online 工作或学校，而其 IT 管理员想要安全的访问。 IT 管理员可以设置条件访问设备策略，以确保公司的资源，而同时允许兼容的设备来访问这些服务上的信息工作者的安全。 可以从 Microsoft Intune 条件访问门户配置到 Office 365 的条件访问策略。

Azure 的 Active Directory 实施条件访问策略，以确保安全访问 Office 365 提供服务。 租户管理员可以创建的条件访问策略，将阻止用户在访问 O365 服务不兼容的设备上。 可授予访问权的服务之前，用户必须符合公司的设备策略。 或者，管理员还可以创建一个策略，它要求用户只需注册其设备，以便访问 O365 服务。 策略可能会应用于所有用户的组织，或限于几个目标组和增强随时间以便包含更多的目标组。

实施设备策略的先决条件是用户在使用 Azure 活动目录设备注册服务注册其设备。 您可以选择要启用的 Azure 活动目录设备注册服务注册设备的多因素身份验证 (MFA)。 MFA Azure 活动目录设备注册服务的建议。 MFA 启用时，用户使用 Azure 活动目录设备注册服务注册其设备面临的第二个因素的身份验证。

##<a name="how-does-conditional-access-policy-work"></a>条件接收策略是如何工作的？

当用户请求访问 O365 服务从支持的设备平台时，Azure Active Directory 进行身份验证的用户和设备从该用户启动该请求;并授予访问该服务仅当用户符合服务设置的策略。 没有注册其设备为用户提供补救说明如何注册和达标访问公司的 O365 服务。 IOS 和 Android 设备上的用户需要注册其设备，使用公司门户应用程序。 当用户注册其设备时，该设备是 Azure Active Directory 中注册，登记为设备管理和法规遵从性。 客户必须使用与 Microsoft Intune Azure 活动目录设备注册服务来启用 Office 365 提供服务的移动设备管理。 设备注册时强制执行设备策略为用户访问 Office 365 提供服务的先决条件。

当用户成功注册其设备时，该设备将成为受信任。 Azure 的 Active Directory 提供了单点登录访问公司应用程序并强制条件访问策略来授予对用户请求的访问权限，但每一次用户请求继续访问服务不仅第一次访问。 用户将被拒绝访问服务的登录凭据已更改、 设备是丢失/被盗，或在续订请求的时间不符合策略时。

## <a name="deployment-considerations"></a>部署考虑事项︰
必须使用设备注册 Azure 活动目录设备注册服务。

当用户在本地进行身份验证时，Active Directory 联合身份验证服务 (AD FS) （1.0 和以上） 是必需的。 标识提供程序不支持的多因素身份验证时，加入工作区的多因素身份验证将失败。 例如，AD FS 2.0 不是多因素身份验证功能。 租户管理员必须确保内部部署 AD FS MFA 能够并且有效的 MFA 方法已启用，在启用 MFA Azure 活动目录设备注册服务之前。 例如，在 Windows Server 2012 R2 上的 AD FS 有 MFA 能力。 在启用 MFA Azure 活动目录设备注册服务之前，您还必须启用 AD FS 服务器上的其他有效的身份验证 (MFA) 方法。 在 AD FS 中的受支持的 MFA 方法的详细信息，请参阅 AD fs 配置其他身份验证方法。

## <a name="frequently-asked-questions-faq"></a>常见问题 (FAQ)

问︰ 什么是不受支持的设备平台的默认排除策略？

答︰ 目前，在 iOS 和 Android 设备上的用户有选择地实施条件访问策略。 其他设备平台上的应用程序，默认情况下，受 iOS 和 Android 设备的条件访问策略。 租户管理，但是，可以覆盖全球的政策，不允许不受支持的平台上的用户访问。
它位于发展规划，将条件访问策略扩展到包括 Windows 在内的其他平台上的用户。

问︰ 当将 Office 365 提供服务的条件访问策略扩展到基于浏览器的应用程序 （例如，OWA，基于浏览器的 SharePoint）。

答︰ 目前，Office365 服务的条件访问仅限于设备上的丰富应用程序。 它位于发展规划，将条件访问策略扩展到用户从浏览器访问服务。
