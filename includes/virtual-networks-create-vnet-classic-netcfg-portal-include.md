## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>如何创建使用 Azure 门户网络配置文件 VNet

Azure 使用 xml 文件来定义所有可用的 VNets 与订阅。 可以下载此文件，并对其进行修改或删除现有 VNets，编辑和创建新记录。 在本文中，将了解如何下载此文件，称为网络配置 （或 netcgf） 文件中，并编辑它以创建新的 VNet。 检查的[Azure 的虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)若要了解有关网络配置文件的详细信息。

若要创建使用 netcfg 文件通过 Azure 的门户网站 VNet，请按照下面的步骤。

1. 从浏览器中，定位到 http://manage.windowsazure.com，如果有必要，请使用 Azure 的帐户。
2. 向下滚动列表的服务，然后单击在**网络**上，如下所示。

    ![Azure 的虚拟网络](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. 在页面底部，单击**导出**按钮，如下所示。

    ![导出按钮](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. 在**导出网络配置**页上，选择您要导出的虚拟网络配置，的订阅，然后单击页面的左下角上的复选标记按钮。
5. 按照您的浏览器说明保存的**NetworkConfig.xml**文件。 请确保您注意要保存该文件。
6. 打开您在上面使用任何 XML 或文本编辑器应用程序，步骤 5 中保存的文件并查找**<VirtualNetworkSites>**元素。 如果您已创建的所有网络，每个网络都将显示为自己**<VirtualNetworkSite>**元素。
7. 若要创建此方案中描述的虚拟网络，添加以下 XML 略低于**<VirtualNetworkSites>**元素︰

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  保存此网络配置文件。
9.  在 Azure 的门户，在该页面的左下角单击**新建**，然后单击**网络服务**单击**虚拟网络**，然后在下图中所示，然后单击**导入配置**。

    ![导入配置](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  在**导入网络配置文件**页上，单击**浏览的文件...**，导航到您在第 8 步中保存您的文件的文件夹，选择该文件，然后再单击**打开**。 该网页应类似于下图。 在右下角的页上，单击箭头按钮将移到下一步。

    ![导入网络配置文件页](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   在**构建您的网络**上，请注意该项为您新的 VNet，如下图中所示。

    ![构建网络页面](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   单击要创建 VNet 的页面的右下角上的复选标记按钮。 几秒钟后您 VNet 将显示在列表中可用的 VNets，如下图中所示。

    ![新的虚拟网络](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)