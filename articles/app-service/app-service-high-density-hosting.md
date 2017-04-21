<properties
    pageTitle="在 Azure 应用程序服务上承载密度较高 |Microsoft Azure"
    description="高密度承载 Azure 应用程序服务"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>高密度承载 Azure 应用程序服务

当使用应用程序服务，您的应用程序分离从两个概念所分配给它的容量︰

- **应用程序︰**表示应用程序和运行时配置。 例如，它包括.NET 运行时加载，应用程序设置等的版本。

- **应用程序服务计划︰**定义容量、 可用的功能集和应用程序的局部性的特征。 例如，特征可能大 （四核） 机，四个实例，在美国东部的高级功能。

应用程序始终链接到应用程序服务计划，但应用程序服务计划可提供到一个或多个应用程序的容量。

结果，该平台提供了灵活性，隔离某个应用程序或具有多个应用程序共享资源，通过共享应用程序服务计划。

但是，当多个应用程序共享应用程序服务计划时，该应用程序的一个实例运行该应用程序服务计划的每个实例。

## <a name="per-app-scaling"></a>每个应用程序扩展
*每个应用程序扩展*是一种功能，可以在应用程序服务计划级别启用，然后使用每个应用程序。

每个应用程序比例缩放应用程序独立于承载它的应用程序服务计划。 这种方式，可以配置应用程序服务计划，提供 10 个实例，但可以设置应用程序扩展到只有 5 个。

以下的 Azure 资源管理器模板创建放大到 10 个实例和应用程序配置为使用每个应用程序扩展，扩展到仅 5 实例的应用程序服务计划。

应用程序服务计划将**每个网站的缩放比例**属性设置为 true ( `"perSiteScaling": true`)。 应用程序设置为 5 使用的**工作线程数**(`"properties": { "numberOfWorkers": "5" }`)。

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>用于承载密度较高的推荐的配置

每个应用程序扩展是一种功能，可以在 Azure 的公共地区和服务应用程序的环境中启用。 但是，推荐采用的策略是使用应用程序服务环境利用其高级功能以及更大容量的池。  

请按照以下步骤来配置用于您的应用程序宿主的密度较高︰

1. 配置应用程序服务环境，然后选择致力于高密度承载方案辅助池。

1. 创建一个应用程序服务计划，并扩展它要在辅助池上使用所有可用的产能。

1. 应用程序服务计划，每个网站的缩放标志设置为 true。

1. 新网站创建和分配给应用程序服务计划的**numberOfWorkers**属性设为**1**。 采用这种配置会产生最高的密度可能在此辅助池。

1. 每个站点授予更多的资源，根据需要可以单独配置的工作人员数量。 例如，高使用站点可能设置为**3** ，以便有更多的处理能力，为该应用程序，而使用率较低的站点会将**numberOfWorkers**设置为**1**的**numberOfWorkers** 。
