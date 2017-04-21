<properties
   pageTitle="从经典到资源管理器中移动 ExpressRoute 电路 |Microsoft Azure"
   description="此页面介绍了如何将经典电路移到资源管理器的部署模型。"
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>从经典的 ExpressRoute 电路移到资源管理器部署模型

## <a name="configuration-prerequisites"></a>配置系统必备组件

- 您需要最新版本的 Azure PowerShell 模块 (至少 1.0 版)。
- 请确保您已查看了[先决条件](expressroute-prerequisites.md)、[路由的要求](expressroute-routing.md)，以及[工作流](expressroute-workflows.md)开始配置之前。
- 在前进一步之前, 检查下[移动 ExpressRoute 电路从经典到资源管理器](expressroute-move.md)提供的信息。 确保完全了解的限制和限制所能做到的。
- 如果您想要将 Azure ExpressRoute 电路从经典的部署模型移到 Azure 资源管理器部署模型，您必须具有电路完全配置和运营中的经典的部署模型。
- 确保您具有在资源管理器部署模型中创建的资源组。

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>将 ExpressRoute 电路移到资源管理器部署模型

以便可以使用它在经典和资源管理器部署模型，则必须移到资源管理器部署模型 ExpressRoute 电路。 您可以通过运行以下 PowerShell 命令来执行此操作。

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>步骤 1︰ 收集经典的部署模型电路详细信息

您需要先收集 ExpressRoute 电路有关的信息。

登录到 Azure 的传统环境中，并收集服务密钥。 可以使用下面的 PowerShell 段收集的信息︰

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

复制您要移到资源管理器部署模型的电路**服务密钥**。

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>步骤 2︰ 登录到资源管理器环境中，并创建新的资源组

您可以通过使用下面的代码段创建新的资源组︰

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

如果您已经拥有一个，您也可以使用现有资源组。

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>步骤 3︰ 将 ExpressRoute 电路移到资源管理器部署模型

现在您可以通过 ExpressRoute 电路从经典移到资源管理器的部署模型。 检查下[移动从资源管理器的部署模型为经典的 ExpressRoute 电路](expressroute-move.md)进一步操作之前提供的信息。

你可以通过运行下面的代码段︰

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] 移动完成后，新列入上一个 cmdlet 的名称将用于填补资源。 电路本质上是将被重命名。

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>启用这两种部署模型的 ExpressRoute 电路

控制访问的部署模型前，必须到资源管理器部署模型移动 ExpressRoute 电路。

运行以下 cmdlet 以允许访问这两种部署模型︰

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

成功完成此操作后，您将能够查看电路中经典的部署模型。

运行以下操作来获取 ExpressRoute 电路的详细信息︰

    get-azurededicatedcircuit

您必须能够看到列出的服务注册表项。 现在，您可以管理 ExpressRoute 电路使用 ARM VNETs 的经典的 VNets 标准的经典部署模型命令和标准的 ARM 命令的链接。 下面的文章将指导您完成如何管理 ExpressRoute 电路的链接︰

- [链接到您的 ExpressRoute 电路资源管理器部署模型中的虚拟网络](expressroute-howto-linkvnet-arm.md)
- [将虚拟网络链接到您的 ExpressRoute 电路中经典的部署模型](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>禁用对经典的部署模型的 ExpressRoute 电路

运行以下 cmdlet 以禁止访问的经典的部署模型︰

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

成功完成此操作后，您将不能查看电路中经典的部署模型。

## <a name="next-steps"></a>下一步行动

创建您的电路后，请确保您进行以下︰

- [创建和修改 ExpressRoute 电路的路由](expressroute-howto-routing-arm.md)
- [将虚拟网络链接到 ExpressRoute 电路](expressroute-howto-linkvnet-arm.md)
