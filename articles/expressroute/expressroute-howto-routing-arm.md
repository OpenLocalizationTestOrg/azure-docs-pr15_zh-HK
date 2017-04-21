<properties
   pageTitle="如何配置 ExpressRoute 电路的路由 |Microsoft Azure"
   description="这篇文章将引导您完成创建和设置私有、 公共和 Microsoft ExpressRoute 电路的对等的步骤。 本文还介绍如何检查状态，更新或删除您电路的 peerings。"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>创建和修改 ExpressRoute 电路的路由


> [AZURE.SELECTOR]
[Azure 门户的资源管理器](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell 的资源管理器](expressroute-howto-routing-arm.md)
[PowerShell 的经典](expressroute-howto-routing-classic.md)



本文将引导您一步步地创建和管理使用 PowerShell 和 Azure 资源管理器部署模型 ExpressRoute 电路的路由配置。  下面的步骤将还显示如何检查状态、 更新或删除，并取消设置 peerings ExpressRoute 电路。 


**关于 Azure 的部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>配置系统必备组件

- 您需要最新版本的 Azure PowerShell 模块版本 1.0 或更高版本。 
- 请确保您已查看了[先决条件](expressroute-prerequisites.md)页、[路由要求](expressroute-routing.md)页和[工作流](expressroute-workflows.md)页上配置之前。
- 您必须具有活动的 ExpressRoute 电路。 按照说明[创建 ExpressRoute 电路](expressroute-howto-circuit-arm.md)并已启用您连接的提供程序，请在继续之前的电路。 在您可以运行如下所述的 cmdlet 的调配并已启用状态必须是 ExpressRoute 电路。

以下说明仅适用于与服务提供商提供第二层连接性服务创建的电路。 如果您正在使用服务管理第 3 层 (通常 IPVPN，像 MPLS) 是服务提供商，您的连接的提供程序将配置和管理路由为您。

>[AZURE.IMPORTANT] 我们当前不公布 peerings 由服务提供商通过服务管理门户配置。 我们正在尽快启用此功能。 请与服务提供商配置 BGP peerings 之前。

您可以配置一个、 两个或者所有三个 peerings （Azure 的私有的 Azure 公共和 Microsoft） 为 ExpressRoute 电路。 您可以按任何顺序选择配置 peerings。 但是，您必须确保您完成配置的每个对等的一次。 

## <a name="azure-private-peering"></a>Azure 专用对等

本节说明如何创建、 获取、 更新和删除 ExpressRoute 电路的 Azure 专用等配置。 

### <a name="to-create-azure-private-peering"></a>若要创建 Azure 专用对等

1. 导入 ExpressRoute PowerShell 模块。
    
    必须从[PowerShell 库](http://www.powershellgallery.com/)安装最新的 PowerShell 安装程序，并将 Azure 资源管理器模块导入 PowerShell 会话，才能开始使用 ExpressRoute cmdlet。 您需要以管理员身份运行 PowerShell。

        Install-Module AzureRM

        Install-AzureRM

    导入所有的已知的语义版本范围之内的 AzureRM.* 模块

        Import-AzureRM

    还可以导入选择模块的已知语义版本范围之内 
        
        Import-Module AzureRM.Network 

    登录到您的帐户

        Login-AzureRmAccount

    选择您想要创建 ExpressRoute 电路的订阅
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. 创建 ExpressRoute 电路。
    
    请按照说明进行操作来创建[ExpressRoute 电路](expressroute-howto-circuit-arm.md)，将它连接提供程序配置。 

    如果您连接的提供商提供第三层的托管的服务，您可以请求连接提供程序以启用 Azure 专用对等的。 在这种情况下，您不必按照下一节中列出的说明进行操作。 但是，如果您连接的提供程序不会管理，路由创建您电路后按照下面的说明进行操作。 

3. 检查 ExpressRoute 电路，以确保其配置。

    您必须先签 ExpressRoute 电路是否是调配，同时启用。 下面的示例，请参阅。

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    响应将类似于下面的示例︰

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. 配置 Azure 专用对等电路。

    请确保在继续下一步行动之前，有以下各项︰

    - /30 主链接的子网。 这不能保留虚拟网络的所有地址空间的一部分。
    - /30 辅助链接的子网。 这不能保留虚拟网络的所有地址空间的一部分。
    - 建立在此对等有效的 VLAN ID。 确保没有其它对等电路中使用相同的 VLAN id。
    - 作为对等号。 您可以使用 2 字节和 4 字节数字。 您可以使用专用数字为该对等。 请确保没有使用 65515。
    - 如果您选择使用一个 MD5 散列。 **这是可选的**。
    
    您可以运行以下 cmdlet 配置 Azure 专用对等为您电路。

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    如果您选择使用 MD5 哈希值，则可以使用以下 cmdlet。

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] 确保您指定为对等的 ASN，不是客户 ASN AS 编号。

### <a name="to-view-azure-private-peering-details"></a>若要查看 Azure 专用等详细信息

您可以使用以下 cmdlet 的配置详细信息

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>若要更新 Azure 专用的对等配置

您可以更新使用以下 cmdlet 的配置的任何部分。 在下面的示例中，电路的 VLAN ID 正在更新从 100 到 500。

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>若要删除 Azure 专用对等

通过运行以下 cmdlet，您可以删除您的对等配置。

>[AZURE.WARNING] 您必须确保在运行该 cmdlet 之前所有的虚拟网络是 ExpressRoute 电路中取消链接。 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure 公共对等

本节说明如何创建、 获取、 更新和删除 ExpressRoute 电路的 Azure 公共对等配置。

### <a name="to-create-azure-public-peering"></a>若要创建 Azure 公共对等

1. 导入 ExpressRoute PowerShell 模块。
    
    必须从[PowerShell 库](http://www.powershellgallery.com/)安装最新的 PowerShell 安装程序，并将 Azure 资源管理器模块导入 PowerShell 会话，才能开始使用 ExpressRoute cmdlet。 您需要以管理员身份运行 PowerShell。

        Install-Module AzureRM

        Install-AzureRM

    导入所有的已知的语义版本范围之内的 AzureRM.* 模块

        Import-AzureRM

    还可以导入选择模块的已知语义版本范围之内 
        
        Import-Module AzureRM.Network 

    登录到您的帐户

        Login-AzureRmAccount

    选择您想要创建 ExpressRoute 电路的订阅
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. 创建 ExpressRoute 电路。
    
    请按照说明进行操作来创建[ExpressRoute 电路](expressroute-howto-circuit-arm.md)，将它连接提供程序配置。 

    如果您连接的提供商提供第三层的托管的服务，您可以请求连接提供程序以启用 Azure 公共对等的。 在这种情况下，您不必按照下一节中列出的说明进行操作。 但是，如果您连接的提供程序不会管理，路由创建您电路后按照下面的说明进行操作。

3. 检查 ExpressRoute 电路，以确保其配置。

    您必须先签 ExpressRoute 电路是否是调配，同时启用。 下面的示例，请参阅。

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    响应将类似于下面的示例︰

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. 配置 Azure 公共对等电路。

    请确保具有以下信息，然后再进一步继续。

    - /30 主链接的子网。 这必须是一个有效的公用 IPv4 前缀。
    - /30 辅助链接的子网。 这必须是一个有效的公用 IPv4 前缀。
    - 建立在此对等有效的 VLAN ID。 确保没有其它对等电路中使用相同的 VLAN id。
    - 作为对等号。 您可以使用 2 字节和 4 字节数字。
    - 如果您选择使用一个 MD5 散列。 **这是可选的**。
    
    您可以运行以下 cmdlet 配置 Azure 公共对等为您电路

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    如果您选择使用 MD5 哈希值，则可以使用以下 cmdlet

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] 确保您指定为对等的 ASN 并不是客户 ASN AS 编号。

### <a name="to-view-azure-public-peering-details"></a>若要查看 Azure 公共对等的详细信息

您可以使用以下 cmdlet 的配置详细信息

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>若要更新 Azure 公用的对等配置

您可以更新配置使用以下 cmdlet 的任何一部分

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

电路的 VLAN ID 正在更新从 200 到 600 在上面的示例。

### <a name="to-delete-azure-public-peering"></a>若要删除 Azure 公共对等

您可以通过运行以下 cmdlet 删除您的对等配置

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft 对等

本节说明如何创建、 获取、 更新和删除的 Microsoft ExpressRoute 电路的对等配置。 

### <a name="to-create-microsoft-peering"></a>若要创建 Microsoft 对等

1. 导入 ExpressRoute PowerShell 模块。
    
    必须从[PowerShell 库](http://www.powershellgallery.com/)安装最新的 PowerShell 安装程序，并将 Azure 资源管理器模块导入 PowerShell 会话，才能开始使用 ExpressRoute cmdlet。 您需要以管理员身份运行 PowerShell。

        Install-Module AzureRM

        Install-AzureRM

    导入所有的已知的语义版本范围之内的 AzureRM.* 模块

        Import-AzureRM

    还可以导入选择模块的已知语义版本范围之内 
        
        Import-Module AzureRM.Network 

    登录到您的帐户

        Login-AzureRmAccount

    选择您想要创建 ExpressRoute 电路的订阅
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. 创建 ExpressRoute 电路。
    
    请按照说明进行操作来创建[ExpressRoute 电路](expressroute-howto-circuit-arm.md)，将它连接提供程序配置。 

    如果您连接的提供商提供第三层的托管的服务，您可以请求连接提供程序以启用 Azure 专用对等的。 在这种情况下，您不必按照下一节中列出的说明进行操作。 但是，如果您连接的提供程序不会管理，路由创建您电路后按照下面的说明进行操作。

3. 检查 ExpressRoute 电路，以确保其配置。

    您必须先签 ExpressRoute 电路是否是调配，同时启用。 下面的示例，请参阅。

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    响应将类似于下面的示例︰

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. 配置 Microsoft 对电路等。

    请确保具有以下信息，然后再继续。

    - /30 主链接的子网。 这必须是有效公用 IPv4 前缀由您拥有并注册在 RIR / IRR。
    - /30 辅助链接的子网。 这必须是有效公用 IPv4 前缀由您拥有并注册在 RIR / IRR。
    - 建立在此对等有效的 VLAN ID。 确保没有其它对等电路中使用相同的 VLAN id。
    - 作为对等号。 您可以使用 2 字节和 4 字节数字。
    - 通告前缀︰ 您必须提供您打算公布对 BGP 会话的所有前缀的列表。 接受公共 IP 地址的前缀。 如果您计划发送一组前缀，您可以发送一个逗号分隔列表。 这些前缀必须在 RIR 注册到您 / IRR。
    - 客户 ASN︰ 如果是未注册的编号为对等的广告前缀，您可以指定注册到 AS 编号。 **这是可选的**。
    - 路由的注册表名称︰ 您可以指定 RIR / IRR 根据其注册数和前缀。
    - MD5 哈希值，如果您选择使用其中一个。 **这是可选的。**
    
    您可以运行以下 cmdlet 来配置 Microsoft 为您电路对等

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>若要获取 Microsoft 等详细信息

您可以使用以下 cmdlet 的配置详细信息。

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>若要更新对等配置，Microsoft

您可以更新使用以下 cmdlet 的配置的任何部分。

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>若要删除 Microsoft 对等

通过运行以下 cmdlet，您可以删除您的对等配置。

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>下一步行动

下一步，[链接到 ExpressRoute 电路 VNet](expressroute-howto-linkvnet-arm.md)。

-  有关 ExpressRoute 工作流的详细信息，请参阅[ExpressRoute 工作流](expressroute-workflows.md)。

-  有关电路对等的详细信息，请参阅[ExpressRoute 电路和路由域](expressroute-circuit-peerings.md)。

-  有关如何使用虚拟网络的详细信息，请参阅[虚拟网络概述](../virtual-network/virtual-networks-overview.md)。

