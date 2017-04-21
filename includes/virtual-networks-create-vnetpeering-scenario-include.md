## <a name="scenario"></a>方案

为了更好地说明如何创建 VNet 和子网，该文档将使用下面的场景。

![VNet 方案](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

在此方案中，您将创建名为**TestVNet**与**192.168.0.0./16**保留 CIDR 块 VNet。 您 VNet 将包含下面的子网︰ 

- **前端**，使用**192.168.1.0/24**作为其 CIDR 块。
- **后端**，使用**192.168.2.0/24**作为其 CIDR 块。

 