### <a name="noconnection"></a>如何添加或删除前缀的任何网关连接

- **若要添加**其他地址前缀到本地网络网关，您创建，但它并没有，但是又具有网关连接，请使用下面的示例。 请务必更改您自己的值。

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **若要删除**一个地址前缀从本地网络网关没有 VPN 连接，使用下面的示例。 丢掉了不再需要的前缀。 在此示例中，我们不再需要前缀 20.0.0.0/24 （从上面的示例），因此我们会更新本地网络网关和排除该前缀。

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>如何添加或删除前缀-现有网关连接

如果您已创建网关连接，并且想要添加或移除包含在您的本地网络的网关的 IP 地址前缀，您将需要执行下列步骤的顺序。 这将导致某些 VPN 连接的停机时间。 在更新您的前缀时，将首先删除该连接、 和修改前缀，然后创建一个新连接。 在下面的示例中，一定要将值更改为您自己。

>[AZURE.IMPORTANT] 不要删除的 VPN 网关。 如果这样做，您必须回请执行这些步骤以重新创建它，并重新配置内部路由器使用新设置。
 
1. 删除该连接。

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. 修改本地网络网关的地址前缀。

    将变量设置为 LocalNetworkGateway。

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    修改前缀。

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. 创建连接。 在此示例中，我们配置 IPsec 连接类型。 当您重新创建您的连接时，为您的配置使用指定的连接类型。 对于其他的连接类型，请参阅[PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx)页。

    将变量设置为 VirtualNetworkGateway。

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    创建连接。 请注意，此示例使用您在上一步中设置的变量 $local。


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
