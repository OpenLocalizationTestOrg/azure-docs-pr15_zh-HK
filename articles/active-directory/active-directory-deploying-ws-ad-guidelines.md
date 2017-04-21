<properties
   pageTitle="部署 Windows Azure 的虚拟机服务器 Active Directory 的准则 |Microsoft Azure"
   description="如果您知道如何部署 AD 域服务和内部部署的广告联合身份验证服务，了解它们如何工作在 Azure 的虚拟机上。"
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>在 Azure 的虚拟机中部署 Windows 服务器的 Active Directory 的准则

本文介绍部署 Windows 服务器 Active Directory 域服务 (AD DS) 和 Active Directory 联合身份验证服务 (AD FS) 内部而不是将它们部署 Microsoft Azure 的虚拟机之间的重要差异。

## <a name="scope-and-audience"></a>范围和访问群体

文章是为那些已经部署 Active Directory 部署经验丰富。 它包括部署 Active Directory Microsoft Azure Azure 虚拟机/虚拟网络和传统的内部部署 Active Directory 部署之间的差异。 Azure 的虚拟机和 Azure 的虚拟网络是基础结构作为-服务 (IaaS) 提供的组织能够利用云计算资源的一部分。

对于那些还不熟悉广告部署，请参阅适当的[AD DS 部署指南](https://technet.microsoft.com/library/cc753963)或[AD FS 部署规划](https://technet.microsoft.com/library/dn151324.aspx)。

本文假定读者熟悉以下概念︰

- Windows 服务器 AD DS 部署和管理
- 部署和配置 DNS 以支持 Windows 服务器 AD DS 基础结构
- Windows 服务器 AD FS 部署和管理
- 部署、 配置和管理信赖方应用程序 （网站和 web 服务） 可以使用 Windows 服务器 AD FS 标记
- 一般的虚拟机的概念，例如，如何配置虚拟机、 虚拟磁盘和虚拟网络

本文着重介绍了混合部署方案中的 Windows 服务器 AD DS 或 AD FS 部分已部署的内部部署，并且在部分部署在 Azure 的虚拟机上的要求。 本文档首先讨论在 Azure 与内部部署和重要决策影响的设计和部署的虚拟机上运行 Windows Server AD DS 和 AD FS 的关键区别。 纸张的其余部分说明每个决策点更详细地介绍的准则以及如何适用于各种部署方案的指导原则。

本文不讨论配置的[Azure Active Directory](http://azure.microsoft.com/services/active-directory/)，即为云应用程序提供身份管理和访问控制功能的基于 REST 的服务。 Azure 活动目录 (AD Azure) 和 Windows 服务器 AD DS，但是，旨在协同工作以提供身份和访问管理解决方案为今天的混合 IT 环境和现代的应用程序。 为了帮助理解的差异和 Windows 服务器 AD DS 和 Azure 的广告之间的关系，考虑以下方面︰

1. 使用 Azure 将内部数据中心扩展到云时，可能会运行在云中 Azure 的虚拟机上的 Windows 服务器 AD DS。
2. 您可以使用 Azure 的广告来向您的用户单一登录软件作为-服务 (SaaS) 应用程序。 例如，Microsoft Office 365 提供使用这种技术，而其他云平台 Azure 上运行的应用程序也可以使用它。
3. 您可以使用 Azure AD （其访问控制服务） 来让用户签名中使用从 Facebook、 Google、 微软和其他标识提供程序到云或内部中承载的应用程序的标识。

有关这些差异的详细信息，请参阅[Azure 标识](fundamentals-identity.md)。

## <a name="related-resources"></a>相关的资源

您可以下载并运行[Azure 虚拟机准备情况评估](https://www.microsoft.com/download/details.aspx?id=40898)。 评估将自动检查内部环境并生成自定义的报告根据本主题可帮助您迁移到 Azure 的环境中找到的指导。

我们建议您也第一次阅读教程、 参考线和视频，介绍下列主题︰

- [在 Azure 门户配置仅云虚拟网络](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [在 Azure 门户配置站点到站点 VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [在 Azure 的虚拟网络上安装一个新的活动目录林](active-directory-new-forest-virtual-machine.md)
- [在 Azure 上安装复制 Active Directory 域控制器](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: (01) 虚拟机基础知识](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) 创建虚拟网络和跨内部连接](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>介绍

Azure 的虚拟机中部署 Windows 服务器的 Active Directory 的基本要求不同，很少从内部虚拟机中，在某种程度上，物理机） 对其进行部署。 例如，在 Windows 服务器 AD DS 的情况下如果 Azure 的虚拟机将部署域控制器 (Dc) 是在现有的复制副本内部公司域林，然后 Azure 部署可以很大程度上被视为相同的方式可能将任何其它附加的 Windows 服务器的 Active Directory 站点。 也就是说，必须在 Windows 服务器 AD DS，创建的网站，链接到该网站，并连接到其他使用适当的站点链接的站点的子网定义子网。 有，但是，一些差异所共有的所有 Azure 部署和一些因特定部署方案。 两个基本区别如下所示︰

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure 的虚拟机可能需要连接到内部企业网络。

将 Azure 的虚拟机连接到内部企业网络要求 Azure 的虚拟网络，其中包括站点到站点或站点到点虚拟私有网络 (VPN) 组件可以无缝地连接 Azure 的虚拟机和本地计算机。 此 VPN 组件还可使内部域成员计算机以访问 Windows 服务器的 Active Directory 域的域控制器承载以独占方式在 Azure 的虚拟机。 很重要，但是请注意，是否 VPN 失败，身份验证和其他依赖于 Windows 服务器的 Active Directory 的操作也会失败。 虽然用户可能会到登录使用现有缓存凭据，所有对等或为其门票尚未发出，或已成为过时的客户端到服务器的身份验证尝试都将失败。

查看演示视频和列表的分步教程，包括[配置在 Azure 门户站点到站点 VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)[虚拟网络](http://azure.microsoft.com/documentation/services/virtual-network/)。

> [AZURE.NOTE] 您还可以在 Azure 的虚拟网络没有连接的内部网络上部署 Windows 服务器的当前目录。 本主题中的准则但是，假定使用 Azure 的虚拟网络是因为它提供了 IP 地址是不可或缺的 Windows 服务器的功能。

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Azure PowerShell 必须配置静态 IP 地址。

动态地址分配的默认值，但使用集 AzureStaticVNetIP cmdlet 改为分配一个静态 IP 地址。 该设置将保留通过康复服务和 VM 关机/重启一个静态 IP 地址。 有关详细信息，请参阅[虚拟机内部的静态 IP 地址](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)。

## <a name="BKMK_Glossary"></a>术语和定义

以下是各种 Azure 技术，这将在本文中引用的条款的不完整列表。

- **Azure 的虚拟机**︰ IaaS 解决方案使客户能够部署虚拟机运行几乎所有传统的 Azure 中部署服务器工作负荷。

- **Azure 的虚拟网络**︰ 网络服务可以让客户创建和管理虚拟网络在 Azure 中的，安全地将它们链接到自己的 Azure 中内部网络基础结构通过使用虚拟专用网络 (VPN)。

- **虚拟 IP 地址**︰ 未绑定到特定的计算机或网络接口卡的面向 internet 的 IP 地址。 云服务分配用于接收网络通信重定向到 Azure VM 的一个虚拟 IP 地址。 一个虚拟 IP 地址是一个属性，该属性可以包含一个或多个 Azure 的虚拟机的云服务。 此外请注意 Azure 的虚拟网络，可以包含一个或多个云服务。 虚拟 IP 地址提供本地负载平衡功能。

- **动态的 IP 地址**︰ 这是仅供内部 IP 地址。 它应配置为静态 IP 地址 （通过使用一组 AzureStaticVNetIP cmdlet） 主持的 DC/DNS 服务器角色的虚拟机。

- **康复服务**︰ 在 Azure 自动返回服务运行状态再次后检测到的服务失败，它的过程。 康复服务是 Azure 支持可用性和可恢复性的方面之一。 尽管可能性不大后康复事件为 DC 在虚拟机上运行的服务, 的结果类似于未计划的重新启动，但有少数的副作用︰

 - 在虚拟机中的虚拟网络适配器将更改
 - 将虚拟网络适配器的 MAC 地址
 - 将更改虚拟机的 CPU 处理器/ID
 - 只要虚拟机连接到虚拟网络和虚拟机的 IP 地址是静态的不会更改的虚拟网络适配器的 IP 配置。

 这些行为的任何会影响 Windows 服务器的 Active Directory，因为它具有不依赖的 MAC 地址或处理器/CPU ID，建议所有的 Windows 服务器的 Active Directory 部署在 Azure 上运行 Azure 的虚拟网络上的上述。

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>是否安全虚拟 Windows 服务器的 Active Directory 域控制器？

在 Azure 的虚拟机上部署 Windows 服务器活动目录域控制器是按照同一原则为运行在虚拟机中部署域控制器。 运行虚拟的 DCs 是一种安全的做法，只要遵循备份和还原 Dc 的准则。 有关约束条件和运行虚拟的 DCs 的准则的详细信息，请参阅[运行 Hyper-V 中的域控制器](https://technet.microsoft.com/library/dd363553)。

虚拟机管理程序提供，或 trivialize 可能会导致很多分布式系统，包括 Windows 服务器的 Active Directory 的问题的技术。 例如，在物理服务器上，可以克隆磁盘，也可以使用不受支持的方法来回滚状态的服务器，包括使用 San 等等，但在物理服务器上执行，是比还原虚拟机快照虚拟机监控程序中的要困难得多。 Azure 提供的功能可能会导致相同的不良条件。 例如，不应将 VHD 文件的域控制器复制而不是执行定期备份，因为恢复它们可能会导致使用快照还原功能相似的情况。

等回滚引入 DCs 之间，可能会产生分歧永久状态的 USN 气泡。 它如可以导致问题︰

- 延迟对象
- 密码不一致
- 不一致的特性值
- 如果回滚架构主机的架构不匹配

有关如何影响到域控制器的详细信息，请参阅[USN 和 USN 回滚](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback)。

从开始 Windows Server 2012，[其他安全措施都内置于 AD DS](https://technet.microsoft.com/library/hh831734.aspx)。 这些安全措施可帮助保护虚拟的域控制器针对上述的问题，只要底层的虚拟机管理程序平台支持虚拟机 GenerationID。 Azure 支持虚拟机-GenerationID，这意味着域控制器上运行 Windows Server 2012 或以后 Azure 的虚拟机的附加安全措施。

> [AZURE.NOTE] 您应该关闭然后重新启动 VM 来宾操作系统，而不是使用 Azure 的经典门户中的**关机**选项中运行在 Azure 中的域控制器角色。 如今，使用经典的门户关闭虚拟机将导致 VM 被解除分配。 解除分配虚拟机的优点会招致的费用，但它还重置虚拟机-GenerationID，这是令人不快的 DC。 重置虚拟机 GenerationID，AD DS 数据库的 invocationID 也重置、 RID 池将被丢弃，和 SYSVOL 标记为非权威。 有关详细信息，请参阅[简介 Active Directory 域服务 (AD DS) 虚拟化](https://technet.microsoft.com/library/hh831734.aspx)和[安全地虚拟化 DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx)。

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>为什么要部署 Windows 服务器 AD DS 在 Azure 的虚拟机？

许多 Windows 服务器 AD DS 部署方案非常适合在 Azure 上的虚拟机的部署。 例如，假设您有需要在亚洲地区的远程位置的用户进行身份验证的欧洲公司。 公司尚未部署它们和有限的专业技能，来管理服务器部署后的成本由于以前部署 Windows 服务器活动目录域控制器在亚洲。 结果是，来自亚洲的身份验证请求由提供服务 Dc 在欧洲并不理想的结果。 在这种情况下，您可以部署一个您指定必须在亚洲的 Azure 数据中心中运行的虚拟机上的 DC。 将该直流附加到 Azure 的虚拟网络连接到远程位置直接将提高身份验证性能。

Azure 也非常适合向否则为代价高昂的灾难恢复 (DR) 站点代替。 相对较低成本的宿主域控制器和一个虚拟网络在 Azure 上的一小部分代表了有吸引力的替代。

最后，您可能需要部署 Azure，如 SharePoint，要求 Windows 服务器的 Active Directory 但不依赖内部网络或企业的 Windows 服务器 Active Directory 中的网络应用程序。 在这种情况下，部署在以满足 SharePoint 的 Azure 上独立的林中服务器的要求是最佳的。 再次，还支持部署的网络应用程序都需要连接到内部网络和企业活动目录。

> [AZURE.NOTE] 因为它提供第 3 层连接，提供 Azure 的虚拟网络和内部网络之间的连接的 VPN 组件还可以启用运行内部利用作为 Azure 的虚拟机运行在 Azure 的虚拟网络的域控制器中的成员服务器。 但如果无法使用 VPN，之间的通信部署计算机和基于 Azure 的域控制器将无法正常工作，导致身份验证和各种其他错误。  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>部署 Windows 服务器的 Active Directory 域控制器上 Azure 虚拟机而不是内部之间的对比度

- 对于超过单个 VM 包括任何 Windows 服务器的 Active Directory 部署方案，有必要使用 Azure 的虚拟网络的 IP 地址的一致性。 请注意，本指南假定域控制器都运行在 Azure 的虚拟网络。

- 与内部 Dc，建议使用静态 IP 地址。 使用 Azure PowerShell，只可以配置一个静态 IP 地址。 更多详细信息，请参阅[虚拟机内部的静态 IP 地址](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)。 如果您有监视系统或其他检查在来宾操作系统中的静态 IP 地址配置的解决方案，可以将相同的静态 IP 地址分配虚拟机的网络适配器的属性。 但请注意如果 VM 都会康复服务，或关闭在经典的门户和已释放其地址的网络适配器将被放弃。 在这种情况下，将需要重置来宾中的静态 IP 地址。

- 在虚拟的网络上部署虚拟机并不意味着 （或要求） 连接到内部网络。虚拟的网络只是使这种可能性。 您必须创建虚拟网络中的专用 Azure 和内部网络之间的通信。 您需要部署在内部网络上的 VPN 端点。 VPN 将打开 Azure 到内部网络。 有关详细信息，请参阅[虚拟网络概述](../virtual-network/virtual-networks-overview.md)和[Azure 门户站点到站点 VPN 配置](../vpn-gateway/vpn-gateway-site-to-site-create.md)。

> [AZURE.NOTE] 创建[点到站点 VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md)选项位于单独的基于 Windows 的计算机直接连接到 Azure 的虚拟网络。

- 无论您是否创建一个虚拟网络，Azure 外出通信量而不入的费用。 各种 Windows 服务器的 Active Directory 设计选择会影响多少外出通信量生成的部署。 例如，部署只读域控制器 (RODC) 限制外出通信量因为它不会复制出站。 但是部署 RODC 的决策需要与执行针对 DC 和[兼容](https://technet.microsoft.com/library/cc755190)的应用程序和服务的站点中有 Rodc 的写入操作的需要进行权衡。 关于通信费用的详细信息，请参阅[Azure 的价格看一眼即可](http://azure.microsoft.com/pricing/)。

- 当您完成时控制通过哪个服务器资源要用于虚拟机内部，如 RAM、 磁盘大小和等，在 Azure 上您必须从列表中选择预配置的服务器规模。 对于 DC，除了操作系统所在磁盘来存储 Windows 服务器的 Active Directory 数据库需要数据磁盘。

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>您可以部署 Windows 服务器 AD FS Azure 的虚拟机？

是的您可以部署 Windows 服务器 AD FS Azure 的虚拟机，和内部部署[AD FS 部署的最佳实践](https://technet.microsoft.com/library/dn151324.aspx)同样适用于 AD FS 部署在 Azure 上。 但一些最佳做法，如负载平衡和高可用性需要超越什么 AD FS 提供本身的技术。 他们必须提供的基础结构。 我们回顾一下这些最佳实践，看它们如何可以实现使用 Azure Vm 和 Azure 的虚拟网络。

1. **永远不会公开直接与互联网的安全令牌服务 (STS) 服务器。**

    这是重要的因为 STS 颁发安全令牌。 因此，STS 服务器如 AD FS 服务器应被视为相同级别的保护为域控制器。 如果 STS 受到攻击，恶意用户能够发出访问令牌可能包含其选择信赖方应用程序和其他 STS 服务器信任的组织中的索赔。

2. **部署 Active Directory 域控制器与 AD FS 服务器位于同一网络中的所有用户域。**

    AD FS 服务器使用 Active Directory 域服务来验证用户的身份。 建议部署与 AD FS 服务器位于同一网络上的域控制器。 在 Azure 的网络和内部网络之间的链路中断，而且实现了低延迟和提高登录性能的情况下，这提供了业务连续性。

3. **部署多个 AD FS 节点实现高可用性和负载平衡。**

    在大多数情况下，失败的应用程序启用 AD FS 的不可接受的这是因为关键任务的应用程序需要安全令牌通常是。 因此，并且 AD FS 服务因为 AD FS 现在位于关键路径访问关键任务应用程序，必须通过多个 AD FS 代理和 AD FS 服务器高可用性。 为分发的请求，在 AD FS 代理和 AD FS 服务器之前通常部署负载平衡器。

4. **部署一个或多个 Web 应用程序代理节点，互联网接入。**

    当用户需要访问受 AD FS 服务的应用程序时，则需要可以从 internet AD FS 服务。 这被通过部署 Web 应用程序代理服务。 强烈建议将多个节点部署高可用性的目的和负载平衡。

5. **从 Web 应用程序代理节点限制对内部网络资源的访问。**

    若要允许外部用户从 internet 访问 AD FS，需要部署 Web 应用程序代理节点 （或在早期版本的 Windows 服务器的 AD FS 代理）。 Web 应用程序代理节点直接暴露给 Internet。 它们不是需要加入域，只需要访问 AD FS 服务器通过 TCP 端口 443 和 80。 强烈建议对所有其他计算机 （尤其是域控制器） 的通信将被阻止。

    这是通常达到的内部通过 DMZ。 防火墙使用白名单模式下的操作限制流量从 DMZ 到内部网络，（也就是说，只允许从指定的 IP 地址，并在指定的端口上的通信，并阻止所有其他通信）。

下的图显示了一个传统内部 AD FS 部署。

![传统的内部部署 Active Directory 联合身份验证服务部署关系图](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

但是，由于 Azure 不提供本机，全功能的防火墙功能，其他选项需要用于限制通信。 下表显示了每个选项的优点和缺点。

| 选项 | 优势 | 缺点 |
| ------ | --------- | ------------ |
| [Azure 网络 Acl](virtual-networks-acl.md) | 低费用高昂且操作简单的初始配置 | 如果任何新的虚拟机添加到部署需要进行额外的网络 ACL 配置 |
| [Barracuda NG 防火墙](https://www.barracuda.com/products/ngfirewall) | 白名单模式的操作，它不需要网络 ACL 配置 | 增加的成本和复杂性以进行初始设置 |

在这种情况下部署 AD FS 的高级步骤，如下所示是︰

1. 通过跨内部连接，使用 VPN 或[ExpressRoute](http://azure.microsoft.com/services/expressroute/)创建虚拟网络。

2. 部署在虚拟网络上的域控制器。 此步骤是可选的但建议。

3. 部署虚拟网络上的域加入 AD FS 服务器。

4. 创建[内部负载平衡设置](http://azure.microsoft.com/blog/internal-load-balancing/)，包括 AD FS 服务器并使用虚拟网络 （动态的 IP 地址） 中新的专用 IP 地址。

  1. 更新 DNS 创建 FQDN 指向内部负载平衡设置的 （动态） 的专用 IP 地址。

5. 创建 Web 应用程序代理节点云服务 （或一个单独的虚拟网络）。

6. 部署在云服务或虚拟网络中的 Web 应用程序代理节点

  1. 创建包含 Web 应用程序代理节点外部负载平衡组。

  2. 更新外部 DNS 名称 (FQDN)，使其指向云服务公共 IP 地址 （虚拟 IP 地址）。

  3. 配置 AD FS 代理使用对应的内部负载平衡组的 AD FS 服务器的 FQDN。

  4. 更新基于声明的网站为其声明提供程序使用外部的 FQDN。

7. 限制到 AD FS 虚拟网络中任何计算机的 Web 应用程序代理之间的访问。

要限制流量，Azure 内部负载平衡器的负载平衡设置需要配置为仅对 TCP 端口 80 和 443，流量和负载平衡设置的内部动态 IP 地址的所有其他通信将被丢弃。

![允许 TCP 443 + 80 ADFS 网络 Acl 的关系图。](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

AD FS 服务器通信允许仅通过以下来源︰

- Azure 内部负载平衡器。
- 管理员在内部网络上的 IP 地址。

> [AZURE.WARNING] 设计必须防止 Web 应用程序代理节点到达 Azure 的虚拟网络中的任何其他 Vm 或内部网络上的任何位置。 它可以通过快速通道连接内部装置或站点到站点 VPN 连接的 VPN 设备配置防火墙规则。

此选项的一个缺点是需要配置多台设备，包括内部负载平衡器、 AD FS 服务器和添加到虚拟的网络的其他任何服务器的网络的 Acl。 如果不配置网络通信流限制 Acl 添加到部署任何设备时，整个部署可能会面临风险。 如果不断更改的 Web 应用程序代理节点的 IP 地址，网络 Acl 必须重置 （这意味着应将代理服务器配置为使用[静态动态 IP 地址](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)）。

![与网络 ACL 的 Azure 的 ADF。](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

另一个选项是使用[Barracuda NG 防火墙](https://www.barracuda.com/products/ngfirewall)装置来控制 AD FS 服务器 AD FS 代理服务器之间的通信。 此选项符合安全性和高可用性的最佳做法，并且要求较少管理初始安装后的因为 Barracuda NG 防火墙设备提供防火墙管理白名单模式下，它可以直接在 Azure 的虚拟网络上安装。 它无需配置新的服务器添加到部署任何时候的网络 Acl。 但该选项添加初始部署的复杂性和成本。

在这种情况下，两个虚拟网络而不是一个部署。 VNet1 和 VNet2，我们将调用它们。 VNet1 包含代理，VNet2 包含 STSs 和网络连接到公司网络。 因此，VNet1 是物理上 （尽管实际上） 从 VNet2 和，反过来，企业的网络隔离。 VNet1 连接到 VNet2 使用特殊的隧道技术识别作为传输独立网络体系结构 （唐）。 唐隧道连接到每个虚拟网络使用 Barracuda NG 防火墙 — 一个 Barracuda 上每个虚拟网络。  为了获得高可用性，建议您部署两个 Barracudas 每个虚拟网络;一个活动的其他被动。 它们提供了极其丰富的防火墙功能使得我们能够模仿传统的内部部署 DMZ Azure 中的操作。

![在 Azure 防火墙上的 ADF。](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

有关详细信息，请参阅[AD FS︰ 扩展到互联网的声明感知内部部署前端应用程序](#BKMK_CloudOnlyFed)。

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>如果目标是单独的 Office 365 SSO 的 AD FS 部署一种替代方法

还有另一种选择部署 AD FS 完全如果您的目标是只是为了支持中的 Office 365 的符号。 在这种情况下，您只需使用密码同步内部部署目录同步并最少部署的复杂性与实现相同的最终结果，因为这种方法不需要 AD FS 或 Azure。

下表比较有和没有部署 AD FS 签入过程的工作原理。

| Office 365 单一登录方式进行登录使用 AD FS 和目录同步 | Office 365 相同登录使用目录同步 + 密码同步 |
| ------------- | ------------- |
| 1.用户登录到公司网络，并与 Windows 服务器的 Active Directory 进行身份验证。 | 1.用户登录到公司网络，并与 Windows 服务器的 Active Directory 进行身份验证。 |
| 2.用户尝试访问 Office 365 (我是@contoso.com)。 | 2.用户尝试访问 Office 365 (我是@contoso.com)。 |
| 3.office 365 将用户重定向到 Azure 的广告。 | 3.office 365 将用户重定向到 Azure 的广告。 |
| 4.由于 Azure 的广告不能对用户进行身份验证，并且明白与 AD FS 部署的信任，它将用户重定向到 AD FS。 | 4.azure 的广告不能直接接受 Kerberos 票证并没有信任关系存在，因而它请求用户输入凭据。 |
| 5.用户发送到 AD FS STS 的 Kerberos 票证。 | 5.用户输入内部密码相同，并对照的用户名称和密码已通过目录同步同步的 Azure AD 验证它们。 |
| 6.AD FS 转换所需的令牌格式/索赔的 Kerberos 票证并将用户重定向到 Azure 的广告。 | 6.azure AD 用户重定向到 Office 365。 |
| 7.该用户验证到 Azure AD （另一个转换发生）。 |  7.用户可以登录到 Office 365 和 OWA 使用 Azure AD 标记。 |
| 8.azure AD 用户重定向到 Office 365。 |  |
| 9.用户自动登录到 Office 365。 |  |

在目录同步密码同步方案 (没有 AD FS) 与 Office 365，单一登录都替换为"同一登录"位置"相同"只是意味着访问 Office 365 时，用户必须重新输入其同一个本地凭据。 注意可以通过用户的浏览器以帮助减少后续提示记住这些数据。

### <a name="additional-food-for-thought"></a>其他精神食粮

- 如果您部署一个 AD FS 代理 Azure 的虚拟机上，需要连接到 AD FS 服务器。 如果它们内部部署，建议您利用提供虚拟网络以允许 Web 应用程序代理节点与 AD FS 服务器进行通信的站点到站点 VPN 连接。

- 如果您部署一个 AD FS 服务器 Azure 的虚拟机上，连接到 Windows 服务器的 Active Directory 域控制器、 属性存储数据库和配置数据库是必需的可能还需要 ExpressRoute 或 Azure 的虚拟网络和内部网络之间的站点到站点 VPN 连接。

- 费用从 Azure 的虚拟机 （出站通信） 应用于所有通信。 如果成本是决定的因素，最好部署在 Azure，离开 AD FS 服务器上部署的 Web 应用程序代理节点。 如果 AD FS 服务器部署在 Azure 虚拟机上，将发生附加成本内部用户进行身份验证。 外出通信量会招致无论遍历 ExpressRoute 或 VPN 站点到站点连接的成本。

- 如果您决定使用 Azure 的本机服务器负载平衡功能的 AD FS 服务器高可用性，请注意，负载平衡提供了探测器，用于确定在云服务中的虚拟机的运行状况。 在 Azure 的虚拟机 （相对于 web 或工作人员的角色），由于默认探测器响应的代理不在 Azure 的虚拟机上，则必须使用自定义的探测。 为简单起见，您可以使用自定义的 TCP 探测 — — 这只需要，成功地建立一个 TCP 连接 （TCP SYN 段发送和回复与 TCP SYN ACK 段） 确定虚拟机运行状况。 您可以配置自定义的探测，以使用虚拟机主动侦听到的任何 TCP 端口。

> [AZURE.NOTE] 公开一组相同的端口 （如端口 80 和 443） 互联网直接向所需的机器不能共享相同的云服务。 它，因此，建议您创建专用的云服务您的 Windows 服务器 AD FS 服务器以避免潜在的重叠端口要求应用程序和 Windows 服务器 AD FS 之间。

## <a name="deployment-scenarios"></a>部署方案

下面一节概述了常见部署方案，以引起对必须采取的重要事项考虑在内。 每个方案都有关于决策和因素要考虑的更多详细信息的链接。

1. [AD DS︰ 部署支持 AD DS 的公司网络连接不要求应用程序](#BKMK_CloudOnly)

    例如，面向 Internet 的 SharePoint 服务部署在 Azure 的虚拟机上。 该应用程序并不依赖于公司网络的资源。 应用程序确实需要 Windows 服务器 AD DS，但不需要公司的 Windows 服务器 AD DS。

2. [AD FS︰ 扩展到互联网的声明感知内部部署前端应用程序](#BKMK_CloudOnlyFed)

    例如，声明感知应用程序已成功部署的内部部署和公司用户使用需要变得可从互联网。 应用程序需要的业务伙伴使用自己公司的标识和现有公司用户直接通过 Internet 访问。

3. [AD DS︰ 部署 Windows 服务器 AD DS 感知应用程序需要连接到公司网络](#BKMK_HybridExt)

    例如，LDAP 感知的应用程序支持 Windows 集成身份验证，并使用配置和用户配置文件数据存储库作为 Windows 服务器 AD DS 部署 Azure 的虚拟机上。 最好是可以利用公司现有 Windows 服务器 AD DS 提供单一登录应用程序。 不声明感知应用程序。

### <a name="BKMK_CloudOnly"></a>1.AD DS︰ 部署支持 AD DS 的公司网络连接不要求应用程序

![仅云 AD DS 部署](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**图 1**

#### <a name="description"></a>说明

SharePoint 部署在 Azure 的虚拟机和应用程序并不依赖于公司网络的资源。 应用程序确实需要 Windows 服务器 AD DS 但不**需要公司的 Windows 服务器 AD DS。 因为用户是通过该应用程序还在云中 Azure 的虚拟机上承载的 Windows 服务器 AD DS 域到自主，任何 Kerberos 或联合信任关系就是必需的。

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>方案事项和方案将技术领域的应用

- [网络拓扑结构](#BKMK_NetworkTopology)︰ 创建了一个 Azure 的虚拟网络而无需跨本地连接 （也称为站点对站点连接）。

- [DC 的部署配置](#BKMK_DeploymentConfig)︰ 部署到新的单一域 Windows 服务器的 Active Directory 目录林中的新域控制器。 这应与 Windows DNS 服务器部署。

- [Windows 服务器的 Active Directory 站点拓扑](#BKMK_ADSiteTopology)︰ 使用默认的 Windows 服务器的 Active Directory 站点 （所有计算机中都将默认第一个站点名称）。

- [IP 地址和 DNS](#BKMK_IPAddressDNS):

 - 使用一组 AzureStaticVNetIP Azure PowerShell cmdlet dc 设置静态的 IP 地址。
 - 上安装和配置 Windows 服务器 DNS 域控制器在 Azure 上。
 - 配置的名称和 IP 地址主机的域控制器和 DNS 服务器角色的虚拟机的虚拟网络属性。

- [全局编录](#BKMK_GC)︰ 在目录林中第一个域控制器必须是全局编录服务器。 其他域控制器还应配置为 Gc，因为在单域林中，全局编录不需要任何额外的工作，从 DC 中。

- [Windows 服务器 AD DS 数据库和 SYSVOL 的位置](#BKMK_PlaceDB)︰ 将数据磁盘添加到要存储 Windows 服务器的 Active Directory 数据库、 日志和 SYSVOL 作为 Azure 虚拟机正在运行的域控制器。

- [备份和还原](#BKMK_BUR)︰ 确定您希望存储系统状态备份。 如有必要，添加 DC VM 备份存储其他数据磁盘。

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS︰ 扩展到互联网的声明感知内部部署前端应用程序

![跨内部连接与联盟](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**图 2**

#### <a name="description"></a>说明

声明感知应用程序已成功部署的内部部署和公司用户使用需要成为直接从 Internet 进行访问。 应用程序作为 web 前端到 SQL 数据库中存储数据。 该应用程序所使用的 SQL 服务器也位于公司网络上。 两个 Windows 服务器 AD FS STSs 和负载平衡器已经部署的内部为企业用户提供访问。 现在，应用程序需要另外的商业伙伴使用自己公司的标识和现有企业用户直接通过互联网进行访问。

为了简化并满足这一新要求的部署和配置需求，决定两个附加 web frontends 和两个 Windows 服务器 AD FS 代理服务器被安装在 Azure 的虚拟机上。 所有四个虚拟机将直接向 Internet 公开，并将提供连接到内部网络使用 Azure 虚拟网络站点到站点 VPN 功能。

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>方案事项和方案将技术领域的应用

- [网络拓扑结构](#BKMK_NetworkTopology)︰ 创建 Azure 的虚拟网络和[配置跨内部连接](../vpn-gateway/vpn-gateway-site-to-site-create.md)。

 > [AZURE.NOTE] 对于每个 Windows 服务器 AD FS 证书，请确保在 Azure 上运行的 Windows 服务器 AD FS 实例可以访问定义证书模板和生成的证书中的 URL。 这可能需要跨内部连接到您的 PKI 基础结构的各个部分。 对于示例如果 CRL 的终结点是基于 LDAP 和承载以独占方式内部，然后跨本地连接将需要。 如果这是不可取的它可能需要使用的 CRL 是可以访问 Internet 上的 CA 颁发的证书。

- [云服务配置](#BKMK_CloudSvcConfig)︰ 确保您具有两个云服务的顺序提供两种负载平衡虚拟 IP 地址。 第一个云服务的虚拟 IP 地址将被定向到端口 80 和 443 上两个 Windows 服务器 AD FS 代理服务器虚拟机。 Windows 服务器 AD FS 代理的虚拟机将被配置为指向该正面 Windows 服务器 AD FS STSs 内部负载平衡器的 IP 地址。 第二个云服务的虚拟 IP 地址将被定向到端口 80 和 443 上再次运行 web 前端两个虚拟机。 配置自定义的探测，以确保负载平衡器仅将定向到运行 Windows 服务器 AD FS 代理服务器和 web 前端的虚拟机的通信。

- [联合身份验证服务器配置](#BKMK_FedSrvConfig)︰ 配置 Windows 服务器 AD FS 联合身份验证服务器 (STS) 生成 Windows 服务器的 Active Directory 目录林在云中创建安全令牌。 设置联合声明提供程序信任关系与不同的伙伴想要接受身份，并使用不同的应用程序，您想要生成标记配置信赖方信任关系。

    在大多数情况下，Windows 服务器 AD FS 代理服务器部署面向 Internet 的能力，为了安全起见在 Windows 服务器 AD FS 联合身份验证与对应保持独立于直接的 Internet 连接。 无论部署方案，都必须使用公开提供的 IP 地址和端口，可以为负载平衡跨您的两个 Windows 服务器 AD FS STS 实例或代理实例提供一个虚拟 IP 地址配置您的云服务。

- [高可用性配置 Windows 服务器 AD FS](#BKMK_ADFSHighAvail)︰ 建议您部署 Windows 服务器 AD FS 场至少两个服务器故障转移和负载平衡。 您可能要考虑使用 Windows 内部数据库 (WID) 为 Windows 服务器 AD FS 配置数据，并使用 Azure 的内部负载平衡功能在服务器场中服务器之间分发传入的请求。

有关详细信息，请参阅[AD DS 部署指南 》](https://technet.microsoft.com/library/cc753963)。


### <a name="BKMK_HybridExt"></a>3.AD DS︰ 部署 Windows 服务器 AD DS 感知应用程序需要连接到公司网络

![跨内部 AD DS 部署](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**图 3**

#### <a name="description"></a>说明

注意 LDAP 应用程序部署在 Azure 的虚拟机上。 它支持 Windows 集成身份验证，并使用 Windows 服务器 AD DS 存储库作为配置和用户配置文件数据。 目标是利用现有公司的 Windows 服务器 AD DS 和提供了单一登录应用程序。 不声明感知应用程序。 用户也需要直接从 Internet 访问应用程序。 为了优化性能和成本，决定是公司域中的一部分的两个额外的域控制器，Azure 上的应用程序一起部署。

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>方案事项和方案将技术领域的应用

- [网络拓扑结构](#BKMK_NetworkTopology)︰ 创建带有[跨内部连接](../vpn-gateway/vpn-gateway-site-to-site-create.md)的 Azure 的虚拟网络。

- [安装方法](#BKMK_InstallMethod)︰ 部署副本从公司的 Windows 服务器的 Active Directory 域的域控制器。 为复制副本的 DC，可以在 VM 上, 安装 Windows 服务器 AD DS 和 （可选） 使用安装从介质 (IFM) 功能来减少需要在安装过程中复制到新的 DC 的数据量。 有关的教程，请参阅[安装在 Azure 上的复制 Active Directory 域控制器](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)。 即使您使用 IFM，它可能是更有效地生成虚拟直流内部，而不是在安装过程中复制 Windows 服务器 AD DS 云到移动整个虚拟硬盘 (VHD)。 为了安全，建议您从内部网络中删除 VHD 后它已被复制到 Azure。

- [Windows 服务器的 Active Directory 站点拓扑结构](#BKMK_ADSiteTopology)︰ 在 Active Directory 站点和服务中创建新的 Azure 站点。 创建一个 Windows 服务器的 Active Directory 子网对象来代表 Azure 的虚拟网络，并向网站中添加子网。 创建新站点链接，其中包括新的 Azure 站点和 Azure 的虚拟网络的 VPN 端点所在以控制并优化 Windows 服务器的 Active Directory 通讯与 Azure 网站。

- [IP 地址和 DNS](#BKMK_IPAddressDNS):

 - 使用一组 AzureStaticVNetIP Azure PowerShell cmdlet dc 设置静态的 IP 地址。
 - 上安装和配置 Windows 服务器 DNS 域控制器在 Azure 上。
 - 配置的名称和 IP 地址主机的域控制器和 DNS 服务器角色的虚拟机的虚拟网络属性。

- [地理分布式 Dc](#BKMK_DistributedDCs)︰ 根据需要配置其他的虚拟网络。 如果 Active Directory 站点拓扑结构需要地理位置对应到 Azure 的不同地区，不是您想要创建相应的 Active Directory 站点中的域控制器。

- [只读的域控制器](#BKMK_RODC)︰ 可能部署 RODC Azure 站点中的，根据您的需求进行编写针对域控制器的操作和应用程序和服务的兼容性 Rodc 的网站中。 有关应用程序兼容性的详细信息，请参见[只读域控制器应用程序兼容性指南 》](https://technet.microsoft.com/library/cc755190)。

- [全局编录](#BKMK_GC)︰ Gc 需要为多域林状结构中登录请求提供服务。 如果不部署 GC Azure 站点中的，因为身份验证请求其他站点中会导致 Gc 的查询将会外出的通信成本。 若要最小化的通信量，您可以启用通用组成员身份缓存中为该 Azure 站点 Active Directory 站点和服务。

    如果您部署一个 GC，配置站点链接和站点链接开销，这样就不需要复制相同的部分域分区其他 Gc 的源 DC 作为首选 Azure 站点中的 GC。

- [Windows 服务器 AD DS 数据库和 SYSVOL 的位置](#BKMK_PlaceDB)︰ 将数据磁盘添加到要存储 Windows 服务器的 Active Directory 数据库、 日志和 SYSVOL 在 Azure 的虚拟机上运行的域控制器。

- [备份和还原](#BKMK_BUR)︰ 确定您希望存储系统状态备份。 如有必要，添加 DC VM 备份存储其他数据磁盘。

## <a name="deployment-decisions-and-factors"></a>部署决策和因素

下表汇总了在前面的方案中，需要考虑使用下面的更多详细信息的链接的相应决策影响 Windows 服务器的 Active Directory 技术领域。 某些技术领域不能适用于每个部署方案，以及某些技术领域可能会比其他技术领域至关重要的部署方案。

例如，如果部署在虚拟网络上的复制副本 DC 并且您的目录林中只有一个域，然后选择要部署全局编录服务器在这种情况下不会部署方案的关键因为它不会创建任何额外复制要求。 另一方面，如果目录林中有多个域，然后部署在虚拟网络上的全局编录的决定可能会影响可用带宽、 性能、 身份验证、 目录查找等。

| Windows 服务器的 Active Directory 技术领域 | 决定 | 因素 |
| ---- | ---- | ---- |
| [网络拓扑](#BKMK_NetworkTopology) | 创建虚拟网络？ | <li>访问公司资源的要求</li> <li>身份验证</li> <li>帐户管理</li> |
| [DC 的部署配置](#BKMK_DeploymentConfig) | <li>部署不任何信任的情况下单独的目录林中？</li> <li>部署新的目录林使用联盟？</li> <li>部署 Windows 服务器活动目录林信任使用新的目录林或 Kerberos？</li> <li>通过部署副本 DC 扩展 Corp 林？</li> <li>通过部署新的子域或域树中扩展 Corp 林？</li> | <li>安全</li> <li>法规遵从性</li> <li>成本</li> <li>恢复能力和容错能力</li> <li>应用程序兼容性</li> |
| [Windows 服务器的 Active Directory 站点拓扑](#BKMK_ADSiteTopology) | 您如何配置子网、 站点和站点链接到 Azure 虚拟网络来优化通信和成本降到最低？ | <li>子网和站点定义</li> <li>站点链接的属性和更改通知</li> <li>复制压缩</li> |
| [IP 地址和 DNS](#BKMK_IPAddressDNS) | 如何配置 IP 地址和名称解析？ | <li>使用一组 AzureStaticVNetIP 使用 cmdlet 来分配一个静态 IP 地址</li> <li>安装 Windows 服务器的 DNS 服务器和配置的名称和 IP 地址主机的域控制器和 DNS 服务器角色的虚拟机的虚拟网络属性</li> |
| [地理分布域控制器](#BKMK_DistributedDCs) | 如何将复制到域控制器上单独的虚拟网络？ | 如果 Active Directory 站点拓扑结构需要地理位置对应到 Azure 的不同地区，不是您想要创建相应的 Active Directory 站点中的域控制器。 [配置虚拟网络虚拟网络连接到](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)单独的虚拟网络上的域控制器之间复制。 |
| [只读域控制器](#BKMK_RODC) | 使用只读或可写域控制器？ | <li>筛选器 HBI/PII 属性</li> <li>筛选器的秘密</li> <li>限制出站通信</li> |
| [全局编录](#BKMK_GC) | 安装全局编录吗？ | <li>对于单一域林，使所有 Dc Gc</li> <li>对于多域林来说，Gc 所需的身份验证</li> |
| [安装方法](#BKMK_InstallMethod) | 如何在 Azure 中安装 DC？ | 或者︰ <li>安装 AD DS 使用 Windows PowerShell 或 Dcpromo</li> <li>移动内部虚拟 DC 的 VHD</li> |
| [Windows 服务器 AD DS 数据库和 SYSVOL 的位置](#BKMK_PlaceDB) | Windows 服务器 AD DS 数据库、 日志和 SYSVOL 的存储位置？ | 更改 Dcpromo.exe 默认值。 这些关键 Active Directory 文件*必须*放置在 Azure 数据磁盘上而不是操作系统磁盘实现写入缓存。 |
| [备份和恢复](#BKMK_BUR) | 如何保护和恢复数据？ | 创建系统状态备份 |
| [联合身份验证服务器配置](#BKMK_FedSrvConfig) | <li>部署新的目录林使用云中的联盟？</li> <li>部署部署 AD FS 和公开云中的代理？</li> | <li>安全</li> <li>法规遵从性</li> <li>成本</li> <li>对商业伙伴应用程序访问</li> |
| [云服务配置](#BKMK_CloudSvcConfig) | 云服务隐式部署创建的虚拟机第一次。 您是否需要部署其他云服务？ | <li>虚拟机或虚拟机是否需要直接暴露于 Internet？</li> <li> 该服务要求负载平衡？</li> |
| [公用和专用 IP 地址 (与虚拟 IP 动态 IP) 联合身份验证服务器的要求](#BKMK_FedReqVIPDIP) | <li>不需要直接从 Internet 进行访问的 Windows 服务器 AD FS 实例？</li> <li>在云环境中所部署的应用程序是否需要自己的面向 Internet 的 IP 地址和端口？</li> | 创建每个虚拟 IP 地址所需的部署一个云服务 |
| [Windows 服务器 AD FS 高可用性配置](#BKMK_ADFSHighAvail) | <li>我 Windows 服务器 AD FS 服务器场中多少个节点？</li> <li>在我的 Windows 服务器 AD FS 代理服务器场部署的节点数？</li> | 可恢复性和容错能力 |

### <a name="BKMK_NetworkTopology"></a>网络拓扑

为了满足 IP 地址一致性和 Windows 服务器 AD ds 的 DNS 要求，有必要首先创建了一个[Azure 的虚拟网络](../virtual-network/virtual-networks-overview.md)并向其附加您的虚拟机。 其在创建期间，必须确定是否还可以扩展到内部企业网络，以透明方式将 Azure 的虚拟机连接到本地计算机的连接 — — 这使用传统的 VPN 技术实现，并要求 VPN 端点被公开在企业网络的边缘上。 也就是说，VPN 从 Azure 开始到公司网络，反之不成立。

请注意，额外的费用将扩展到您的内部网络之外，适用于每个 VM 的标准费用的虚拟网络时应用。 具体来说，有费用为 Azure 虚拟网关的 CPU 时间和通过 VPN 与本地计算机进行通信的每个 VM 所产生的出口流量。 有关网络通信费用的详细信息，请参阅[Azure 的价格看一眼即可](http://azure.microsoft.com/pricing/)。

### <a name="BKMK_DeploymentConfig"></a>DC 的部署配置

您的域控制器配置的方式取决于您想要在 Azure 上运行的服务的要求。 例如，可能会部署新林，隔离从自己公司林，用于测试概念验证、 新的应用程序或某些其他需要目录服务，但不是特定访问公司的内部资源的短期项目。

作为福利，DC 将不会复制与独立的目录林内部域控制器，从而少出站网络流量由系统本身，直接降低了成本。 有关网络通信费用的详细信息，请参阅[Azure 的价格看一眼即可](http://azure.microsoft.com/pricing/)。

举一个例子，假设您有保密要求的服务，但服务取决于您内部的 Windows 服务器活动目录的访问权限。 如果允许主机数据在云中的服务，可能会为在 Azure 上内部目录林部署新的子域。 在这种情况下，您可以部署为新的子域 （而不是为了帮助解决隐私问题全局编录） 直流。 这种情况下，副本 DC 的部署，以及要求具有本地 Dc 的连接，虚拟网络。

如果创建新的目录林，请选择是否使用[Active Directory 信任](https://technet.microsoft.com/library/cc771397)或[联合身份验证信任](https://technet.microsoft.com/library/dd807036)。 平衡为由兼容性、 安全性、 法规遵从性、 成本和可恢复性的要求。 例如，利用[选择性身份验证](https://technet.microsoft.com/library/cc755844)您可能选择部署新林在 Azure 上并创建 Windows 服务器的 Active Directory 信任内部林和云林之间。 但是，如果声明感知应用程序，可能会部署而不是有效的目录林信任的联合身份验证信任。 另一个因素是通过扩展到云将内部 Windows 服务器 Active Directory 复制更多数据或生成的身份验证和查询负载更出站通信的成本。

可用性和容错能力的要求也会影响您的选择。 例如，如果链接中断，应用程序利用 Kerberos 信任或联合体相信要所有可能完全断开，除非您已经部署了足够的基础设施在 Azure 上。 可选的部署配置，如副本的域控制器 (可写或 Rodc) 增加能够容忍链路中断的可能性。

### <a name="BKMK_ADSiteTopology"></a>Windows 服务器的 Active Directory 站点拓扑

您需要正确定义站点和站点链接以便优化通信和成本降到最低。 站点、 站点链接和子网会影响身份验证通信的域控制器之间的复制拓扑。 请考虑下面的通信费用再部署和配置域控制器，根据您的部署方案的要求︰

- 还有每小时网关本身的名义费用︰

 - 它可以启动和停止随心所欲

 - 如果停止，Azure Vm 将独立于公司网络

- 入站的通信量是自由

- 根据[Azure 的价格看一眼即可了解](http://azure.microsoft.com/pricing/)，收取的出站通信。 可以优化网站内部网站和云网站之间的链接属性，如下所示︰

 - 如果您正在使用多个虚拟网络，配置站点链接以及它们的成本适当地以防止 Windows 服务器 AD DS 在一个可以提供免费服务的相同级别的优先排序 Azure 站点。 您也可以考虑禁用桥接所有站点链接 (BASL) 选项，该选项默认启用的）。 这样可以确保仅直接连接站点复制与另一个。 间接连接的站点中的域控制器将不再能直接与对方进行复制，但是必须通过公共网站或网站进行复制。 如果由于某种原因，中间站点将变得不可用，间接连接的站点中的域控制器之间的复制不会发生，即使在站点之间的连接性是可用。 最后，其中的可传递的复制行为部分保持理想，创建站点链接桥包含相应的站点链接和站点，如部署上，公司的网络站点。

 - [配置站点链接开销](https://technet.microsoft.com/library/cc794882)相应地以避免意外的通信。 例如，如果**尝试下一个最近的站点**设置被启用，确保虚拟网络站点不是下一个最接近通过增加 Azure 站点重新连接到公司网络的站点链接对象相关联的开销。

 - 配置站点链接[的间隔](https://technet.microsoft.com/library/cc794878)和[安排](https://technet.microsoft.com/library/cc816906)根据一致性要求和对象更改的速率。 复制计划配合延迟容差。 Dc 复制仅最后一次状态的值，以便减少复制时间间隔可以节约成本，如果有足够的对象更改率。

- 如果最小化成本是头等大事，确保复制计划和未启用更改通知。 这是默认配置中，站点间复制时。 这不是要点︰ 如果您正在部署 RODC 上虚拟的网络，由于 RODC 不会复制出站更改。 但是，如果您将部署的可写 DC，应确保不配置站点链接复制频率不必要的更新。 如果您部署全局编录 (GC) 服务器，请确保包含 GC 的每个其他网站将从一个源使用站点链接或有成本要低于 GC Azure 站点中的站点链接连接的站点中的 DC 复制域分区。


- 就可能仍进一步减少通过更改复制压缩算法的站点间复制所生成的网络通讯量。 压缩算法受 REG_DWORD 注册表条目 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator 的压缩算法。 默认值为 3，这对应于的快速压缩算法。 您可以更改的值为 2，从而更改为 $ MSZip 的算法。 在大多数情况下，这将增加压缩，但它会降低 CPU 利用率。 有关详细信息，请参阅[如何 Active Directory 复制拓扑的工作原理](https://technet.microsoft.com/library/cc755994)。

### <a name="BKMK_IPAddressDNS"></a>IP 地址和 DNS

默认情况下分配 azure 的虚拟机"DHCP 租用地址"。 因为 Azure 的虚拟网络动态地址保留与虚拟机虚拟机的寿命，满足 Windows 服务器 AD DS 的要求。

结果，在 Azure 上使用动态地址时，您是在使用静态 IP 地址，因为租约，该期间为可穿绕，租约期等于云服务的生命周期的效果。

但是，如果 VM 关机，被释放的动态地址。 为了防止 IP 地址被解除分配，您可以[使用集中-AzureStaticVNetIP 分配一个静态 IP 地址](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx)。

名称解析部署您自己 （或利用您现有的） DNS 服务器基础结构;只要 azure DNS 不符合 Windows 服务器 AD DS 的高级的名称解析需要。 例如，它不会不支持动态 SRV 记录，等等。 名称解析是 DCs 和加入域的客户端的关键配置项目。 域控制器必须能够注册资源记录，并解决其他 DC 的资源记录。
容错能力和性能原因，最好是在 Azure 上运行的域控制器上安装 Windows 服务器 DNS 服务。 然后配置 Azure 的虚拟网络的属性的名称和 IP 地址的 DNS 服务器。 当虚拟的网络上的其他虚拟机启动时，其 DNS 客户端解析程序设置都配置 DNS 服务器动态分配的 IP 地址的一部分。

> [AZURE.NOTE] 本地计算机不能加入直接通过互联网在 Azure 承载 Windows 服务器 AD DS Active Directory 域。 端口要求为 Active Directory 和加入域的操作呈现它不切实际的直接公开所需的端口和有效的整个直流到互联网。

虚拟机注册其 DNS 名称自动启动或名称更改时。

本示例演示如何设置第一个虚拟机并在其上安装 AD DS 的另一个示例的更多信息，请参阅[安装新的活动目录林在 Microsoft Azure 上](active-directory-new-forest-virtual-machine.md)。 有关使用 Windows PowerShell 的详细信息，请参阅[安装 Azure PowerShell](../powershell-install-configure.md)和[Azure 管理 Cmdlet](https://msdn.microsoft.com/library/azure/jj152841)。

### <a name="BKMK_DistributedDCs"></a>地理分布域控制器

承载不同的虚拟网络上的多个域控制器时，azure 优点︰

- 多站点故障容错

- 物理上的接近到分支办公室 （低滞后时间）

有关配置虚拟网络之间的直接通信的信息，请参阅[配置虚拟网络与虚拟网络连接](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)。

### <a name="BKMK_RODC"></a>只读域控制器

您需要选择是否部署只读或可写的域控制器。 您可能倾向部署 Rodc，因为您不具有物理控制它们，但设计中的位置及其物理安全有危险，例如分支机构部署 Rodc。

Azure 没有物理安全风险的分支机构，但可能仍会 Rodc 会更合算，因为它们提供的功能非常适合于这些环境中虽然是个非常不同的原因。 例如，Rodc 有没有出站复制，并能够有选择地填充机密 （密码）。 在缺点方面，缺乏这些机密信息可能需要按需出站通信，以验证这些权限的用户或计算机进行身份验证。 但是，可以有选择地预填充和缓存的机密信息。

Rodc 提供周围 HBI 和 PII 问题的另一个好处，因为您可以添加包含敏感数据复制到 RODC 的属性筛选属性集 (FAS)。 FAS 是可自定义的一组属性未复制到 Rodc。 不能或不想在 Azure 存储 PII 或 HBI 的情况下，可以使用 FA 作为一种安全措施。 有关详细信息，请参阅 [RODC 筛选设置属性 (https://technet.microsoft.com/library/cc753459)。

请确保应用程序都将与您打算使用的 Rodc 兼容。 已启用 Windows 服务器的 Active Directory 的许多应用程序很好地使用，但某些应用程序可以执行效率低下，或如果它们不能访问的可写 dc 失败。 有关详细信息，请参见[只读域控制器应用程序兼容性指南 》](https://technet.microsoft.com/library/cc755190)。

### <a name="BKMK_GC"></a>全局编录

您需要选择是否安装全局编录 (GC)。 在单一域林，应为全局编录服务器配置所有的域控制器。 因为没有其他的复制通信，它不会增加成本。

在多域林中，Gc 所需的身份验证过程中展开的通用组成员身份。 如果不部署 GC，进行身份验证的 DC 在 Azure 上虚拟的网络上工作负载将间接生成出站身份验证流量查询每个身份验证尝试期间部署 Gc。

与 Gc 相关的成本是较可预测的因为所驻留的每个域 （一部分）。 如果工作负荷承载面向 Internet 的服务和针对 Windows 服务器 AD DS 用户进行身份验证，则成本可能是完全不可预知。 为了降低 GC 查询该站点之外的云，在身份验证过程，则可以[启用通用组成员身份缓存](https://technet.microsoft.com/library/cc816928)。

### <a name="BKMK_InstallMethod"></a>安装方法

您需要选择如何在虚拟的网络上安装域控制器︰

- 升级新的 Dc。 有关详细信息，请参阅[安装新的活动目录林中 Azure 的虚拟网络上](active-directory-new-forest-virtual-machine.md)。

- 迁移到云上部署虚拟直流 VHD。 在这种情况下，您必须确保内部虚拟直流"移动，"无法"复制"或"克隆"。

使用域控制器 （相对于 Azure"web"或"工作人员"角色的虚拟机） 只有 Azure 的虚拟机。 他们是持久和 DC 状态的持续性是必需的。 Azure 虚拟机用于工作负载如 Dc。

不要使用 SYSPREP 来部署或复制域控制器。 能够克隆 Dc 是仅在 Windows Server 2012 中的开始。 克隆功能需要支持的 VMGenerationID 基础虚拟机管理程序中。 在虚拟网络中 Windows Server 2012 和 Azure Hyper-V 都支持 VMGenerationID，与第三方虚拟化软件供应商。

### <a name="BKMK_PlaceDB"></a>Windows 服务器 AD DS 数据库和 SYSVOL 的位置

选择在何处查找 Windows 服务器 AD DS 数据库、 日志和 SYSVOL。 他们必须在 Azure 数据磁盘上部署。

> [AZURE.NOTE] Azure 数据磁盘被限制为 1 TB。

默认情况下，数据的磁盘驱动器执行不缓存写入。 连接到虚拟机的数据磁盘驱动器使用直写高速缓存。 直写高速缓存可确保写入致力于持久的 Azure 存储之前事务已完成从虚拟机的操作系统的角度来看。 它提供了耐用性，代价是速度稍慢的写入操作。

这是非常重要的 Windows 服务器 AD DS，因为后写磁盘缓存使域控制器所做的假设。 Windows 服务器 AD DS 尝试禁用写缓存，但它是磁盘 IO 系统接受它。 未能禁用写缓存在某些情况下，可能会引入 USN 回滚导致延迟对象和其他问题。

作为虚拟 DCs 的最佳做法，执行以下操作︰

- 对于无 Azure 数据磁盘上设置主机缓存首选项设置。 这样可以防止 AD DS 操作的写入缓存的问题。

- 将数据库、 日志和 SYSVOL 存储在任一相同数据磁盘或多个单独的数据磁盘。 通常，这是单独的磁盘从磁盘用于操作系统本身。 要点是，Windows 服务器 AD DS 数据库和 SYSVOL 必须不能存储在 Azure 操作系统磁盘类型。 默认情况下，AD DS 安装过程将这些组件安装在 %systemroot%文件夹，不建议使用 azure。

### <a name="BKMK_BUR"></a>备份和恢复

请注意什么是，一般情况下，不支持备份和还原的 DC 和更具体的来说，那些在虚拟机中运行。 请参阅[备份和还原的虚拟 DCs 的考虑事项](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers)。

通过使用只专门注意 Windows 服务器 AD DS，如 Windows 服务器备份的备份要求备份软件创建系统状态备份。

不要复制或克隆的 Dc 的 VHD 文件，而不是执行定期备份。 应以往任何时候都需要进行还原，这样使用克隆或复制不需要 Windows Server 2012 和受支持的虚拟机监控程序的 Vhd 将引入 USN 气泡。

### <a name="BKMK_FedSrvConfig"></a>联合身份验证服务器配置

配置的 Windows 服务器 AD FS 联合身份验证服务器 (STSs) 部分取决于是否要在 Azure 上部署的应用程序需要访问您的内部网络上的资源。

应用程序如果符合以下条件，您可以从内部网络部署隔离中的应用程序。

- 他们接受 SAML 安全令牌

- 它们是 exposable 到互联网

- 它们不会访问内部资源

在这种情况下，按如下所述配置 Windows 服务器 AD FS STSs:

1. 在 Azure 上配置独立的单一域林中。

2. 通过配置 Windows 服务器 AD FS 联合服务器场提供联合的访问目录林中。

3. 在本地林中配置 Windows 服务器 AD FS 联合服务器场和联合服务器代理场。

4. 建立之间的内部和 Azure 实例的 Windows 服务器 AD FS 联合身份验证信任关系。

另一方面，如果应用程序需要访问内部资源，您可以配置 Windows 服务器 AD FS Azure 上的应用程序，如下所示︰

1. 配置内部网络和 Azure 之间的连接。

2. 在本地林中配置 Windows 服务器 AD FS 联合服务器场。

3. 在 Azure 上配置 Windows 服务器 AD FS 联合服务器代理服务器场。

这种配置的优点减少暴露的内部资源，类似于 Windows 服务器 AD FS 配置外围网络中的应用程序。

注意，在任一方案中，您可以建立信任关系使用多个标识提供程序，如果需要公司间的协作。

### <a name="BKMK_CloudSvcConfig"></a>云服务配置

云服务是有必要的如果要公开直接与 Internet 的 VM 或公开面向 Internet 的负载平衡的应用程序。 这是可能的因为每个云服务提供了一个可配置虚拟 IP 地址。

### <a name="BKMK_FedReqVIPDIP"></a>公用和专用 IP 地址 (与虚拟 IP 动态 IP) 联合身份验证服务器的要求

每个 Azure 的虚拟机接收动态 IP 地址。 动态的 IP 地址是只能在 Azure 内部访问的专用地址。 在大多数情况下，但是，它将需要配置为 Windows 服务器 AD FS 部署一个虚拟 IP 地址。 虚拟 IP 有必要公开 Windows 服务器 AD FS 终结点到互联网，并将由联盟的伙伴和客户端进行身份验证和日常管理。 一个虚拟 IP 地址是一个云服务，它可以包含一个或多个 Azure 的虚拟机的属性。 如果部署在 Azure 和 Windows 服务器 AD FS 声明感知应用程序面向 Internet 并共享公用端口，每个都需要自己的虚拟 IP 地址，因此将需要创建一个云服务的应用程序，另一个用于 Windows Server AD FS。

有关术语的定义虚拟 IP 地址和动态 IP 地址，请参阅[术语和定义](#BKMK_Glossary)。

### <a name="BKMK_ADFSHighAvail"></a>Windows 服务器 AD FS 高可用性配置

虽然部署独立 Windows 服务器 AD FS 联合身份验证服务，但它建议 AD FS STS 和生产环境的代理部署至少两个节点具有一个场。

请参阅[AD FS 2.0 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489)中[AD FS 2.0 设计指南的电源](https://technet.microsoft.com/library/dd807036)来决定最佳的部署配置选项适合您的特定需求。

> [AZURE.NOTE] 为了负载平衡以 Windows 服务器 AD FS 在 Azure 上的终结点，配置 Windows 服务器 AD FS 服务器场中的所有成员在同一个云服务中，并且可以使用 HTTP （默认值为 80） 的 Azure 负载平衡功能和 HTTPS 端口 （默认为 443）。 有关详细信息，请参阅[Azure 负载平衡器探测](https://msdn.microsoft.com/library/azure/jj151530)。
在 Azure 上不支持 Windows 服务器网络负载平衡 (NLB)。
