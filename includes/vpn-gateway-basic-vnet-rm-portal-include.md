要使用 Azure 的门户，在资源管理器部署模型中创建 VNet，请按照下面的步骤。 屏幕抓图作为示例提供。 一定要替换为您自己的值。 有关如何使用虚拟网络的详细信息，请参阅[虚拟网络概述](../articles/virtual-network/virtual-networks-overview.md)。

1. 从浏览器中，导航到[Azure 的门户](http://portal.azure.com)，如果有必要，请使用 Azure 的帐户。

2. 单击**新建**。 在**搜索市场上**字段中，键入"虚拟网络"。 从返回的列表中找到**虚拟的网络**，然后单击以打开**虚拟网络**刀片式服务器。

    ![查找虚拟网络资源刀片式服务器](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "查找虚拟网络资源刀片式服务器")

3. 底部的虚拟网络刀片式服务器，从**选择部署模型**列表中，选择**资源管理器**，然后单击**创建**。


    ![资源管理器中选择](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "资源管理器中选择")

4. 在**创建虚拟网络**刀片式服务器，配置 VNet 设置。 当您填写的字段时，红色感叹号标记将成为一个绿色的复选标记时在字段中输入的字符是有效。

    ![字段验证](./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "字段验证")

5. **创建虚拟网络**刀片式服务器看起来类似于下面的示例。 可能是自动填充的值。 如果是这样，替换为您自己的值。

    ![创建虚拟网络刀片式服务器](./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "创建虚拟网络刀片式服务器")

6. **名称**︰ 输入的虚拟网络名称。

7. **地址空间**︰ 输入的地址空间。 如果您有多个地址空间添加，添加您的第一个地址空间。 创建 VNet 之后，您可以以后，添加其他地址空间。
 
8. **子网名称**︰ 添加的子网名称和子网的地址范围。 创建 VNet 之后，您可以以后，添加其他子网。

10. **订阅**︰ 验证列出的订阅是否正确。 您可以通过使用下拉列表更改订阅。

11. **资源组**︰ 选择一个现有的资源组，或通过键入新的资源组的名称新建一个。 如果您要创建一个新组，命名为资源组根据计划的配置值。 有关资源组的详细信息，请访问[Azure 资源管理器的概述](resource-group-overview.md#resource-groups)。

12. **位置**︰ 选择您的 VNet 的位置。 位置决定您将部署到此 VNet 的资源所在的位置。

13. 如果您希望能够在仪表板中，很容易地找到您的 VNet，然后单击**创建**，请选择**固定到仪表板**。
    
    ![附到仪表板](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "附到仪表板")

14. 单击**创建**，您将看到图块您将反映您的 VNet 的进度的仪表板上。 正在创建 VNet 改变拼贴。

    ![创建虚拟网络平铺](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "创建虚拟网络平铺")