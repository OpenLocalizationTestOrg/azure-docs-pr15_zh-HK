<properties
   pageTitle="在 Azure 中实施安全混合网络体系结构 |Microsoft Azure"
   description="如何在 Azure 中实现安全混合网络体系结构。"
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
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
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>DMZ Azure 和本地数据中心之间实现

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍了实现将内部网络扩展到 Azure 安全混合网络的最佳做法。 此参考体系结构实现了内部网络和使用用户定义的路由 (UDRs) 的 Azure 虚拟网络之间的 DMZ。 DMZ 包括高可用性网络虚拟装置 (NVAs) 实现安全功能，如防火墙和数据包检查。 从 VNet 的所有出站通讯是强制隧道传输到互联网，通过内部网络以便它可被审核。 

此体系结构需要连接到使用任一一个[VPN 网关]实现内部数据中心[ra-vpn]， [ExpressRoute]或[ra-expressroute]连接。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 此参考体系结构使用资源管理器，Microsoft 建议为新的部署。

这种体系结构的典型使用情况包括︰

- 混合应用程序中的工作负载运行部分内部和部分在 Azure。

- 路由入站通信从内部和 internet 的 azure 基础结构。

- 审核的传出通信所需的应用程序。 这通常是许多商业系统的管理法规要求，并有助于防止私人信息的事实公之于众。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示此体系结构中重要的组成部分︰

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此关系图位于"DMZ-私有"的页面上。

[![0]][0]

- **内部网络。** 这是一种计算机网络，通过专用本地网络，在组织中实现连接的设备。

- **Azure 虚拟网络 (VNet)。** VNet 作为宿主的应用程序和其他资源在云中运行。

- **网关。** 网关提供的内部网络中的路由器和 VNet 之间的连接。

- **网络虚拟设备 (NVA)。** NVA 是个通用术语，用于描述虚拟机执行任务，如允许或拒绝为防火墙、 优化 WAN 的操作 （包括网络压缩），定制路由选择事件，或其他网络功能的访问。 

- **Web 层、 业务层和数据层的子网。** 它们承载的虚拟机和实现示例 3 层应用程序在云中运行的服务的子网。 请参见[N 层体系结构在 Azure 上运行 Windows 虚拟机][ra-n-tier]的详细信息。

- **用户定义的路由 (UDR)。** [用户定义的路由][udr-overview]定义的 IP 流量在 Azure VNets。 

> [AZURE.NOTE] 根据您的 VPN 连接的要求，可以作为一种替代方法，可以使用 UDRs 来实现直接通信的转发规则后通过内部网络配置边界网关协议 (BGP) 路由。

- **管理子网。** 此子网包含虚拟机实现管理和监视功能，在 VNet 中运行的组件。 

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构应遵循这些建议，除非有推荐不适合特定的要求。

### <a name="rbac-recommendations"></a>RBAC 的建议

创建几个 RBAC 角色来管理应用程序中的资源。 请考虑创建[自定义角色]DevOps[rbac-custom-roles]与管理的基础结构的应用程序的权限。 请考虑创建一个集中式的 IT 管理员[自定义角色][rbac-custom-roles]管理网络资源和 IT 管理员[自定义角色]的独立安全[rbac-custom-roles]来管理安全的网络资源，如 NVAs。 

DevOps 角色应包括权限要部署的应用程序组件以及监视并重新启动虚拟机。 集中式的 IT 管理员角色应包括监控网络资源的权限。 这两种都不应当限于安全 IT 管理员角色，角色应具有访问权限 NVA 资源。

### <a name="resource-group-recommendations"></a>资源组建议

将它们组合在一起到资源组可以方便地管理 azure 的资源，如虚拟机、 VNets 和负载平衡器。 您可以将上面的角色分配给每个资源组来限制访问。

我们建议您创建以下︰

- 包含子网 （不包括虚拟机）、 NSGs 和网关连接到内部网络资源的资源组。 将集中式的 IT 管理员角色分配给此资源组中。

- （包括负载平衡器） NVAs、 跳转框和其他管理的虚拟机，以及强制所有通信通过 NVAs 网关网的 UDR 中包含虚拟机的资源组。 将安全 IT 管理员角色分配给此资源组中。

- 单独为每个应用程序层包含负载平衡器和虚拟机的资源组。 请注意，此资源组不应包括每一层的子网。 将 DevOps 角色分配给此资源组中。

### <a name="virtual-network-gateway-recommendations"></a>虚拟网络网关建议

内部通讯到 VNet 通过虚拟网络网关。 我们建议[Azure VPN 网关][guidance-vpn-gateway]或[Azure ExpressRoute 网关][guidance-expressroute]。 

### <a name="nva-recommendations"></a>NVA 建议

NVAs 提供用于管理和监视网络通信的不同服务。 Azure 市场上提供多种第三方供应商 NVAs，其中包括︰

- [Barracuda Web 应用程序防火墙][barracuda-waf]和[Barracuda NextGen 防火墙][barracuda-nf]

- [内聚性网络 VNS3 防火墙/路由器/VPN][vns3]

- [Fortinet FortiGate 虚拟机][fortinet]

- [SecureSphere Web 应用程序防火墙][securesphere]

- [DenyAll Web 应用程序防火墙][denyall]

- [检查点 vSEC][checkpoint]

如果这些第三方 NVAs 的满足您的需求，您可以创建自定义 NVA，使用虚拟机。 有关创建自定义的 NVAs 的示例，请参见 DMZ 中此参考体系结构，实现了以下功能︰

- 通信将被路由使用[IP 转发][ ip-forwarding] NVA Nic 上。

- 允许通信通过 NVA，才应该这样做。 参考体系结构中的每个 NVA VM 是简单 Linux 路由器与网络接口*eth0*和出站通信流匹配规则定义的自定义脚本调度通过网络接口*eth1*上到达的入站通信。 

- 管理子网路由的通信不经过 NVAs 和 NVAs 只能从管理子网配置。 管理子网的通信需要通过 NVAs 进行路由，如果没有到要解决 NVAs，如果他们失败的管理子网路由。  

- 对于 NVA Vm 都包括在负载平衡器后面设置可用性。 网关网中的 UDR NVA 将请求定向到负载平衡器。

另一个要考虑的建议与执行专用的安全任务每个 NVA 连接多个系列中的 NVAs。 这允许每个安全函数在每个 NVA 基础上进行管理。 例如，部署防火墙 NVA 无法系列中放有 NVA 运行身份服务中。 易管理性的代价是添加了额外的网络跃点计数可能会增加延迟时间，因此请确保这不会影响应用程序的性能。

### <a name="nsg-recommendations"></a>NSG 的建议

VPN 网关公开连接到内部网络的公用 IP 地址。 我们建议您创建网络安全组 (NSG) 的入站 NVA 网实现规则以阻止并非从内部网络发出的所有通信。

我们还建议您实现为每个子网的 NSGs，以提供第二层保护以防止未正确配置或已禁用 NVA 绕过的入站通信。 例如，web 层子网中的参考体系结构实现的规则的 NSG 忽略以外来自内部网络 (192.168.0.0/16) 或 VNet，并忽略所有不在端口 80 上发出请求的另一个规则的所有请求。 

### <a name="internet-access-recommendations"></a>互联网访问的建议

[强制隧道][azure-forced-tunneling]通过内部网络使用的站点到站点 VPN 隧道和互联网使用的工艺路线的所有出站 internet 通讯网络地址 tranlation (NAT)。 这将防止意外泄露任何机密信息存储在数据层中的又还，可以检查和审核的所有出站通讯。

> [AZURE.NOTE] 不完全阻止从网络、 业务和应用程序层的互联网通信。 如果这些层使用 Azure PaaS 服务依赖于 VM 的诊断日志记录的公用 IP 地址，下载的 VM 扩展和其他功能。 Azure 的诊断也需要组件可以读取和写入依赖于互联网的 Azure 存储帐户。

我们进一步建议您验证出站 internet 通讯被强制隧道正确。 如果您使用 VPN 连接与[路由和远程访问服务][routing-and-remote-access-service]在内部服务器上，使用像[WireShark]的工具[wireshark]或[Microsoft 消息分析器](https://www.microsoft.com/en-us/download/details.aspx?id=44226)。

### <a name="management-subnet-recommendations"></a>管理子网建议

管理子网包含一个跳转框执行的管理和监视功能。 实现跳转框中的以下建议︰
- 不要创建跳转框的公用 IP 地址。 
- 创建一个通过传入网关访问跳转框并在管理子网要只允许路由从响应请求实现 NSG 的路由。
- 限制对跳转框中的所有安全管理任务的执行。

### <a name="nva-recommendations"></a>NVA 建议

包括第 7 层 NVA 终止 NVA 级别的应用程序连接和维护使用的后端层的关系。 这保证从后端层的响应通信量通过 NVA 返回对称连接。

## <a name="scalability-considerations"></a>可伸缩性注意事项

参考体系结构实现了内部网络将流量引向 NVA 设备一个池的负载平衡器。 如前文所述，NVA 设备是虚拟机执行网络通信量路由规则和部署到[可用性设置][availability-set]。 这种设计使您能够随着时间的推移监视 NVAs 的吞吐量和响应负载的增加而添加 NVA 设备。

标准的 SKU VPN 网关支持持续的吞吐量达 100 Mbps 的速度。 高性能 SKU 提供达 200 Mbps。 更高的带宽，请考虑升级到 ExpressRoute 网关。 ExpressRoute 提供了 VPN 连接比低延迟达 10 Gbps 带宽。

> [AZURE.NOTE] [实现一种混合网络体系结构 Azure 和内部部署 VPN]的文章[guidance-vpn-gateway]和[实施混合网络体系结构与 Azure ExpressRoute] [guidance-expressroute]描述围绕 Azure 的网关的可扩展性问题。

## <a name="availability-considerations"></a>可用性考虑事项

参考体系结构实现分发请求从内部到 Azure NVA 设备的池的负载平衡器。 NVA 设备虚拟机执行网络通信量路由规则，部署到[可用性设置][availability-set]。 负载平衡器定期查询健康探测器在每个 NVA 上实现，并将从池中删除所有响应 NVAs。 

如果您使用 Azure ExpressRoute VNet 和内部网络[配置 VPN 网关提供故障转移]之间提供连接[guidance-vpn-failover]ExpressRoute 连接不可用时。

保持对 VPN 和 ExpressRoute 连接可用性的详细信息，请参阅文章[实现一种混合网络体系结构 Azure 和内部部署 VPN] [guidance-vpn-gateway]和[实施混合网络体系结构与 Azure ExpressRoute][guidance-expressroute]。

## <a name="manageability-considerations"></a>可管理性注意事项

管理子网中的跳转框应执行所有的应用程序和资源监控。 这取决于应用程序的要求，您可能需要添加其他监视资源管理子网，但这些其他资源应通过跳转框中访问的再次。

如果从内部网络到 Azure 的网关连接处于断开状态时，仍可以通过部署骰，将其添加到跳转框中和中的远程处理从互联网通过与跳转框。

参考体系结构中的每一层子网受 NSG 规则，则可能需要创建一个规则来打开端口 3389 RDP 访问 Windows 虚拟机或 Linux 虚拟机上的 SSH 访问端口 22。 其他管理和监视工具可能需要打开其他端口的规则。

如果您使用 ExpressRoute 提供的内部数据中心和 Azure 之间连接，使用[Azure 连接 Toolkit (AzureCT)] [azurect]监视和诊断连接问题。

> [AZURE.NOTE] 您可以找到专门针对监视和管理 VPN 和 ExpressRoute 的连接，[实现一种混合网络体系结构 Azure 和内部部署 VPN]的文章中的其他信息[guidance-vpn-gateway]和[实施混合网络体系结构与 Azure ExpressRoute][guidance-expressroute]。

## <a name="security-considerations"></a>安全注意事项

此参考体系结构实现的多个级别的安全︰ 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>通过 NVA 所有内部用户请求都路由

网关网中的 UDR 阻止来自内部的以外的所有用户请求。 UDR 刀路中的专用 DMZ 子网，允许对 NVAs 请求，这些请求都将传递给应用程序如果 NVA 规则允许它们。 其他路由可以添加到 UDR，但确保他们不小心不要绕过 NVAs 或阻止管理通信用于管理子网。

负载平衡器在 NVAs 也通过忽略未在负载平衡规则的端口上的通信作为安全保护装置。 参考体系结构中的负载平衡器只为在端口 80 上的 HTTP 请求和 HTTPS 请求，在端口 443 上的侦听。 任何其他规则添加到负载平衡器必须记录并应监视的通信以确保没有任何安全问题。

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>使用 NSGs 应用程序层之间的块密码/通信

每个 web、 业务层和数据层限制它们之间的通信使用 NSGs。 即在业务层使用 NSG 来阻止所有通信在 web 层中，并不是源自和数据层使用 NSG 都阻止不是源自在业务层中的所有通信。 如果您有需要扩展 NSG 规则，以允许更广泛访问这些存储层，权衡这些要求的安全风险。 每个新入站的路径表示意外或有意的数据泄露或应用程序损坏的机会。

### <a name="devops-access"></a>DevOps 的访问

DevOps 可以使用[RBAC]每一层执行的操作限制[rbac]管理权限。 在授予权限时，使用[最小特权原则][security-principle-of-least-privilege]。 记录所有管理操作并执行定期审核，以确保规划的任何配置更改。

> [AZURE.NOTE] 有关详细信息、 示例、 以及管理 Azure，与网络安全有关的方案，请参阅[Microsoft 云服务和网络安全][cloud-services-network-security]。 有关保护云中的资源的详细信息，请参阅[Microsoft Azure 安全入门][getting-started-with-azure-security]。 有关通过 Azure 的网关的连接，解决安全问题的其他详细信息，请参见[实现一种混合网络体系结构 Azure 和内部部署 VPN] [guidance-vpn-gateway]和[实施混合网络体系结构与 Azure ExpressRoute][guidance-expressroute]。

## <a name="solution-deployment"></a>解决方案部署

为实现这些建议的参考体系结构部署是在 Github 上可用。 此参考体系结构包括一个虚拟网络 (VNet)、 网络安全组 (NSG)、 负载平衡器和两个虚拟机 (Vm)。

参考体系结构可以按照下面的说明使用 Windows 或 Linux 虚拟机部署︰ 

1. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. 一旦在 Azure 的门户已经打开链接，您必须设置的某些输入值︰ 
    - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-private-dmz-rg`在文本框中。
    - 从**位置**下拉框中选择的地区。
    - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
    - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
    - 单击**购买**按钮。

3. 等待部署完成。

4. 参数文件包括硬编码的管理员用户名和密码适用于所有虚拟机，以及强烈建议您立即更改两个。 部署中的每台虚拟机，Azure 门户中将其选中，然后单击**支持 + 疑难解答**刀片式服务器中的上**重设密码**。 在**模式**下拉列表框中，选择**重置密码**，然后选择一个新**的用户名**和**密码**。 单击**更新**按钮来保持不变。

## <a name="next-steps"></a>下一步行动

- 了解如何实现[DMZ Azure 和 Internet 之间](./guidance-iaas-ra-secure-vnet-dmz.md)。
- 了解如何实现[高可用混合网络体系结构](./guidance-hybrid-network-expressroute-vpn-failover.md)。

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "安全的混合网络体系结构"
