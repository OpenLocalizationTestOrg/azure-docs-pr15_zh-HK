<properties
   pageTitle="实施混合网络体系结构与 Azure ExpressRoute |参考体系结构 |Microsoft Azure"
   description="如何实现跨越 Azure 的虚拟网络，通过使用 Azure ExpressRoute 连接内部网络的安全站点到站点网络体系结构。"
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>实施与 Azure ExpressRoute 混合网络体系结构

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

本文介绍了使用 ExpressRoute 连接在 Azure 上虚拟的网络到内部网络的最佳做法。 ExpressRoute 连接使用专用的专用的连接，通过第三方连接提供程序进行。 专用连接将内部网络扩展到 Azure 提供访问到 Azure 的公共使用 PaaS 服务和 Office365 SaaS 服务的终结点您自己 IaaS 的基础架构。 本文重点介绍使用 ExpressRoute 连接到一个 Azure 虚拟网络 (VNet) 使用所谓私人对等。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 该蓝图使用资源管理器，Microsoft 建议为新的部署。

这种体系结构的典型使用情况包括︰

- 在内部网络和 Azure 之间分发工作负载混合应用程序。

- 运行需要很高的可扩展性的大规模、 关键工作负载的应用程序。

- 数据必须保存在现场以外的地方进行大规模备份和还原功能。

- 处理大数据工作负载。

- 作为灾难恢复站点使用 Azure。

> [AZURE.NOTE] [ExpressRoute 技术概述][expressroute-technical-overview]介绍了 ExpressRoute。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示此体系结构中重要的组成部分︰

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此图是"混合网络-ER"页上。

![[0]][0]

- **Azure 虚拟网络 (VNets)。** 每个 VNet 驻留在单个 Azure 区域，并可以承载多个应用程序层。 应用程序层可以在每个 VNet 中使用的子网划分和/或网络安全组 (NSGs)。 

- **Azure 的公共服务。** 它们可以在混合应用程序使用的 Azure 服务。 这些服务也可通过公共互联网，但 ExpressRoute 电路通过访问这些由于通信不进入互联网提供低延迟和更可预测的性能。 连接是通过执行**公共对等**，由您的组织拥有或连接提供商所提供的地址。 

- **Office 365 的服务。** 它们是公开可用 Office 365 提供应用程序和由 Microsoft 提供的服务。 连接是通过执行**Microsoft 对等**，由您的组织拥有或连接提供商所提供的地址。

>[AZURE.NOTE] 您还可以直接向在线通过 Microsoft 对等的 Microsoft CRM 进行连接。

- **内部公司网络。** 这是一种计算机和设备，通过组织内部运行的专用本地网络连接的网络。

- **本地的边界路由器。** 它们是由提供程序管理的电路连接内部网络的路由器。 根据如何配置您的连接，您需要提供的路由器使用的公用 IP 地址。 

- **Microsoft 的边界路由器。** 这些都是主动-主动高可用性配置中的两个路由器。 这些路由器启用其电路直接连接到其数据中心的连接提供。 根据如何配置您的连接，您需要提供的路由器使用的公用 IP 地址。

- **ExpressRoute 的电路。** 第 2 层或第 3 层电路由连接提供商边缘路由器通过 Azure 该连接内部网络。 电路使用托管连接提供了硬件基础结构。

- **连接提供程序。** 这些是公司所提供的连接使用第 2 层或第 3 层数据中心和 Azure 数据中心之间的连接。

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构应遵循这些建议，除非有推荐不适合特定的要求。

### <a name="connectivity-providers"></a>连接提供程序

选择合适的 ExpressRoute 连接提供有关您所在的位置。 要获得连接在您的场所的提供程序的列表，请使用下面的 Azure PowerShell 命令︰

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute 连接提供程序连接您的数据中心到 Microsoft 在以下方面︰

- **共同位于云交换。** 如果共同在位于云交换功能，您可以订购到 Microsoft 云通过主机托管提供商的以太网交换的虚拟交叉连接。 主机托管提供商可以提供第 2 层的交叉连接或托管的 3 层交叉连接机构托管您的基础架构和 Microsoft 云之间.

- **点到点的以太网连接。** 您可以连接到 Microsoft 云通过点对点以太网链路的内部数据中心/办事处。 点对点以太网提供商可以提供第二层连接或管理您的站点和 Microsoft 云之间的第 3 层连接。

- **任意对任意的 (IPVPN) 网络。** 您可以使用 Microsoft 云集成您的 WAN。 IPVPN 提供商 (通常 MPLS VPN) 提供任意到任意数据中心和分支办公室之间的连接。 到您 WAN 来使它看上去就像任何其他分支机构一样，Microsoft 云可以相互连接。 WAN 提供商通常提供管理第 3 层连接。

有关连接的提供程序的详细信息，请参阅[ExpressRoute 电路和路由域][connectivity-providers]。

### <a name="expressroute-circuit"></a>ExpressRoute 电路

确保您的组织满足了[ExpressRoute 系统必备要求][expressroute-prereqs]连接到 Azure。

如果尚未执行此操作，添加名为子网`GatewaySubnet`到 Azure VNet 和创建使用 Azure VPN 网关服务 ExpressRoute 虚拟网络网关。 有关此过程的详细信息，请参见[ExpressRoute 工作流电路资源调配和电路状态为][ExpressRoute-provisioning]。

创建 ExpressRoute 电路，如下所示︰

1. 运行以下 PowerShell 命令︰

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. 发送`ServiceKey`的服务提供程序的新电路。

3. 等待要调配电路的提供程序。 可以通过使用以下 PowerShell 命令验证电路的配置状态︰

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

`Provisioning state`字段中`Service Provider`输出部分将从`NotProvisioned`到`Provisioned`电路时准备就绪。

>[AZURE.NOTE]如果您使用的第 3 层连接，提供程序应配置和管理路由为您;提供启用提供程序实现适当的路由所需的信息。

如果您使用的第二层连接，请按照以下步骤操作︰

1. 保留两个/30 的子网组成，对于每种类型有效的公用 IP 地址所需的对等来实现。 这些/30 的子网将用于提供电路所用路由器的 IP 地址。 如果您要实现专用、 公众和 Microsoft 对等，将需要 6 /30 提供有效的公共 IP 地址的子网。     

2. 配置为 ExpressRoute 电路的路由。 您需要运行以下命令，将为每种类型您想要配置 （私有、 公众和 Microsoft） 的对等。

    >[AZURE.NOTE]请参阅[创建和修改路由 ExpressRoute 电路][configure-expressroute-routing]的详细信息。 使用下面的 PowerShell 命令来添加路由通信的对等网络︰

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. 保留为公共的使用 NAT 和 Microsoft 对等有效的公用 IP 地址的另一个池。 建议有不同的池，每个对等的。 因此他们可以配置这些区域的 BGP 通告指定到您连接的提供程序，该池。

[链接到 ExpressRoute 电路您私有云中的 VNet(s)][link-vnet-to-expressroute]。 使用下面的 PowerShell 命令︰

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

请注意以下几点︰

- ExpressRoute 使用的交换路由信息网络和 Azure 之间的边界网关协议 (BGP)。

- 您可以连接多个位于不同地区对相同的 ExpressRoute 电路，只要所有的 VNets 和 ExpressRoute 电路位于相同的地缘政治区域内的 VNets。

## <a name="scalability-considerations"></a>可伸缩性注意事项

ExpressRoute 电路提供高带宽之间的网络路径。 通常情况下，更高的带宽更高的成本。 尽管某些提供程序使您可以更改您的带宽，请确保选择初始带宽超过您的需要，并提供发展的空间。 以防您需要在将来增加带宽，留下两个选项。

增加带宽。 请记住，并非所有提供程序使您得以动态地做到这一点。 然后您应该避免为此尽最大可能。 但如果需要请与您的提供商，检查后运行下面的命令。

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

您可以增加而不会丢失连接的带宽。 降级的带宽会中断连接。 您必须删除电路并使用新的配置重新创建它。

如果您使用的标准的 SKU 的 ExpressRoute 以及需要升级到特优，或更改您正在您的定价计划 （计量或不受限制），运行下面的命令。 `Sku.Tier`属性可以是`Standard`或`Premium`;`Sku.Name`属性可以是`MeteredData`或`UnlimitedData`。

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] 请确保`Sku.Name`属性匹配`Sku.Tier`， `Sku.Family`。 如果您更改系列层，而不是名称，您的连接将被禁用。

您可以升级 SKU 无中断的方式，但不是能无限制的定价计划，以计量从切换。 当降级 SKU，带宽消耗必须保持在标准的 SKU 的默认限制内。

> [AZURE.NOTE] ExpressRoute 提供两个定价计划客户，根据计量或无限制的数据。 请参阅[定价 ExpressRoute] [expressroute-pricing]的详细信息。 费用因电路带宽。 可用带宽可能会有所不同从提供者到提供程序。 使用`Get-AzureRmExpressRouteServiceProvider`cmdlet 以查看在您的地区和他们提供的带宽供应商。

单个 ExpressRoute 电路可以支持大量的 peerings 和 VNet 的链接。 请参阅[ExpressRoute 限制][expressroute-limits]的详细信息。

额外的费用，提供了 ExpressRoute 高级加载项︰

- 每个电路的达 10000 个路由。 ExpressRoute 高级加载项，而限制为 4000 每个电路的工艺路线。

- 全局启用任意位置位于世界上任何地区而不是只是在同一个洲地区访问资源 ExpressRoute 电路连接。

- 每个电路从 10 到更大的限制，这取决于线路的带宽 VNet 链接的数目增加。

ExpressRoute 电路设计为允许将两倍的带宽限制，您获得的免费的临时网络突发情况。 这被通过使用冗余的链接。 但是，并不是所有连接提供程序都支持此功能。请验证您的连接的提供程序使依赖此功能之前。

## <a name="availability-considerations"></a>可用性考虑事项

您可以以不同的方式，具体取决于您使用时，提供程序的类型和数目 ExpressRoute 电路和虚拟网络网关连接，您可以配置 Azure 连接配置高可用性。 下面几点总结可用性选项︰

- ExpressRoute 不支持路由器冗余协议来实现高可用性的 HSRP 和 VRRP 等。 相反，它使用冗余对 BGP 会话，每个对等。 为了促进高可用性连接到您的网络，Microsoft Azure 提供您使用主动-主动配置两个路由器 （Microsoft 边缘的一部分） 上的两个冗余端口。

- 如果您使用的第二层连接，将部署中的主动-主动配置内部网络中的冗余路由器。 将主电路连接到一台路由器，并与其他的辅助电路。 这将使您的高可用性连接在连接的两端。 这是有必要的如果您需要 ExpressRoute SLA。 请参阅[SLA Azure ExpressRoute] [sla-for-expressroute]的详细信息。

下的图显示了一个具有内部部署冗余路由器配置连接到主要和辅助电路。 每个电路处理公共对等和私人对等通信 (每个对等的 /30 一对指定地址空间为，在上一节中所述)。

![[1]][1]

如果您使用的第 3 层连接，验证它是否提供了冗余的 BGP 会话的可用性会为您处理。

可以将虚拟网络连接到多个 ExpressRoute 电路，每种电路可以提供不同的服务提供商。 此策略提供更好的高可用性和灾难恢复能力。

ExpressRoute 作为故障切换路径配置站点到站点 VPN。 这仅适用于专用的对等。 对于 Azure 和 Office 365 提供服务，互联网是唯一的故障切换路径。

默认情况下，BGP 会话使用的空闲超时值为 60 秒。 一旦会话超时 3 次，路由标记为不可用，并且所有通信流重定向到其他路由器。 这 180 秒的超时可能太长，关键应用程序。 在这种情况下，您可以更改上内部路由器的 BGP 超时设置为较小的值。

## <a name="manageability-considerations"></a>可管理性注意事项

您可以使用[Azure 连接 Toolkit (AzureCT)] [azurect]来监视您的内部数据中心和 Azure 之间的连接性。

## <a name="security-considerations"></a>安全注意事项

以不同的方式，具体取决于您的安全问题和法规遵从性需要，可以为 Azure 连接配置安全选项。 下面几点总结安全选项。

ExpressRoute 在第 3 层工作。 通过限制对合法资源的通信与网络安全装置，可防止应用程序层中的威胁。 此外，只可以从内部开始 ExpressRoute 连接使用公共对等。 这样可以防止恶意服务中访问和危害公共 Internet 的内部数据。

若要最大限度地安全，添加提供商边缘路由器的内部网络之间的网络安全设备。 这将有助于限制未经授权的通信流入 VNet:

![[2]][2]

用于审核或法规遵从性目的，可能有必要禁止在 VNet 互联网运行的组件直接访问并实施[强制隧道][forced-tuneling]。 在此情况下，Internet 通信应被重定向回通过代理运行内部可以进行审核。 可以配置代理服务器以阻止未经授权的通信流出，并筛选潜在的恶意的入站的通信。

![[3]][3]

默认情况下，Azure Vm 公开用于为基于 Linux 的 Vm 通过 Azure 门户部署时提供用于管理目的的 RDP 远程 Powershell 的 Windows 虚拟机和 SSH 登录访问的终结点。 若要最大限度地安全，不使一个公用 IP 地址为您的 Vm，并使用 NSGs，以确保这些虚拟机不是可公开访问。 虚拟机仅应使用的内部 IP 地址。 这些地址可通过 ExpressRoute 网络，使内部 DevOps 的员工来执行任何必需的配置或维护。

如果必须为虚拟机公开管理终结点与外部网络，使用 NSGs，和/或访问控制列表来为白名单的 IP 地址或网络限制这些端口的可见性。

## <a name="troubleshooting-considerations"></a>故障排除注意事项

如果以前正常工作的 ExpressRoute 电路现在无法连接，或者您专用的 VNet 中的任何配置更改内部没有可能需要联系该连接提供程序并使用它们来解决问题。 您还可以使用以下的 Azure PowerShell 命令执行一些有限的检查，并帮助您确定问题可能出在︰

- 验证已设置电路︰

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

此命令的输出显示您电路的几个属性包括`ProvisioningState`， `CircuitProvisioningState`，和`ServiceProviderProvisioningState`如下所示。

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

如果`ProvisioningState`未设置为`Succeeded`您尝试创建新的电路后，使用下面的命令删除电路并尝试重新创建。

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

如果您的提供商已有提供电路，和`ProvisioningState`设置为`Failed`，或`CircuitProvisioningState`不是`Enabled`，与您的提供商联系以获得进一步帮助。

## <a name="solution-deployment"></a>解决方案部署

如果您有现有的内部部署基础结构已经用一种合适的网络设备配置，您可以按照下面的步骤部署参考体系结构︰

如果您已配置 VPN 装置与现有的内部基础结构，您可以按照下面的步骤部署参考体系结构︰

1. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. 等待该链接打开在 Azure 的门户中，然后按照下列步骤操作︰ 
  - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-hybrid-er-rg`在文本框中。
  - 从**位置**下拉框中选择的地区。
  - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
  - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
  - 单击**购买**按钮。

3. 等待部署完成。

4. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. 等待该链接打开在 Azure 的门户中，然后按照下列步骤操作︰ 
  - 在**资源组**部分中选择**使用现有的**和输入`ra-hybrid-er-rg`在文本框中。
  - 从**位置**下拉框中选择的地区。
  - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
  - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
  - 单击**购买**按钮。

6. 等待部署完成。

## <a name="next-steps"></a>下一步行动

- [实现高可用性的混合网络体系结构]，请参阅[highly-available-network-architecture]有关增加混合网络的可用性信息基于 ExpressRoute 通过 VPN 连接到故障切换。

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "混合使用 Azure ExpressRoute 的网络体系结构"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "使用 ExpressRoute 的主要和辅助电路的冗余路由器"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "添加到内部网络的安全设备"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "使用强制隧道审核通往 Internet 的通信"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "查找 ExpressRoute 电路 ServiceKey"
