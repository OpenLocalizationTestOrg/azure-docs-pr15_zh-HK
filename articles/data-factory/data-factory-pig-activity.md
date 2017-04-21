<properties 
    pageTitle="猪的活动" 
    description="了解如何使用猪的活动在 Azure 数据工厂上请求/您自己的 HDInsight 群集上运行 Pig 脚本。" 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>猪的活动
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)

数据工厂[管线](data-factory-create-pipelines.md)中的 HDInsight 猪的活动[您自己](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或[按需](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)基于 Windows/Linux HDInsight 群集上执行猪的查询。 本文基于[数据变换活动](data-factory-data-transformation-activities.md)文章，介绍了数据转换和受支持的转换活动的一般概述。

## <a name="syntax"></a>语法

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>语法的详细信息

属性 | 说明 | 必填
-------- | ----------- | --------
名称 | 活动名称 | 是的
说明 | 描述该活动的用途的文本 | 不
类型 | HDinsightPig | 是的
输入 | 由猪的活动的一个或多个输入 | 不
输出 | 猪的活动所产生的一个或多个输出 | 是的
linkedServiceName | 对 HDInsight 群集注册为链接服务数据工厂中引用 | 是的
脚本 | 指定 Pig 脚本内联 | 不
脚本路径 | 在 Azure blob 存储中存储 Pig 脚本并提供文件的路径。 使用脚本或 scriptPath 属性。 两者不能同时使用。 文件名称是区分大小写。 | 不
定义 | 猪的脚本中引用的键/值对的形式指定参数 | 不

## <a name="example"></a>示例

让我们考虑一种游戏日志分析中需要确定由贵公司启动游戏玩家所花费的时间。
 
下面的示例游戏日志是一个逗号 （，） 分隔文件。 它包含下列字段 – 档案 Id、 SessionStart、 持续时间、 SrcIPAddress 和 GameType。

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**猪的脚本**来处理此数据︰

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

若要在数据工厂管道中执行此猪的脚本，请执行以下操作︰

1. 创建链接的服务注册[您自己的 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或配置[拨号 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 让我们来调用此链接的服务**HDInsightLinkedService**。
2.  创建[链接的服务](data-factory-azure-blob-connector.md)来配置到 Azure Blob 存储存放数据的连接。 让我们来调用此链接的服务**StorageLinkedService**。
3.  创建指向输入和输出数据的[数据集](data-factory-create-datasets.md)。 所谓输入数据集**PigSampleIn**和**PigSampleOut**输出数据集。
4.  在第 2 步中配置的 Azure Blob 存储的文件中复制猪的查询。 如果承载数据的 Azure 存储不同从一个承载该查询文件，创建单独的 Azure 存储链接服务。 请参阅活动配置中链接的服务。 使用**scriptPath**指定的路径 pig 脚本文件和**scriptLinkedService**。 
    
    > [AZURE.NOTE] 您还可以提供 Pig 脚本内联在活动定义中的使用的**脚本**属性。 但是，我们建议不要采用这种方法为所有在脚本需要进行转义的特殊字符，可能会导致调试的问题。 最佳做法是按照步骤 4。
5. 与 HDInsightPig 活动创建管线。 此活动通过 HDInsight 群集上运行 Pig 脚本处理输入的数据。

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. 部署该管道。 有关详细信息请参阅[创建管线](data-factory-create-pipelines.md)文章。 
7. 监视使用数据工厂监视和管理视图的管道。 请参阅[监视并管理数据工厂管线](data-factory-monitor-manage-pipelines.md)了解详细信息。

## <a name="specifying-parameters-for-a-pig-script"></a>指定猪的脚本的参数 

请考虑以下示例︰ 游戏日志是每天 ingested 到 Azure Blob 存储和存储在一个文件夹分区基于的日期和时间。 您想要进行参数化处理的猪的脚本在运行时动态地将输入的文件夹位置和还产出分区与日期和时间。
 
若要使用参数化的猪的脚本，请执行以下操作︰

- 在**定义**中定义的参数。

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- 在猪的脚本中，请在下面的示例所示，使用**$parameterName**的参数︰

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>请参见
- [配置单元活动](data-factory-hive-activity.md)
- [MapReduce 活动](data-factory-map-reduce.md)
- [Hadoop 流活动](data-factory-hadoop-streaming-activity.md)
- [调用触发程序](data-factory-spark.md)
- [调用 R 脚本](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


