<properties
    pageTitle="Web 应用程序克隆使用 PowerShell"
    description="了解如何复制到新的 Web 应用程序使用 PowerShell Web 应用程序。"
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>克隆使用 PowerShell 的 azure 应用程序服务应用程序#

与 Microsoft Azure PowerShell 版本 1.1.0 版本已新选项添加到新建 AzureRMWebApp，将为用户复制到新创建的应用程序，在其他地区或在同一个地区对现有 Web 应用程序的能力。 这将使客户能够快速而轻松地在不同地区中部署多个应用程序。

应用程序克隆目前仅支持高级层的应用程序服务规划。 新的功能作为 Web 应用程序备份功能使用相同的限制，请参阅[备份在 Azure 应用程序服务 web 应用程序](web-sites-backup.md)。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

若要了解有关使用 Azure 资源管理器基于 Azure PowerShell cmdlet 来管理您的 Web 应用程序检查[Azure 资源管理器基于 PowerShell Azure Web 应用程序的命令](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>克隆一个现有的应用程序 ##

方案︰ 在美国南部中部地区对现有 web 应用程序，用户想要克隆到新的 web 应用程序，在美国中北部地区的内容。 这可以通过使用 PowerShell cmdlet 的 Azure 资源管理器版本使用-SourceWebApp 选项来创建新的 web 应用程序。

了解包含源 web 应用程序的资源组名称，我们可以使用下面的 PowerShell 命令来获取源 web 应用程序的信息 （在本例中称为源 webapp）︰

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

若要创建新的应用程序服务计划，我们可以使用 New AzureRmAppServicePlan 命令，如以下示例所示

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

使用 New AzureRmWebApp 命令，我们可以在美国中北部区域，创建新的 web 应用程序并将它绑定到应用程序服务计划现有特优层，而且我们可以使用相同的资源组作为源 web 应用程序中，或定义新的资源组，以下演示︰

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

要克隆现有 web 应用程序包括所有关联的部署插槽，用户需要使用 IncludeSourceWebAppSlots 参数，以下 PowerShell 命令演示了如何使用 New AzureRmWebApp 命令该参数︰

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

要克隆现有 web 应用程序中的同一个区域内，用户需要创建新的资源组和新的应用程序服务在同一区域，计划，然后使用下面的 PowerShell 命令复制 web 应用程序

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>克隆到一个应用程序服务环境，对现有应用程序 ##

方案︰ 在美国南部中部地区对现有 web 应用程序，用户想要复制到新的 web 应用程序与现有的应用程序服务环境 (ASE) 的内容。

了解包含源 web 应用程序的资源组名称，我们可以使用下面的 PowerShell 命令来获取源 web 应用程序的信息 （在本例中称为源 webapp）︰

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

了解 ASE 的名称及 ASE 所属的资源组名称，用户可以使用新建 AzureRmWebApp 命令在现有的 ASE 中创建新的 web 应用程序，以下说明︰

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

位置参数是必需的由于传统的原因，但在 ASE 中创建应用程序的情况下它将被忽略。 

## <a name="cloning-an-existing-app-slot"></a>克隆一个现有应用程序插槽 ##

方案︰ 用户想要克隆现有的 Web 应用程序插槽到或者新 Web 应用程序或一个新的 Web 应用程序槽。 新的 Web 应用程序可以在同一个地区作为 Web 应用程序原来所在的插槽或在其他地区。

了解包含源 web 应用程序的资源组名称，我们可以使用下面的 PowerShell 命令来获取与 Web 应用程序源 webapp 源 web 应用程序插槽信息 （在本例中称为源 webappslot）︰

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

下面演示如何创建一个克隆到一个新的 web 应用程序的源 web 应用程序︰

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>克隆一个应用程序同时配置通信管理器 ##

创建多区域 web 应用程序和配置 Azure 流量管理器将通信路由到所有这些 web 应用程序，是 n 的重要方案，以确保客户的应用程序高度可用，克隆现有的 web 应用程序时您可以选择要连接到新的流量管理器配置文件或现有的两个 web 应用程序-请注意，只有 Azure 资源管理器支持流量管理器的版本。

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>克隆的应用程序时创建新的流量管理器配置文件 ###

方案︰ 用户希望克隆到另一个地区，一个 web 应用程序配置 Azure 资源管理器通信管理器配置文件包含两个 web 应用程序时。 下面演示如何配置新的流量管理器配置文件时创建新的 web 应用程序源 web 应用程序的克隆︰

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>添加新克隆到现有的流量管理器配置文件的 Web 应用程序 ###

方案︰ 用户已拥有他想要将这两个 web 应用程序添加终结点为 Azure 资源管理器通信管理器配置文件。 要这样做，我们首先需要装配现有的流量管理器配置文件 id，我们将需要订阅 id、 资源组的名称和现有的流量管理器配置文件名称。

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

通信管理器 id 后，下面演示如何创建克隆到一个新的 web 应用程序的源 web 应用程序时将其添加到现有的流量管理器配置文件︰

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>当前限制 ##

此功能目前在预览中，我们正在随着时间的推移添加新的功能，下面的列表对克隆应用程序的当前版本的已知的限制︰

- 自动缩放设置不被克隆
- 不是被克隆的备份时间表设置
- VNET 设置不被克隆
- 应用程序的见解都没有自动设置目标 web 应用程序上
- 简单身份验证设置不被克隆
- Kudu 扩展名不是被克隆
- 提示规则不被克隆
- 数据库内容不被克隆


### <a name="references"></a>引用 ###
- [Azure 的资源管理器基于 PowerShell 命令的 Azure Web 应用程序](app-service-web-app-azure-resource-manager-powershell.md)
- [使用 Azure 门户 web 应用程序克隆](app-service-web-app-cloning-portal.md)
- [备份在 Azure 应用程序服务 web 应用程序](web-sites-backup.md)
- [Azure Azure 流量管理器预览资源管理器支持](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [应用程序服务环境简介](app-service-app-service-environment-intro.md)
- [使用 Azure PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager.md)
