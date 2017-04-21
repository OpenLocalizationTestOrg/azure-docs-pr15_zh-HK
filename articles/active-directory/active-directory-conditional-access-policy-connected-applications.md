<properties
    pageTitle="设置 Azure Active Directory 连接应用程序的基于设备的条件访问策略 |Microsoft Azure"
    description="设置 Azure AD 连接的应用程序的基于设备的条件访问策略。"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>设置 Azure Active Directory 连接应用程序的基于设备的条件访问策略


Azure 活动目录 (AD Azure) 基于设备的条件访问可以帮助您保护组织的资源︰

- 来自未知或非托管设备尝试访问。
- 设备不能满足您组织的安全策略。

条件接收的概述，请参阅[Azure Active Directory 条件访问](active-directory-conditional-access.md)。

您可以设置基于设备的条件访问策略，以保护这些应用程序︰

- Office 365 SharePoint Online，来保护您的组织的网站和文档
- Office 365 Exchange 联机，以保护您的组织的电子邮件
- 软件即服务 (SaaS) 应用程序连接到 Azure AD 身份验证
- 内部发布的应用程序通过使用 Azure AD 应用程序代理服务

设置基于设备的条件访问策略，请在 Azure 的门户网站，请转到特定应用程序目录中。


  ![在 Azure 门户目录的应用程序列表](./media/active-directory-conditional-access-policy-connected-applications/01.png "应用程序")


选择的应用程序，然后单击**配置**选项卡设置的条件访问策略。  


  ![配置应用程序](./media/active-directory-conditional-access-policy-connected-applications/02.png "基于设备的访问规则")




若要设置一个基于设备的条件访问策略，**基于设备的访问规则**部分中的**启用访问规则**中，选择**上**。

基于设备的条件访问策略有三个组件︰

- **应用于**。 该策略应用到用户的范围。

- **设备规则**。 条件，它可以访问应用程序之前，必须满足一个设备。

- **应用程序强制执行**。 该策略适用于客户端应用程序 （而不是浏览器的本机）。

  ![基于设备的访问权限策略的三个组件](./media/active-directory-conditional-access-policy-connected-applications/03.png "基于设备的访问规则")


## <a name="select-the-users-the-policy-applies-to"></a>选择该策略应用到的用户

在**应用于**部分中，可以选择此策略应用于用户的范围。

当您创建的用户的访问权限策略的作用域时，您有两种︰

- **所有的用户**。 将策略应用于所有用户访问该应用程序。

- **组**。 限制到特定组的成员的用户的策略。

![应用于所有用户或组的策略](./media/active-directory-conditional-access-policy-connected-applications/11.png "应用于")


 若要从策略中排除用户，选择**除**复选框。 当您需要向特定的用户临时访问该应用程序授予权限，这是很有帮助。 选择此选项，例如，如果某些用户有未准备条件访问的设备。 设备可能还没有注册，或者它们是将要从法规遵从性。


## <a name="select-the-conditions-that-devices-must-meet"></a>选择设备必须满足的条件

使用**设备规则**设置条件设备必须满足访问该应用程序。

对于这些设备类型，您可以设置基于设备的条件访问︰

- Windows 10 周年更新、 Windows 8.1 和 Windows 7
- Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2
- iOS 设备 （iPad、 iPhone）
- Android 设备

对 Mac 支持即将登场。

  ![将策略应用到设备](./media/active-directory-conditional-access-policy-connected-applications/04.png "应用程序")

 >[AZURE.NOTE] 有关加入域和 Azure AD 连接的设备之间的差异的信息，请参阅[使用 Windows 10 设备在您的工作区中](active-directory-azureadjoin-windows10-devices.md)。

您有设备规则的两个选项︰

- **所有设备都必须符合标准**。 访问应用程序的所有设备平台都必须符合标准。 在不支持基于设备的条件访问的平台运行的设备将被拒绝访问。

- **只有所选的设备必须是符合标准**。 只有特定的设备平台必须符合标准。 其他平台或其他平台，可以访问该应用程序具有访问权限。

  ![设置设备规则的范围](./media/active-directory-conditional-access-policy-connected-applications/05.png "应用程序")

Azure 加入广告的设备都符合标准，如果它们被标记为**符合要求**的目录中，通过 Intune 或第三方移动设备管理系统，它集成了 Azure 的广告。

加入域的设备是否是兼容如果︰

- 它被注册 Azure 的广告。 许多组织将加入域的设备视为受信任设备。
- 它是由系统中心配置管理器 2016年标记 Azure AD 中**满足法规遵从性**。

 ![加入域的设备都符合规范](./media/active-directory-conditional-access-policy-connected-applications/06.png "设备规则")

Windows 个人设备都符合标准，如果它们被标记为**符合要求**的目录中，通过 Intune 或第三方移动设备管理系统，它集成了 Azure 的广告。

非 Windows 设备都符合标准，如果他们用 Intune 标记为**符合要求**的目录中。

 >[AZURE.NOTE] 有关如何在不同的管理系统中设置 Azure 广告设备法规遵从性的更多信息，请参见[Azure Active Directory 条件访问](active-directory-conditional-access.md)。


您可以选择一个或多个设备平台基于设备的访问策略。 这包括 Android、 iOS、 Windows Mobile （Windows 8.1 手机和平板电脑） 和 Windows （所有其他 Windows 设备，包括所有 Windows 10 设备）。
仅在选定的平台上进行策略计算。 如果试图访问的设备没有运行所选平台之一，该设备可以访问应用程序，如果用户具有访问权限。 没有设备策略进行评估。

![选择平台的设备规则](./media/active-directory-conditional-access-policy-connected-applications/07.png "设备规则")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>设置策略评估的一种类型的应用程序

在**应用程序执行**部分中，设置策略的计算结果将为用户或设备访问的应用程序的类型。

具有类型的应用程序包括两个选项︰

- 浏览器和本机应用程序
- 本机应用程序

![选择浏览器或本机应用程序](./media/active-directory-conditional-access-policy-connected-applications/08.png "应用程序")

若要强制应用程序的访问策略，请选择**浏览器和本机应用程序**。 然后，您可以包括︰

- （例如，Microsoft 边缘 Windows 10 中） 或在 iOS 的 Safari 浏览器。
- 在任何平台上，使用活动目录身份验证库 (ADAL)，或使用 WebAccountManager (WAM) API 中 Windows 10 的应用程序。

>[AZURE.NOTE] 有关浏览器支持和设备基础，访问证书颁发机构保护的应用程序，用户的其他注意事项的详细信息请参阅[Azure Active Directory 条件访问](active-directory-conditional-access.md)。

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>帮助保护免受基于 Exchange ActiveSync 的应用程序的电子邮件访问

在 Office 365 Exchange 联机应用程序，可以使用 Exchange ActiveSync 来阻止电子邮件访问基于 Exchange ActiveSync 的邮件应用程序。

![Exchange ActiveSync 法规遵从性选项](./media/active-directory-conditional-access-policy-connected-applications/09.png "应用程序")

![需要符合的设备访问电子邮件](./media/active-directory-conditional-access-policy-connected-applications/10.png "应用程序")


## <a name="next-steps"></a>下一步行动

- [Azure Active Directory 条件访问](active-directory-conditional-access.md)
