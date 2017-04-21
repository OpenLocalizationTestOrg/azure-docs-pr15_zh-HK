<properties 
    pageTitle="数据工厂中的使用资源管理器模板 |Microsoft Azure" 
    description="了解如何创建和使用 Azure 资源管理器模板来创建数据工厂实体。" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>使用模板来创建实体 Azure 数据工厂

## <a name="overview"></a>概述
同时使用 Azure 数据工厂的数据集成的需要，您可能会发现自己在不同的环境或实施同一解决方案中反复执行同一任务中重用相同的模式。 模板帮助您实施和轻松的方式管理这些方案。 Azure 数据工厂模板仅适用于涉及可重用性和重复的方案。
 
请考虑组织，遍及世界各地有 10 个生产厂的情况。 每个工厂中的日志存储在单独的内部部署 SQL Server 数据库中。 该公司想要构建云特别分析，在一个数据仓库。 它还想要具有相同的逻辑，但不同的开发、 测试和生产环境的配置。 

在这种情况下，任务需要重复在同一环境中，但使用不同的值之间的 10 个数据工厂的每个制造工厂。 实际上，**重复**是存在的。 模板允许的抽象化的这个一般流量 （即管道中每个数据工厂有相同的活动），但对每个生产工厂使用一个单独的参数文件。

此外，因为该组织要在不同的环境中部署这些 10 个数据工厂多次，模板可以通过利用开发、 测试和生产环境的单独的参数文件使用此**可重用性**。

## <a name="templating-with-azure-resource-manager"></a>模板化与 Azure 资源管理器
[Azure 资源管理器模板](../azure-resource-manager/resource-group-overview.md#template-deployment)是实现 Azure 数据工厂中的模板化的好方法。 资源管理器模板通过 JSON 文件定义的基础结构和 Azure 解决方案配置。 Azure 资源管理器模板处理所有/大多数 Azure 服务，因为它可以广泛用于轻松地管理 Azure 资产的所有资源。 请参阅[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)若要了解有关资源管理器模板一般。 

## <a name="tutorials"></a>教程
下面的教程，若要通过使用资源管理器的模板创建数据工厂实体的分步说明，请参阅︰

- [教程︰ 创建管线使用 Azure 资源管理器模板中复制数据](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [教程︰ 创建用于处理数据管道使用 Azure 资源管理器模板](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>数据工厂在 Github 上的模板
签出在 Github 上以下的 Azure 快速启动模板︰ 

- [创建将从 Azure Blob 存储的数据复制到 SQL Azure 数据库的数据工厂](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [在 Azure HDInsight 群集配置单元活动创建数据工厂](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [创建将从队伍的数据复制到 Azure Blob 数据工厂](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

随意共享 Azure 数据工厂在[Azure 快速启动](https://azure.microsoft.com/documentation/templates/)模板。 同时开发可通过此存储库中共享的模板指[贡献指南](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE)。 

以下各节提供了有关资源管理器模板中定义数据工厂资源的详细信息。 

## <a name="defining-data-factory-resources-in-templates"></a>在模板中定义数据工厂资源
用于定义数据工厂的顶级模板是︰

    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
    {
        "name": "[parameters('dataFactoryName')]",
        "apiVersion": "[variables('apiVersion')]",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "westus",
        "resources": [
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>定义数据工厂

下面的示例中所示，资源管理器模板中定义数据工厂︰

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

DataFactoryName 在"变量"中定义如下︰

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>定义链接的服务 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


有关您要部署的特定链接服务的 JSON 属性的详细信息，请参阅[存储链接服务](data-factory-azure-blob-connector.md#azure-storage-linked-service)或[计算链接服务](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 "取决于"参数指定相应的数据工厂的名称。 在下面的 JSON 定义显示定义 Azure 存储的链接的服务的示例︰

### <a name="define-datasets"></a>定义数据集

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

有关您要部署的特定的数据集类型的 JSON 属性的详细信息，请参阅[受支持的数据存储区](data-factory-data-movement-activities.md#supported-data-stores-and-formats)。 注意"取决于"参数指定的名称对应的数据工厂和存储链接服务。 在下面的 JSON 定义显示定义的 Azure blob 存储的数据集类型的示例︰

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>定义管道

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

请参阅[定义管道](data-factory-create-pipelines.md#pipeline-json)的 JSON 属性，用于定义特定的管线和要部署的活动有关的详细信息。 请注意"取决于"参数指定的数据工厂的名称和任何对应链接服务或数据集。 以下的 JSON 段中显示管线将 Azure Blob 存储的数据复制到 SQL Azure 数据库的示例︰

    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('azureSqlLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "activities": [
        {
            "name": "CopyFromAzureBlobToAzureSQL",
            "description": "Copy data frm Azure blob to Azure SQL",
            "type": "Copy",
            "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
            ],
            "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
            ],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink",
                    "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "columnMappings": "Column0:FirstName,Column1:LastName"
                }
            },
            "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }
        ],
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>参数化数据工厂模板
在参数化，请参阅[最佳实践来创建 Azure 资源管理器模板](../resource-manager-template-best-practices.md#parameters)文章的最佳做法。 一般情况下，参数的用法应该是最小化，尤其是可以改为使用变量。 仅提供以下方案中的参数︰

- 设置随环境而变化 (示例︰ 开发、 测试和生产)
- 机密信息 （例如密码）

如果您需要从[Azure 密钥存储库](../key-vault/key-vault-get-started.md)中提取秘密部署 Azure 数据工厂实体使用模板时，，指定的**密钥存储库**和**密钥名称**如下面的示例所示︰

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] 导出模板的现有数据工厂当前尚不支持，这是在工作。 


