<properties
   pageTitle="Azure 架构引用 — IaaS︰ 将 Active Directory 扩展到 Azure |Microsoft Azure"
   description="如何实现在 Azure 使用 Active Directory 授权安全混合网络体系结构。"
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>扩展到 Azure 的 Active Directory 目录服务 （添加）

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍了将活动目录 (AD) 环境扩展到 Azure，以使用[Active Directory 域服务 (AD DS)]提供分布式身份验证服务的最佳实践[active-directory-domain-services]。 这种体系结构扩展中[实施安全混合网络体系结构在 Azure]的文章介绍[implementing-a-secure-hybrid-network-architecture]和[实施与 Azure 中的 Internet 访问的安全的混合网络体系结构][implementing-a-secure-hybrid-network-architecture-with-internet-access]。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 此参考体系结构使用资源管理器，Microsoft 建议为新的部署。

可以使用 AD DS 来对标识进行身份验证。 这些标识可以属于用户、 计算机、 应用程序或其他资源安全域构成。 可以承载 AD DS 内部，但它可以更有效地将这一功能和 AD 存储库复制到云混合方案中应用程序的元素在 Azure 中的位置。 这种方法有助于减少由发送身份验证滞后时间，云从本地授权请求回到 AD DS 运行内部。 

有两种方式来承载您的目录服务在 Azure 中︰

1. 您可以使用[Azure 活动目录 (AAD)] [azure-active-directory]在云中创建一个新的 AD 域并将其链接到内部部署 AD 域。 然后设置[Azure AD 连接][azure-ad-connect]上的承诺来复制标识保存在云的内部存储库。 请注意，在云环境中的目录****在本地部署系统的扩展，而是包含相同标识的副本。 这些内部将被复制到云，但**不是会**云所做的更改标识为所做的更改复制回内部域。 例如，密码重置必须执行的内部部署和使用 Azure AD 连接将更改复制到云。 另外，请注意 AAD 的同一个实例可以被链接到多个实例的 AD DS。AAD 将包含它所链接到的每个 AD 存储库的标识。

    AAD 是适用于其中的内部网络和 Azure 托管的云资源的虚拟网络不直接链接通过 VPN 隧道或 ExpressRoute 电路的情况。 此解决方法很简单，虽然它可能不适合系统按这可能需要重新配置 AAD 的跨边界上的部署/云迁移组件无法位置。 此外，AAD 只处理用户身份验证，而不是计算机身份验证。 某些应用程序和服务，例如，SQL Server 可能会要求计算机身份验证本解决方案不适合在这种情况下。

2. 您可以部署虚拟机作为 Azure 中的域控制器运行 AD DS，从您的本地数据中心扩展现有的 AD 架构。 这种方法是为内部网络和 Azure 的虚拟网络连接由 VPN 和/或 ExpressRoute 连接的方案更为常见。 此解决方案还支持双向复制使您在云和内部，进行更改，只要它是最合适的。 这取决于您的安全要求，可以是云中的 AD 安装︰
    - 作为该保持内部的同一个域的一部分
    - 新域中共享目录林
    - 一个单独的目录林

<!-- we might want to add info on how to choose from the three options above -->

这种体系结构解决方案 2，使用相同的 AD DS 域在 Azure 和内部的重点。

这种体系结构的典型使用情况包括︰

- 混合应用程序中的工作负载运行部分内部和部分在 Azure。

- 应用程序和服务，通过使用 Active Directory 来执行身份验证。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示 （*单击以放大*） 此体系结构中的重要组成部分。 关于灰元素的详细信息，阅读[实施安全混合网络体系结构在 Azure] [implementing-a-secure-hybrid-network-architecture]和[实施与 Azure 中的 Internet 访问的安全的混合网络体系结构][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[![0]][0]

- **内部网络。** 内部网络包括本地 AD 服务器，可以对组件内部执行身份验证和授权。

- **AD 服务器。** 这些都是实施目录服务 (AD DS) 作为虚拟机在云中运行的域控制器。 这些服务器可以提供身份验证的 Azure 虚拟网络中运行的组件。

- **活动目录的子网。** AD DS 服务器承载在单独的子网中。 NSG 规则保护 AD DS 的服务器，可以提供防火墙，以阻止来自意外源的通讯。

- **Azure 网关和 AD 同步。**。 Azure 的网关提供的内部网络和 Azure VNet 之间的连接。 这可以是一个[VPN 连接][ azure-vpn-gateway] [Azure ExpressRoute]或[azure-expressroute]。 云中的 AD 服务器和内部之间的所有同步请求都通过网关。 用户定义的路由 (UDRs) 处理同步通信，它通过直接向云中的 AD 服务器并且不通过现有网络虚拟装置 (NVAs) 在此方案中使用的路由。

    有关配置 UDRs 和 NVAs 的详细信息，请参见[实现 Azure 中的安全混合网络体系结构][implementing-a-secure-hybrid-network-architecture]。

## <a name="recommendations"></a>建议

本节概述了用于在 Azure，实现 AD DS 的建议包括︰

- 虚拟机的建议。

- 网络的建议。

- 安全建议。 

- 活动目录站点建议。

- 活动目录 FSMO 布置建议。

>[AZURE.NOTE] 文档[部署 Windows 服务器 Active Directory 在 Azure 的虚拟机的准则][ad-azure-guidelines]包含许多事宜上的更多详细的信息。

### <a name="vm-recommendations"></a>VM 的建议

使用足够的资源来处理流量的预期的卷创建虚拟机。 使用承载 AD DS 在场所作为起始点的计算机的大小。 监视资源使用情况;您可以调整 Vm 并缩小它们是否太大。 有关调整 AD DS 域控制器的详细信息，请参阅[的 Active Directory 域服务容量计划][capacity-planning-for-adds]。

创建一个单独的虚拟数据磁盘来将数据库、 日志和 SYSVOL 存储为 AD。 不要在同一操作系统磁盘上存储这些项目。 请注意，默认情况下，连接到虚拟机的数据磁盘使用直写高速缓存。 但是，这种形式的缓存可以与 AD DS 的要求发生冲突。 鉴于此，为*无*的数据磁盘上设置*主机缓存首选项*设置。 有关详细信息，请参阅[Windows 服务器 AD DS 数据库和 SYSVOL 的位置][adds-data-disks]。

部署到 Azure 虚拟网络[可用性](#Availability-considerations)原因作为域控制器运行 AD DS 的至少两个虚拟机。

### <a name="networking-recommendations"></a>网络的建议

为每个虚拟机承载 AD DS，用专用的静态 IP 地址配置的网络接口。 这种配置更好地支持每个 AD 虚拟机的 DNS。 有关详细信息，请参阅[如何设置 Azure 门户中专用的静态 IP 地址][set-a-static-ip-address]。

> [AZURE.NOTE] 不要为 AD DS Vm 的公用 IP 地址。 [安全注意事项]，请参阅[security-considerations]了解详情。

您必须确保通信可以自由流动，在云中的 AD 服务器和内部之间︰

- 从内部允许传入通信的广告子网中添加 NSG 规则。 使用 AD DS 的端口的详细信息，请参阅[Active Directory 和活动目录域服务端口要求][ad-ds-ports]。

- 请确保 UDR 表不传送 AD DS 通信通过此方案中使用 NVAs。 对于您自己的部署，具体取决于您使用，什么 NVAs 您可能希望重定向的通信量。

### <a name="security-recommendations"></a>安全建议

AD DS 服务器处理身份验证，因此非常敏感的项目。 避免直接暴露于 Internet 的 AD DS 服务器。 将 AD DS 服务器放在一个单独的子网，通过自己的防火墙。 确保为身份验证和授权，以及 synchronzing 服务器使用 AD DS 所需的端口保持打开状态，但关闭所有其他的端口。 有关详细信息，请参阅[Active Directory 和活动目录域服务端口要求][ad-ds-ports]。 [解决方案组件][solution-components]在本文档后面的部分将显示 NSG 规则示例解决方案使用打开的端口。

NSG 规则可用于创建简单的防火墙。 如果您需要更全面的保护可以实现其他安全外围服务器通过使用的子网和 NVAs，对[实现与 Azure 中的 Internet 访问的安全的混合网络体系结构]的文档所述[implementing-a-secure-hybrid-network-architecture-with-internet-access]。

使用 BitLocker 或 Azure 磁盘加密来加密 AD DS 数据库所在的磁盘。

### <a name="active-directory-site-recommendations"></a>活动目录站点建议

您可以使用站点组在一起的域控制器，通过快速链接连接到 AD DS 中。 相同的 AD DS 站点中的域控制器复制目录更改都自动，和很少配置有必要处理复制。

若要控制复制 Azure 和本地数据中心之间的通信，建议添加一个单独的 AD DS 站点来表示使用 Azure 的地址空间。 您可以配置站点链接之间您内部 AD DS 站点和更有效地控制跨站点复制。

您可以使用站点分离应用不同的组策略对象 (Gpo) 联接的计算机在 Azure，并利用"网站知道"，如系统中心配置管理器的应用程序。

### <a name="active-directory-fsmo-placement-recommendations"></a>活动目录 FSMO 布置建议

灵活单主机操作 (FSMO) 服务器是专用的域控制器时，reposnsible 的不同在 AD DS 中，下面列出的设置之间的数据一致性。

- **架构主机**。 这是维护使用 AD DS 架构的结构的全林性角色。

- **域命名主机**。 这是林范围内的角色管理林中的域名有关的信息。

- **主域控制器 (PDC)**。 这是全域性角色。 PDC 处理更新的密码和帐户锁定。 它通过其他 Dc 商议时服务身份验证请求具有不匹配的密码。 PDC 处理组策略更新，而且是旧版应用程序和执行一些写操作的一些管理工具的目标 DC。

- **相对 ID (RID) 主机**。 Rid 用于唯一地标识目录中的对象的帮助。 此服务器负责生成由域中的所有 AD 服务器供使用全域性池的 Rid。

- **基础结构角色**。 在一个域中的对象可以引用另一个域中的对象。 此域范围内的角色是负责更新对象的 SID 和跨域对象引用中的可分辨的名称。 请注意，也不能实现该角色的服务器充当全局编录 (GC) 服务器。

有关详细信息，请参阅[Windows 中的活动目录 FSMO 角色][AD-FSMO-roles-in-windows]。

对于这种情况下，我们建议您避免部署到 Azure 中的域控制器的 FSMO 角色。 

## <a name="availability-considerations"></a>可用性考虑事项

创建设置 AD 服务器的可用性。 确保组中至少两个服务器。 在云中的 AD 服务器应该是同一个域内的域控制器。 这将使服务器之间自动复制。

另外，还要考虑将服务器指定为[备用操作主机][standby-operations-masters]作为 FSMO 角色的服务器的连接失败的情况下。

## <a name="security-considerations"></a>安全注意事项

如果所有域管理员任务都很可能使用内部域控制器来执行，考虑 Dc 在云中只读的。 只读的 DC 只维护用户的凭据 （足以执行本地身份验证） 的子集，可以配置为仅对特定用户的缓存信息。 因此，如果只读 DC 受到攻击，只缓存用户的信息会受到影响，而不是域中的每个帐户的详细信息。 有关详细信息，请参阅[只读域控制器][read-only-dc]。

以帮助最小化每个用户帐户，此漏洞并遏制非法闯入尝试，按照最佳做法，用于设置和维护在 AD 中的用户的密码。 有关详细信息，请参阅[强制实施密码策略的最佳][best_practices_ad_password_policy]。 此外，要小心您指定用户所属的组。 例如，请使普通用户组的成员企业管理员、 架构管理员组和域管理员组。

## <a name="scalability-considerations"></a>可伸缩性注意事项

广告是属于同一个域的域控制器的自动扩展。 请求被分发到域中的所有控制器。 您可以添加另一个域控制器，然后它将自动同步的域。 不要配置单独的负载平衡器将流量引导至控制器的域中。 确保所有域控制器都具有足够的内存和存储资源，来处理域数据库中;理想情况下，使所有域控制器虚拟机具有相同的大小。

## <a name="management-considerations"></a>管理注意事项

不要复制域控制器，而不是执行定期备份，因为恢复它们可能导致域控制器之间的状态不一致的 VHD 的文件。

关闭并重新启动 VM 来宾操作系统，而不是使用 Azure 门户中的关机选项中运行在 Azure 中的域控制器角色。 使用 Azure 门户关闭 VM 会导致虚拟机被解除分配。 此操作将重置虚拟机-GenerationID，这是令人不快的域控制器。 重置虚拟机 GenerationID，AD 存储库的 invocationID 也重置、 RID 池将被丢弃，和 SYSVOL 标记为非权威。

## <a name="monitoring-considerations"></a>监视的注意事项

无法监视和维护 AD 服务器的网络可能会导致问题如︰

- **登录失败**。 登录失败或者可能会出现在整个域或林信任关系或名称解析无法正常工作，如果一台全局编录服务器不能确定通用组成员身份。

- **帐户锁定**。 如果域的主域控制器不可用，或多个域控制器之间的复制将失败，用户帐户和服务帐户可以将被锁定。

- **域控制器出现故障**。 如果包含 Ntds.dit 文件的驱动器运行磁盘空间不足时，域控制器可以停止工作。

- **应用程序失败**。 如果地址簿查询到的目录失败，都是对您的业务，如 Microsoft Exchange 或其他电子邮件应用程序，关键的应用程序可能会失败。

- **不一致的目录数据**。 如果复制失败时间较长，重复的对象可以在目录中创建，并可能需要广泛的诊断和时间来消除。

- **帐户创建失败**。 域控制器不能创建用户或计算机帐户，如果它耗尽其供应的相对 Id 和 RID 主机不可用。

- **安全策略失败**。 如果 SYSVOL 共享的文件夹不正确复制，组策略对象和安全策略是未正确应用到客户端。

仔细监视 AD 服务器，并随时准备迅速采取纠正措施。 创建监视要在适当的间隔执行的任务的清单。 例如您可以每天安排以下关键任务︰

- 检查并解决报告的域控制器的警报 

- 验证所有域控制器进行都通信，并且复制工作。

- 确保 SYSVOL 共享。

- 修正时间同步问题。

- 检查使用 AD 的物理驱动器上的磁盘空间。

您可能经常执行其他日常任务较少。 例如，您无法检查信任关系和每周检查过时或破坏信任并验证所有域控制器正在都运行每月使用相同的服务包和热修补程序的修补程序。

有关详细信息，请参阅[监视 Active Directory][monitoring_ad]。 您可以安装工具，例如[微软系统中心][microsoft_systems_center]在监视服务器上 ([体系结构关系图]，请参阅[architecture]) 以帮助执行这些任务。 

## <a name="solution-components"></a>解决方案组件

提供用于[实现在 Azure 中的安全混合网络体系结构]的文档所述，这种体系结构创建安全混合网络的解决方案[implementing-a-secure-hybrid-network-architecture]和[实施与 Azure 中的 Internet 访问的安全的混合网络体系结构][implementing-a-secure-hybrid-network-architecture-with-internet-access]，但添加了以下各项︰

- Azure 的资源组名为*基名称*的 dns-rg，其中*基名称*是前缀指定在部署解决方案时。

- 两个 Azure Vm 调用*基名称*-ad1-vm 和*基名称*-ad2 的 vm，*基名称*的 dns rg 资源组中创建。 这些虚拟机配置为与目录服务和安装并配置 DNS 的 AD 服务器。

- NSG 名为*基名称*-广告-nsg *basename*-ntwk rg Azure 的资源组中。 此资源组是构成安全的混合网络中，基础结构的一部分，但新的 NSG 是定义如下表中所示的 AD 服务器的入站的安全规则的补充︰


    优先级|名称|来源|目标|服务|操作|
    --------|----|------|-----------|-------|------|
    170|vnet 至 port53|10.0.0.0/16|任何|Custom(ANY/53)|允许|
    180|vnet 至 port88|10.0.0.0/16|任何|Custom(ANY/88)|允许|
    190|vnet 至 port135|10.0.0.0/16|任何|Custom(ANY/135)|允许|
    200|vnet-port137 9|10.0.0.0/16|任何|Custom(ANY/137-139)|允许|
    210|vnet 至 port389|10.0.0.0/16|任何|Custom(ANY/389)|允许|
    220|vnet 至 port464|10.0.0.0/16|任何|Custom(ANY/464)|允许|
    230|rpc 动态 vnet|10.0.0.0/16|任何|Custom(ANY/49152-65535)|允许|
    240|onprem port53 广告|192.168.0.0/24|任何|Custom(ANY/53)|允许|
    250|onprem port88 广告|192.168.0.0/24|任何|Custom(ANY/88)|允许|
    260|onprem port135 广告|192.168.0.0/24|任何|Custom(ANY/135)|允许|
    270|onprem port389 广告|192.168.0.0/24|任何|Custom(ANY/389)|允许|
    280|onprem port464 广告|192.168.0.0/24|任何|Custom(ANY/464)|允许|
    290|管理 rdp 允许|10.0.0.128/25|任何|Custom(ANY/3389)|允许|
    300|网关允许|10.0.255.224/27|任何|Custom(ANY/Any)|允许|
    310|自允许|10.0.255.192/27|任何|Custom(ANY/Any)|允许|
    320|vnet-拒绝|任何|任何|Custom(ANY/Any)|允许|

    AD DS 使用端口 53、 89、 135、 389 和 464 接受传入复制和身份验证通迅流。 在此表中，内部域控制器所在地址空间 192.168.0.0/24 (地址空间可能有所不同-作为参数中指定此信息与部署的解决方案模板。

    NSG 还定义了以下启用同步和授权通信流回内部网络的出站安全规则︰

    优先级|名称|来源|目标|服务|操作|
    --------|----|------|-----------|-------|------|
    100|out port53|任何|192.168.0.0/24|Custom(ANY/53)|允许|
    110|out port88|任何|192.168.0.0/24|Custom(ANY/88)|允许|
    120|out port135|任何|192.168.0.0/24|Custom(ANY/135)|允许|
    130|out port389|任何|192.168.0.0/24|Custom(ANY/389)|允许|
    140|out port445|任何|192.168.0.0/24|Custom(ANY/445)|允许|
    150|out port464|任何|192.168.0.0/24|Custom(ANY/464)|允许|
    160|出 rpc 动态|任何|192.168.0.0/24|Custom(ANY/49152-65535)|允许|

与解决方案中提供的脚本还可以执行以下任务︰

- 它将添加*基名称*-ad1-vm 和*基名称*-ad2-vm 服务器作为域控制器的域。 内部域控制器中的*Active Directory 用户和计算机*控制台中，您可以查看这些服务器︰

![[1]][1]

- 它的 AD 站点到域命名 Azure VNet Ad 站点中创建新的子网 (10.0.0.0/16)。 此网站包含*基名称*-ad1-vm 和*基名称*-ad2-虚拟机服务器。 

- 它将添加将内部站点和域控制器之间的复制时间间隔配置在云中的 IP 站点间传输设置。 您可以看到子网、 站点和传输设置*Active Directory 站点和服务器*控制台中的内部域控制器的设置︰

![[2]][2]

## <a name="deployment"></a>部署

此示例解决方案包括下列必备条件︰

- 您已配置内部域，并且您已配置 DNS，并安装路由和远程访问服务以支持 VPN 连接到 Azure VPN 网关。


- 已安装最新版本的 Azure CLI。 [请按照以下说明以了解详细信息][cli-install]。

- 如果您正在部署从 Windows 解决方案，您必须安装提供一个 bash 外壳，如[GitHub 桌面]的工具，[github-desktop]。

>[AZURE.NOTE] 如果您没有访问现有的内部域，您可以创建一个测试环境，使用[onpremdeploy.sh] [onpremdeploy]指责脚本。 该脚本创建在云中模拟非常基本的内部部署安装程序的网络和虚拟机。 编辑之前运行此脚本，并设置以下变量定义在文件的开头︰
>
> - **BASE_NAME**。 用户定义的资源组和由脚本创建的 VM 的前缀。 此项应**不超过 5 个字符**; 否则为该脚本会失败并生成一个虚拟机名称无效。
> 
> - **订阅**。 您的 Azure 的订阅 id。 将在此 suscription 创建的资源组。
> 
> - **位置**。 要在其中创建的资源组中，如*eastus*或*westus*Azure 的位置。
> 
> - **ADMIN_USER_NAME**。 使用 VM 中的管理员帐户的名称。
> 
> - **ADMIN_PASSWORD**。 管理员帐户的密码。

执行以下步骤来生成该示例解决方案︰

1. 下载和编辑[azuredeploy.sh] [azuredeploy]编写脚本和文件的起始处设置以下参数︰

    - **BASE_NAME**。 资源组和虚拟机由脚本创建用户定义的前缀。 像以前一样，该项目应**不超过 5 个字符**。

    - **订阅**。 您的 Azure 的订阅 id。

    - **操作系统类型**。 操作系统 （*Windows*或*Linux*） 用于 web、 业务和数据访问层的虚拟机。 注意，由脚本创建的所有 AD 服务器中都运行 Windows Server 2012，无论该设置如何。

    - **域名**。 内部域的名称。 请注意，是否您使用由 onpremdeploy.sh 脚本创建的环境，这必须是*contoso.com*。

    - **位置**。 要在其中创建的资源组 Azure 的位置。

    - **ADMIN_USER_NAME**。 要使用不同的虚拟机中的管理员帐户的名称。

    - **ADMIN_PASSWORD**。 管理员帐户的密码。

    - **ON_PREMISES_PUBLIC_IP**。 内部部署 VPN 计算机的公共 IP 地址。

    - **ON_PREMISES_ADDRESS_SPACE**。 内部网络的内部地址空间。 如果您使用的由 onpremdeploy.sh 脚本创建的环境，这必须是 192.168.0.0/16。

    - **VPN_IPSEC_SHARED_KEY**。 用于建立内部网络和 Azure VPN 网关之间的 VPN 连接的 IPSec 共享的密钥。

    - **ON_PREMISES_DNS_SERVER_ADDRESS**。 内部 DNS 服务器的 IP 地址。 如果您使用的由 onpremdeploy.sh 脚本创建的环境，这必须是 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX**内部子网的地址前缀。 如果您使用的由 onpremdeploy.sh 脚本创建的环境，这必须是 192.168.0.0/24。

    >[AZURE.NOTE] 为了节省资源和时间，该脚本不会创建业务和数据访问层。 如果您需要这些项目，您可以取消注释的 azuredeploy.sh 脚本中的以下部分︰
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. 打开 bash shell 提示符并移动到包含 azuredeploy.sh 脚本的文件夹。

3. 登录到您的 Azure 帐户。 在 bash 外壳，请输入以下命令︰

    ```
    azure login
    ```

    按照说明连接到 Azure。

4. 运行命令`./azuredeploy.sh`，该脚本创建的网络基础结构，然后等待。

5. 在出现提示时*请确认已创建了 VNet*，使用 Azure 门户检查，已创建一个名为*基名称*-ntwk rg 的资源组，并且它包含类似于下图中所示的项目︰

    ![[3]][3]

    >[AZURE.NOTE] 在示例中所示，*基名称*设置为*云*时运行脚本。

    单击*basename*vnet VNet、 单击*子网*，并验证已创建了如下所示的子网︰

    ![[4]][4]

6. 在 bash shell 窗口中显示系统提示时，按下某个键，稍候会创建 web 层和负载平衡器。

7. 在出现提示时*请确认已正确创建 Web 层*，使用 Azure 门户网站检查，已创建一个名为*基名称*web 层 rg 的资源组，并且包含与以下所示类似的项目︰

    ![[5]][5]

8. Bash shell 窗口中提示时，按下某个键，创建 NVAs 稍候。

9. 在出现提示时*请确认已正确创建 NVA*，使用 Azure 门户检查已包含以下内容创建了一个名为*基名称*管理-rg 的资源组︰

    ![[6]][6]

10. Bash shell 窗口中提示时，按任意键，jumpbox 正在创建稍候。

11. 在出现提示时*请确认已正确创建 jumpbox*，使用 Azure 门户网站检查以下各项已给*基名称*管理 rg 资源组︰

    - 可用性组称为*basename*jb 的作为。

    - 名为*基名称*-jb vm 虚拟机。

    - 网络接口调用*基名称*-jb 网卡。

12. Bash shell 窗口中提示时，按任意键，创建了 Azure VPN 网关和连接等待。 请注意，此步骤可能需要 30 分钟才能完成。

13. 提示*请验证已正确创建的 VPN 网关*时，使用 Azure 门户以检查以下各项已给*基名称*-ntwk rg 资源组︰

    - 本地网关称为部署 lgw。
    
    - 调用*基名称*vpngw 虚拟网络网关。

    - 网关连接命名为*基名称*-vnet-vpnconn。 请注意，是否您还没有配置的连接; 内部末尾，此连接的状态可能是*未连接*您将解决这一问题以后。

14. Bash shell 窗口中提示时，按任意键，创建虚拟机和其他资源的 DMZ 稍候。

15. 在出现提示时*请确认已正确创建 DMZ*，使用 Azure 门户检查已包含以下内容创建了一个名为*基名称*-dmz rg 的资源组︰

    ![[7]][7]

16. 在 bash shell 窗口中显示系统提示时，按任意键。 应出现以下提示︰

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    以用户身份登录到本地计算机连接到 Azure 的网关并适当地配置连接。 将静态路由添加到内部部署网关设备指示 requestsfor 10.0.0.0/16 地址范围，通过对 VNet 网关。 执行此操作的步骤将因您正在连接的方式。 请参见[实现一种混合网络体系结构 Azure 和内部部署 VPN] [implementing-a-hybrid-network-architecture-with-vpn]的详细信息。

    请注意，可以使用 Azure 门户来检查*basename*-ntwk rg 资源组中的*基名称*-vpngw 网关来找到 Azure VPN 网关的公用 IP 地址︰

    ![[8]][8]

    您可以确定是否已建立连接正确看*基名称*-vnet vpnconn 连接的状态。 它应设置为*已连接*。

    要测试连接，打开远程桌面连接到 jumpbox (10.0.0.254) 从位于内部网络中的计算机。

17. 在 bash shell 窗口中显示系统提示时，按任意键。 在下一个提示，*按任意键进行更新以指向内部 DNS VNet 的 VNet 设置*、 按键和等待 VNet 的 DNS 设置将更新为您指定为 azuredeploy.sh 脚本中的**ON_PREMISES_DNS_SERVER_ADDRESS**参数的值。

18. 系统提示时，*请验证已更新在 VNet 上的 DNS 服务器设置*，请使用 Azure 门户网站检查*basename*-vnet VNet*基名称*-ntwk rg 资源组中的*DNS 服务器*设置。 它应该被设置为*自定义的 DNS*，并且*主 DNS 服务器*应您的内部 DNS 服务器的地址︰

    ![[9]][9]

19. 在 bash shell 窗口提示时*按任意键创建 AD 服务器的资源组*，按某个键，稍候在云中存放 AD 服务器的资源组创建。

20. 在 bash shell 窗口提示时*按任意键创建 AD 服务器的虚拟机*，按某个键，等待虚拟机创建和添加到资源组。

21. *按任意键联接到内部域虚拟机*出现时，转到 Azure 的门户，验证已创建一个名为*基名称*的 dns rg 组，并且它包含两个虚拟机 (*基名称*-ad1-vm 和*基名称*-ad2-vm):

    ![[10]][10]

22. 已创建了 NSG 的检查*basename*-ntwk rg 资源组中，称为*基名称*-广告-nsg。 检查有该 NSG 的入站和出站安全性规则。 它们应匹配[解决方案组件]的表中列出的那些[solution-components]部分。

23. Bash shell 窗口中提示时，按任意键，等待虚拟机添加到内部域。

24. 在*请转到内部部署 AD 服务器，以验证是否已添加了计算机到域*提示、 连接到本地计算机并使用*Active Directory 用户和计算机*控制台来检查这两个虚拟机都已添加到域︰

    ![[11]][11]

25. 在 bash shell 窗口中显示系统提示时，按下某个键，稍候在域中创建 AD 复制站点。

26. 在*请转到内部部署 AD 服务器来验证已创建了复制站点*提示，使用*Active Directory 站点和服务*控制台来检查成功，与名为*AzureToOnpremLink*和 VNet 引用的子网 IP 站点间传输链路创建名为*Azure Vnet Ad 站点*复制站点︰

    ![[12]][12]

27. 在 bash shell 窗口中显示系统提示时，按下某个键和脚本在每个 AD 虚拟机上安装目录服务和 DNS 稍候。

28. 出现提示*请登录到每个 Azure AD 服务器，以验证是否成功配置目录服务*时，从本地计算机中打开远程桌面连接到 jumpbox (*基名称*-jb vm)，然后从 jumpbox 到第一个 AD 服务器，打开另一个远程桌面连接 (*基名称*-ad1-虚拟机)。 使用**域名**、 **ADMIN_USER_NAME**和**ADMIN_PASSWORD**的 azuredeploy.sh 脚本中指定登录。 使用服务器管理器，验证的 AD DS 和 DNS 的角色都已添加了。 重复此过程的第二个 AD 服务器 (*基名称*-ad2-虚拟机)。

29. 在 bash shell 窗口中显示系统提示时，按任意键。 出现提示，*按任意键设置 Azure VNet DNS 设置为指向 Azure 中的 DNS*时，按任意键并让脚本以更新 DNS 设置为 VNet。

30. 当提示*请验证已设置 VNet DNS 更新引用 Azure VM DNS 服务器*显示，使用 Azure 的门户检查*basename*-vnet VNet*基名称*-ntwk rg 资源组中的*DNS 服务器*设置。 现在，主、 辅 DNS 服务器应引用两个广告虚拟机︰

    ![[13]][13]

31. 重新启动每个 AD Vm 然后才能继续。 此步骤是必需的以确保它们都拿起了正确的 DNS 设置从 Azure。 这两个虚拟机运行在继续操作之前，请等待。

32. 在 bash shell 窗口中显示系统提示时，按任意键。 下一个系统提示时，*按任意键可应用网关到网关的子网 （它可能已被删除） 的 UDR*，按某个键，允许脚本刷新 UDR 的网关。

33. 验证已成功完成脚本。

## <a name="next-steps"></a>下一步行动

- 了解[创建添加资源目录林]的最佳方法[adds-resource-forest]Azure 中。

- 了解[创建 ADF 的基础架构]的最佳做法[adfs]Azure 中。

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "安全与 Active Directory 的混合网络体系结构"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "列出为服务器两个 Azure Vm 在 Active Directory 用户和计算机控制台"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "在云环境中显示该站点的复制设置的 Active Directory 站点和服务控制台"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "基名称-ntwk-rg 资源组的内容"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "基名称 vnet VNet 中的子网"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Web 层中的项目"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "基名称-管理-rg 资源组中 NVAs"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "基名称-dmz rg 资源组中的资源"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "查找的 Azure VPN 网关的公用 IP 地址"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "将 DNS 服务器设置为 * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "* Basename *-dns-rg AD 服务器虚拟机所在的资源组"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "列出域的成员作为 AD 服务器虚拟机在 Active Directory 用户和计算机控制台"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Active Directory 站点和服务控制台显示的 Azure AD 服务器的复制站点"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "将 DNS 服务器设置为引用中云的 AD 服务器 basename vnet VNet 的"