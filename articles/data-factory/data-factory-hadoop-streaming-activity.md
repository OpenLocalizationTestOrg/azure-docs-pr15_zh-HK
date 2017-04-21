<properties 
    pageTitle="Hadoop 流活动" 
    description="了解如何使用 Hadoop 流活动在 Azure 数据工厂上请求/您自己的 HDInsight 群集上运行 Hadoop 流程序。" 
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
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop 流活动
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)

您可以使用 HDInsightStreamingActivity 活动调用 Hadoop 流从 Azure 数据工厂管道作业。 下面的 JSON 段显示管道 JSON 文件中使用 HDInsightStreamingActivity 的语法。 

数据工厂[管线](data-factory-create-pipelines.md)中的 HDInsight 流活动对[您自己](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或[按需](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)基于 Windows/Linux HDInsight 群集执行 Hadoop 流程序。 本文基于[数据变换活动](data-factory-data-transformation-activities.md)文章，介绍了数据转换和受支持的转换活动的一般概述。

## <a name="json-sample"></a>JSON 的示例
HDInsight 群集会自动填入示例程序 （wc.exe 和 cat.exe） 和数据 (davinci.txt)。 默认情况下，使用 HDInsight 群集的名称是容器的群集本身的名称。 例如，如果您的群集名称为 myhdicluster，blob 容器相关联的名称是 myhdicluster。 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

请注意以下几点︰

1. 将**linkedServiceName**设置为链接指向 HDInsight 群集流式 mapreduce 作业正在运行的服务的名称。
2. 将活动的类型设置为**HDInsightStreaming**。
3. 为**映射器**属性中，指定映射器可执行文件的名称。 在示例中，cat.exe 是可执行的映射。
4. 对于**变径**属性中，指定变径可执行文件的名称。 在此示例中，wc.exe 是可执行变径。
5. **输入**类型属性中，指定用于映射器 （包括位置） 的输入的文件。 在该示例中︰ "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample 是 blob 容器、 示例/数据/堡是文件夹，和 davinci.txt 是 blob。
6. 对于**输出**类型属性中，指定变径输出文件 （包括位置）。 Hadoop 流式处理作业的输出写入此属性指定的位置。
7. 在**可**部分中，指定映射器和变径的可执行文件的路径。 在该示例中:"adfsample/example/apps/wc.exe"adfsample 是 blob 容器、 示例/应用程序为文件夹，同时 wc.exe 是可执行文件。
8. 为**fileLinkedService**属性中，指定表示 Azure 存储包含可部分中指定的文件的链接的 Azure 存储服务。
9. **参数**属性中，指定作业流的参数。
10. **GetDebugInfo**属性是一个可选元素。 当它设置为失败时，仅在失败上下载日志。 当它设置为 All 时，而不考虑执行状态始终下载日志。

> [AZURE.NOTE] 如示例中所示，Hadoop 流活动的**输出**属性指定一个输出数据集。 此数据集是只推动管道计划所需的虚拟数据集。 您不需要指定任何输入数据集**输入**属性的活动。  

    
## <a name="example"></a>示例
在本演练中的管道在 Azure HDInsight 群集上运行字数流式地图/减少程序。 

### <a name="linked-services"></a>链接的服务

#### <a name="azure-storage-linked-service"></a>Azure 存储链接服务
首先，创建要链接到 Azure 数据工厂 Azure HDInsight 群集使用 Azure 存储的链接的服务。 如果您复制/粘贴下面的代码，请不要忘记替换的名称和键的 Azure 存储的用户名和帐户密码。 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight 链接服务
接下来，您将创建链接的服务链接到 Azure 数据工厂的 Azure HDInsight 群集。 如果您复制/粘贴下面的代码，HDInsight 群集名称替换您的 HDInsight 群集的名称，并更改用户名和密码值。 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>数据集

#### <a name="output-dataset"></a>输出数据集
在此示例中的管道不带任何输入。 您指定一个输出数据集 HDInsight 流活动。 此数据集是只推动管道计划所需的虚拟数据集。 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>管道

在此示例中的管道有只有一个活动类型︰ **HDInsightStreaming**。 

HDInsight 群集会自动填入示例程序 （wc.exe 和 cat.exe） 和数据 (davinci.txt)。 默认情况下，使用 HDInsight 群集的名称是容器的群集本身的名称。 例如，如果您的群集名称为 myhdicluster，blob 容器相关联的名称是 myhdicluster。  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>请参见
- [配置单元活动](data-factory-hive-activity.md)
- [猪的活动](data-factory-pig-activity.md)
- [MapReduce 活动](data-factory-map-reduce.md)
- [调用触发程序](data-factory-spark.md)
- [调用 R 脚本](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

