## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>如何创建使用 PowerShell 从网络配置文件 VNet

Azure 使用 xml 文件来定义所有可用的 VNets 与订阅。 可以下载此文件，并对其进行修改或删除现有 VNets，编辑和创建新记录。 在本文中，将了解如何下载此文件，称为网络配置 （或 netcgf） 文件中，并编辑它以创建新的 VNet。 检查的[Azure 的虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)若要了解有关网络配置文件的详细信息。

若要创建使用 netcfg 文件使用 PowerShell VNet，请按照下面的步骤。

1. 如果您以前从未使用 Azure PowerShell，请参阅[如何安装和配置 Azure PowerShell](../articles/powershell-install-configure.md)并按照说明操作，一直到结尾到 Azure 签名并选择您的订购。
2. 从 Azure PowerShell 控制台，使用**Get AzureVnetConfig** cmdlet 通过运行以下命令下载网络配置文件。 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    预期的输出︰

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. 打开您在上面使用任何 XML 或文本编辑器应用程序，步骤 2 中保存该文件并查找**<VirtualNetworkSites>**元素。 如果您已创建的所有网络，每个网络都将显示为自己**<VirtualNetworkSite>**元素。
4. 若要创建此方案中描述的虚拟网络，添加以下 XML 略低于**<VirtualNetworkSites>**元素︰

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

9.  保存此网络配置文件。
10. 从 Azure PowerShell 控制台中，使用**一组 AzureVnetConfig** cmdlet 通过运行以下命令上载网络配置文件。 注意在该命令的输出，您应该看到在**OperationStatus**下的**成功**。 如果它是不是这样，请检查 xml 文件中的错误。

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    以下是上述命令的预期的输出︰

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. 从 Azure PowerShell 控制台，使用**Get AzureVnetSite** cmdlet 来验证，通过运行以下命令来添加新的网络。 

        Get-AzureVNetSite -VNetName TestVNet

    以下是上述命令的预期的输出︰

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded