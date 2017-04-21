## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>如何在 Azure 的门户网站中创建经典 VNet

若要创建基于上述方案经典 VNet，请按照下面的步骤。

1. 从浏览器中，定位到 http://portal.azure.com，如果有必要，请使用 Azure 的帐户。
2. 单击**新建** > **网络** > **虚拟网络**，请注意**选择部署模型**列表已显示了**经典**，然后单击**创建**，如下图所示。

    ![在 Azure 门户创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. **虚拟网络**刀片上, 键入 VNet，该**名称**，然后单击**地址空间**。 配置地址空间设置为 VNet，其第一个子网，然后单击**确定**。 下图显示了我们的方案的 CIDR 块的设置。

    ![地址空间刀片式服务器](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. 单击**资源组**并选择要添加到 VNet 的资源组或单击**创建新的资源组**将 VNet 添加到新的资源组。 下图显示一个名为**TestRG**的新资源组的组设置的资源。 有关资源组的详细信息，请访问[Azure 资源管理器的概述](../articles/virtual-network/resource-group-overview.md#resource-groups)。

    ![创建资源组刀片](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. 如果有必要，请更改您的 VNet 的**订阅**和**位置**设置。 

6. 如果您不想看到 VNet **Startboard**在平铺视图，则禁用**附到 Startboard**。 

7. 单击**创建**并注意下图中所示，名为**创建虚拟网络**的拼贴。

    ![在门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. 等待要创建 VNet，当您看到下面的图块，请单击它以添加更多子网。

    ![在门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. 您应该看到为您 VNet**配置**，如下所示。 

    ![在门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. 单击**子网** > **添加**，然后键入一个**名称**并指定您的子网的**地址范围 （CIDR 块）** ，然后单击**确定**。 下图显示的设置对于我们当前的情况。

    ![在 Azure 门户创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)