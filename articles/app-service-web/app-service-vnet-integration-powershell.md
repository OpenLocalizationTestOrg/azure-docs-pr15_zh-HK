<properties
    pageTitle="通过使用 PowerShell 将您的应用程序连接到虚拟网络"
    description="有关如何连接到和使用虚拟网络使用 PowerShell 的说明"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>通过使用 PowerShell 将您的应用程序连接到虚拟网络 #

## <a name="overview"></a>概述 ##

在 Azure 应用程序服务，可以您的应用程序 （包括 web、 移动设备、 或 API） 连接到 Azure 的虚拟网络 (VNet) 在您的订阅。 此功能称为 VNet 集成。 不要将 VNet 集成功能与应用程序服务环境功能，使您可以在虚拟网络中运行 Azure 应用程序服务的实例相混淆。

VNet 集成功能方面新的门户，您可以使用与部署使用传统部署模型或 Azure 资源管理器部署模型的虚拟网络集成的用户界面 (UI)。 如果您想要了解有关该功能的详细信息，请参阅[集成您的应用程序使用 Azure 的虚拟网络](web-sites-integrate-with-vnet.md)。

本文是关于如何使用 UI 不但是而是有关如何通过使用 PowerShell 启用集成。 因为每种部署模型的命令不同，这篇文章将包含有关每个部署模型部分。  

在继续本文之前，请确保您具有︰

- 最新的 Azure PowerShell SDK 安装。 您可以使用 Web 平台安装程序安装这个软件。
- 在 Azure 应用程序服务标准或高级 SKU 中运行的应用程序。

## <a name="classic-virtual-networks"></a>经典的虚拟网络 ##

此部分说明了使用传统部署模型的虚拟网络的三个任务︰

1. 将您的应用程序连接到预先存在的虚拟网络，具有网关配置为点到站点的连接。
1. 更新虚拟网络集成信息为您的应用程序。
1. 从虚拟网络上断开您的应用程序。

### <a name="connect-an-app-to-a-classic-vnet"></a>将应用程序连接到经典 VNet ###

若要将应用程序连接到虚拟网络，请按照以下三个步骤︰

1. 为使 web 应用程序声明它将被加入特定的虚拟网络。 应用程序将生成的证书，将给来点到站点的连接的虚拟网络。
1. 上载到虚拟的网络中，web 应用程序的证书，然后检索点到站点 VPN 包 URI。
1. 与点到网站包 URI 更新 web 应用程序的虚拟网络连接。

第一和第三个步骤可完全实现脚本化，但第二步需要通过门户网站或对虚拟的网络端点来 Azure 资源管理器执行**放置**或**修补**操作的访问权限的一次性的手动操作。 与 Azure 支持启用此联系。 在开始之前，请确保您拥有经典的虚拟网络具有点到站点的连接已启用并且已部署网关。 要创建网关并启用点到站点的连接，您需要[创建的 VPN 网关]在所述使用门户网站[createvpngateway]。

经典的虚拟网络必须与包含应用程序，并集成到您的应用程序服务计划相同的订阅中。

##### <a name="set-up-azure-powershell-sdk"></a>设置 Azure PowerShell SDK #####

打开一个窗口，PowerShell 和设置使用 Azure 帐户和订阅︰

    Login-AzureRmAccount

该命令会打开获得 Azure 凭据的提示。 在您登录后，使用以下命令之一来选择您想要使用的订阅。 请确保您使用的虚拟网络和应用程序服务计划中的订阅。

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

或

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>本文中使用的变量 #####

若要简化命令，我们将设置**$Configuration** PowerShell 变量与特定的配置。

设置一个变量，如下所示继续使用以下参数︰

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

应用程序位置应该是不带空格的位置。 例如，美国西部是 westus。

    $Configuration.WebAppLocation = "[Your web app Location]"

下一项是应写入证书的位置。 它应该是您的本地计算机上的可写路径。 请务必包括.cer 的结尾。

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

若要查看您的设置，键入**$Configuration**。

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

本节的其余部分假定您已创建为刚才描述的变量。

##### <a name="declare-the-virtual-network-to-the-app"></a>声明对该应用程序的虚拟网络 #####

使用下面的命令来告诉应用程序，它将使用此特定的虚拟网络。 这将导致应用程序生成所需的证书︰

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

如果此命令成功执行， **$vnet**中它应该有一个**属性**变量。 **属性**变量应包含证书指纹和证书数据。

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>将 web 应用程序证书传到虚拟网络 #####

手动，则需要为每个订阅和虚拟网络组合一次性步骤。 即，如果要在订阅的应用程序连接到虚拟网络，您需要仅在配置无论多少应用程序后再执行此步骤。 如果要到另一个虚拟的网络中添加新的应用程序，您将需要再次执行此操作。 这样做的原因是，在 Azure 应用程序服务的订阅级别生成证书一套，一组应用程序将连接到每个虚拟网络的生成一次。

证书将已设置如果您遵循这些步骤，或者如果您通过门户网站集成在同一个虚拟网络。

第一步是生成的.cer 文件。 第二步是将.cer 文件上载到您的虚拟网络。 从 API 调用在前面的步骤中生成的.cer 文件，请运行下面的命令。

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

证书中找到位置的**$Configuration.GeneratedCertificatePath**指定。

若要手动上载证书，使用[Azure 的门户网站][azureportal]和**浏览虚拟网络 （经典）** > **VPN 连接** > **点到网站** > **管理证书**。 从这里上, 传您的证书。

##### <a name="get-the-point-to-site-package"></a>获取对站点点包 #####

设置 web 应用程序上的虚拟网络连接的第二步是获得对站点点包并将其提供给您的 web 应用程序。

将以下模板保存到您的计算机，例如，C:\Azure\Templates\GetNetworkPackageUri.json 上某一位置称为 GetNetworkPackageUri.json 的文件。

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


设置输入的参数︰

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

调用脚本︰

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


**$Output 变量。Outputs.packageUri**现在将包含包将提供给您的 web 应用程序的 URI。

##### <a name="upload-the-point-to-site-package-to-your-app"></a>点到网站包上传到您的应用程序 #####

最后一步是为应用程序提供此包。 只需运行下一个命令︰

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

如果一条消息，要求您确认覆盖现有的资源，请确保允许它。

此命令成功执行后，现在应为虚拟网络连接您的应用程序。 若要确认成功，转到您的应用程序的控制台，并键入以下命令︰

    SET WEBSITE_

如果没有调用 WEBSITE_VNETNAME 具有值相匹配的目标虚拟网络名称的环境变量，具有成功的所有配置。

### <a name="update-classic-vnet-integration-information"></a>更新传统的 VNet 集成信息 ###

若要更新或重新同步您的信息，只需重复最初创建集成时所遵循的步骤。 这些步骤包括︰

1. 定义您的配置信息。
1. 声明对该应用程序的虚拟网络。
1. 获取对站点点包。
1. 将点到网站包上传到您的应用程序。

### <a name="disconnect-your-app-from-a-classic-vnet"></a>从经典的 VNet 断开您的应用程序 ###

要断开连接的应用程序，您需要在虚拟的网络集成过程中设置的配置信息。 使用该信息，没有再一个命令以从虚拟网络上断开您的应用程序。

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>资源管理器虚拟网络 ##

资源管理器虚拟网络具有 Azure 的资源管理器 Api，从而简化一些流程与传统虚拟网络相比。 我们有一个脚本，它将帮助您完成以下任务︰

- 创建资源管理器的虚拟网络，并与之集成您的应用程序。
- 创建网关在既有的资源管理器虚拟网络中，配置点到站点的连接，然后将您的应用程序集成与它。
- 与现已存在的资源管理器虚拟网络具有网关和点到站点的连接启用集成您的应用程序。
- 从虚拟网络上断开您的应用程序。

### <a name="resource-manager-vnet-app-service-integration-script"></a>资源管理器 VNet 的应用程序服务集成脚本 ###

复制下面的脚本并将其保存到一个文件中。 如果您不想使用此脚本，随意从它以查看如何使用资源管理器的虚拟网络设置的事情中学习。


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

保存脚本的副本。 在本文中，它被称为 V2VnetAllinOne.ps1，但您可以使用另一个名称。 没有此脚本的参数。 您只需运行它。 该脚本将执行的第一件事是提示您登录。 在您登录之后，该脚本将获取有关您的帐户的详细信息并返回的订阅列表。 不计算您的凭据的请求，执行最初的脚本如下所示︰

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) 演示的订阅 (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS 测试 (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) 紫色的演示订阅 (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    选择一个选项︰ 3

    帐户︰ccompy@microsoft.com环境︰ AzureCloud 订阅︰ 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d 租户︰ 722278f-fef1-499f-91ab-2323d011db47

    请输入您的应用程序的资源组︰ hcdemo rg 请输入您的应用程序的名称︰ v2vnetpowershell 要做什么？

    1) 向应用程序添加新的虚拟网络
    2) 向应用程序添加现有虚拟网络
    3) 从应用程序移除虚拟网络

本节的其余部分介绍这三个选项的每个。

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>创建资源管理器 VNet 和它相结合 ###

若要创建新的虚拟网络，使用资源管理器部署模型，并将其集成与您的应用程序，请选择**1) 为应用程序添加新的虚拟网络**。 这将提示您输入的虚拟网络名称。 在我种情况下，您可以看到在下面的设置中，我将使用该名称，v2pshell。

该脚本提供有关正在创建的虚拟网络的详细信息。 如果我要我可以更改任何值。 此示例执行，创建虚拟网络具有以下设置︰

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

如果您想要更改任何值，请键入**Y**并进行更改。 在您满意与虚拟的网络设置，键入**N**或只需按下 enter 键，当系统提示您更改的设置。 据此在完成之前，脚本将告诉您它的一些 ' i 做直到它开始创建虚拟网络网关。 该步骤会占用一个小时。 在此阶段，没有进度指示器，但该脚本将告诉您已创建网关时。

当脚本完成后时，它会显示**已完成**。 在这种情况下，您必须具有名称和所选的设置资源管理器虚拟网络。 这个新的虚拟网络还将与您的应用程序集成。

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>与现已存在的资源管理器 VNet 集成在您的应用程序 ###

当在集成与预先存在的虚拟网络，如果没有网关或点到站点的连接资源管理器虚拟网络提供时，该脚本将设置的。 如果 VNET 已设置了这些事情，该脚本将直接转到应用程序集成。 若要启动这个过程，只需选择**2) 向应用程序添加一个现有的虚拟网络**。

此选项适用您只有在既有资源管理器虚拟网络中为您的应用程序相同的订阅。 选择该选项后，您将看到您的资源管理器虚拟网络的列表。   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    选择一个选项︰ 5

只需选择您想要与集成的虚拟网络。 如果您已启用点到站点连接的网关，该脚本将只与集成，您的应用程序虚拟网络。 如果没有网关，您需要指定网关网。 网关网必须是在虚拟网络地址空间中，并且它不能在任何其他子网中。 如果您没有一个网关的虚拟网络并运行此步骤，操作如下所示︰

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

在此示例中，创建一个虚拟网络网关具有以下设置︰

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

如果您想要更改这些设置，您可以这样做。 否则，请按 enter 键，该脚本将创建您的网关并将您的应用程序连接到您的虚拟网络。 网关创建时间仍然是一个小时的时间，但是，所以请确保您牢记于心的。 一切完成后，该脚本将为**已完成**。

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>从资源管理器 VNet 断开您的应用程序 ###

断开您的应用程序与虚拟网络连接不使用网关下或禁用点到站点的连接。 您可能，毕竟，使用它的其他内容。 它也不会断开它与而不是您提供的任何其他应用程序。 若要执行此操作，请选择**3) 从应用程序移除虚拟网络**。 执行此操作时，您会看到类似下面这样︰

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

尽管此脚本说明删除，它不会删除虚拟网络。 它只删除集成。 这是您要确认后，此命令非常快速地处理并告诉您完成后**，则返回 True** 。

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
