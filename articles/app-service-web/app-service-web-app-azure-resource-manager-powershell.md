<properties
    pageTitle="Azure 的资源管理器基于 PowerShell 命令 Azure Web 应用程序 |Microsoft Azure"
    description="了解如何使用新的基于 Azure 资源管理器的 PowerShell 命令管理 Azure Web 应用程序。"
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
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>使用 Azure 的资源管理器基于 PowerShell 管理 Azure 的 Web 应用程序#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

与 Microsoft Azure PowerShell 1.0.0 版新已添加命令，它为用户提供基于 Azure 资源管理器的 PowerShell 命令用于管理 Web 应用程序的能力。

若要了解有关管理资源组，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager.md)。 

若要了解有关参数和 PowerShell cmdlet 的选项的完整列表，请参阅[完整的 Web 应用程序 Azure 资源管理器基于 PowerShell Cmdlet 的 Cmdlet 引用](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>管理应用程序服务计划 ##

### <a name="create-an-app-service-plan"></a>创建应用程序服务计划 ###
若要创建一个应用程序服务计划，请使用**新建 AzureRmAppServicePlan** cmdlet。

以下是不同参数的说明︰

-   **名称**︰ 应用程序服务计划的名称。
-   **位置**︰ 服务计划的位置。
-   **ResourceGroupName**︰ 资源组包含新创建的应用程序服务计划。
-   **层**︰ 需定价层 （默认是免费，其他选项都共享、 基本、 标准和特优）。
-   **WorkerSize**︰ 大小的工作人员 （默认值为小如果层参数被指定为基本、 标准版或津贴。 其他选项是中型和大型。
-   **NumberofWorkers**︰ 服务应用程序中的工作人员的次数 （默认值为 1） 的计划。 

若要使用此 cmdlet 的示例︰

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>在应用程序服务的环境中创建的应用程序服务计划 ###
若要在应用程序服务环境中创建的应用程序服务计划，使用相同的命令**新建 AzureRmAppServicePlan**命令额外的参数来指定 ASE 的名称和 ASE 的资源组名称。

若要使用此 cmdlet 的示例︰

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

若要了解有关应用程序的服务环境，检查[应用程序服务环境简介](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>列出现有的应用程序服务计划 ###

若要列出现有的应用程序服务计划，请使用**Get AzureRmAppServicePlan** cmdlet。

若要列出所有的应用程序服务计划下您的订购，请使用︰ 

    Get-AzureRmAppServicePlan

若要列出所有的应用程序服务计划下的特定资源组，请使用︰

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

若要获取一个特定的应用程序的服务计划，请使用︰

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>配置现有的应用程序服务规划 ###

若要更改现有的应用程序服务计划的设置，请使用**组 AzureRmAppServicePlan** cmdlet。 您可以更改的层、 工作大小和工作人员数量 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>扩展的应用程序服务计划 ####

为了扩展现有的应用程序服务计划，请使用︰

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>更改应用程序服务计划工作人员的大小 ####

若要更改现有的应用程序服务计划中的工作人员的大小，请使用︰

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>更改层的应用程序服务计划 ####

若要更改现有的应用程序服务计划的层，请使用︰

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>删除现有的应用程序服务计划 ###

若要删除现有的应用程序服务计划，需要移动或删除所有指定的 web 应用程序。 然后使用**删除 AzureRmAppServicePlan** cmdlet，您可以删除对应用程序的服务方案。

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>管理应用程序服务 Web 应用程序 ##

### <a name="create-a-web-app"></a>创建 Web 应用程序 ###

若要创建 web 应用程序，请使用**New AzureRmWebApp** cmdlet。

以下是不同参数的说明︰

- **名称**︰ web 应用程序的名称。
- **AppServicePlan**︰ 服务计划用于承载该 web 应用程序的名称。
- **ResourceGroupName**︰ 承载的应用程序服务计划的资源组。
- **位置**︰ web 应用程序的位置。

若要使用此 cmdlet 的示例︰

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>在一个应用程序服务的环境中创建 Web 应用程序 ###

若要创建 web 应用程序中的应用程序服务环境 (ASE)。 使用具有额外的参数相同的**新 AzureRmWebApp**命令指定的 ASE 名称和 ASE 所属的资源组名称。

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

若要了解有关应用程序的服务环境，检查[应用程序服务环境简介](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>删除现有的 Web 应用程序 ###

若要删除现有的 web 应用程序可以使用**删除 AzureRmWebApp** cmdlet，您需要指定 web 应用程序的名称和资源组的名称。

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>列出现有的 Web 应用程序 ###

若要列出现有的 web 应用程序，请使用**Get AzureRmWebApp** cmdlet。

若要列出所有的 web 应用程序在您的订购，请使用︰

    Get-AzureRmWebApp

若要列出特定资源组下的所有 web 应用程序，请使用︰

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

若要获取一个特定的 web 应用程序，请使用︰

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>配置一个现有的 Web 应用程序 ###

若要更改设置，对现有的 web 应用程序的配置，使用**一组 AzureRmWebApp** cmdlet。 参数的完整列表，请检查该[Cmdlet 引用链接](https://msdn.microsoft.com/library/mt652487.aspx)

例 (1): 使用此 cmdlet 以更改连接字符串

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

示例 (2): 添加或更改应用程序设置

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


示例 (3): 设置使 web 应用程序在 64 位模式下运行

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>更改现有 Web 应用程序的状态 ###

#### <a name="restart-a-web-app"></a>重新启动 web 应用程序 ####

要重新启动 web 应用程序，您必须指定 web 应用程序的名称和资源组。

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>停止一个 web 应用程序 ####

若要停止 web 应用程序，必须指定 web 应用程序的名称和资源组。

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>启动 web 应用程序 ####

若要启动 web 应用程序，必须指定 web 应用程序的名称和资源组。

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>管理 Web 应用程序发布配置文件 ###

每个 web 应用程序可用于发布您的应用程序的发布配置文件，可执行多个操作发布配置文件。

#### <a name="get-publishing-profile"></a>获取发布配置文件 ####

要获得 web 应用程序的发布配置文件，请使用︰

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

此命令回显到命令行并输出为发布配置文件到文本文件的发布配置文件。

#### <a name="reset-publishing-profile"></a>重置的发布配置文件 ####

若要重置两个 FTP 和 web 发布的密码部署的 web 应用程序中，使用︰

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>管理 Web 应用程序的证书 ###

若要了解有关如何管理 web 应用程序的证书，请参阅[使用 PowerShell 的 SSL 证书绑定](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>下一步行动 ###
- 要了解有关 Azure 资源管理器 PowerShell 支持信息，请参阅[使用 Azure 资源管理器中使用 Azure PowerShell。](../powershell-azure-resource-manager.md)
- 若要了解详细信息的应用程序服务的环境，请参阅[应用程序服务环境介绍。](app-service-app-service-environment-intro.md)
- 若要了解有关管理使用 PowerShell 的应用程序服务的 SSL 证书，请参阅[SSL 证书绑定使用 PowerShell。](app-service-web-app-powershell-ssl-binding.md)
- 要为 Azure Web 应用程序了解 Azure 资源管理器基于 PowerShell cmdlet 的完整列表，请参阅[Azure Cmdlet 引用的 Web 应用程序 Azure 资源管理器 PowerShell Cmdlet。](https://msdn.microsoft.com/library/mt619237.aspx)
- - 若要了解有关管理使用 CLI 应用程序服务，请参阅[Using Azure Resource Manager-Based XPlat CLI 的 Azure Web 应用程序](app-service-web-app-azure-resource-manager-xplat-cli.md)
