<properties 
    pageTitle="配置单元活动" 
    description="了解如何使用该配置单元活动在 Azure 数据工厂上请求/您自己的 HDInsight 群集上运行配置单元查询。" 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>配置单元活动
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)

数据工厂[管线](data-factory-create-pipelines.md)中的 HDInsight 配置单元活动执行[您自己](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)的或[按需](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)基于 Windows/Linux HDInsight 群集上配置单元查询。 本文基于[数据变换活动](data-factory-data-transformation-activities.md)文章，介绍了数据转换和受支持的转换活动的一般概述。

## <a name="syntax"></a>语法

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
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
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>语法的详细信息

属性 | 说明 | 必填
-------- | ----------- | --------
名称 | 活动名称 | 是的
说明 | 描述该活动的用途的文本 | 不
类型 | HDinsightHive | 是的
输入 | 输入由配置单元活动 | 不
输出 | 该配置单元活动所产生的输出 | 是的 
linkedServiceName | 对 HDInsight 群集注册为链接服务数据工厂中引用 | 是的 
脚本 | 指定配置单元脚本内联 | 不
脚本路径 | 在 Azure blob 存储中存储配置单元脚本，并提供该文件的路径。 使用脚本或 scriptPath 属性。 两者不能同时使用。 文件名称是区分大小写。 | 不 
定义 | 使用 hiveconf 的配置单元脚本中引用的键/值对的形式指定参数  | 不

## <a name="example"></a>示例

让我们考虑一种游戏日志分析中需要鉴别用户玩游戏启动公司所用的时间。 

下面的日志是游戏日志示例，它是逗号 (`,`) 分隔，并且包含以下字段 – 档案 Id、 SessionStart、 持续时间、 SrcIPAddress 和 GameType。

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**配置单元脚本**来处理此数据︰

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

若要在数据工厂管道中执行此配置单元脚本，您需要执行以下

1. 创建链接的服务注册[您自己的 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或配置[拨号 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 所谓"HDInsightLinkedService"此链接的服务。
2. 创建[链接的服务](data-factory-azure-blob-connector.md)来配置到 Azure Blob 存储存放数据的连接。 所谓"StorageLinkedService"此链接的服务
3. 创建指向输入和输出数据的[数据集](data-factory-create-datasets.md)。 所谓输入数据集"HiveSampleIn"和"HiveSampleOut"的输出数据集
4. 第 2 步中配置复制配置单元查询为 Azure Blob 存储到一个文件。 如果承载数据的存储与承载该查询文件的不同，创建单独的 Azure 存储链接服务，在该活动中引用它。 **ScriptPath**用于指定配置单元查询文件和**scriptLinkedService**指定 Azure 存储包含脚本文件的路径。 

    > [AZURE.NOTE] 您还可以使用**脚本**属性提供在活动定义中的配置单元脚本内联。 我们建议不要采用这种方法为要转义的 JSON 文档需要在脚本中的所有特殊字符，可能会导致调试的问题。 最佳做法是按照步骤 4。
5.  与 HDInsightHive 活动创建管道。 活动流程/转换数据。

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  部署该管道。 有关详细信息请参阅[创建管线](data-factory-create-pipelines.md)文章。 
7.  监视使用数据工厂监视和管理视图的管道。 请参阅[监视并管理数据工厂管线](data-factory-monitor-manage-pipelines.md)了解详细信息。 


## <a name="specifying-parameters-for-a-hive-script"></a>指定配置单元脚本的参数  
在此示例中，游戏日志每天 ingested 到 Azure Blob 存储，并且存储在文件夹分区与日期和时间。 您想要进行参数化处理的配置单元脚本在运行时动态地将输入的文件夹位置和还产出分区与日期和时间。

若要使用配置单元的参数化的脚本，请执行以下

- 在**定义**中定义的参数。

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- 在配置单元脚本中，引用参数使用**${hiveconf:parameterName}**。 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>请参见
- [猪的活动](data-factory-pig-activity.md)
- [MapReduce 活动](data-factory-map-reduce.md)
- [Hadoop 流活动](data-factory-hadoop-streaming-activity.md)
- [调用触发程序](data-factory-spark.md)
- [调用 R 脚本](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









