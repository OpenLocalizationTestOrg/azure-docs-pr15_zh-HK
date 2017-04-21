<properties
   pageTitle="HDInsight 使用 Azure 资源管理器模板在创建基于 Windows 的 Hadoop 群集 |Microsoft Azure"
    description="了解如何为 Azure HDInsight 使用 Azure 资源管理器模板创建群集。"
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>HDInsight 使用 Azure 资源管理器模板在创建基于 Windows 的 Hadoop 群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

了解如何创建使用 Azure 资源管理器模板 HDInsight 群集。 有关详细信息，请参阅[部署了 Azure 资源管理器模板的应用程序](../resource-group-template-deploy.md)。 其他群集创建工具和功能单击此页面顶部选项卡中的选择或者查看[群集创建方法](hdinsight-provision-clusters.md#cluster-creation-methods)。

##<a name="prerequisites"></a>系统必备组件︰

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


在开始这篇文章中的说明操作之前，必须具有下列︰

- [Azure 的订阅](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- Azure PowerShell 或 Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>资源管理器模板

资源管理器模板轻松地为您的应用程序在单一的协调操作中创建 HDInsight 群集、 及其依赖的资源 （如默认存储帐户） 和其他资源 （例如，要使用 Apache Sqoop Azure SQL 数据库）。 在模板中，您定义应用程序所需的资源，并指定部署参数输入不同的环境的值。 模板包含 JSON 和您可以使用它来构建您的部署的值的表达式。

用于创建 HDInsight 群集和从属的 Azure 存储帐户的资源管理器模板可在[附录 A](#appx-a-arm-template)。 使用文本编辑器将该模板保存到您的工作站上的文件。 您将学习如何使用各种工具的模板调用。

有关资源管理器模板的详细信息，请参阅

- [作者 Azure 资源管理器模板](../resource-group-authoring-templates.md)
- [将与 Azure 资源管理器模板的应用程序部署](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>使用 PowerShell 部署

下面的过程创建一个 HDInsight 的群集。

**部署群集使用资源管理器模板**

1. 在[附录 A](#appx-a-arm-template)中 json 文件保存到您的工作站。
2. 如果需要请设置参数。
3. 运行使用以下 PowerShell 脚本的模板︰

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    PowerShell 脚本仅配置群集名称和存储帐户名称。  在资源管理器模板中，您可以设置其他值。 
    
有关详细信息，请参阅[使用 PowerShell 部署](../resource-group-template-deploy.md#deploy-with-powershell)。

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 部署

下面的示例通过调用资源管理器模板来创建群集及其从属存储帐户和容器︰

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>部署时使用 REST API，

请参阅[使用 REST API 部署](../resource-group-template-deploy.md#deploy-with-the-rest-api)。

## <a name="deploy-with-visual-studio"></a>使用 Visual Studio 的部署

使用 Visual Studio 中，可以创建资源组项目，并通过用户界面将其部署到 Azure。 选择要包括在项目中的资源的类型，这些资源将自动添加到资源管理器模板。 该项目还提供了 PowerShell 脚本以部署模板。

资源组使用 Visual Studio 的简介，请参见[创建和部署通过 Visual Studio Azure 的资源组](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)。

##<a name="next-steps"></a>下一步行动
在本文中，您学习了几种方法来创建一个 HDInsight 的群集。 若要了解详细信息，请参阅下列文章︰


- 通过.NET 客户端库的资源部署的示例，请参见[部署资源使用.NET 库和模板](../virtual-machines/virtual-machines-windows-csharp-template.md)。
- 部署应用程序的详细示例，请参见[规定部署预知在 Azure 中的 microservices 和](../app-service-web/app-service-deploy-complex-application-predictably.md)。
- 将解决方案部署到不同的环境的指导，请参见[开发和测试环境中 Microsoft Azure](../solution-dev-test-environments.md)。
- 若要了解详细信息的 Azure 资源管理器模板部分，请参阅[创作模板](../resource-group-authoring-templates.md)。
- 您可以在 Azure 资源管理器模板中使用的函数的列表，请参阅[模板函数](../resource-group-template-functions.md)。



##<a name="appx-a-resource-manager-template"></a>Appx 答︰ 资源管理器模板

以下的 Azure 资源管理器模板创建基于 Windows 的 Hadoop 群集与从属 Azure 存储帐户。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

