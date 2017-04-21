<properties 
    pageTitle="调用从 Azure 数据工厂的 MapReduce 程序" 
    description="了解如何通过从 Azure 数据工厂 Azure HDInsight 群集上运行 MapReduce 程序来处理数据。" 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>调用数据工厂的 MapReduce 节目
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)

数据工厂[管线](data-factory-create-pipelines.md)中的 HDInsight MapReduce 活动对[您自己](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或[按需](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)基于 Windows/Linux HDInsight 群集执行 MapReduce 程序。 本文基于[数据变换活动](data-factory-data-transformation-activities.md)文章，介绍了数据转换和受支持的转换活动的一般概述。

## <a name="introduction"></a>介绍 
在 Azure 数据工厂管道处理链接的存储服务中的数据通过使用链接的计算服务。 它包含一系列活动，其中每个活动执行特定处理操作。 本文介绍如何使用 HDInsight MapReduce 活动。
 
小猪/配置单元脚本群集上运行基于 Windows/Linux HDInsight 从管线使用 HDInsight 小猪和配置单元活动有关的详细信息，请参阅[猪](data-factory-pig-activity.md)和[配置单元](data-factory-hive-activity.md)。 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON 对于 HDInsight MapReduce 活动 

在 HDInsight 活动的 JSON 定义︰ 
 
1. 将**活动****类型**设置为**HDInsight**。
3. 指定**类名**属性的类的名称。
4. 指定的路径包括文件名为**jarFilePath**属性的 JAR 文件。
5. 指定链接指的是所包含的 JAR 文件的**jarLinkedService**属性的 Azure Blob 存储的服务。   
6. 在**参数**部分中指定的 MapReduce 程序的任何参数。 在运行时，您会看到一些额外的参数 (例如︰ mapreduce.job.tags) 从 MapReduce 框架。 若要区分您使用 MapReduce 参数的参数，考虑下面的示例中所示作为参数使用选项和值 (-s-输入，--输出等等，都跟其值的选项)。

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

您可以使用 HDInsight MapReduce 活动运行 HDInsight 群集上的任何 MapReduce jar 文件。 下面的示例 JSON 的定义中的管道，HDInsight 活动配置为运行 Mahout JAR 文件。

## <a name="sample-on-github"></a>在 GitHub 上的示例
您可以下载一个示例使用来自 HDInsight MapReduce 活动︰[在 GitHub 上数据工厂采样](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample)。  

## <a name="running-the-word-count-program"></a>运行的字数统计程序
在此示例中的管道 Azure HDInsight 群集上运行的字数统计地图/减少程序。   

### <a name="linked-services"></a>链接的服务
首先，创建要链接到 Azure 数据工厂 Azure HDInsight 群集使用 Azure 存储的链接的服务。 如果您复制/粘贴下面的代码，请不要忘记替换的名称和键的 Azure 存储的**用户名**和**帐户密码**。 

#### <a name="azure-storage-linked-service"></a>Azure 存储链接服务

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
接下来，您将创建链接的服务链接到 Azure 数据工厂的 Azure HDInsight 群集。 如果您复制/粘贴下面的代码， **HDInsight 群集名称**替换您的 HDInsight 群集的名称，并更改用户名和密码值。   

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
在此示例中的管道不带任何输入。 您指定一个输出数据集 HDInsight MapReduce 活动。 此数据集是只推动管道计划所需的虚拟数据集。  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>管道
在此示例中的管道有只有一个活动类型︰ HDInsightMapReduce。 下面是一些在 JSON 中重要的属性︰ 

属性 | 备注
:-------- | :-----
类型 | 该类型必须设置为**HDInsightMapReduce**。 
类名 | 类的名称是︰**字数统计**
jarFilePath | 包含类的 jar 文件的路径。 如果您复制/粘贴下面的代码，不要忘记更改群集的名称。 
jarLinkedService | Azure 存储链接服务包含的 jar 文件。 此链接的服务指的是与 HDInsight 群集关联的存储。 
参数 | 字数统计程序采用两个参数、 输入和输出。 输入的文件是 davinci.txt 文件。
频率/间隔 | 这些属性的值匹配的输出数据集。 
linkedServiceName | 是指必须在以前创建的 HDInsight 链接的服务。   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>触发程序
您可以使用 MapReduce 活动 HDInsight 触发群集上运行触发程序。 详细信息，请参阅[从 Azure 数据工厂调用触发程序](data-factory-spark.md)。  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>请参见
- [配置单元活动](data-factory-hive-activity.md)
- [猪的活动](data-factory-pig-activity.md)
- [Hadoop 流活动](data-factory-hadoop-streaming-activity.md)
- [调用触发程序](data-factory-spark.md)
- [调用 R 脚本](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
