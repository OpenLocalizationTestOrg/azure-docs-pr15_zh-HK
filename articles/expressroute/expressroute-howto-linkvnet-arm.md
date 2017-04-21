<properties 
   pageTitle="链接到 ExpressRoute 电路虚拟网络，通过使用 PowerShell |Microsoft Azure"
   description="本文概述如何链接虚拟网络 (VNets) 到 ExpressRoute 电路使用资源管理器部署模型和 PowerShell。"
   services="expressroute"
   documentationCenter="na"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>将虚拟网络链接到 ExpressRoute 电路

> [AZURE.SELECTOR]
- [Azure 门户的资源管理器](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell 的资源管理器](expressroute-howto-linkvnet-arm.md)
- [PowerShell 的经典](expressroute-howto-linkvnet-classic.md)


这篇文章将帮助您使用资源管理器部署模型和 PowerShell 链接到 Azure ExpressRoute 电路虚拟网络 (VNets)。 在相同的订阅向导或属于另一个订阅可以是虚拟网络。

**关于 Azure 的部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>配置系统必备组件

- 您需要最新版本的 Azure PowerShell 模块 (至少 1.0 版)。 有关安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。
- 您需要在开始配置之前检查[系统必备组件](expressroute-prerequisites.md)、[路由的要求](expressroute-routing.md)和[工作流](expressroute-workflows.md)。
- 您必须具有活动的 ExpressRoute 电路。 
    - 按照说明[创建 ExpressRoute 电路](expressroute-howto-circuit-arm.md)，具有电路连接提供程序启用。 
    - 确保您有 Azure 专用对等配置为您的电路。 请参阅[配置路由](expressroute-howto-routing-arm.md)路由的说明的文章。 
    - 确保 Azure 专用对等配置，以便您可以启用的端到端连接您的网络和 Microsoft 之间的 BGP 对等是向上。
    - 确保您有一个虚拟网络和虚拟网络网关创建且完全配置。 按照说明进行操作以创建[VPN 网关](../articles/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)，但一定要使用`-GatewayType ExpressRoute`。

可以将最多 10 个虚拟网络链接到标准 ExpressRoute 电路。 使用标准的 ExpressRoute 电路时，所有的虚拟网络必须是在同一的地缘政治区域。 

您可以链接 ExpressRoute 电路中，地缘政治区域外部虚拟网络或 ExpressRoute 电路连接更多的虚拟网络，如果启用 ExpressRoute 高级加载项。 检查高级加载项的详细的[常见问题解答](expressroute-faqs.md)。

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>在相同的订阅将虚拟网络连接到电路

您可以通过使用以下 cmdlet 连接 ExpressRoute 电路的虚拟网络网关。 请确保虚拟网络网关创建并准备好运行该 cmdlet 之前链接︰

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>在不同的订阅将虚拟网络连接到电路

您可以跨多个订阅共享 ExpressRoute 电路。 下图显示一个简单的跨多个订阅共享如何起作用，对 ExpressRoute 电路的示意图。

每个较小的群，大群中用于表示属于组织内部的不同部门的订阅。 每个组织内的部门可用于部署其服务--但它们自己订阅可以共享单个 ExpressRoute 电路连接回您的内部网络。 一个部门 (在此示例中︰ IT) 可以拥有 ExpressRoute 电路。 在组织内的其他订阅可以使用 ExpressRoute 电路。

>[AZURE.NOTE] 专用的电路连接和带宽费用将应用于 ExpressRoute 电路所有者。 所有的虚拟网络共享相同的带宽。

![跨订阅连接](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>管理

*电路的所有者*是 ExpressRoute 电路资源的授权的权限高的用户。 电路所有者可以创建可以兑换*电路*用户的授权。 *电路的用户*都是虚拟的网络网关 （不是内作为 ExpressRoute 电路相同的订阅） 的所有者。 *电路的用户*可以兑换授权 （一个授权每个虚拟网络）。

*电路的所有者*有权修改并随时撤销授权。 撤销授权将导致所有链路连接被删除从其访问权限被吊销该订阅。

### <a name="circuit-owner-operations"></a>电路的所有者操作 

#### <a name="creating-an-authorization"></a>创建授权
    
该电路所有者创建一种授权。 这会导致电路用户可用于连接到 ExpressRoute 电路及其虚拟网络网关使用身份验证密钥的创建。 一种授权，是只能有一个连接有效。

下面的 cmdlet 代码段演示如何创建一种授权︰

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

        $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
        

对此响应将包含身份验证密钥和状态︰

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded

        

#### <a name="reviewing-authorizations"></a>检查授权

电路所有者可以查看特定电路通过运行以下 cmdlet 发布的所有授权︰

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
    

#### <a name="adding-authorizations"></a>添加授权

电路的所有者可以通过使用以下 cmdlet 添加授权︰

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    
    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit

    
#### <a name="deleting-authorizations"></a>删除授权

电路所有者可以撤销或删除授权的用户通过运行以下 cmdlet:

    Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit    

### <a name="circuit-user-operations"></a>电路的用户操作

电路用户需要等 ID 和电路所有者授权码。 授权码是一个 GUID。

对等机 ID 时，可以从以下命令检查。

    Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

#### <a name="redeeming-connection-authorizations"></a>获取连接的授权

电路的用户可以运行以下 cmdlet 兑换链接授权︰

    $id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"  
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

#### <a name="releasing-connection-authorizations"></a>释放连接的授权

您可以通过删除链接到虚拟网络的 ExpressRoute 电路的连接释放一种授权。

## <a name="next-steps"></a>下一步行动

有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 的常见问题解答](expressroute-faqs.md)。
