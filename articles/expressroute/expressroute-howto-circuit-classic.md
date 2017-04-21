<properties
   pageTitle="创建和修改 ExpressRoute 电路使用传统部署模型和 PowerShell |Microsoft Azure"
   description="这篇文章将引导您完成创建和设置 ExpressRoute 电路的步骤。 本文还介绍了如何检查状态、 更新或删除，并取消设置您的电路。"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>创建和修改 ExpressRoute 电路

> [AZURE.SELECTOR]
[Azure 门户的资源管理器](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell 的资源管理器](expressroute-howto-circuit-arm.md)
[PowerShell 的经典](expressroute-howto-circuit-classic.md)

本文将引导您一步步地创建 Azure ExpressRoute 电路，使用 PowerShell cmdlet 和经典的部署模型。 本文还将显示您如何检查状态、 更新或删除，并取消设置 ExpressRoute 电路。

**关于 Azure 的部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>在开始之前

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1.审查的系统必备组件和工作流的文章

请确保您已查看[系统必备组件](expressroute-prerequisites.md)和[工作流](expressroute-workflows.md)在开始配置之前。  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2.安装最新版本的 Azure PowerShell 模块 

有关如何将计算机配置为使用 Azure PowerShell 模块按照分步指导说明如何[安装和配置 Azure PowerShell](../powershell-install-configure.md) 。

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3.以您 Azure 帐户登录，然后选择预订

1. 运行以下 cmdlet 使用提升的权限的 Windows PowerShell 提示符︰

        Add-AzureAccount
2. 在登录屏幕中出现，向您的帐户登录。

3. 获取您的订阅的列表。

        Get-AzureSubscription
4. 选择您要使用的订阅。
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>创建和设置 ExpressRoute 电路

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1.为导入的 PowerShell 模块 ExpressRoute

 如果您有不这样做，必须要开始使用 ExpressRoute cmdlet 中将 Azure 和 ExpressRoute 模块导入 PowerShell 会话。 从您的本地计算机已安装到的位置中导入模块。 这取决于您用来安装模块的方法，该位置可能不同于下面的示例演示。 如有必要，请修改示例。  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2.获取支持的提供程序、 位置和带宽的列表

创建 ExpressRoute 电路之前，您需要提供程序支持的连接性、 位置和带宽选项的列表。

PowerShell cmdlet`Get-AzureDedicatedCircuitServiceProvider`返回此信息，您将在后续步骤中使用︰

    Get-AzureDedicatedCircuitServiceProvider

检查以查看是否那里列出您的连接的提供程序。 因为您将在以后需要时创建的电路它，请记下的以下信息︰

- 名称

- PeeringLocations

- BandwidthsOffered

现在准备创建 ExpressRoute 电路。         

### <a name="3-create-an-expressroute-circuit"></a>3.创建 ExpressRoute 电路

下面的示例演示如何在硅谷创建 200 Mbps 通过 Equinix ExpressRoute 电路。 如果您使用不同的提供程序和其他设置，替换该信息时使您的要求。

>[AZURE.IMPORTANT] 从服务密钥颁发的时刻将货款 ExpressRoute 电路。 请确保您可以调配电路连接提供程序时执行此操作。


下面是示例请求为新的服务密钥︰

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

或者，如果您想要使用高级加载项创建 ExpressRoute 电路，使用下面的示例。 请参阅有关高级加载项的详细信息的[ExpressRoute 的常见问题解答](expressroute-faqs.md)。

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


响应将包含服务密钥。 您可以通过运行以下获取所有参数的详细的说明︰

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4.列出所有 ExpressRoute 电路

您可以运行`Get-AzureDedicatedCircuit`命令来获取您所创建的所有 ExpressRoute 电路的列表︰


    Get-AzureDedicatedCircuit

响应将类似于以下示例︰

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

您可以通过使用检索此信息在任何时候`Get-AzureDedicatedCircuit`cmdlet。 调用不带任何参数列出所有电路。 服务密钥将*ServiceKey*字段中列出。

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

您可以通过运行以下获取所有参数的详细的说明︰

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5.服务密钥发送给您的连接的提供程序的资源调配


*ServiceProviderProvisioningState*提供有关服务提供商一侧设置的当前状态的信息。 *状态*在 Microsoft 端提供状态。 有关电路供应状态的详细信息，请参阅[工作流](expressroute-workflows.md#expressroute-circuit-provisioning-states)项目。

当您创建新的 ExpressRoute 电路时，电路将处于以下状态︰

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


在连接提供程序的过程中为您启用，电路将转到以下状态︰

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

ExpressRoute 电路必须在您可以使用它的以下状态︰

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6.定期检查的状态和电路键的状态

这样您就知道您的提供商已启用您的电路。 在配置电路后， *ServiceProviderProvisioningState*将显示为*Provisioned* ，如下面的示例所示︰

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7.创建路由配置

请参阅[ExpressRoute 电路路由配置 （创建和修改电路 peerings）](expressroute-howto-routing-classic.md)的分步说明。

>[AZURE.IMPORTANT] 以下说明仅适用于与提供层 2 连接服务的服务提供程序创建的电路。 如果您使用的提供托管服务提供商图层 3 服务 (通常 IP VPN，像 MPLS)，连接提供程序将配置和管理路由为您。

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8.链接到 ExpressRoute 电路虚拟网络

接下来，将虚拟网络链接到 ExpressRoute 电路。 分步指导，请参阅[与虚拟网络的链接 ExpressRoute 电路](expressroute-howto-linkvnet-classic.md)。 如果您需要使用 ExpressRoute 的经典部署模型创建虚拟网络，请参阅[创建虚拟网络中的 ExpressRoute](expressroute-howto-vnet-portal-classic.md) 。

## <a name="getting-the-status-of-an-expressroute-circuit"></a>获取 ExpressRoute 电路的状态

您可以通过使用检索此信息在任何时候`Get-AzureCircuit`cmdlet。 调用不带任何参数列出所有电路。

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

可以通过将服务密钥作为参数传递给调用来获取特定的 ExpressRoute 电路上的信息︰

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


您可以通过运行以下获取所有参数的详细的说明︰

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>修改 ExpressRoute 电路

您可以修改 ExpressRoute 电路的某些属性，而不会影响连接。

您可以执行以下操作而不需要停机︰

- 启用或禁用 ExpressRoute 电路 ExpressRoute 高级加载项。
- 增加 ExpressRoute 电路的带宽。 请注意，不支持降级电路的带宽。
- 从计量数据计量计划变成无限的数据。 请注意，不支持将计量计划从无限的数据更改为计量数据。
- 您可以启用和禁用*允许经典操作*。

请参阅限制和限制的详细信息的[ExpressRoute 的常见问题解答](expressroute-faqs.md)。

### <a name="to-enable-the-expressroute-premium-add-on"></a>要启用 ExpressRoute 高级加载项

通过使用以下 PowerShell cmdlet，您可以为您现有的电路启用 ExpressRoute 高级加载项︰

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

您电路将具有 ExpressRoute 高级加载项功能都已启用。 请注意，我们将开始计费您特优附加功能就该命令已成功运行。

### <a name="to-disable-the-expressroute-premium-add-on"></a>若要禁用 ExpressRoute 高级加载项

>[AZURE.IMPORTANT] 如果您使用大于什么为标准的电路所允许的资源，此操作可能会失败。

请注意以下事项︰

- 您必须确保虚拟网络链接到电路数小于 10 之前从特优降级为标准。 如果不这样做，更新请求将失败，并且您将记帐的优质率。

- 必须取消链接其他地缘政治区域中的所有虚拟网络。 如果不这样做，更新请求将失败，并且您将记帐的优质率。

- 路由表必须少于 4000 个工艺路线的专用的对等。 如果路由表的大小大于 4000 路由，BGP 会话放不下并且直到通告前缀数低于 4000 不会重新启用。

通过使用以下 PowerShell cmdlet，您可以为您现有的电路禁用 ExpressRoute 高级加载项︰

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>若要更新 ExpressRoute 电路带宽

请检查您的提供程序支持的带宽选项的[ExpressRoute 的常见问题解答](expressroute-faqs.md)。 您可以选择任何大于您现有电路的大小，只要 （不在其创建您的电路） 的物理端口允许的大小。

>[AZURE.IMPORTANT] 无中断的方式 ExpressRoute 电路的带宽，您无法将其降低。 降级的带宽将要求您取消设置 ExpressRoute 电路，然后重新设置新的 ExpressRoute 电路。

决定您需要什么尺寸后，可以使用下面的命令来调整您的电路︰

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

您电路将有已调整 Microsoft 一侧。 您必须联系您的连接提供程序更新配置以匹配此更改其侧。 请注意，我们将开始计费您更新的带宽选项，从这一点上。

如果增加电路带宽时，您会看到以下错误，则说明那里没有足够的带宽保留在其中创建您现有电路的物理端口。 您必须删除此电路并创建所需的大小的新电路。 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>撤消和删除 ExpressRoute 电路

请注意以下事项︰

- 必须取消所有的虚拟网络，从 ExpressRoute 电路，此操作才能成功。 检查以查看是否有任何虚拟网络链接电路如果此操作失败。

- **资源调配**或**Provisioned** ExpressRoute 电路服务提供程序设置状态是否必须与您的服务提供商，以取消设置其侧电路中进行合作。 我们将继续保留资源和服务提供商完成撤消电路并通知我们之前向您收取费用。

- 如果服务提供商具有 deprovisioned 的电路 （服务提供商设置状态设置为**未设置**），然后可以删除电路。 这将停止电路的帐单。

您可以通过运行下面的命令删除 ExpressRoute 电路︰

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>下一步行动

创建您的电路后，请确保您进行以下︰

- [创建和修改 ExpressRoute 电路的路由](expressroute-howto-routing-classic.md)
- [将虚拟网络链接到 ExpressRoute 电路](expressroute-howto-linkvnet-classic.md)
