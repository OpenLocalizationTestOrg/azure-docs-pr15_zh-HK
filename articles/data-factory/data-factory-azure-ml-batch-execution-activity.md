<properties 
    pageTitle="使用机器学习活动 |Microsoft Azure" 
    description="描述如何创建创建预测管道使用 Azure 数据工厂和 Azure 机器学习" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>创建预测管道使用 Azure 机器学习活动   
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)

## <a name="introduction"></a>介绍

[Azure 机器学习](https://azure.microsoft.com/documentation/services/machine-learning/)使您能够生成、 测试和部署预测分析的解决方案。 从高层次的角度来看，它是完成三个步骤︰ 

1. **创建培训实验**。 您可以通过使用 Azure ML Studio 执行此步骤。 ML studio 是协作可视化开发环境，用于训练和测试使用培训数据的预测分析模型。
2. **将其转换为预测实验**。 一旦您的模型已经过培训与包含现有数据，您就可以使用它来成绩新数据准备并优化实验进行评分。
3. **将其部署为 web 服务**。 作为 Azure 的 web 服务，您可以发布计分实验。 您可以将数据发送到您通过此 web 服务终结点的模型并接收有关模型的预测结果。  

Azure 数据工厂使您能够轻松地创建使用已发布的[Azure 机器学习]的管线[azure-machine-learning]web 服务的预测分析。 请参阅[Azure 数据工厂简介](data-factory-introduction.md)和[创建您的第一个渠道](data-factory-build-your-first-pipeline.md)快速入门 Azure 数据工厂服务的文章。 

在 Azure 数据工厂管道中使用**批处理执行活动**，您可以调用批处理中的数据进行预测的 Azure ML web 服务。 有关详细信息，请参见[调用 Azure ML 的 web 服务使用批处理执行活动](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity)部分。

随着时间的推移中评分试验 Azure ML 的预测模型需要重新训练使用新输入的数据集。 可以通过执行以下步骤来重新培训数据工厂管线从 Azure ML 模型︰ 

1. 作为 web 服务发布培训实验 （不预测实验）。 确实要为前面的方案中的 web 服务公开预测实验 Azure ML Studio 中的此步骤。
2. 使用 Azure ML 批处理执行活动来调用 web 服务的培训试验。 基本上，Azure ML 批处理执行活动可用于调用 web 服务的培训和计分的 web 服务。 
  
已完成再培训之后，要用新培训的模型更新计分的 web 服务 （作为 web 服务公开的预测实验）。 步骤如下︰ 

1. 将非默认终结点添加到记分的 web 服务。 不能更新默认终结点的 web 服务，因此您需要创建非默认终结点使用 Azure 的门户。 请参阅有关的概念信息和过程步骤[创建终结点](../machine-learning/machine-learning-create-endpoint.md)文章。
2. 更新现有的 Azure ML 链接服务的评分，以使用非默认终结点。 开始使用新的终结点以使用已更新的 web 服务。
3. 使用**Azure ML 更新资源活动**与培训新的模型更新 web 服务。  

有关详细信息请参见[更新 Azure ML 模型使用更新资源活动](#updating-azure-ml-models-using-the-update-resource-activity)部分。 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>调用 web 服务使用批处理执行活动

您使用 Azure 数据工厂协调数据移动和处理，然后再执行批处理执行使用 Azure 机器学习。 这里是顶级的步骤︰

1. 创建链接的 Azure 机器学习服务。 您需要以下各项︰
    1. 对批处理执行 API 的**请求 URI** 。 可以通过单击 web 服务页中的**批处理执行**链接来查找请求 URI。
    1. 已发布 Azure 机器学习的 web 服务的**API 密钥**。 可以通过单击您已发布的 web 服务来查找 API 密钥。 
 2. 使用**AzureMLBatchExecution**活动。

    ![机器学习仪表板](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![批处理的 URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>方案︰ 试验使用 Web 服务的输入/输出到 Azure Blob 存储中的数据引用
在这种情况下，Azure 机器学习 Web 服务使使用 Azure 的 blob 存储在文件中的数据的预测，将预测结果存储 blob 存储在。 下面的 JSON 定义数据工厂管线与 AzureMLBatchExecution 活动。 活动有数据集作为输入的**DecisionTreeInputBlob**和**DecisionTreeResultBlob**作为输出。 **DecisionTreeInputBlob**使用**webServiceInput** JSON 属性传递作为 web 服务的输入。 **DecisionTreeResultBlob**作为输出到 Web 服务传递 JSON 使用**webServiceOutputs** 。  

> [AZURE.IMPORTANT] 
> 如果 web 服务接受多个输入，而不是使用**webServiceInput**使用**webServiceInputs**属性。 请参阅使用 webServiceInputs 属性的示例[Web 服务需要多个输入](#web-service-requires-multiple-inputs)部分。
>  
> 数据集所引用的**webServiceInput**/**webServiceInputs**和**webServiceOutputs**属性 （ **typeProperties**) 还必须包括在活动**的输入**和**输出**。
> 
> 在 Azure ML 实验，web 服务的输入和输出端口和全局参数具有可自定义的默认名称 （"输入 1"，"输入 2"）。 用于 webServiceInputs、 webServiceOutputs 和 globalParameters 设置的名称必须完全匹配试验中的名称。 您可以在批处理执行帮助页上以验证预期的映射您 Azure ML 终结点查看示例请求负载。 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] 唯一的输入和输出的 AzureMLBatchExecution 活动可以被作为参数传递给 Web 服务。 例如，在上面的 JSON 段中，DecisionTreeInputBlob 是 AzureMLBatchExecution 活动，作为输入传递给 Web 服务通过 webServiceInput 参数的输入。   

### <a name="example"></a>示例

此示例使用 Azure 存储来保存输入和输出数据。 

我们建议通过[创建数据工厂与您第一个渠道]转[adf-build-1st-pipeline]之前通过本例教程。 数据工厂编辑器用于创建此示例中的数据工厂项目 （链接的服务、 数据集、 管道）。   
 

1. 为您的**Azure 存储**创建**链接的服务**。 如果输入和输出文件在不同的存储帐户，您需要两个链接的服务。 这里是一个 JSON 的示例︰

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. 创建**输入**Azure 数据工厂**数据集**。 与某些其他数据工厂数据集，这些数据集必须包含**采用文件夹路径**和**文件名**的值。 可以使用分区，以使每个批处理执行 （每个数据片） 来处理或产生唯一的输入和输出文件。 您可能需要包括一些上游活动将输入转换为 CSV 文件的格式并将其放置在每个层切面的存储帐户。 在这种情况下，不会包含在下面的示例中，显示**外部**和**externalData**设置，并且您 DecisionTreeInputBlob 会输出数据集的其他活动。

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    您输入的 csv 文件必须具有列标题行。 如果您使用**复制活动**创建/移动 csv 到 blob 存储的则应设置为**true**的接收器属性**blobWriterAddHeader** 。 例如︰
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    如果 csv 文件没有标题行，您可能会看到以下错误︰**活动中的错误︰ 读取字符串时出错。意外的标记︰ StartObject。路径，行 1，将 1**。
3. 创建**输出**Azure 数据工厂**数据集**。 此示例使用分区来创建每个扇区执行的唯一的输出路径。 没有分区，该活动将覆盖该文件。

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. 创建**链接的服务**的类型︰ **AzureMLLinkedService**，提供 API 键以及批处理执行 URL 创建模型。
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. 最后，作者包含**AzureMLBatchExecution**活动的管道。 在运行时，管道执行以下步骤︰
    1. 您输入的数据集中获取输入文件的位置。
    2. Azure 机器学习执行批处理 API 调用
    3. 将批处理执行输出复制到输出数据集中给定的 blob。 

    > [AZURE.NOTE] AzureMLBatchExecution 活动可以具有零个或多个输入和一个或多个输出。

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Start**和**end** datetimes 必须为[ISO 格式](http://en.wikipedia.org/wiki/ISO_8601)。 例如︰ 2014年-10-14T16:32:41Z。 **结束**时间是可选的。 如果不指定**end**属性的值，它将计算为"**开始 + 48 小时。**" 要无限期地运行管道，请指定**9999-09-09** **end**属性的值。 有关 JSON 属性的详细信息，请参阅[JSON 脚本参考](https://msdn.microsoft.com/library/dn835050.aspx)。

    > [AZURE.NOTE] 指定输入的 AzureMLBatchExecution 则是可选的活动。 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>方案︰ 试验使用读取器/编写器模块来指代各种存储中的数据

创建 Azure ML 试验时的另一个常见方案是使用读取器和编写器模块。 读卡器模块用于将数据加载到一个实验，该编写器模块是从实验过程中保存的数据。 读取器和编写器模块的详细信息，请在 MSDN 库参阅[读取器](https://msdn.microsoft.com/library/azure/dn905997.aspx)和[编写器](https://msdn.microsoft.com/library/azure/dn905984.aspx)的主题。     

使用读取器和编写器模块，则最好使用这些读取器/编写器模块的每个属性的 Web 服务参数。 这些 web 参数使您能够在运行时配置的值。 例如，使用 SQL Azure 数据库读取器模块开始创建实验︰ XXX.database.windows.net。 在部署 web 服务后，您要启用的 web 服务使用者，若要指定另一个称为 YYY.database.windows.net 的 Azure SQL Server。 可以使用 Web 服务参数以允许配置此值。

> [AZURE.NOTE] Web 服务的输入和输出是不同的 Web 服务参数。 在第一个方案中，您已经看到如何为 Azure ML Web 服务指定输入和输出。 在这种情况下，您将 Web 服务所对应的参数传递给属性的读取器/编写器模块。 

让我们看一下使用 Web 服务参数的一种方案。 已经部署了 Azure 机器学习 web 服务读取器模块用于从一个 Azure 机器学习所支持的数据源中读取数据 (例如︰ SQL Azure 数据库)。 执行批处理后，结果将写入使用编写器模块 （在 SQL Azure 的数据库中）。  在试验中定义任何 web 服务的输入和输出。 在这种情况下，我们建议您配置为读取器和编写器模块的相关 web 服务参数。 这种配置允许读取器/编写器模块配置时使用的 AzureMLBatchExecution 活动。 按如下所述活动 JSON 中**globalParameters**部分中指定 Web 服务参数。 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

此外可以使用[数据工厂函数](https://msdn.microsoft.com/library/dn835056.aspx)中传递值，如下面的示例中所示的 Web 服务参数︰

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Web 服务参数不区分大小写，因此请确保的名称指定如果在活动中 JSON 匹配的 Web 服务公开的。 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>使用读取器模块从 Azure Blob 中的多个文件中读取数据
大数据的活动，如猪的管线和配置单元会产生一个或多个输出文件没有扩展名。 例如，当您指定外部配置单元表，外部配置单元表的数据可以存储在与以下名称 000000_0 Azure blob 存储。 可以在实验中使用读卡器模块读取多个文件，并将其用于预测。 

在 Azure 机器学习实验中使用读卡器模块时，可以指定作为输入 Azure Blob。 在 Azure blob 存储的文件可以是输出文件 (示例︰ 000000_0)，由猪和配置单元脚本运行在 HDInsight 上产生的。 读卡器模块允许您通过配置读取扩展名的文件 （没有）**到容器中，路径目录/blob**。 **容器路径**指向容器和**目录/blob**指向文件夹包含的文件，如下面的图像所示。 星号，即\*)**指定的容器/文件夹中的所有文件 (即数据/aggregateddata/年 = 6-2014年/月 /\*)**阅读作为实验的一部分。

![Azure Blob 属性](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>示例 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>与 Web 服务参数与 AzureMLBatchExecution 活动管线

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
在上面的 JSON 示例︰

- 部署了 Azure 机器学习 Web 服务使用读取器和编写器模块为读/写数据从/到 Azure SQL 数据库。 此 Web 服务公开的以下四个参数︰ 数据库服务器名称、 数据库名称、 服务器用户帐户的名称，以及服务器用户帐户密码。  
- **Start**和**end** datetimes 必须为[ISO 格式](http://en.wikipedia.org/wiki/ISO_8601)。 例如︰ 2014年-10-14T16:32:41Z。 **结束**时间是可选的。 如果不指定**end**属性的值，它将计算为"**开始 + 48 小时。**" 要无限期地运行管道，请指定**9999-09-09** **end**属性的值。 有关 JSON 属性的详细信息，请参阅[JSON 脚本参考](https://msdn.microsoft.com/library/dn835050.aspx)。

### <a name="other-scenarios"></a>其他方案

#### <a name="web-service-requires-multiple-inputs"></a>Web 服务需要多个输入
如果 web 服务接受多个输入，而不是使用**webServiceInput**使用**webServiceInputs**属性。 **WebServiceInputs**所引用的数据集还必须包含在所活动的**输入**。
 
在 Azure ML 实验，web 服务的输入和输出端口和全局参数具有可自定义的默认名称 （"输入 1"，"输入 2"）。 用于 webServiceInputs、 webServiceOutputs 和 globalParameters 设置的名称必须完全匹配试验中的名称。 您可以在批处理执行帮助页上以验证预期的映射您 Azure ML 终结点查看示例请求负载。


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Web 服务不需要输入

Azure ML 批处理执行 web 服务可用于运行所有的工作流示例 R 或 Python 脚本，可能不需要任何输入。 或者，可能与不公开任何 GlobalParameters 的读卡器模块配置试验。 在这种情况下，AzureMLBatchExecution 活动将配置如下︰

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web 服务不需要输入/输出
Azure ML 批处理执行 web 服务可能没有配置任何 Web 服务的输出。 在此示例中，没有任何 Web 服务输入或输出，也不任何 GlobalParameters 配置。 仍然没有输出配置活动本身，但不是将其指定为 webServiceOutput。

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web 服务使用读取器和编写器，以及活动运行仅当其他活动已成功

Azure ML web 服务读取器和编写器模块可能配置为运行带有或不带任何 GlobalParameters。 但是，您可能需要在使用数据集的依赖项时要调用该服务仅完成一些上游处理管道中嵌入服务调用。 此外可以使用这种方法完成执行批处理后触发其他操作。 在这种情况下，可以快速使用活动的输入和输出，而无需命名其中的任何 Web 服务输入或输出的依赖关系。

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

**结论**是︰

-   如果实验端点使用 webServiceInput︰ 它由 blob 数据集，包含在所活动的输入和 webServiceInput 属性。 否则，webServiceInput 属性被忽略。 
-   如果实验端点使用 webServiceOutput(s)︰ 他们由 blob 数据集，并且在活动输出和在 webServiceOutputs 属性中包含。 活动将输出和 webServiceOutputs 映射的实验中的每个输出的名称。 否则，webServiceOutputs 属性被忽略。
-   如果实验终结点公开 globalParameter(s)，他们给出了活动 globalParameters 属性作为键，值对。 否则，globalParameters 属性被忽略。 密钥是区分大小写。 可能的值中使用[azure 数据工厂函数](data-factory-scheduling-and-execution.md#data-factory-functions-reference)。 
- 其他数据集可能包含在活动的输入和输出属性中，而不在活动 typeProperties 中引用。 这些数据集控制执行使用切片相关性，但 AzureMLBatchExecution 的活动，否则将忽略。 


## <a name="updating-models-using-update-resource-activity"></a>更新模型使用更新资源活动
随着时间的推移中评分试验 Azure ML 的预测模型需要重新训练使用新输入的数据集。 您已完成再培训之后，要使用 retrained 的 ML 模型更新计分的 web 服务。 若要启用的 web 服务通过再培训和更新 Azure ML 模型的典型步骤如下︰ 

1. 在[Azure ML Studio](https://studio.azureml.net)中创建一个实验。 
2. 当您感到满意模型时，使用 Azure ML 工作室发布 web 服务的两个**培训试验**和评分 /**预测性实验**。

下表描述了在本例中使用的 web 服务。  有关详细信息，请参阅[重新培训机器学习模型以编程方式](../machine-learning/machine-learning-retrain-models-programmatically.md)。

| Web 服务类型的 | 说明 
| :------------------ | :---------- 
| **培训 web 服务** | 接收的培训数据，并生成培训的模型。 再培训的输出是 Azure Blob 存储中的.ilearner 文件。  发布培训实验作为 web 服务时，会自动为您创建**默认终结点**。 您可以创建多个终结点，但该示例使用的默认终结点 |
| **计分的 web 服务** | 接收未标记的数据示例，并进行预测。 输出的预测可能有多种形式，例如.csv 文件或 SQL Azure 数据库，具体取决于配置的试验中的行。 发布预测实验作为 web 服务时，会自动为您创建默认终结点。 通过使用[Azure 门户](https://manage.windowsazure.com)创建第二个**非默认的和可更新的终结点**。 您可以创建多个终结点，但此示例使用一个非默认的可更新的终结点。 请参阅步骤[创建终结点](../machine-learning/machine-learning-create-endpoint.md)文章。       
 
下图描绘了培训和 Azure ML 的计分终结点之间的关系。 

![Web 服务](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


通过使用**Azure ML 批处理执行活动**可以调用**web 服务培训**。 调用 web 服务的培训是与评分数据的调用 （评分 web 服务） 的 Azure ML web 服务相同。 前面的几节介绍了如何调用详细 Azure 数据工厂管线从 Azure ML web 服务。 
  
可以通过使用**Azure ML 更新资源活动**与培训新的模型更新 web 服务调用**评分 web 服务**。 如上面的表中所述，必须创建并使用可更新的非默认终结点。 此外，更新数据工厂要使用非默认终结点，以便它们始终使用最新版本的 retrained 的模型中的任何现有链接的服务。 

下面的方案提供更多详细信息。 它有为再培训和更新从 Azure 数据工厂管道的 Azure ML 模型的一个示例。 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>方案︰ 重新进行培训和更新 Azure ML 模型
本部分提供示例管道使用**Azure ML 批处理执行活动**对一个模型进行重新培训。 管道还使用**Azure ML 更新资源活动**更新中计分的 web 服务的模型。 部分中还提供了 JSON 段所有链接的服务、 数据集和示例中的管道。 

下面是示例管道的关系图视图。 如您所见，Azure ML 批处理执行活动接受培训输入和输出培训 （iLearner 文件）。 Azure ML 更新资源活动采用此培训输出并更新计分的 web 服务终结点中的模型。 更新资源活动不会产生任何输出。 PlaceholderBlob 是只是伪输出数据集所需的 Azure 数据工厂服务运行管道。 

![管道图](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure Blob 存储链接的服务︰
Azure 存储包含以下数据︰

- 培训数据。 Azure ML 培训 web 服务输入的数据。  
- iLearner 文件。 Azure ML 培训 web 服务的输出。 该文件也是更新资源活动的输入。  
   
下面是服务的示例 JSON 定义的链接: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>培训输入数据集︰
以下数据集表示为 Azure ML 培训 web 服务的输入的训练数据。 Azure ML 批处理执行活动将此数据集作为输入。 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>培训输出数据集︰
以下数据集表示从 Azure ML 培训 web 服务输出 iLearner 文件。 Azure ML 批处理执行活动产生此数据集。 此数据集也是 Azure ML 更新资源活动的输入。

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Azure ML 培训端点的链接的服务 
下面的 JSON 段定义指向默认终结点的培训 web 服务链接的 Azure 机器学习服务。 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

在**Azure ML Studio**中，执行下列操作来获取**mlEndpoint**和**apiKey**的值︰

1. 单击左侧菜单上的**WEB 服务**。
2. 单击列表中的 web 服务的**web 服务的培训**。 
3. 单击复制**API 密钥**文本框旁边。 将密钥在剪贴板中粘贴到工厂 JSON 数据编辑器。
4. 在**Azure ML studio**中，单击**批处理执行**链接。
5. 从**请求**部分复制**请求 URI**并将其粘贴到编辑器中数据工厂 JSON。   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>链接为 Azure ML 更新计分终结点的的服务︰
下面的 JSON 段定义指向的计分的 web 服务终结点非默认更新链接的 Azure 机器学习服务。  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


创建和部署 Azure ML 链接服务之前，请按照[创建终结点](../machine-learning/machine-learning-create-endpoint.md)以创建第二个 （非默认和更新） 终结点为评分的 web 服务的文章中的步骤。

创建非默认更新终结点后，执行下列操作︰

- 单击以获取此 URI 的值**mlEndpoint** JSON 属性**执行批处理**。
- 单击**更新资源**链接，以获取此 URI 的值**updateResourceEndpoint** JSON 属性。 在终结点页本身 （位于右下角） 是 API 密钥。 

![可更新的终结点](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>占位符的输出数据集︰
Azure ML 更新资源活动不会生成任何输出。 但是，Azure 数据工厂需要一个输出数据集来推动管道的计划。 因此，我们在此示例中使用的占位符 dummy/数据集。  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>管道
管道有两个活动︰ **AzureMLBatchExecution**和**AzureMLUpdateResource**。 Azure ML 批处理执行活动采用培训数据作为输入，并生成 iLearner 文件作为输出。 活动的输入的训练数据调用培训 web 服务作为 web 服务公开 （培训实验），并从 web 服务接收的 ilearner 文件。 PlaceholderBlob 是只是伪输出数据集所需的 Azure 数据工厂服务运行管道。 

![管道图](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>读取器和编写器模块

使用 Web 服务参数的一种常见情况是使用 SQL Azure 的读者和作者。 读卡器模块用于将数据加载到 Azure 机器学习 Studio 之外的数据管理服务从实验。 编写器模块是从实验数据保存到 Azure 机器学习 Studio 之外的数据管理服务。  

Blob/Azure SQL Azure 读取器/编写器的详细信息，请在 MSDN 库参阅[读取器](https://msdn.microsoft.com/library/azure/dn905997.aspx)和[编写器](https://msdn.microsoft.com/library/azure/dn905984.aspx)的主题。 上一节中的示例使用 Azure Blob 读取器和编写器 Azure Blob。 本节讨论如何使用 SQL Azure 读卡器和 Azure SQL 编写器。


## <a name="frequently-asked-questions"></a>常见问题及的解答

**Q:**我有多个文件，都由我大的数据管道。 可以使用 AzureMLBatchExecution 活动来处理所有文件？

**A:**是的。 请参见**使用读卡器模块读取 Azure Blob 中的多个文件中的数据**的详细信息。 

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML 批评分活动
如果使用的**AzureMLBatchScoring**活动 Azure 机器学习与集成，我们建议使用最新的**AzureMLBatchExecution**活动。 

AzureMLBatchExecution 活动引入 Azure SDK 和 Azure PowerShell 的 8 月 2015年发行版中。

如果您想继续使用 AzureMLBatchScoring 活动，继续阅读过此部分。  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>输入/输出使用 Azure 存储 azure ML 批评分活动 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Web 服务参数
若要指定 Web 服务参数的值，添加到管线 JSON，如下面的示例中所示的**AzureMLBatchScoringActivty**部分**typeProperties**节︰ 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


此外可以使用[数据工厂函数](https://msdn.microsoft.com/library/dn835056.aspx)中传递值，如下面的示例中所示的 Web 服务参数︰

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Web 服务参数不区分大小写，因此请确保的名称指定如果在活动中 JSON 匹配的 Web 服务公开的。 

## <a name="see-also"></a>请参见

- [Azure 的博客文章︰ Azure 数据工厂和 Azure 机器学习入门](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
