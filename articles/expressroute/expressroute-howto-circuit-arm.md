<properties
   pageTitle="创建和修改 ExpressRoute 电路，通过使用资源管理器和 PowerShell |Microsoft Azure"
   description="本文介绍如何创建、 配置、 验证、 更新、 删除和取消设置 ExpressRoute 电路。"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>创建和修改 ExpressRoute 电路

> [AZURE.SELECTOR]
[Azure 门户的资源管理器](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell 的资源管理器](expressroute-howto-circuit-arm.md)
[PowerShell 的经典](expressroute-howto-circuit-classic.md)


本文介绍如何通过使用 Windows PowerShell cmdlet 和 Azure 资源管理器部署模型创建 Azure ExpressRoute 电路。 本文还将显示您如何检查电路的状态、 更新，或删除和取消设置它。

**关于 Azure 的部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>在开始之前


- 获取最新版本的 Azure PowerShell 模块 (至少 1.0 版)。 有关如何将计算机配置为使用 PowerShell 模块的分步指导，按照如何[安装和配置 Azure PowerShell](../powershell-install-configure.md)的说明。

- 在开始配置之前检查[系统必备组件](expressroute-prerequisites.md)和[工作流](expressroute-workflows.md)。

## <a name="create-and-provision-an-expressroute-circuit"></a>创建和设置 ExpressRoute 电路

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1.注册到 Azure 帐户，然后选择您的订购

要开始您的配置，请到 Azure 帐户登录。 有关 PowerShell 的详细信息，请参阅[使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)。 使用下面的示例可帮助您连接︰

    Login-AzureRmAccount

请检查帐户订阅︰

    Get-AzureRmSubscription

选择您想要创建的 ExpressRoute 电路的订阅︰

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2.获取支持的提供程序、 位置和带宽的列表

创建 ExpressRoute 电路之前，您需要提供程序支持的连接性、 位置和带宽选项的列表。

PowerShell cmdlet`Get-AzureRmExpressRouteServiceProvider`返回此信息，您将在后续步骤中使用︰

    Get-AzureRmExpressRouteServiceProvider

检查以查看是否那里列出您的连接的提供程序。 因为您将在以后需要时创建的电路它，请记下的以下信息︰

- 名称

- PeeringLocations

- BandwidthsOffered

现在准备创建 ExpressRoute 电路。   

### <a name="3-create-an-expressroute-circuit"></a>3.创建 ExpressRoute 电路

如果您尚没有一个资源组，您必须创建一个创建 ExpressRoute 电路之前。 您可以通过运行下面的命令来执行操作︰


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


下面的示例演示如何在硅谷创建 200 Mbps 通过 Equinix ExpressRoute 电路。 如果您使用不同的提供程序和其他设置，替换该信息时使您的要求。 下面是示例请求为新的服务密钥︰

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

请确保您指定正确的 SKU 层和 SKU 系列︰

- SKU 层确定一种标准，ExpressRoute 或 ExpressRoute 高级加载项是否已启用。 您可以指定*标准*来获得最优加载项标准的 SKU 或*津贴*。

- SKU 家族决定记帐类型。 您可以指定*Metereddata*计量的数据计划和*Unlimiteddata*是包月。 请注意，您可以将计费类型从*Metereddata*更改为*Unlimiteddata*，但不是能更改类型从*Unlimiteddata*到*Metereddata*。


>[AZURE.IMPORTANT] 从服务密钥颁发的时刻将货款 ExpressRoute 电路。 请确保您可以调配电路连接提供程序时执行此操作。

该响应包含服务密钥。 您可以通过运行以下获取所有参数的详细的说明︰


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4.列出所有 ExpressRoute 电路

若要获取所创建的所有 ExpressRoute 电路的列表，请运行`Get-AzureRmExpressRouteCircuit`命令︰


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

您可以通过使用检索此信息在任何时候`Get-AzureRmExpressRouteCircuit`cmdlet。 不带参数调用列出所有电路。 将*ServiceKey*字段中列出服务密钥︰


    Get-AzureRmExpressRouteCircuit


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


您可以通过运行以下获取所有参数的详细的说明︰


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5.服务密钥发送给您的连接的提供程序的资源调配

*ServiceProviderProvisioningState*提供有关服务提供商一侧设置的当前状态信息。 状态在 Microsoft 端提供状态。 有关电路供应状态的详细信息，请参阅[工作流](expressroute-workflows.md#expressroute-circuit-provisioning-states)项目。

当您创建新的 ExpressRoute 电路时，电路将处于以下状态︰


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



在连接提供程序的过程中为您启用，电路将更改到以下状态︰

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

您可以使用 ExpressRoute 电路，它必须是处于以下状态︰

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6.定期检查的状态和电路键的状态

检查状态和状态的电路参数，可以知道当您的提供商已启用您的电路。 在电路后已配置， *ServiceProviderProvisioningState*显示为*Provisioned*，如下面的示例所示︰


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

### <a name="7-create-your-routing-configuration"></a>7.创建路由配置

分步说明，请参阅创建和修改电路 peerings 的[ExpressRoute 电路路由配置](expressroute-howto-routing-arm.md)项目。


>[AZURE.IMPORTANT] 以下说明仅适用于与提供层 2 连接服务的服务提供程序创建的电路。 如果您使用的提供托管服务提供商图层 3 服务 (通常 IP VPN，像 MPLS)，连接提供程序将配置和管理路由为您。

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8.链接到 ExpressRoute 电路虚拟网络

接下来，将虚拟网络链接到 ExpressRoute 电路。 使用资源管理器部署模型时，请使用[链接到 ExpressRoute 电路的虚拟网络](expressroute-howto-linkvnet-arm.md)文章。

## <a name="getting-the-status-of-an-expressroute-circuit"></a>获取 ExpressRoute 电路的状态

您可以通过使用检索此信息在任何时候`Get-AzureRmExpressRouteCircuit`cmdlet。 不带参数调用列出所有电路。

    Get-AzureRmExpressRouteCircuit


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


可以通过资源组的名称和电路名称作为参数传递给调用来获得特定的 ExpressRoute 电路的信息︰


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


您可以通过运行以下获取所有参数的详细的说明︰

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>修改 ExpressRoute 电路

您可以修改 ExpressRoute 电路的某些属性，而不会影响连接。

您可以执行以下操作而不需要停机︰

- 启用或禁用 ExpressRoute 电路 ExpressRoute 高级加载项。
- 增加 ExpressRoute 电路的带宽。 请注意，不支持降级电路的带宽。
- 从计量数据计量计划变成无限的数据。 请注意，不支持将计量计划从无限的数据更改为计量数据。
-  您可以启用和禁用*允许经典操作*。

有关限制和限制的详细信息，请参阅[ExpressRoute 的常见问题解答](expressroute-faqs.md)。

### <a name="to-enable-the-expressroute-premium-add-on"></a>要启用 ExpressRoute 高级加载项

您可以通过使用下面的 PowerShell 段为您现有的电路启用 ExpressRoute 高级加载项︰

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


电路将具有 ExpressRoute 高级加载项功能都已启用。 请注意，我们将开始计费您特优附加功能就该命令已成功运行。

### <a name="to-disable-the-expressroute-premium-add-on"></a>若要禁用 ExpressRoute 高级加载项

>[AZURE.IMPORTANT] 如果您使用大于什么为标准的电路所允许的资源，此操作可能会失败。

请注意以下事项︰

- 从最优降级为标准之前，必须确保链接到电路的虚拟网络的数目是少于 10。 如果不这样做，更新请求就会失败，我们将向您收取费用津贴率。

- 必须取消链接其他地缘政治区域中的所有虚拟网络。 如果不这样做，您更新的请求将会失败，并且我们将向您收取费用津贴率。

- 路由表必须少于 4000 个工艺路线的专用的对等。 如果路由表的大小大于 4000 路由，BGP 会话降之前公布的前缀数低于 4000, 不会重新启用。

通过使用以下 PowerShell cmdlet，您可以禁用现有电路的 ExpressRoute 高级加载项︰


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>若要更新 ExpressRoute 电路带宽

您的提供程序支持的带宽选项，请检查[ExpressRoute 的常见问题解答](expressroute-faqs.md)。 您可以选择任意大小大于您现有的电路。

>[AZURE.IMPORTANT] 无中断的方式 ExpressRoute 电路的带宽，您无法将其降低。 降级的带宽要求您取消设置 ExpressRoute 电路，然后重新设置新的 ExpressRoute 电路。

决定您需要什么尺寸后，请使用下面的命令来调整您的电路︰


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Microsoft 一侧，将调整您的电路。 然后您必须联系您的连接提供程序更新配置以匹配此更改其侧。 此通知后，我们将开始计费您更新的带宽选项。


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>若要移动从 SKU 计量为无限

您可以通过使用下面的 PowerShell 段更改 ExpressRoute 电路的 SKU:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>控制访问经典和资源管理器环境  

查看[从资源管理器的部署模型为经典的移动 ExpressRoute 电路](expressroute-howto-move-arm.md)中的说明进行操作。  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>撤消和删除 ExpressRoute 电路

请注意以下事项︰

- 必须取消所有的虚拟网络，从 ExpressRoute 电路。 如果此操作失败，请检查如果任何虚拟网络链接到电路。

- **资源调配**或**Provisioned** ExpressRoute 电路服务提供程序设置状态是否必须与您的服务提供商，以取消设置其侧电路中进行合作。 我们将继续保留资源和服务提供商完成撤消电路并通知我们之前向您收取费用。

- 如果服务提供商具有 deprovisioned 的电路 （服务提供商设置状态设置为**未设置**），然后可以删除电路。 这将停止电路的帐单

您可以通过运行下面的命令删除 ExpressRoute 电路︰

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>下一步行动

创建您的电路后，请确保您进行以下︰

- [创建和修改 ExpressRoute 电路的路由](expressroute-howto-routing-arm.md)
- [将虚拟网络链接到 ExpressRoute 电路](expressroute-howto-linkvnet-arm.md)
