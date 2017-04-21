## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>如何在 Azure 的门户网站中创建的 VNet

若要创建基于上述方案通过 Azure 预览门户网站 VNet，请按照下面的步骤。

1. 从浏览器中，定位到 http://portal.azure.com，如果有必要，请使用 Azure 的帐户。
2. 单击**新建** > **网络** > **虚拟网络**中，然后从**选择部署模型**列表中，单击**资源管理器**，然后单击**创建**，如下图所示。

    ![在 Azure 门户创建 VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. 在**创建虚拟网络**刀片式服务器，配置 VNet 设置，如下图中所示。

    ![创建虚拟网络刀片式服务器](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. 单击**资源组**并选择要添加到 VNet 的资源组或单击**新建**将 VNet 添加到新的资源组。 下图显示一个名为**TestRG**的新资源组的组设置的资源。 有关资源组的详细信息，请访问[Azure 资源管理器的概述](../articles/resource-group-overview.md#resource-groups)。

    ![资源组](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. 如果有必要，请更改您的 VNet 的**订阅**和**位置**设置。 

6. 如果您不想看到 VNet **Startboard**在平铺视图，则禁用**附到 Startboard**。 

7. 单击**创建**并注意下图中所示，名为**创建虚拟网络**的拼贴。

    ![创建虚拟网络图块](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. 等待要创建 VNet，然后在**虚拟网络**刀片式服务器，请单击**所有设置** > **的子网** > **添加**如下所示。

    ![在 Azure 的门户网站中添加子网](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. 如下所示，指定*后端*的子网，子网设置，然后单击**确定**。 

    ![子网设置](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. 注意到子网的列表，如下图中所示。

    ![在 VNet 中的子网列表](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
