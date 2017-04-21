若要修改的网关 IP 地址，请使用`New-AzureRmVirtualNetworkGatewayConnection`cmdlet。 只要您保留本地网络网关的名称与现有名称完全相同，则设置将覆盖。 到目前为止，"设置"cmdlet 不支持修改的网关 IP 地址。

### <a name="gwipnoconnection"></a>如何修改的网关 IP 地址的任何网关连接

若要更新还没有连接到本地网络网关的网关 IP 地址，请使用下面的示例。 您还可以在同一时间更新地址前缀。 您指定的设置将覆盖现有的设置。 请务必使用您的本地网络的网关的现有名称。 如果不这样做，您将创建一个新的本地网络网关，不覆盖现有联系人。

使用以下示例中，替换为您自己的值。

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>如何修改现有网关连接的网关 IP 地址-

如果网关连接已存在，您首先需要删除该连接。 然后，您可以修改的网关 IP 地址并重新创建一个新的连接。 这将导致某些 VPN 连接的停机时间。


>[AZURE.IMPORTANT] 不要删除的 VPN 网关。 如果这样做，您必须回请执行这些步骤以重新创建它，并重新配置内部路由器将分配给新创建网关的 IP 地址。
 

1. 删除该连接。 您可以通过查找您的连接的名称`Get-AzureRmVirtualNetworkGatewayConnection`cmdlet。

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. 修改的 GatewayIpAddress 值。 此外可以修改地址前缀时，如有必要。 请注意，这将覆盖现有的局域网网关设置。 当修改这些设置将覆盖使用现有局域网网关的名称。 如果不这样做，您将创建一个新的本地网络网关，不修改现有联系人。

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. 创建连接。 在此示例中，我们配置 IPsec 连接类型。 当您重新创建您的连接时，为您的配置使用指定的连接类型。 对于其他的连接类型，请参阅[PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx)页。  若要获取 VirtualNetworkGateway 名，可以运行`Get-AzureRmVirtualNetworkGateway`cmdlet。

    设置变量︰

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    创建此连接︰
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

