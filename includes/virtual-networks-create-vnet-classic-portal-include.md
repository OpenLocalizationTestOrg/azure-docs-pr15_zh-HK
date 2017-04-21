## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>如何在 Azure 的门户网站中创建的 VNet

若要创建基于上述方案 VNet，请按照下面的步骤。

1. 从浏览器中，定位到 http://manage.windowsazure.com，如果有必要，请使用 Azure 的帐户。
2. 单击**新建** > **网络服务** > **虚拟网络** > **自定义创建**下图所示。

    ![在门户中创建 VNet](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. 在**虚拟的网络详细信息**页上，键入 VNet 的**名称**，选择它的**位置**，要前进到第 2 步的页的右下角上的箭头然后单击。 下图显示了我们的方案的设置。

    ![虚拟网络详细信息页](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. 在**DNS 服务器和 VPN 连接**页上指定的名称和最多 9 DNS 服务器使用的 IP 地址。 如果您没有指定 DNS 服务器，您的 VNet 将使用 Azure 所提供的内部名称解析解决方案。 对于我们的情况，我们将不配置 DNS 服务器。
5. 如果您想要为您的 VNet 提供点到站点的 VPN 访问，启用**点到站点 VPN 配置**复选框。 如果您不配置点到站点 VPN，您可以将其添加到您的 VNet 之后创建的任何时候。 对于我们的情况，我们将不配置点到站点 VPN。
6. 如果您想要提供您的 VNet 和另一个 VNet 或内部网络之间的站点到站点 VPN 连接，启用**站点到站点 VPN 配置**复选框，并指定您是否希望使用**ExpressRoute**或注释，以及网络的名称来连接到。 如果您不配置站点到站点 VPN，您可以将其添加到您的 VNet 之后创建的任何时候。 对于我们的情况，我们将不配置站点到站点 VPN。
7. 请单击要转到步骤 3.下图显示了我们的方案的设置的页面的右下角上的箭头。

    ![DNS 服务器和 VPN 连接页](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. 在**虚拟的网络地址空间**页上，在**启动 IP**下, *10.0.0.0*更改 VNet 地址空间，请单击，然后键入要使用的起始地址空间。 我们的方案中，请键入*192.168.0.0*。 
9. 在**CIDR （地址计数）**下选择子网掩码的比特数。 对于我们的情况，选择*16 (65536)*。
10. 下面**的子网**，请单击*子网 1*和必要时重命名子网。 我们的方案，对其重命名为*前端*。

    >[AZURE.NOTE] 如果您单击名称文本框中为子网之外您将不能编辑该名称，如果再次的子网。 若要解决此问题，您需要在其右边的 X 按钮单击可删除子网，然后在下面的步骤 13 所述，添加新的子网。

11. **启动 IP**的第一个子网，在指定起始 IP 地址的子网。 我们的方案中，请键入*192.168.1.0*。
12. 在**CIDR （地址计数）**下，选择第一个子网的子网掩码的比特数。 我们的方案中，请选择*24 (256)*。
13. 如有必要，请单击**添加子网**中添加新的子网。 对于我们的情况，添加子网，请重复步骤 10 至 12，VNet 在下图中所示的配置。

    ![虚拟网络地址空间页](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. 单击要创建 VNet 的页面的右下角上的复选标记按钮。 几秒钟后您 VNet 将显示在列表中可用的 VNets，如下图中所示。

    ![新的虚拟网络](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)