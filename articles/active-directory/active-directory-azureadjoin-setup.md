<properties
    pageTitle="为用户设置了 Azure 广告加入 |Microsoft Azure"
    description="解释如何管理员可以设置 Azure AD 连接的内部目录和设备注册。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>您的组织中设置 Azure 广告加入

设置 Azure 活动目录联接 （Azure AD 连接） 之前，您需要同步用户对云的内部目录，或者在 Azure AD 中手动创建托管的帐户。

[将内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)介绍同步您的内部用户对 Azure AD 的详细的说明。


要手动创建和管理在 Azure AD 用户，是指[在 Azure AD 用户管理](https://msdn.microsoft.com/library/azure/hh967609.aspx)。

## <a name="set-up-device-registration"></a>设置设备注册
1. 以管理员身份登录到 Azure 的门户。
2. 在左窗格中，选中**Active Directory**。
3. 在**目录**选项卡上，选择您的目录。
4. 选择**配置**选项卡。
5. 请转到**设备**部分。
6. 在**设备**选项卡上，设置下列各项︰  
   * **最大数量的设备每用户**︰ 选择设备，用户可以在 Azure AD 中具有最大数量。  如果用户达到此配额，它们不能够添加额外的设备，直到删除一个或多个其现有设备。
   * **需要多因素身份验证到联接设备**︰ 设置是否要求用户提供第二个身份验证因素将其设备加入到 Azure 的广告。 Azure 多因素身份验证的详细信息，请参阅[入门 Azure 云环境中的多因素身份验证](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md)。
   * **用户可能 AZURE AD 连接设备**︰ 选择的用户和组，可以将设备加入到 Azure 的广告。
   * **其他管理员在 AZURE AD 连接设备**︰ 使用 Azure AD 津贴或企业移动套件 (EMS)，您可以选择哪些用户被授予对该设备的本地管理员权限。 全局管理员和设备所有者默认情况下授予本地管理员权限。

<center>![设置设备 regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

为用户设置了 Azure 广告加入后，它们可以连接到 Azure 广告通过他们公司或者个人的设备。

以下是可用于使用户可以设置 Azure 广告加入的三种情形︰

- 用户加入直接到 Azure AD 公司所拥有的设备。
- 用户域联接到内部部署 Active Directory 的公司拥有设备，然后将该设备扩展到 Azure 的广告。
- 用户工作或学校将帐户添加到 Windows 的个人设备上

## <a name="additional-information"></a>其他信息
* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
