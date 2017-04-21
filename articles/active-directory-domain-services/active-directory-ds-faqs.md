<properties
    pageTitle="常见问题解答-Azure 的 Active Directory 域服务 |Microsoft Azure"
    description="关于 Azure Active Directory 域服务常见问题"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure 的 Active Directory 域服务︰ 常见问题 (Faq)

此页回答 Azure Active Directory 域服务有关的常见问题。 请检查更新。

### <a name="troubleshooting-guide"></a>故障排除指南
请参阅我们的配置或管理 Azure AD 域服务时遇到的常见问题的解决方案的[故障排除指南](active-directory-ds-troubleshooting.md)。


### <a name="configuration"></a>配置

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>可以创建多个域的单个 Azure 的广告目录吗？
不。 您只能创建一个域提供服务，由单一的 Azure AD 域服务 Azure 的广告目录。  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>是否可以在 Azure 资源管理器虚拟网络的 Azure AD 域服务？
不。 Azure AD 域服务只能在经典的 Azure 虚拟网络中启用。 您可以连接到资源管理器虚拟网络使用虚拟网络使用管理的域资源管理器的虚拟网络中对等的传统的虚拟网络。

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>可以使 Azure AD 域服务可在多个虚拟网络中我的订阅？
服务本身不直接支持此方案。 一次只能有一个虚拟的网络中提供了 azure AD 域服务。 但是，您可以配置多个虚拟网络，以公开到其他虚拟网络的 Azure AD 域服务之间的连接。 本文介绍了您可以[在 Azure 中的虚拟网络连接](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)。

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>可以启用使用 PowerShell 的 Azure AD 域服务吗？
PowerShell/自动化部署 Azure AD 域服务当前不可用。

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>在 Azure AD 域服务中用新的 Azure 门户？
不。 Azure AD 域服务可以配置仅在[Azure 的传统门户网站](https://manage.windowsazure.com)中。 我们希望将来扩展支持的[Azure 的门户](https://portal.azure.com)。

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>可以将域控制器添加到 Azure AD 域服务托管域？
不。 Azure AD 域服务所提供的域是一个管理的域。 不需要设置、 配置或以其他方式管理此域的域控制器这些管理活动是作为一种服务由 Microsoft 提供。 因此，您不能添加托管域的其他域控制器 （读写或只读模式）。

### <a name="administration-and-operations"></a>管理和操作

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>可以连接到域控制器使用远程桌面我托管域？
不。 您没有权限连接到通过远程桌面管理的域的域控制器。 AAD DC 管理员组的成员可以管理使用 AD 管理工具，如活动目录管理中心 (ADAC) 或 AD PowerShell 托管的域。 加入到托管域的 Windows 服务器上使用远程服务器管理工具功能，则会安装这些工具。

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>我已启用 AD 域 Azure 服务。 到到此域的域联接计算机是否使用哪个用户帐户？
您已经添加到管理组 （例如，为 AAD DC 管理员） 的用户都可以加入域的计算机。 此外，此组中的用户授予对加入到域的计算机的远程桌面访问。

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>我可以驾驭 Azure AD 域服务所提供的域的域管理员特权？
不。 未被授予管理权限的托管域。 域管理员和企业管理员权限不可用于您的域中使用。 现有的域管理员或企业管理员组在 Azure 的广告目录内未还授予域/企业域上的管理员权限。

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>是否可以修改组成员身份在 Azure AD 域服务所提供的域中使用 LDAP 或其他广告管理工具？
不。 组成员身份不能修改由 Azure AD 域服务的域上。 这同样适用于用户的属性。 在 Azure 广告中或在您的内部域，但是可能会更改组成员身份或用户属性。 此类更改将自动同步到 Azure AD 域服务。

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>可以将扩展域 Azure AD 域服务所提供的架构？
不。 对于托管域，Microsoft 管理架构。 架构扩展不支持通过 Azure AD 域服务。

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>可以修改或添加我受管理域中的 DNS 记录？
是的。 属于 AAD DC 管理员组的用户授予 DNS 管理员的权限，以修改托管域中的 DNS 记录。 这些用户可以加入到托管域，运行 Windows 的计算机上使用 DNS 管理器控制台来管理 DNS。 若要使用 DNS 管理器控制台，请安装 DNS 服务器工具，即远程服务器管理工具在服务器上的可选功能的一部分。 [实用程序管理、 监视和疑难解答 DNS](https://technet.microsoft.com/library/cc753579.aspx)的详细信息出现在 TechNet 上。


### <a name="billing-and-availability"></a>帐单邮寄地址和可用性

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>是 Azure AD 域服务付费的服务吗？
是的。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/active-directory-ds/)。

#### <a name="is-there-a-free-trial-for-the-service"></a>有免费试用服务吗？
此项服务包括在 Azure 的免费试用版。 您可以注册[Azure 的免费一个月试用期](https://azure.microsoft.com/pricing/free-trial/)。

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>我可以获得一部分企业移动套件 (EMS) Azure AD 域服务？
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>我需要 Azure AD 津贴使用 Azure AD 域服务吗？
不。 Azure AD 域即付即用 Azure 服务和服务不是 EMS 的一部分。 Azure AD 域服务都可用于所有版本的 Azure 的广告 （自由、 基本的、 和，特优），按小时收费，根据具体使用情况。

#### <a name="what-azure-regions-is-the-service-available-in"></a>Azure 的哪些区域是服务中可用？
请参阅[Azure 服务区域的](https://azure.microsoft.com/regions/#services/)页后，可以在 Azure AD 域服务可用的 Azure 地区的列表，请参阅。
