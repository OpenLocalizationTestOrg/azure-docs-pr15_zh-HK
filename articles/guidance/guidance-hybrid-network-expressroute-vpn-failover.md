<properties
   pageTitle="实现高可用性的混合网络体系结构 |Microsoft Azure"
   description="如何实现跨越 Azure 的虚拟网络，并通过 ExpressRoute 使用 VPN 网关故障转移连接内部网络的安全站点到站点网络体系结构。"
   services="guidance,virtual-network,vpn-gateway,expressroute"
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

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>实现高可用性的混合网络体系结构

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

本文介绍了 ExpressRoute，使用站点到站点虚拟专用网络 (VPN) 作为故障转移连接到 Azure 上虚拟网络连接内部网络的最佳做法。 内部网络和 ExpressRoute 连接到 Azure 的虚拟网络 (VNet) 之间流动通信。  如果没有 ExpressRoute 电路中的连接丢失，则将通过 IPSec VPN 隧道路由通信。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 该蓝图使用资源管理器，Microsoft 建议为新的部署。

这种体系结构的典型使用情况包括︰

- 在内部网络和 Azure 之间分发工作负载混合应用程序。

- 运行需要很高的可扩展性的大规模、 关键工作负载的应用程序。

- 数据必须保存在现场以外的地方进行大规模备份和还原功能。

- 处理大数据工作负载。

- 作为灾难恢复站点使用 Azure。

请注意，ExpressRoute 电路不可用，是否 VPN 路由将只处理专用对等连接。 通过互联网将会通过公共对等和 Microsoft 对等连接。

## <a name="architecture-diagram"></a>体系结构关系图

>[AZURE.NOTE] [Azure VPN 网关服务][azure-vpn-gateway]实现两种类型的虚拟网络网关;VPN 虚拟网络网关和 ExpressRoute 虚拟网络网关。 在本文中，的指*VPN 网关*的 Azure 服务，而短语*VPN 虚拟网络网关*和*ExpressRoute 虚拟网络网关*用于分别引用 VPN 和 ExpressRoute 网关的实现。

下图突出显示此体系结构中重要的组成部分︰

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此图是在"混合网络-ER VPN"页。

![[0]][0]

- **Azure 虚拟网络 (VNets)。** 每个 VNet 驻留在单个 Azure 区域，并可以承载多个应用程序层。 应用程序层可以在每个 VNet 中使用的子网划分和/或网络安全组 (NSGs)。

- **内部公司网络。** 这是一种计算机和设备，通过组织内部运行的专用本地网络连接的网络。

- **VPN 设备。** 这是一个设备或提供外部连接到内部网络的服务。 VPN 设备可能是硬件设备，也可以是一个软件解决方案，如路由和 Windows Server 2012 中的远程访问服务 (RRAS)。

> [AZURE.NOTE] 有关支持的 VPN 设备和配置选定的 VPN 装置到 Azure VPN 网关连接信息的列表，请参阅[支持的 Azure 的 VPN 设备的列表]中适当的设备说明[vpn-appliance]。

- **VPN 虚拟网络网关。** 该 VPN 虚拟网络网关使 VNet 连接到内部网络中的 VPN 设备。 配置 VPN 虚拟网络网关通过 VPN 设备仅接受来自内部网络的请求。 有关详细信息，请参阅[连接到 Microsoft Azure 虚拟网络的内部网络][connect-to-an-Azure-vnet]。

- **ExpressRoute 虚拟网络网关。** ExpressRoute 虚拟网络网关允许连接到 ExpressRoute 电路用于连接与您的内部网络 VNet。

- **网关的子网。** 虚拟网络网关保存在同一子网中。

- **VPN 连接。** 所指定的连接类型 (IPSec) 和与内部部署 VPN 设备共享的密钥来加密通信属性的连接。

- **ExpressRoute 的电路。** 第 2 层或第 3 层电路由连接提供商边缘路由器通过 Azure 该连接内部网络。 电路使用托管连接提供了硬件基础结构。

- **N 层云应用程序。** 这是 Azure 中承载的应用程序。 它可能包括多个层，具有多个通过 Azure 负载平衡器连接的子网。 每个子网中的流量可能会产生规则使用[Azure 网络安全组]定义的[azure-network-security-group](NSGs)。 有关详细信息，请参阅[Microsoft Azure 安全入门][getting-started-with-azure-security]。

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构应遵循这些建议，除非有推荐不适合特定的要求。

### <a name="vnet-and-gatewaysubnet"></a>VNet 和 GatewaySubnet

创建相同的 VNet ExpressRoute 虚拟网络网关和 VPN 虚拟网络网关。 这意味着他们应该共享名为**GatewaySubnet**的相同子网

如果 VNet 中已经包含一个名为**GatewaySubnet**的子网，请确保它具有 /27 或更大的地址空间。 如果太小，现有的子网，然后按如下所述将其删除并创建一个新的下一个项目符号中所示︰

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

如果 VNet 不包含一个名为**GatewaySubnet**的子网，然后创建一个新，如下所示︰

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>虚拟专用网和 ExpressRoute 网关

验证您的组织是否符合[ExpressRoute 系统必备要求][expressroute-prereq]连接到 Azure。

在 Azure VNet 已经 VPN 虚拟网络网关，如果删除它，如下所示。

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

按照[实施混合网络体系结构与 Azure ExpressRoute] [ implementing-expressroute] ExpressRoute 连接建立。

按照[实施混合网络体系结构与 Azure 和内部部署 VPN] [implementing-vpn]以建立 VPN 虚拟网络网关连接。

建立虚拟网络网关连接后，测试作为以下环境︰

1. 请确保您可以从您的内部网络连接到 Azure VNet。

2. 使提供程序可以停止测试 ExpressRoute 连接，请与联系。

3. 验证，您仍然可以连接从内部网络到使用 VPN 虚拟网络网关连接您 Azure VNet。

4. 联系 reestabilish ExpressRoute 连接到提供商。

## <a name="considerations"></a>注意事项

ExpressRoute 的注意事项，请参见[实施混合网络体系结构与 Azure ExpressRoute] [guidance-expressroute]指南。

有关站点到站点 VPN 注意事项的信息，请参见[实现一种混合网络体系结构 Azure 和内部部署 VPN] [guidance-vpn]指南。

一般的 Azure 安全注意事项，请参见[Microsoft 云服务和网络安全][best-practices-security]。

## <a name="solution-deployment"></a>解决方案部署

如果您有现有的内部部署基础结构已经用一种合适的网络设备配置，您可以按照下面的步骤部署参考体系结构︰

1. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. 等待该链接打开在 Azure 的门户中，然后按照下列步骤操作︰ 
  - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-hybrid-vpn-er-rg`在文本框中。
  - 从**位置**下拉框中选择的地区。
  - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
  - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
  - 单击**购买**按钮。

3. 等待部署完成。

4. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. 等待该链接可以打开在 Azure 的门户中，然后输入，然后请执行下列步骤︰ 
  - 在**资源组**部分中选择**使用现有的**和输入`ra-hybrid-vpn-er-rg`在文本框中。
  - 从**位置**下拉框中选择的地区。
  - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
  - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
  - 单击**购买**按钮。

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "使用 ExpressRoute 和 VPN 网关的高可用混合网络体系结构的体系结构"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
