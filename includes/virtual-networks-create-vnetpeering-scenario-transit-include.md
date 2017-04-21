## <a name="service-chaining---transit-through-peered-vnet"></a>服务链接-通过对 VNet 的途

虽然系统路由的使用方便了通信会自动为您的部署，有希望用来控制数据包路由通过虚拟应用装置的情况。
在这种情况下，有一些在订阅中，HubVNet 和 VNet1，如下面的关系图中所述两个 VNets。 部署在 VNet HubVNet 的网络虚拟 Appliance(NVA)。 建立后 VNet HubVNet 和 VNet1 之间的对等，可以设置用户定义工艺路线，并在 HubVNet 中指定 NVA 下一跃点。

![NVA 途](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] 为简单起见，假设这里所有的 VNets 是在相同的订阅。 但是它也适用于跨订阅方案。

要启用传输路由键属性是"允许转发通讯"参数。 这使得接受和发送通信量与 NVA peered VNet。  
