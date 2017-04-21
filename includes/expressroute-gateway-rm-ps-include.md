此任务的步骤使用基于下面的值 VNet。 此列表中也概述了其他设置和名称。 我们并不使用此列表中的任何步骤，直接尽管我们不要添加基于此列表中的值的变量。 您可以复制列表中要用作参照替换为您自己的值。

配置引用列表︰
    
- 虚拟网络名称 ="TestVNet"
- 虚拟网络地址空间 = 192.168.0.0/16
- 资源组 ="TestRG"
- Subnet1 名 ="前端" 
- Subnet1 地址空间 ="192.168.0.0/16"
- 网关的子网名称:"GatewaySubnet"，则必须始终命名一个网关的子网*GatewaySubnet*。
- 网关的子网地址空间 ="192.168.200.0/26"
- 区域 ="东亚美国"
- 网关名称 ="网关"
- 网关 IP 名称 ="GWIP"
- 网关 IP 配置名称 ="gwipconf"
-  类型 ="ExpressRoute"这种类型是 ExpressRoute 配置需要。
- 网关公用 IP 名称 ="gwpip"


## <a name="add-a-gateway"></a>添加网关

1. 连接到 Azure 订购。 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. 声明变量对于本练习。 本示例将使用变量中使用下面的示例。 请务必编辑此文件以反映您要使用的设置。 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. 将虚拟网络对象存储为一个变量。

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. 将网关子网添加到虚拟网络。 网关网必须被命名为"GatewaySubnet"。 您需要创建一个网关，即 /27 或更大 (/ 26 日/25，等等。)。
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. 设置配置。

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. 作为变量存储网关网。

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. 请求的公用 IP 地址。 创建网关之前请求的 IP 地址。 您不能指定的 IP 地址，您想要使用;它是动态分配的。 在下一步的配置部分中，您将使用此 IP 地址。 AllocationMethod 必须是动态的。

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. 创建网关配置。 网关配置定义的子网和要使用的公用 IP 地址。 在此步骤中，指定将创建网关时使用的配置。 此步骤不会实际创建网关对象。 使用下面的示例创建网关配置。 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. 创建网关。 在此步骤中， **-GatewayType**是尤其重要的。 您必须使用**ExpressRoute**的值。 请注意，在运行这些 cmdlet 之后，网关可以花 20 分钟或更多来创建。

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>验证已创建网关

使用以下命令验证已创建网关。

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>调整了网关

有多个[网关 Sku](../articles/expressroute/expressroute-about-virtual-network-gateways.md)。 可以使用下面的命令在任何时候更改网关 SKU。

>[AZURE.IMPORTANT] 此命令不起作用的 UltraPerformance 网关。 若要更改到 UltraPerformance 网关到网关，首先删除现有的 ExpressRoute 网关，然后创建一个新的 UltraPerformance 网关。 若要降级从 UltraPerformance 网关到网关，首先删除 UltraPerformance 网关，，然后创建一个新的网关。

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>删除一个网关

使用以下命令来删除一个网关

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
