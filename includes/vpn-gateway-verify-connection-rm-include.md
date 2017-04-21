### <a name="to-verify-your-connection-by-using-powershell"></a>若要验证您的连接，使用 PowerShell

您可以验证您的连接成功通过`Get-AzureRmVirtualNetworkGatewayConnection`cmdlet，带有或不带`-Debug`。 

1. 使用以下 cmdlet 示例中，配置值以满足自己。 如果出现提示，运行 'All' 选择 'A'。 在示例中，`-Name`指的连接的创建并且想要测试的名称。

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. 该 cmdlet 完成后，查看值。 在下面的示例中，连接状态将显示为已连接，您可以查看入站和出站字节。

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>若要验证您的连接使用 Azure 门户

在 Azure 的门户中，可以查看连接状态，通过导航到该连接。 有多种方法来执行此操作。 下列步骤显示的导航到您的连接和验证的一种方法。

1. 在[Azure 的门户](http://portal.azure.com)中，单击**所有资源**，并导航到虚拟网络网关。
2. 在虚拟的网络网关刀片式服务器，单击**连接**。 您可以查看每个连接的状态。
3. 单击您想要验证打开**精要**的连接的名称。 在一些基础知识，您可以查看有关连接的详细信息。 **状态**是成功和已连接时所做的成功连接。

    ![验证连接](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)