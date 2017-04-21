## <a name="virtual-network-basics"></a>虚拟网络基础知识

### <a name="what-is-an-azure-virtual-network-vnet"></a>Azure 虚拟网络 (VNet) 是什么？

可以使用 VNets 资源调配和管理 Azure 中的虚拟专用网络 (Vpn) 和，（可选） 链接 VNets 与其他 VNets 在 Azure，或您在内部创建混合或跨内部部署解决方案的 IT 基础结构。 您创建的每个 VNet 都有它自己的 CIDR 块，并可以链接到其他 VNets 和内部网络中，只要不互相冲突的 CIDR 块。 您还可以控制的 DNS 服务器设置为 VNets 和 VNet 细分为多个子网。

使用 VNets 为︰

- 创建一个专用的云专用虚拟网络

    有时您不需要跨部署配置为您的解决方案。 当您创建 VNet 时，您的服务和您 VNet 中的虚拟机可以互相直接和安全地在云中。 这使通信安全地保存在 VNet 中，但仍允许您配置的虚拟机和服务需要作为解决方案的一部分的互联网通信的终结点连接。

- 安全地扩展您的数据中心

    使用 VNets，您可以构建传统站点到站点 (S2S) Vpn 安全地扩展您的数据中心容量。 S2S Vpn 使用 IPSEC 来提供您公司的 VPN 网关和 Azure 之间的安全连接。

- 启用混合云方案

    VNets 为您提供的灵活性来支持广泛的混合云方案。 可以安全地连接到任何类型的内部系统如大型机和 Unix 系统的基于云的应用程序。

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>如何知道我是否需要一个虚拟的网络？

请访问[虚拟网络概述](../articles/virtual-network/virtual-networks-overview.md)将帮助您确定适合您的最佳网络设计选项的决策表。

### <a name="how-do-i-get-started"></a>如何开始？

[虚拟网络文档](https://azure.microsoft.com/documentation/services/virtual-network/)开始，请访问。 此网页包含指向常见配置步骤以及将帮助您了解您需要设计虚拟网络时需要考虑的事项的信息。

### <a name="what-services-can-i-use-with-vnets"></a>使用 VNets 可以使用哪些服务？

VNets 可以使用各种不同的 Azure 服务，如云服务 (PaaS)、 虚拟机和 Web 应用程序。 但是，有几个 VNet 不支持的服务。 请检查您想要使用并验证它兼容的特定服务。

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>可以跨部署无法连接使用 VNets 吗？

是的。 不使用站点到站点连接，您可以使用 VNet。 这是特别有用如果您想要运行在 Azure 中的域控制器和 SharePoint 服务器场。

## <a name="virtual-network-configuration"></a>虚拟网络配置

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>使用何种工具来创建 VNet？

以下工具可用于创建或配置虚拟网络︰

- Azure 门户 (经典的和资源管理器 VNets)。

- 网络配置文件 (netcfg-只经典的 VNets)。 请参阅[配置虚拟网络使用网络配置文件](../articles/virtual-network/virtual-networks-using-network-configuration-file.md)。

- PowerShell (经典的和资源管理器 VNets)。

- Azure CLI (经典的和资源管理器 VNets)。

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>在我的 VNets 可以使用哪些地址范围？

您可以使用公用 IP 地址范围和在[RFC 1918](http://tools.ietf.org/html/rfc1918)定义任何 IP 地址范围。

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>我有在我的 VNets 的公用 IP 地址？

是的。 有关公用 IP 地址范围的详细信息，请参阅[在虚拟网络 (VNet) 的公用 IP 地址空间](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md)。 请记住您公用 Ip 不会直接通过 Internet 访问。

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>有限制的虚拟网络中的子网数目吗？

在 VNet 中使用的子网的数量没有限制。 所有子网必须完全包含在虚拟网络地址空间中，不应相互重叠。

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>是否有任何限制使用这些子网中的 IP 地址？

Azure 保留某些 IP 地址在每个子网内。 子网的第一个和最后一个 IP 地址被保留用于协议的一致性，以及 3 用于 Azure 服务的多个地址。

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>小型和大型如何可以如何 VNets 和子网是吗？

我们支持的最小的子网 /29，并且最大值 （使用 CIDR 子网定义） / 8。

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>可我带我的 Vlan 到 Azure 使用 VNets？

不。 VNets 是三层叠加。 Azure 不支持任何第 2 层语义。

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>我在我的 VNets 和子网指定自定义的路由策略

是的。 您可以使用用户定义路由 (UDR)。 UDR 有关详细信息，请访问[用户定义的路由和 IP 转发](../articles/virtual-network/virtual-networks-udr-overview.md)。

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNets 是否支持多播或广播？

不。 我们不支持多播或广播。

### <a name="what-protocols-can-i-use-within-vnets"></a>在 VNets 中可以使用哪些协议？

您可以使用 VNets 中的标准的基于 IP 的协议。 但是，多址广播，广播，IP 中的 IP 封装的数据包和通用路由封装 (GRE) 的数据包是在 VNets 内阻塞。 使用的标准协议包括︰

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>可以 ping 命令我在 VNet 中的缺省路由器

不。

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>可以使用 tracert 诊断连接？

不。

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>VNet 创建后是否可以添加子网？

是的。 子网能否将 VNets 在任何时候，只要不属于 VNet 中的另一个子网的子网地址。。

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>我在创建后是否可以修改我的子网的大小？

您可以添加、 删除、 展开或收缩子网，如果没有虚拟机或在其中使用 PowerShell cmdlet 或 NETCFG 文件部署服务。 您还可以添加、 删除、 展开或收缩，只要包含虚拟机或服务的子网不受此更改的任何前缀。

### <a name="can-i-modify-subnets-after-i-created-them"></a>我在创建后是否可以修改子网？

是的。 您可以添加、 删除和修改使用 VNet 的 CIDR 块。

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>我如果能够连接到互联网我在 VNet 运行我的服务？

是的。 部署在 VNet 中的所有服务可以都连接到互联网。 每个部署在 Azure 的云服务都有公开寻址 VIP 分配给它。 您将需要定义输入终结点的 PaaS 角色和虚拟机，以启用这些服务，以接受来自 internet 连接的终结点。

### <a name="do-vnets-support-ipv6"></a>VNets 支持 IPv6 吗？

不。 这一次，不能与 VNets 使用 IPv6。

### <a name="can-a-vnet-span-regions"></a>VNet 可以跨区域？

不。 VNet 被限制为单个区域。

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>我可以连接到 Azure 中的另一个 VNet VNet？

是的。 您可以创建 VNet VNet 通信使用 REST Api 或 Windows PowerShell。 您还可以连接 VNets 通过 VNet 对等。 有关对等的更多详细信息，请参阅[这里。](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>名称解析 (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>VNets 我的 DNS 选项有哪些？

使用在页上[的虚拟机和角色实例名称解析](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)决策表来指导您完成所有可用的 DNS 选项。

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>可以指定 DNS 服务器 VNet？

是的。 您可以 VNet 设置中指定的 DNS 服务器 IP 地址。 这将应用为默认 DNS 服务器的 VNet 中的所有虚拟机。

### <a name="how-many-dns-servers-can-i-specify"></a>可以指定多个 DNS 服务器？

您可以指定最多 12 个 DNS 服务器。

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>我已创建网络后是否可以修改我的 DNS 服务器？

是的。 您可以随时更改您 VNet 的 DNS 服务器列表。 如果您更改 DNS 服务器列表，需要重新启动每个 Vm 以便您 VNet 拾取新的 DNS 服务器。


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>什么是 Azure 提供 DNS，它是否适用于 VNets？

只要 azure DNS 是由 Microsoft 提供的多租户 DNS 服务。 Azure 将所有虚拟机和角色实例注册此服务中。 此服务提供名称解析由 Vm 和角色实例包含在同一个云服务中，主机名和 FQDN Vm 和角色实例在相同的 VNet。

> [AZURE.NOTE] 没有这次虚拟网络中的第一个 100 的云服务使用 Azure 提供 DNS 解析跨租户名称的限制。 如果使用 DNS 服务器，此限制不适用。

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>可以重写我的 DNS 设置，在每个 VM/服务的基础？

是的。 基于每个云服务来重写默认网络设置，可以设置 DNS 服务器。 但是，我们建议使用网络范围内尽最大可能的 DNS。

### <a name="can-i-bring-my-own-dns-suffix"></a>我可以带我自己的 DNS 后缀吗？

不。 不能为您 VNets 指定自定义的 DNS 后缀。

## <a name="vnets-and-vms"></a>VNets 和虚拟机

### <a name="can-i-deploy-vms-to-a-vnet"></a>可以将虚拟机部署到 VNet？

是的。

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>可以将 Linux 虚拟机部署到 VNet？

是的。 您可以部署 Linux 支持 Azure 的任何 distro。

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>公共 VIP 和内部 IP 地址之间的区别是什么？

- 内部的 IP 地址是通过 DHCP 分配给 VNet 中的每个虚拟机的 IP 地址。 它不是公共的面向。 如果您创建了 VNet，您在您的 VNet 的子网设置中指定的范围从分配的内部 IP 地址。 如果没有 VNet，还是会分配一个内部 IP 地址。 内部 IP 地址将一直与 VM 其生命周期内，除非 VM 被释放的。

- 公共 VIP 被分配到云服务或负载平衡器的公共 IP 地址。 它不直接分派给您的 VM 网卡。 VIP 随云服务，它被分配给所有虚拟机之前，云服务中释放或删除。 在这种情况下，它被释放。

### <a name="what-ip-address-will-my-vm-receive"></a>我的虚拟机将收到哪些 IP 地址？

- **内部的 IP 地址-**如果将虚拟机部署到 VNet，VM 将收到内部 IP 地址从您指定的内部 IP 地址池。 虚拟机在 VNets 中使用的内部 IP 地址进行通信。 虽然 Azure 分配动态的内部 IP 地址，您可以请求静态地址的 VM。 若要了解有关静态内部 IP 地址的详细信息，请访问[如何设置内部一个静态 IP](../articles/virtual-network/virtual-networks-reserved-private-ip.md)。

- **VIP 的**虽然 VIP 永远不会直接分配给 VM，程序也与 VIP，VM。 VIP 是可以分配给您的云服务的公共 IP 地址。 （可选） 可以保留云服务的 VIP。

- **-ILPIP**您还可以配置的实例级的公共 IP 地址 (ILPIP)。 ILPIPs 是直接与虚拟机，而不是云服务相关联。 若要了解有关 ILPIPs 的详细信息，请访问[实例级公用 IP 概述](../articles/virtual-network/virtual-networks-instance-level-public-ip.md)。

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>我将在以后创建 vm 保留内部 IP 地址

不。 不能预留一个内部 IP 地址。 如果一个内部 IP 地址可用它将由 DHCP 服务器分配给 VM 或角色实例。 该 VM 可能会或可能不是您想要分配到的内部 IP 地址。 但是，您可以到任何可用的内部 IP 地址更改已创建的虚拟机的内部 IP 地址。

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>不要在 VNet 中的虚拟机的内部 IP 地址更改？

是的。 内部 IP 地址使用 VM 的生存期内保持不除非解除 VM。 当虚拟机被释放时，除非您的 vm 中定义一个静态的内部 IP 地址被释放的内部 IP 地址。 如果只是停止 （并且不放入状态**已停止 (Deallocated)**） 虚拟机的 IP 地址仍分配给 VM。

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>可以手动分配 IP 地址的虚拟机中的 Nic？

不。 您不得更改虚拟机的任何接口属性。 任何更改可能会导致给 VM 中可能会丢失连接。

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>怎样为我的 IP 地址如果关闭虚拟机？

执行任何操作。 IP 地址 （VIP 公共和内部 IP 地址） 将保持您的云服务或虚拟机。

> [AZURE.NOTE] 如果想要简单地关闭虚拟机，请不要使用管理门户来做到这一点。 目前，关机按钮将取消分配虚拟机。

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>移动虚拟机从一个子网到另一个子网 VNet 而不重新部署？

是的。 您可以找到更多的信息[在此处](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md)。

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>我为我的虚拟机配置静态 MAC 地址

不。 不能静态配置的 MAC 地址。

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>将 MAC 地址保持不变我的虚拟机创建后？

是的 MAC 地址将保持不变的 VM 即使 VM 已停止 （释放） 并再次运行。

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>可以连接到互联网从一个 VNet 中的虚拟机？

是的。 部署在 VNet 中的所有服务可以都连接到互联网。 此外，每个部署在 Azure 的云服务有公开寻址 VIP 分配给它。 您必须定义输入终结点的 PaaS 角色和虚拟机以启用这些服务，以接受来自 Internet 的连接的终结点。

## <a name="vnets-and-services"></a>VNets 和服务

### <a name="what-services-can-i-use-with-vnets"></a>使用 VNets 可以使用哪些服务？

您只能使用 VNets 中的计算服务。 计算服务会被限制到云服务 （web 和辅助角色） 和虚拟机。

### <a name="can-i-use-web-apps-with-virtual-network"></a>可以使用虚拟网络使用 Web 应用程序？

是的。 您可以将部署 Web 应用程序内使用 ASE （应用程序服务环境） VNet。 将添加到 Web 应用程序，可以安全地连接并访问 Azure VNet 中的资源，如果必须点到站点为您 VNet 配置。 有关详细信息，请参阅以下文章︰


- [在一个应用程序服务的环境中创建 Web 应用程序](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Web 应用程序虚拟网络集成](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Web 应用程序中使用 VNet 集成和混合连接](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [将与 Azure 虚拟网络集成在 web 应用程序](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>可以将部署 web 和工作人员中的角色 (PaaS) VNet 与云服务？

是的。 您可以部署在 VNets 内的 PaaS 服务。

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>如何将 PaaS 角色部署到 VNet？

可以通过在您的服务配置网络配置部分中指定的 VNet 名称和角色 /subnet 映射来实现此目的。 您不需要更新任何二进制文件的位置。

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>可以将移动我和 VNets 的服务？

不。 不能移动和 VNets 的服务。 您将需要删除和重新部署该服务以将其移动到另一个 VNet。

## <a name="vnets-and-security"></a>VNets 和安全

### <a name="what-is-the-security-model-for-vnets"></a>VNets 的安全模型是什么？

VNets 是完全独立于另一个，和 Azure 的基础结构中托管的其它服务。 VNet 是一种信任边界。

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>可以将 Acl 或 NSGs 定义上我 VNets？

不。 不能将 Acl 或 NSGs 与 VNets 相关联。 但是，可以为已部署到 VNets 的虚拟机上输入终结点定义 Acl 和 NSGs 可关联到的子网或 Nic。

### <a name="is-there-a-vnet-security-whitepaper"></a>是否有 VNet 安全白皮书？

是的。 您可以下载它[这里](http://go.microsoft.com/fwlink/?LinkId=386611)。

## <a name="apis-schemas-and-tools"></a>Api、 架构和工具

### <a name="can-i-manage-vnets-from-code"></a>可以从代码管理 VNets？

是的。 可以使用 REST Api 来管理 VNets 和跨内部连接。 详细信息，请参阅[此处](http://go.microsoft.com/fwlink/?LinkId=296833)。

### <a name="is-there-tooling-support-for-vnets"></a>有 VNets 的刀具支持吗？

是的。 PowerShell 和命令行工具可用于多种平台。 详细信息，请参阅[此处](http://go.microsoft.com/fwlink/?LinkId=317721)。
