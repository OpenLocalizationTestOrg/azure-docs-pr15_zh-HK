<properties
    pageTitle="构建您的第一个数据工厂 （资源管理器模板） |Microsoft Azure"
    description="在本教程中，您将创建使用 Azure 资源管理器模板示例 Azure 数据工厂管线。"
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
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>教程︰ 构建第一个 Azure 数据工厂使用 Azure 资源管理器模板
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-build-your-first-pipeline.md)
- [Azure 门户](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [资源管理器模板](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API，](data-factory-build-your-first-pipeline-using-rest-api.md)

在本文中，您使用 Azure 资源管理器模板来创建您的第一个 Azure 数据工厂。

## <a name="prerequisites"></a>系统必备组件
- [教程概述](data-factory-build-your-first-pipeline.md)文章阅读，完成**先决条件**步骤。
- 按照[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)文章中的说明进行操作，您的计算机上安装最新版本的 Azure PowerShell。
- 请参阅以了解 Azure 资源管理器模板[创作 Azure 资源管理器模板](../resource-group-authoring-templates.md)。 

## <a name="in-this-tutorial"></a>在本教程中
实体 | 说明  
------ | ----------- 
Azure 存储链接服务 | 链接到数据工厂 Azure 存储帐户。 Azure 存储帐户在此示例中包含管道的输入和输出数据。 
HDInsight 拨号链接的服务| 数据工厂到群集的点播 HDInsight 的链接。 群集将自动为您创建用于处理数据并处理完成后删除。
Azure Blob 输入数据集 | 引用到 Azure 存储链接服务。 链接的服务指的是 Azure 存储帐户和 Azure Blob 数据集指定容器、 文件夹和文件的名称保存输入的数据存储区中。 
Azure Blob 输出数据集 | 引用到 Azure 存储链接服务。 链接的服务指的是 Azure 存储帐户和 Azure Blob 数据集包含的输出数据存储中指定容器、 文件夹和文件的名称。 
数据管道 | 管道有一个 HDInsightHive 类型的活动所消耗的输入数据集，并生成输出数据集。   

数据工厂可以有一个或多个管道。 管道可以有一个或多个活动的。 有两种类型的活动︰[数据移动活动](data-factory-data-movement-activities.md)和[数据转换活动](data-factory-data-transformation-activities.md)。 在本教程中，您创建管道与一个活动 （复制活动）。

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

创建一个名为**ADFTutorialARM.json** **C:\ADFGetStarted**文件夹使用以下内容中的 JSON 文件︰

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
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
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
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
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
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
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] 可以查找模板资源管理器的另一个示例创建 Azure 数据工厂在[教程︰ 使用 Azure 资源管理器模板复制活动与创建管线](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)。  

## <a name="parameters-json"></a>JSON 的参数 
创建一个名为**ADFTutorialARM Parameters.json** ，其中包含的 Azure 资源管理器模板参数的 JSON 文件。  

> [AZURE.IMPORTANT] 在此参数文件中指定的名称和 Azure 存储帐户的**storageAccountName**和**storageAccountKey**参数的键。 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] 您可能必须将 JSON 文件单独的参数用于开发、 测试和生产环境，您可以使用同一个工厂 JSON 数据模板。 通过使用电源外壳脚本，您可以自动在这些环境中的部署数据工厂实体。 

## <a name="create-data-factory"></a>创建数据工厂

1. 启动**Azure PowerShell**并且运行下面的命令︰ 
    - 运行`Login-AzureRmAccount`，输入的用户名称和密码用于登录到 Azure 的门户。  
    - 运行`Get-AzureRmSubscription`来查看该帐户的所有订阅。
    - 运行`Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext`来选择您想要使用的订阅。 此订阅应在 Azure 门户使用相同。
1. 运行下面的命令来部署数据工厂实体使用资源管理器模板在步骤 1 中创建。 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>监视器管线
 
1.  之后登录到[Azure 门户](https://portal.azure.com/)，单击**浏览**，然后选择**数据工厂**。
        ![浏览-> 数据工厂](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  在**数据工厂**刀片式服务器，请单击创建数据工厂 (**TutorialFactoryARM**)。   
2.  在数据工厂**数据工厂**刀片式服务器，单击**图表**。
        ![平铺关系图](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  在**关系图视图**中，您将看到概述的管线，以及在本教程中使用的数据集。
    
    ![关系图视图](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. 在关系图视图中，双击数据集**AzureBlobOutput**。 您将看到当前正在处理的扇区。

    ![数据集](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. 当处理完成后时，您会看到**就绪**状态中的扇区。 创建一个按需 HDInsight 群集通常需要一段时间 （大约 20 分钟）。 因此，预计要花**大约 30 分钟**来处理切片的管线。

    ![数据集](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. 当切片处于**就绪**状态时，检查**adfgetstarted**的输出数据的 blob 存储在容器中的**partitioneddata**文件夹。  

您在本教程中创建的说明如何使用 Azure 门户刀片来监视管道和数据集，请参阅[监视数据集和管道](data-factory-monitor-manage-pipelines.md)。

您还可以使用监视和管理应用程序来监视数据管道。 请参阅[显示器和管理 Azure 数据工厂管线使用监视应用程序](data-factory-monitor-manage-app.md)有关使用应用程序的详细信息。 

> [AZURE.IMPORTANT] 成功处理切片后，输入的文件被删除。 因此，如果您要重新运行该切片或再次执行该教程，将输入的文件 (input.log) 上载到 adfgetstarted 容器的 inputdata 文件夹中。

## <a name="data-factory-entities-in-the-template"></a>在模板中的数据工厂实体
### <a name="define-data-factory"></a>定义数据工厂
下面的示例中所示，资源管理器模板中定义数据工厂︰  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName 被指︰ 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

它是唯一字符串基于资源组 id。  

### <a name="defining-data-factory-entities"></a>定义实体数据工厂
JSON 模板中定义了以下的数据工厂实体︰ 

- [Azure 存储链接服务](#azure-storage-linked-service)
- [HDInsight 拨号链接的服务](#hdinsight-on-demand-linked-service)
- [Azure blob 输入数据集](#azure-blob-input-dataset)
- [Azure blob 输出数据集](#azure-blob-output-dataset)
- [数据管道与复制活动](#data-pipeline)

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

**连接字符串**中使用的 storageAccountName 和 storageAccountKey 参数。 通过使用配置文件传递这些参数的值。 该定义还使用变量︰ 在模板中定义的 azureStroageLinkedService 和 dataFactoryName。 
    
#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight 拨号链接的服务
请参阅[计算链接的服务](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)文章有关 JSON 属性用于定义 HDInsight 拨号链接的服务的详细信息。  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

请注意以下几点︰ 

- 数据工厂创建**基于 Windows 的**HDInsight 群集与上面的 JSON。 您也可以让它创建一个**基于 Linux 的**HDInsight 群集。 有关详细信息，请参阅[按需 HDInsight 链接服务](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 
- 您可以使用**HDInsight 群集**，而不是使用按需 HDInsight 群集。 有关详细信息，请参阅[HDInsight 链接的服务](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)。
- HDInsight 群集指定 json 格式 (**linkedServiceName**) 的 blob 存储区中创建一个**默认容器**。 删除群集时，HDInsight 不会删除该容器。 这种现象是设计使然。 与按需 HDInsight 链接服务，每次需要除非已有处理切片创建群集 HDInsight live 群集 (**timeToLive**) 并完成处理后删除。

    处理多个切片，您会看到在 Azure blob 存储许多容器。 如果排除故障的作业不需要它们，可能要删除它们以降低存储成本。 这些容器的名称遵循某一模式:"adf**yourdatafactoryname**-**linkedservicename**datetimestamp"。 使用[Microsoft 存储浏览器](http://storageexplorer.com/)这样的工具删除在 Azure blob 存储容器。

有关详细信息，请参阅[按需 HDInsight 链接服务](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。



#### <a name="azure-blob-input-dataset"></a>Azure blob 输入数据集
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
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

此定义使用下列参数模板中定义的参数︰ blobContainer，inputBlobFolder，和 inputBlobName。 

#### <a name="azure-blob-output-dataset"></a>Azure Blob 输出数据集
您指定的 blob 容器和保存的输出数据的文件夹的名称。 有关 JSON 属性用于定义 Azure Blob 数据集的详细信息，请参阅[Azure Blob 数据集属性](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)。  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

此定义使用下列参数模板中定义的参数︰ blobContainer 和 outputBlobFolder。 

#### <a name="data-pipeline"></a>数据管道
定义通过点播 Azure HDInsight 群集上运行配置单元脚本转换数据管道。 JSON 元素用来定义管道在此示例中的说明，请参阅[管道 JSON](data-factory-create-pipelines.md#pipeline-json) 。 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>重复使用的模板 
在本教程中，您创建了一个模板用于定义数据工厂实体和传递参数的值的模板。 若要使用相同的模板将数据工厂实体部署到不同的环境，创建为每个环境参数文件，并使用它时部署到该环境。     

示例︰  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

注意第一个命令使用的开发环境、 测试环境，第二个和生产环境的第三个参数文件。  

您还可以重用模板来执行重复的任务。 例如，您需要创建许多数据工厂实现相同逻辑的一个或多个管道，但每个数据工厂使用不同 Azure 存储和 SQL Azure 数据库帐户。 在这种情况下，您使用相同的模板，在相同环境 （开发、 测试或生产） 中使用不同的参数文件创建数据工厂。 

## <a name="resource-manager-template-for-creating-a-gateway"></a>资源管理器创建网关的模板
下面是一个示例资源管理器模板在后面创建逻辑的网关。 在本地计算机或 Azure IaaS VM 安装网关和注册使用密钥数据工厂服务网关。 有关详细信息，请参阅[移动内部和云之间的数据](data-factory-move-data-between-onprem-and-cloud.md)。

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

此模板可创建名为的网关名称 GatewayUsingArmDF 的数据工厂︰ GatewayUsingARM。 

## <a name="see-also"></a>请参见
| 主题 | 说明 |
| :---- | :---- |
| [数据转换活动](data-factory-data-transformation-activities.md) | 这篇文章列出了数据转换活动 （如您在本教程中使用的 HDInsight 配置单元转换） 的 Azure 数据工厂支持。 |
| [安排和执行](data-factory-scheduling-and-execution.md) | 本文介绍了 Azure 数据工厂应用程序模型的调度和执行方面。 |
| [管道](data-factory-create-pipelines.md) | 这篇文章将帮助您了解管线和 Azure 数据工厂，以及如何使用它们来构建端到端方案或业务数据驱动的工作流中的活动。 |
| [数据集](data-factory-create-datasets.md) | 这篇文章将帮助您了解在 Azure 数据工厂的数据集。
| [监视和管理管道使用监视应用程序](data-factory-monitor-manage-app.md) | 本文介绍了如何监视、 管理和调试使用监视和管理应用程序的管道。 

  

