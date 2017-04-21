您必须创建 VNet 和关网第一次之前从事下列任务。 请参阅详细信息的文章[来配置虚拟网络使用经典的门户](../articles/expressroute/expressroute-howto-vnet-portal-classic.md)。   

## <a name="add-a-gateway"></a>添加网关

使用以下命令来创建一个网关。 一定要替换为您自己的任何值。

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>验证已创建网关

使用以下命令验证已创建网关。 此命令还会检索网关 ID，您需要的其他操作。

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>调整了网关

有多个[网关 Sku](../articles/expressroute/expressroute-about-virtual-network-gateways.md)。 可以使用下面的命令在任何时候更改网关 SKU。

>[AZURE.IMPORTANT] 此命令不起作用的 UltraPerformance 网关。 若要更改到 UltraPerformance 网关到网关，首先删除现有的 ExpressRoute 网关，然后创建一个新的 UltraPerformance 网关。 若要降级从 UltraPerformance 网关到网关，首先删除 UltraPerformance 网关，，然后创建一个新的网关。 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>删除一个网关

使用以下命令来删除一个网关

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>