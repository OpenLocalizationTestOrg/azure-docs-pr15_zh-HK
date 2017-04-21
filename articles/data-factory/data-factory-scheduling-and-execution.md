<properties
    pageTitle="计划和执行数据工厂 |Microsoft Azure"
    description="学习 Azure 数据工厂应用程序模型的调度和执行的方面。"
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
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>数据工厂计划编制和执行
本文介绍了 Azure 数据工厂应用程序模型的调度和执行方面。 

## <a name="prerequisites"></a>系统必备组件
本文假设您了解数据工厂应用程序模型的概念，包括活动、 管线、 链接的服务和数据集的基础知识。 Azure 数据工厂的基本概念，请参阅下列文章︰

- [数据工厂简介](data-factory-introduction.md)
- [管道](data-factory-create-pipelines.md)
- [数据集](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>计划活动

与活动 JSON 的计划部分中，您可以指定为活动循环执行的时间表。 例如，您可以安排一个活动每隔一小时，如下所示︰

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![计划程序示例](./media/data-factory-scheduling-and-execution/scheduler-example.png)

图中所示，指定活动计划创建一系列翻转窗口。 翻转窗口是一系列的固定大小非重叠、 连续的时间间隔。 这些逻辑翻转窗口的活动称为*活动窗口*。

对于当前正在执行的活动窗口，您可以访问与[WindowStart](data-factory-functions-variables.md#data-factory-system-variables)和[WindowEnd](data-factory-functions-variables.md#data-factory-system-variables)系统变量活动 JSON 中的活动窗口相关联的时间间隔。 JSON 活动中，可以用于不同的目的使用这些变量。 例如，您可以使用它们从表示的时间序列数据的输入和输出数据集选择数据。

**计划程序**属性支持数据集作为**可用性**属性相同的子属性。 有关详细信息，请参阅[数据集可用性](data-factory-create-datasets.md#Availability)。 示例︰ 计划在特定时间的偏移量，或设置对齐的开头或末尾的活动窗口的时间间隔的处理模式。

您可以指定**计划程序**属性的活动，但此属性是**可选**的。 如果您指定一个属性，它必须匹配您在输出数据集定义中指定的节奏性变化。 目前，输出数据集是驱使计划，因此即使该活动不会产生任何输出，您必须创建一个输出数据集。 如果活动并不需要输入任何内容，则可以跳过创建输入数据集。

## <a name="time-series-datasets-and-data-slices"></a>时间序列数据集和数据切片

时间序列数据是通常包含在一个时间间隔，进行连续测量的数据点的连续序列。 时间序列数据的常见示例包括传感器数据和应用程序的遥测数据。

与数据工厂可以处理的时间系列数据与活动成批处理的方式运行。 通常情况下，是所输入的数据到达并输出产生的数据需要定期的节奏。 这节奏是建立模型，如下所示指定集内的数据的**可用性**︰

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

消耗和活动运行所产生的数据的每个单元被称为数据切片。 下图显示一个活动与一个输入数据集的示例和一个输出数据集。 这些数据集有**可用性**设置为每小时频率。

![可用性计划](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

上图显示的输入和输出数据集的每小时数据片。 该图显示了可供处理的三个输入的切片。 10-11 日上午的活动过程中，产生 10-11 日上午输出切片。

您可以访问与当前切片与变量[SliceStart](data-factory-functions-variables.md#data-factory-system-variables)和[SliceEnd](data-factory-functions-variables.md#data-factory-system-variables)的 JSON 数据集中所产生的时间间隔。

当前，数据工厂需要严格指定如果在活动中的计划符合指定输出数据集的**可用性**方面的计划。 因此， **WindowStart**、 **WindowEnd**、 **SliceStart**和**SliceEnd**始终映射到相同的时间段和单个输出切片。

可用于可用性部分的不同属性的详细信息，请参阅[创建数据集](data-factory-create-datasets.md)。

## <a name="move-data-from-sql-database-to-blob-storage"></a>将数据从 SQL 数据库移动到 Blob 存储

让我们放一些在一起和操作中通过创建将数据从 SQL Azure 数据库表到 Azure Blob 存储每个小时的管线。

**输入︰ Azure 的 SQL 数据库数据集**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**频率**设置为**小时**和在可用性部分中的**间隔**设置为**1** 。

**输出︰ Azure Blob 存储数据集**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**频率**设置为**小时**和在可用性部分中的**间隔**设置为**1** 。



**活动︰ 复制活动**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


该示例显示了活动计划和数据集的可用性部分设置为每小时频率。 此示例说明如何可以使用**WindowStart**和**WindowEnd**来选择活动的相关数据运行，并将其复制到具有适当**采用文件夹路径**的 blob。 **采用文件夹路径**被参数化，每个小时有一个单独的文件夹。

三扇区之间 8 – 11 日上午当执行时，SQL Azure 数据库中的数据是，如下所示︰

![示例输入](./media/data-factory-scheduling-and-execution/sample-input-data.png)

管道将部署之后，Azure blob 填充，如下所示︰

-   Mypath/2015年/1/1/8/数据文件。&lt;Guid&gt;数据与.txt

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;Guid&gt;将被替换为实际的 guid。 示例文件的名称︰ Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   Mypath/2015年/1/1/9/数据文件。&lt;Guid&gt;.txt 的数据︰

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   Mypath/2015年/1/1/10/数据文件。&lt;Guid&gt;.txt 不包含任何数据。


## <a name="active-period-for-pipeline"></a>活动管线段

[创建管线](data-factory-create-pipelines.md)引入了通过设置**start**和**end**属性指定管道活动期的概念。

在过去，可以设置管道有效期间的开始日期。 数据工厂自动计算过去所有数据切片的 （背景填充），并开始处理它们。

## <a name="parallel-processing-of-data-slices"></a>并行处理的数据切片
您可以配置后填充数据切片，通过活动 JSON 的策略中设置**并发**运行并行。 此属性的详细信息，请参阅[创建管线](data-factory-create-pipelines.md)。

## <a name="rerun-a-failed-data-slice"></a>重新运行失败的数据切片 
您可以监视执行切片中丰富且形象的方式。 有关详细信息，请参阅[监视和管理使用 Azure 门户刀片服务器的管道](data-factory-monitor-manage-pipelines.md)或[监视和管理应用程序](data-factory-monitor-manage-app.md)。

请考虑下面的示例演示两个活动。 Activity1 作为输入由 Activity2 以生成最终的输出时间序列数据集的输出产生时间序列数据集切片。

![失败的切片](./media/data-factory-scheduling-and-execution/failed-slice.png)

关系图显示出三个新段，故障产生的 Dataset2 上午 9-10 片。 数据工厂自动跟踪时间系列数据集的依赖项。 因此，无法启动活动为上午 9-10 点下游片运行。

数据工厂监视和管理工具允许您深入查看诊断日志失败的扇区，很容易地找到问题的根本原因并进行修复。 解决了该问题后，很轻松地启动运行产生故障扇区的活动。 如何重新运行和理解数据切片的状态转换的详细信息，请参阅[监视和管理使用 Azure 门户刀片服务器的管道](data-factory-monitor-manage-pipelines.md)或[监视和管理应用程序](data-factory-monitor-manage-app.md)。

重新上午 9-10 片运行的**Dataset2**后，数据工厂将开始最终数据集上上午 9-10 依赖片运行。

![重新运行失败的切片](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>按顺序运行活动
您可以通过设置其他活动的输入数据集作为输出数据集的一个活动链接 （运行后另一个活动） 的两个活动。 活动可以是同一管道中或在不同的管道。 第二个活动执行仅当第一个完成成功。

例如，请考虑以下情况︰

1.  管线 P1 有活动 A1，需要外部的输入数据集 D1，并生成输出数据集 D2。
2.  P2 管道具有活动 A2，要求输入数据集 D2，并生成输出数据集 D3。

在这种情况下，活动 A1 和 A2 是在不同的管道。 活动 A1 运行时到达预定的可用性频率并有可用的外部数据。 该活动 A2 时从 D2 计划的片变为可用，并且达到预定的可用性频率运行。 如果在其中一个数据集 D2 中的扇区时出错，A2 不运行为该片，直到连接器可用。

关系图视图将如下所示如下图︰

![串联两个管道中的活动](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

前面已提到，活动可以是相同的管道中。 这两个活动在同一管道中图视图将如下所示如下图︰

![链接同一管线中的活动](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>连续复制
就可以逐个运行多个复制操作顺序/排序方式。 例如，您有两个副本活动在管道 （CopyActivity1 和 CopyActivity2） 与下面的数据输入的输出数据集︰   

CopyActivity1

输入︰ 数据集。 输出︰ Dataset2。

CopyActivity2

输入︰ Dataset2。  输出︰ Dataset3。

已成功地运行了 CopyActivity1 和 Dataset2 是可用时，才会运行 CopyActivity2。

下面是示例管道 JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

请注意，在示例中，第一个复制活动 (Dataset2) 的输出数据集指定作为第二个活动的输入。 因此，第二个活动运行时输出数据集的第一个活动准备就绪。  

在示例中，CopyActivity2 可以有不同的输入，如 Dataset3，但使该活动不会运行直至完成 CopyActivity1 CopyActivity2，输入作为指定 Dataset2。 例如︰

CopyActivity1

输入︰ Dataset1。 输出︰ Dataset2。

CopyActivity2

输入︰ Dataset3，Dataset2。 输出︰ Dataset4。

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


请注意，在示例中，两个输入数据集指定为第二个复制活动。 当指定多个输入时，只有第一个的输入数据集用来复制数据，而其他数据集用作依赖项。 只有在满足以下条件后，CopyActivity2 将启动︰

- 已成功完成了 CopyActivity1 和 Dataset2 是可用。 在将数据复制到 Dataset4 时，则不使用此数据集。 它只相当于计划的依赖项为 CopyActivity2。   
- Dataset3 是可用的。 此数据集表示复制到目标的数据。  



## <a name="model-datasets-with-different-frequencies"></a>具有不同频率的模型数据集

采样中的频率的输入和输出数据集和活动时间表窗口都相同。 某些情况下需要的能力，以产生输出频率不同的一个或多个输入的频率。 数据工厂支持建模这些方案。

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>示例 1︰ 生成输入数据是每隔一小时，每日输出报告

请考虑在其中您输入可用的传感器的测量数据 Azure Blob 存储中每隔一小时一个方案。 您想要产生每日统计信息，如平均值、 最大值和最小值与聚合报表与[数据工厂配置单元活动](data-factory-hive-activity.md)的那一天。

下面是如何建立模型与数据工厂的这种情况下︰

**输入数据集**

每小时的输入的文件的文件夹中删除某一天。 输入的可用性在**小时**设置 (频率︰ 小时、 间隔︰ 1)。

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**输出数据集**

一个输出文件日期的文件夹中创建每一天。 可用性的输出设置为**日期**(频率︰ 日期和时间间隔︰ 1)。


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**活动︰ 在管道中的配置单元活动**

该配置单元脚本接收作为使用**WindowStart**变量，如下面的代码段所示的参数的适当*日期时间*信息。 该配置单元脚本使用此变量的那一天，从正确的文件夹中加载数据并运行聚合生成的输出。

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

下图显示了从数据相关性角度的方案。

![数据依赖关系](./media/data-factory-scheduling-and-execution/data-dependency.png)

每天输出片取决于输入数据集从 24 小时切片。 数据工厂落在相同的时间段为输出片要生产的切片测算出输入数据自动计算这些依赖项。 如果 24 输入扇任何的区不是可用的数据工厂等待输入的切片，开始运行的日常活动前准备就绪。


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>示例 2︰ 使用表达式和数据工厂函数指定依赖项

让我们来看另一种方案。 假设您有一个配置单元的活动，处理两个输入数据集。 其中之一每日都有新数据，但其中之一获取新数据每周。 假设您想要跨两个输入进行联接，然后每天生成的输出。

哪些数据工厂的简单方法找出右自动输入切片处理对齐到输出数据切片的时间段不起作用。

您必须指定每个活动的运行，数据工厂使用上周的数据切片每周输入数据集。 您使用 Azure 数据工厂函数如下面的代码段所示实现此行为。

**输入 1: Azure blob**

第一个输入是 Azure blob 正在每日更新。

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**输入 2: Azure blob**

输入 2 是 Azure blob 每周进行更新。

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**输出︰ Azure blob**

一个输出文件是每天的文件夹中创建的那一天。 可用性的输出设置为**天**(频率︰ 每天、 间隔︰ 1)。

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**活动︰ 在管道中的配置单元活动**

配置单元活动采用两个输入，并生成输出切片每一天。 您可以指定每日输出片依赖与前一周每周输入的输入切片，如下所示。

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>数据工厂函数和系统变量   

函数和数据工厂支持的系统变量的列表，请参见[数据工厂函数和系统变量](data-factory-functions-variables.md)。

## <a name="data-dependency-deep-dive"></a>数据依赖关系深入探究

若要生成数据集切片的活动运行，数据工厂使用以下*依赖性模型*来确定消耗和活动所产生的数据集之间的关系。

生成输出数据集切片所需的输入数据集的时间范围被称为*依赖期*。

仅在可用依赖项内的输入数据集内的数据切片后，活动运行生成数据集切片。 换句话说，所有输入的扇区组成的依赖项期间必须处于**就绪**状态为活动运行以生成输出数据集切片。

若要生成数据集切片 [**开始**、**结束**]，函数必须映射数据集切片到其依赖期。 此函数是本质上是一个转换的开始和结束的依赖期的开始和结尾的数据集切片的公式。 更加正式的名称︰

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F**和**g**映射函数可用于计算的开始和末尾输入每个活动的依赖期。

如示例所示，依赖期是相同数据切片生成的期间。 在这些情况下，数据工厂自动计算输入依赖项内落的切片。  

例如，在其中每日产生输出，输入的数据是每小时可聚合示例，数据切片期是 24 小时。 数据工厂查找相关的每小时输入此时间段切片，并使输出切片依赖于输入的扇区。

您还可以提供每天生成依赖项期间所示的示例中，在其中输入之一就是每周一次和输出切片的映射。

## <a name="data-dependency-and-validation"></a>数据相关性和有效性

数据集可以具有定义指定如何切片执行生成的数据可以供利用之前验证的验证策略。 有关详细信息，请参见[创建数据集](data-factory-create-datasets.md)。

在这种情况下，切片完成执行后输出切片状态更改为**等待**与**验证**的子状态。 扇区进行验证后，切片状态将更改为**已准备好**。

如果数据切片已生产但未通过验证，则不会处理取决于此切片器的下游切片的活动运行。

[显示器和管理管道](data-factory-monitor-manage-pipelines.md)介绍数据切片数据工厂中的各种状态。

## <a name="external-data"></a>外部数据

数据集可以被标记为外部 （下面的 JSON 段所示），这意味着它不生成数据工厂。 在这种情况下，数据集策略可以有一组额外的参数描述的验证和重试该数据集的策略。 所有属性的说明，请参阅[创建管线](data-factory-create-pipelines.md)。

类似于数据集所产生的数据工厂，外部数据的数据片需要依赖切片处理之前准备就绪。

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>一次性的管道
您可以创建和安排定期运行的管道 (例如︰ 每小时或每日) 内管道定义中指定的开始和结束时间。 有关详细信息，请参阅[计划活动](#scheduling-and-execution)。 您还可以创建管线只运行一次。 若要执行此操作，您将**pipelineMode**属性设置管道定义中**一次性**JSON 下例中所示。 此属性的默认值是**计划**。

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

请注意以下事项︰

- 不指定管道的**开始**和**结束**时间。
- 输入和输出数据集的**可用性**是指定 （**频率**和**时间间隔**），即使数据工厂不会使用这些值。  
- 关系图视图中不显示一次性的管道。 这种现象是设计使然。
- 无法更新一次性的管道。 可以克隆一个一次性的管道，将其重命名、 更新属性，并部署创建另一个。
