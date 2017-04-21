<properties
    pageTitle="Azure 基于资源管理器的跨平台命令行工具来 Azure 的 Web 应用程序 |Microsoft Azure"
    description="了解如何使用新的基于 Azure 资源管理器的跨平台命令行工具来管理 Azure Web 应用程序。"
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>对 Azure 的 Web 应用程序中使用 Azure 的资源管理器基于 XPlat CLI#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

随着 Microsoft Azure 跨平台命令行工具版本 0.10.5 的发布，添加新的命令。 这些命令为用户提供基于 Azure 资源管理器的 PowerShell 命令用于管理 Web 应用程序的能力。

若要了解有关管理资源组，请参阅[使用 Azure CLI 管理 Azure 的资源和资源组](../xplat-cli-azure-resource-manager.md)。 


## <a name="managing-app-service-plans"></a>管理应用程序服务计划 ##

### <a name="create-an-app-service-plan"></a>创建应用程序服务计划 ###
若要创建一个应用程序服务计划，请使用**azure appserviceplan 创建**命令。

以下是不同参数的说明︰

-   **-资源组**︰ 资源组包含新创建的应用程序服务计划。
-   **-名称**︰ 应用程序服务计划的名称。
-   **-位置**︰ 应用程序服务计划的位置。
-   **-层**︰ 需定价 sku (选项︰ F1 （免费）。 D1 （共享）。 B1 （基本小），B2 （基本中），和 B3 （基本大）。 S1 （标准小），S2 （标准中），和 S3 （标准大）。 （特优小） P1、 P2 （特优中） 和 P3 （大特优）。)
-   **-实例**︰ 服务应用程序中的工作人员的次数 （默认值为 1） 的计划。

若要使用此 cmdlet 的示例︰

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>列出现有的应用程序服务计划 ###

若要列出现有的应用程序服务计划，请使用**azure appserviceplan list**命令。

若要列出所有的应用程序服务计划下的特定资源组，请使用︰

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

若要获取一个特定的应用程序的服务计划，请使用**azure appserviceplan show**命令︰

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>配置现有的应用程序服务规划 ###

若要更改现有的应用程序服务计划的设置，请使用**azure appserviceplan 配置**命令。 您可以更改该 sku 和工作人员数量 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>扩展的应用程序服务计划 ####

为了扩展现有的应用程序服务计划，请使用︰

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>更改应用程序服务计划的 SKU ####

若要更改现有的应用程序服务计划的 sku，请使用︰

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>删除现有的应用程序服务计划 ###

若要删除现有的应用程序服务计划，需要移动或删除所有指定的 web 应用程序。 然后使用**azure webapp delete**命令可删除对应用程序的服务方案。

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>管理应用程序服务 Web 应用程序 ##

### <a name="create-a-web-app"></a>创建 Web 应用程序 ###

若要创建 web 应用程序，使用**azure webapp 创建**命令。

以下是不同参数的说明︰

- **-名称**: web 应用程序的名称。
- **-计划**︰ 服务计划用于承载该 web 应用程序的名称。
- **-资源组**︰ 承载的应用程序服务计划的资源组。
- **-位置**: web 应用程序的位置。

若要使用此 cmdlet 的示例︰

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>删除现有的 Web 应用程序 ###

若要删除现有的 web 应用程序可以使用**azure webapp delete**命令，您必须指定 web 应用程序的名称和资源组的名称。

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>列出现有的 Web 应用程序 ###

若要列出现有的 web 应用程序，请使用**azure webapp 列表**命令。

若要列出特定资源组下的所有 web 应用程序，请使用︰

    azure webapp list --resource-group ContosoAzureResourceGroup

若要获得特定的 web 应用程序，请使用**azure webapp 显示**命令。

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>配置一个现有的 Web 应用程序 ###

若要更改的设置和对现有的 web 应用程序的配置，使用**azure webapp 配置设置**的命令。

示例 (1): 更改 web 应用程序的 php 版本 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

示例 (2): 添加或更改应用程序设置

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

若要了解其他配置能发生更改时，使用**azure webapp 配置集-h**命令。

### <a name="change-the-state-of-an-existing-web-app"></a>更改现有 Web 应用程序的状态 ###

#### <a name="restart-a-web-app"></a>重新启动 web 应用程序 ####

要重新启动 web 应用程序，您必须指定 web 应用程序的名称和资源组。

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>停止一个 web 应用程序 ####

若要停止 web 应用程序，必须指定 web 应用程序的名称和资源组。

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>启动 web 应用程序 ####

若要启动 web 应用程序，必须指定 web 应用程序的名称和资源组。

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>管理 Web 应用程序发布配置文件 ###

每个 web 应用程序具有可用于发布您的应用程序的发布配置文件。

#### <a name="get-publishing-profile"></a>获取发布配置文件 ####

要获得 web 应用程序的发布配置文件，请使用︰

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

发布配置文件的用户名和密码在命令行，此命令回显。

### <a name="manage-web-app-hostnames"></a>管理 Web 应用程序的主机名 ###

要管理您的 web 应用程序的主机名绑定，请使用**azure webapp 配置主机名**的命令  

#### <a name="list-hostname-bindings"></a>列表中的主机名称绑定 ####

若要获取 web 应用程序的当前主机名称绑定，请使用︰

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>添加主机名称绑定 ####

若要添加主机名称绑定到一个 web 应用程序，请使用︰

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>删除主机名称绑定 ####

要删除主机名称绑定，请使用︰

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>下一步行动 ###
- 要了解有关 Azure 资源管理器 CLI 支持信息，请参阅[使用 Azure CLI 管理 Azure 的资源和资源组。](../xplat-cli-azure-resource-manager.md)
- 若要了解有关管理使用 PowerShell 的应用程序服务，请参阅[管理 Azure Web 应用程序的 Using Azure Resource Manager-Based PowerShell。](app-service-web-app-azure-resource-manager-powershell.md)
