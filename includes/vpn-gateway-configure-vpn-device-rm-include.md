
若要配置 VPN 设备，您需要配置内部部署 VPN 设备的公用 IP 地址的虚拟网络网关。 使用特定的配置信息与设备制造商联系，并配置您的设备。 请参阅[VPN 设备](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md)很好地使用 Azure 的 VPN 设备的详细信息。

若要查找虚拟网络网关使用 PowerShell 的公用 IP 地址，请使用下面的示例︰

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

您还可以使用 Azure 门户虚拟网络网关查看公用 IP 地址。 导航到**虚拟网络网关**，然后单击您的网关的名称。