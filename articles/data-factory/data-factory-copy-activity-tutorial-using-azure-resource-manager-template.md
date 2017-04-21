<properties
    pageTitle="教程︰ 创建使用资源管理器模板的管道 |Microsoft Azure"
    description="在本教程中，您创建 Azure 数据工厂管道与复制活动使用 Azure 资源管理器模板。"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>教程︰ 创建管线与使用 Azure 资源管理器模板复制活动
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [复制向导](data-factory-copy-data-wizard-tutorial.md)
- [Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure 的资源管理器模板](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API，](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


本教程演示如何创建并监控使用 Azure 资源管理器模板 Azure 数据工厂。 数据工厂中的管道将从 Azure Blob 存储的数据复制到 SQL Azure 数据库。

## <a name="prerequisites"></a>系统必备组件
- 经过[教程概述和系统必备组件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)，并完成**先决条件**步骤。
- 按照[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)文章中的说明进行操作，您的计算机上安装最新版本的 Azure PowerShell。 在本教程中，您使用 PowerShell 部署数据工厂实体。 
- （可选）请参阅以了解 Azure 资源管理器模板[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)。


## <a name="in-this-tutorial"></a>在本教程中

在本教程中，您将创建数据工厂与以下数据工厂实体︰

实体 | 说明  
------ | ----------- 
Azure 存储链接服务 | 链接到数据工厂 Azure 存储帐户。 Azure 存储源数据存储区，并且 SQL Azure 数据库复制活动接收器数据存储区教程中。 它指定包含复制活动的输入的数据的存储帐户。 
Azure 的 SQL 数据库链接服务| 链接到数据工厂的 SQL Azure 数据库。 它指定 SQL Azure 数据库包含输出数据的复制活动。 
Azure Blob 输入数据集 | 引用到 Azure 存储链接服务。 链接的服务指的是 Azure 存储帐户和 Azure Blob 数据集指定容器、 文件夹和文件的名称保存输入的数据存储区中。 
Azure 的 SQL 输出数据集 | 是指链接的 Azure SQL 服务。 链接的 Azure SQL 服务指的是 Azure SQL 服务器和 SQL Azure 数据集指定包含输出数据的表的名称。 
数据管道 | 管道有一个活动类型的复制采用 Azure blob 数据集作为输入和 SQL Azure 数据集作为输出。 复制活动从 Azure 将数据复制到 SQL Azure 数据库的表中的 blob。  

数据工厂可以有一个或多个管道。 管道可以有一个或多个活动的。 有两种类型的活动︰[数据移动活动](data-factory-data-movement-activities.md)和[数据转换活动](data-factory-data-transformation-activities.md)。 在本教程中，您创建管道与一个活动 （复制活动）。

![将 Azure Blob 复制到 SQL Azure 数据库](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

以下部分提供完整资源管理器模板用于定义数据工厂实体，以便您可以快速运行本教程和测试模板。 要了解数据工厂的每个实体的方式定义，请参见[数据工厂在模板中的实体](#data-factory-entities-in-the-template)部分。

## <a name="data-factory-json-template"></a>数据工厂 JSON 模板
用于定义数据工厂的顶级资源管理器模板是︰ 

    {
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
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

创建一个名为**ADFCopyTutorialARM.json** **C:\ADFGetStarted**文件夹使用以下内容中的 JSON 文件︰


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
        },
        "resources": [
          {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
              {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
                }
              },
              {
                "type": "linkedservices",
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
                  }
                }
              },
              {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  }
                }
              },
              {
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
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>JSON 的参数 
创建一个名为**ADFCopyTutorialARM Parameters.json** ，其中包含的 Azure 资源管理器模板参数的 JSON 文件。 

> [AZURE.IMPORTANT] 指定的名称和 Azure 存储帐户的**storageAccountName**和**storageAccountKey**参数的键。  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] 您可能必须将 JSON 文件单独的参数用于开发、 测试和生产环境，您可以使用同一个工厂 JSON 数据模板。 通过使用电源外壳脚本，您可以自动在这些环境中的部署数据工厂实体。  

## <a name="create-data-factory"></a>创建数据工厂
1. 启动**Azure PowerShell**并且运行下面的命令︰
    - 运行`Login-AzureRmAccount`，输入的用户名称和密码用于登录到 Azure 的门户。  
    - 运行`Get-AzureRmSubscription`来查看该帐户的所有订阅。
    - 运行`Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext`来选择您想要使用的订阅。 
2. 运行下面的命令来部署数据工厂实体使用资源管理器模板在步骤 1 中创建。

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>监视器管线
1. 登录到[Azure 门户](https://portal.azure.com)使用 Azure 帐户。
2. 在左侧的菜单上单击**数据工厂**（或） 单击**更多的服务**，单击**智能 + 分析**分类下的**数据工厂**。

    ![数据工厂菜单](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. 在**工厂的数据**页中，搜索并查找您的数据工厂。 

    ![搜索数据工厂](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. 单击 Azure 数据工厂。 您将看到主页的数据工厂。

    ![数据工厂的主页上](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. 单击**关系图**图块以查看数据工厂的关系图视图。

    ![关系图视图的数据工厂](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. 在关系图视图中，双击数据集**SQLOutputDataset**。 看片的状态。 当复制操作完成后时，您的状态设置为**就绪**。

    ![处于就绪状态的输出切片](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. 当切片处于**就绪**状态时，验证数据被复制到了**emp**表的 SQL Azure 数据库中。

您在本教程中创建的说明如何使用 Azure 门户刀片来监视管道和数据集，请参阅[监视数据集和管道](data-factory-monitor-manage-pipelines.md)。

您还可以使用监视和管理应用程序来监视数据管道。 请参阅[显示器和管理 Azure 数据工厂管线使用监视应用程序](data-factory-monitor-manage-app.md)有关使用应用程序的详细信息。


## <a name="data-factory-entities-in-the-template"></a>在模板中的数据工厂实体

### <a name="define-data-factory"></a>定义数据工厂
下面的示例中所示的资源管理器模板中定义的数据工厂︰  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName 被指︰ 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

它是唯一的字符串根据资源组 id。  

### <a name="defining-data-factory-entities"></a>定义实体数据工厂
JSON 模板中定义了以下的数据工厂实体︰ 

1. [Azure 存储链接服务](#azure-storage-linked-service)
2. [Azure SQL 链接服务](#azure-sql-database-linked-service)
3. [Azure blob 数据集](#azure-blob-dataset)
4. [Azure SQL 数据集](#azure-sql-dataset)
5. [数据管道与复制活动](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure 存储链接服务
在此部分中指定的名称和键的 Azure 存储帐户。 有关用于定义 Azure 存储链接服务的 JSON 属性的详细信息，请参阅[Azure 存储链接服务](data-factory-azure-blob-connector.md#azure-storage-linked-service)。 

    {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureStorage",
            "description": "Azure Storage linked service",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
            }
        }
    }

连接字符串中使用的 storageAccountName 和 storageAccountKey 参数。 通过使用配置文件传递这些参数的值。 该定义还使用变量︰ 在模板中定义的 azureStroageLinkedService 和 dataFactoryName。 
    
#### <a name="azure-sql-database-linked-service"></a>Azure 的 SQL 数据库链接服务
在此部分中指定 SQL Azure 服务器名称、 数据库名称、 用户名和用户密码。 有关用于定义 Azure SQL 链接服务的 JSON 属性的详细信息，请参阅[SQL Azure 链接服务](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)。  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

连接字符串将使用 sqlServerName、 数据库名称、 sqlServerUserName 和 sqlServerPassword 参数，其值通过使用配置文件。 该定义还使用下列模板中的变量︰ azureSqlLinkedServiceName，dataFactoryName。

#### <a name="azure-blob-dataset"></a>Azure blob 数据集
您指定的 blob 容器、 文件夹以及包含输入的数据的文件的名称。 有关 JSON 属性用于定义 Azure Blob 数据集的详细信息，请参阅[Azure Blob 数据集属性](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)。 


    {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL 数据集
您持有从 Azure Blob 存储复制的数据的 SQL Azure 数据库中指定的表的名称。 有关 JSON 属性用于定义 SQL Azure 数据集的详细信息，请参阅[SQL Azure 数据集属性](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)。 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>数据管道
定义将 Azure blob 数据集的数据复制到 SQL Azure 数据集的管道。 JSON 元素用来定义管道在此示例中的说明，请参阅[管道 JSON](data-factory-create-pipelines.md#pipeline-json) 。 

    {
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
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>重复使用的模板 
在本教程中，您创建了一个模板用于定义数据工厂实体和传递参数的值的模板。 该管道将数据复制到指定的参数通过 SQL Azure 数据库 Azure 存储帐户。 若要使用相同的模板将数据工厂实体部署到不同的环境，创建为每个环境参数文件，并使用它时部署到该环境。     

示例︰  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

注意第一个命令使用的开发环境、 测试环境，第二个和生产环境的第三个参数文件。  

您还可以重用模板来执行重复的任务。 例如，您需要创建许多数据工厂实现相同逻辑的一个或多个管道，但每个数据工厂使用不同 Azure 存储和 SQL Azure 数据库帐户。 在这种情况下，您使用相同的模板，在相同环境 （开发、 测试或生产） 中使用不同的参数文件创建数据工厂。   

