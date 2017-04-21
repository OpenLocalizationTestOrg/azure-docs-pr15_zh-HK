<properties
   pageTitle="配置可以共存的 Expressroute 和站点到站点 VPN 连接 |Microsoft Azure"
   description="这篇文章将引导您完成配置 ExpressRoute 和可以共存，经典的部署模型站点到站点 VPN 连接。"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-expressroute-and-site-to-site-coexisting-connections-for-the-classic-deployment-model"></a>ExpressRoute 和站点之间共存的传统部署模型连接中配置


> [AZURE.SELECTOR]
- [PowerShell 的资源管理器](expressroute-howto-coexist-resource-manager.md)
- [PowerShell 的经典](expressroute-howto-coexist-classic.md)

能够配置站点到站点 VPN 和 ExpressRoute 有很多优点。 您可以为 ExressRoute，为安全的故障切换路径配置站点到站点 VPN 或使用站点到站点 Vpn 连接到未连接到 ExpressRoute 的网站。 我们将介绍配置这两种情况，本文中的步骤。 本文对经典的部署模型。 这种配置不在门户中可用。

**关于 Azure 的部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

>[AZURE.IMPORTANT] 请按照下面的说明操作之前，必须预先配置 ExpressRoute 电路。 请确保已遵循[ExpressRoute 电路的创建](expressroute-howto-circuit-classic.md)和[配置路由](expressroute-howto-routing-classic.md)指南之前执行以下步骤。

## <a name="limits-and-limitations"></a>限制和局限性

- **不支持传输路由。** 您不能通过站点到站点 VPN 连接本地网络和通过 ExpressRoute 连接本地网络之间路由 （通过 Azure)。
- **不支持点到网站。** 不能启用点到站点 VPN 连接到相同的 VNet 连接到 ExpressRoute。 点到站点 VPN 和 ExpressRoute 不能共存的同一个 VNet。
- **不能强制隧道启用站点到站点 VPN 网关。** 您可以只"强制"所有通往 Internet 的通信流回内部网络通过 ExpressRoute。
- **不支持基本的 SKU 网关。** [ExpressRoute 网关](expressroute-about-virtual-network-gateways.md)和[VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)，则必须使用非基本 SKU 网关。
- **支持仅基于路由的 VPN 网关。** 您必须使用一个基于路由的[VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)。
- **VPN 网关配置静态路由。** 如果您的本地网络连接到 ExpressRoute 和站点到站点 VPN，您必须在您的本地网络路由到公用的 Internet 站点到站点 VPN 连接中配置的静态路由。
- **必须首先配置 ExpressRoute 网关。** 添加站点到站点 VPN 网关之前，您必须首先创建 ExpressRoute 网关。

## <a name="configuration-designs"></a>配置设计

### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>ExpressRoute 站点到站点 VPN 配置为故障转移路径

对于 ExpressRoute，可以作为备份配置站点到站点 VPN 连接。 这仅适用于虚拟的网络链接到 Azure 的专用等路径。 通过 Azure 公用和 Microsoft peerings 是没有基于 VPN 的故障切换解决方案的服务。 ExpressRoute 电路始终是主链接。 数据将只有 ExpressRoute 电路失败流动的站点到站点 VPN 路径。 

![共存](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>配置站点到站点 VPN 连接到未连接到 ExpressRoute 的网站

您可以配置您的网络，其中一些网站通过站点到站点 VPN 连接直接到 Azure，某些站点连接到 ExpressRoute。 

![共存](media/expressroute-howto-coexist-classic/scenario2.jpg)

>[AZURE.NOTE] 不能配置虚拟网络为传输路由器。

## <a name="selecting-the-steps-to-use"></a>选择要使用的步骤

有两组不同的过程，可供选择来配置连接可以共存的。 您选择的配置过程取决于您是否有一个现有的虚拟网络，您想要连接，或者您想要创建新的虚拟网络。


- 我不要有 VNet，需要创建一个。
    
    如果没有虚拟网络，此过程将引导您完成创建新的虚拟网络使用传统部署模型和创建新的 ExpressRoute 和站点到站点 VPN 连接。 若要配置，请按照文章部分，[若要创建新的虚拟网络和共存的连接](#new)。

- 我已经有一个经典的部署模型 VNet。

    在与现有的站点到站点 VPN 连接或 ExpressRoute 连接的地方，可能已拥有虚拟网络。 [已存在的 VNet coexsiting 连接中配置](#add)该文章节将指导您完成删除网关，然后再创建新的 ExpressRoute 和站点到站点 VPN 连接。 请注意，在创建新的连接时，必须完成步骤中非常特定的顺序。 不要使用说明在其他文章中来创建您的网关和连接。

    在此过程中，创建可以共存的连接将需要您删除您的网关，然后配置新的网关。 这意味着您必须删除并重新创建您的网关和连接，但您不需要将任何虚拟机或服务迁移到新的虚拟网络跨内部连接的停机时间。 您的虚拟机和服务将仍能向外传播，通过负载平衡器时您配置您的网关，如果它们配置来执行此操作。


## <a name="new"></a>若要创建新的虚拟网络和共存的连接

此过程将指导您完成创建 VNet 并创建站点对站点和 ExpressRoute 连接可以共存的。

1. 您将需要安装最新版本的 Azure PowerShell cmdlet。 有关安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。 请注意您将使用此配置的 cmdlet 可能比您可能已熟悉略有不同。 一定要使用这些指令中指定的 cmdlet。 

2. 创建虚拟网络的架构。 有关配置模式的详细信息，请参阅[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)。

    当您创建架构时，请确保使用以下值︰

    - 虚拟网络的网关子网必须是 /27 或更短的前缀 （如 /26 或 /25）。
    - 网关连接类型"专用"。

              <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
                <AddressSpace>
                  <AddressPrefix>10.17.159.192/26</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Subnet-1">
                    <AddressPrefix>10.17.159.192/27</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.17.159.224/27</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>

3. 在创建并配置您的 xml 架构文件之后, 上传文件。 这将创建虚拟网络。

    使用以下 cmdlet 上载您的文件替换为您自己的值。

        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'

4. <a name="gw"></a>创建的 ExpressRoute 网关。 一定要指定为*标准*，*高性能*，或*UltraPerformance*和*DynamicRouting*为 GatewayType GatewaySKU。

    使用以下示例中，替换为您自己的值。

        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance

5. 链接到 ExpressRoute 电路的 ExpressRoute 网关。 在完成此步骤后，被建立内部网络和 Azure，ExpressRoute，通过之间的连接。

        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET

6. <a name="vpngw"></a>接下来，创建您的站点到站点 VPN 网关。 GatewaySKU 必须是*标准*，*高性能*，或必须*DynamicRouting* *UltraPerformance*和 GatewayType。

        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance

    若要检索虚拟网络网关设置，包括网关 ID 和公用 IP，并使用`Get-AzureVirtualNetworkGateway`cmdlet。

        Get-AzureVirtualNetworkGateway

        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for the following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded

7. 创建一个本地站点的 VPN 网关实体。 此命令不会将内部部署 VPN 网关配置。 相反，它使您可以提供本地网关设置，如公用 IP 和本地地址空间，以便 Azure VPN 网关可以连接到它。

    >[AZURE.IMPORTANT] 在 netcfg 中未定义本地站点的站点到站点 vpn。 相反，您必须使用此 cmdlet 指定本地站点参数。 不能定义使用门户网站或 netcfg 文件。

    使用以下示例中，替换为您自己的值。

        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>

    > [AZURE.NOTE] 如果您的本地网络有多个路由，则可以传递它们都集中在作为一个数组。  $MyLocalNetworkAddress =@("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")  


    若要检索虚拟网络网关设置，包括网关 ID 和公用 IP，并使用`Get-AzureVirtualNetworkGateway`cmdlet。 下面的示例，请参阅。

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


8. 配置本地 VPN 设备要连接到的新网关。 使用您在步骤 6 中配置 VPN 设备时检索的信息。 VPN 设备配置有关的详细信息，请参阅[VPN 设备配置](../vpn-gateway/vpn-gateway-about-vpn-devices.md)。

9. 链接到本地网关在 Azure 上的站点到站点 VPN 网关。

    在此示例中，connectedEntityId 是本地网关 ID，您可以通过运行查找`Get-AzureLocalNetworkGateway`。 您可以通过查找 virtualNetworkGatewayId `Get-AzureVirtualNetworkGateway` cmdlet。 建立这一步之后, 通过站点到站点 VPN 连接本地网络和 Azure 之间的连接。


        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <a name="add"></a>若要配置已有的 VNet 的 coexsiting 连接

如果您有一个现有的虚拟网络，请检查网关网大小。 如果网关网是 /28 或 /29，必须首先删除虚拟网络网关和子网增大网关。 本节中的步骤显示如何做到这一点。

如果网关网是 /27 或更大的虚拟网络连接通过 ExpressRoute，您可以跳过下面的步骤并进入["第 6 步-创建的站点到站点 VPN 网关"](#vpngw)一节中。

>[AZURE.NOTE] 当您删除现有的网关时，您本地的场所将丢失与虚拟网络连接正在使用此配置。

1. 您将需要安装最新版本的 Azure 资源管理器 PowerShell cmdlet。 有关安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。 请注意您将使用此配置的 cmdlet 可能比您可能已熟悉略有不同。 一定要使用这些指令中指定的 cmdlet。 

2. 删除现有的 ExpressRoute 或站点到站点 VPN 网关。 使用以下 cmdlet，替换为您自己的值。

        Remove-AzureVNetGateway –VnetName MyAzureVNET

3. 导出虚拟网络架构。 使用以下 PowerShell cmdlet，替换为您自己的值。

        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”

4. 编辑网络配置文件架构，使网关网是 /27 或更短的前缀 （如 /26 或 /25）。 下面的示例，请参阅。 
>[AZURE.NOTE] 如果您没有足够留在虚拟网络子网增大网关的 IP 地址，需要添加多个 IP 地址空间。 有关配置模式的详细信息，请参阅[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)。

          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>

5. 如果您以前的网关站点到站点 VPN，您还必须**专用**于更改连接类型。

                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>

6. 在这种情况下，您将有没有网关 VNet。 若要创建新网关并完成您的连接，就可以继续[步骤 4-创建 ExpressRoute 网关](#gw)，发现在前面的步骤集。

## <a name="next-steps"></a>下一步行动

有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 常见问题解答](expressroute-faqs.md)
