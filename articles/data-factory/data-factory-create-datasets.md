<properties 
    pageTitle="在 Azure 数据工厂创建的数据集 |Microsoft Azure" 
    description="了解如何在 Azure 数据工厂创建的数据集，使用偏移量和 anchorDateTime 等属性的示例。"
    keywords="创建数据集，数据集示例、 示例的偏移量"
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
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>在 Azure 数据工厂的数据集
本文介绍了 Azure 数据工厂中的数据集，并包括如偏移量、 anchorDateTime 和数据库偏移/样式的示例。

当您创建一个数据集时，创建指向您想要处理的数据的指针。 处理数据 （输入/输出） 的活动中，活动也包含在管道中。 输入数据集表示为管道中活动的输入和输出数据集表示活动的输出。

数据集识别中不同的数据存储，如表、 文件、 文件夹和文档的数据。 创建一个数据集之后，可以使用它在管道中的活动。 例如，数据集可以复制活动或 HDInsightHive 活动的输入/输出数据集。 Azure 的门户网站为您提供了所有的管道和数据的输入和输出的可视布局。 一眼，看所有的关系和依赖项的管线跨所有源以便您总是知道数据的来源和发展方向。

在 Azure 数据工厂，您可以在管道中使用复制活动从数据集中获取数据。

> [AZURE.NOTE] 如果您是新手 Azure 数据工厂，看到[Azure 数据工厂简介](data-factory-introduction.md)有关 Azure 数据工厂服务的概述。 请参阅[生成第一个数据工厂](data-factory-build-your-first-pipeline.md)有关教程，说明如何创建您的第一个数据工厂。 以下两篇文章提供了背景信息需要更好地理解这篇文章。 

## <a name="define-datasets"></a>定义数据集
在 Azure 数据工厂数据集定义如下︰ 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

下表描述了上面的 json 格式的属性︰   

| 属性 | 说明 | 必填 | 默认 |
| -------- | ----------- | -------- | ------- |
| 名称 | 数据集的名称。 命名规则，请参阅[Azure 数据工厂的命名规则](data-factory-naming-rules.md)。 | 是的 | NA |
| 类型 | 数据集的类型。 指定一个 Azure 数据工厂所支持的类型 (例如︰ AzureBlob、 AzureSqlTable)。 <br/><br/>有关详细信息，请参阅[数据集类型](#Type)。 | 是的 | NA |
| 结构 | 数据集架构<br/><br/>有关详细信息，请参见[数据集结构](#Structure)部分。 | 不。 | NA |
| typeProperties | 与所选类型相对应的属性。 有关受支持的类型和它们的属性的详细信息请参见[数据集类型](#Type)一节。 | 是的 | NA |
| 外部 | Boolean 标志，用于指定是否数据集显式生成数据工厂管道或不。  | 不 | 假 | 
| 可用性 | 定义在处理窗口或数据集生产的切片模型。 <br/><br/>有关详细信息，请参阅[数据集可用性](#Availability)一节。 <br/><br/>有关数据集进行切片模型，请参阅[计划和执行](data-factory-scheduling-and-execution.md)项目的详细信息。 | 是的 | NA
| 策略 | 定义条件或数据集切片必须满足的条件。 <br/><br/>有关详细信息，请参见[数据集策略](#Policy)部分。 | 不 | NA |

## <a name="dataset-example"></a>数据集的示例
在以下示例中，数据集表示名为**MyTable**的**SQL Azure 数据库**中的表。 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

请注意以下几点︰ 

- 类型被设置为 AzureSqlTable。
- 表名 （特定于 AzureSqlTable 类型） 的类型属性设置为 MyTable。
- linkedServiceName 是指链接类型 AzureSqlDatabase 的服务。 请参阅下面的链接服务的定义。 
- 可用性频率设置为天和时间间隔设置为 1，这意味着切片每日产生。  

AzureSqlLinkedService 定义，如下所示︰

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

上面的 json 格式︰ 

- 类型设置为 AzureSqlDatabase
- 连接字符串类型属性指定要连接到的 SQL Azure 数据库信息。  


正如您可以看到，链接的服务定义如何连接到 SQL Azure 数据库。 数据集定义哪些表用于为输入/输出管线中的活动。 在[管线](data-factory-create-pipelines.md)JSON 的活动部分指定是否将数据集用作输入或输出的数据集。


> [AZURE.IMPORTANT] 除非 Azure 数据工厂生产数据集，则它应标记为**外部**。 此设置通常应用于管道中的第一个活动的输入。   

## <a name="Type"></a>数据集类型
对齐的支持的数据源和数据集类型。 请参阅有关信息类型和配置数据集的[数据移动活动](data-factory-data-movement-activities.md#supported-data-stores)文章中提到的主题。 例如，如果您使用的 SQL Azure 数据库中的数据，单击列表中的受支持的数据存储的详细的信息，请参阅 SQL Azure 数据库。  

## <a name="Structure"></a>数据集结构
**结构**部分中定义数据集的架构。 它包含的名称和数据类型的列的集合。  在以下示例中，数据集有三列 slicetimestamp、 项目名称，以及 pageviews、 它们的类型︰ 字符串、 字符串和小数分别。

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>数据集可用性
在数据集中**可用性**部分定义处理窗口 （每小时、 每天、 每周等） 或为数据集切片模型。 请参阅[计划和执行](data-factory-scheduling-and-execution.md)的更多详细信息的数据集进行切片和相关性模型的文章。 

下面的可用性部分指定输出数据集是生产每小时 （或者） 输入数据集是每小时可用︰

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

下表描述了可以在可用性部分中使用的属性︰ 

| 属性 | 说明 | 必填 | 默认 |
| -------- | ----------- | -------- | ------- |
| 频率 | 指定数据集切片生产的时间单位。<br/><br/>**支持频率**︰ 分钟、 小时、 天、 周、 月 | 是的 | NA |
| 时间间隔 | 指定频率的倍频<br/><br/>"X 频率间隔"决定切片生成频率。<br/><br/>如果您需要的数据集进行切片在每小时的基础上，您将设置**时间间隔**为**1****小时**，与**频率**。<br/><br/>**注意︰**如果您指定频率为分钟，建议将间隔设置为不小于 15 | 是的 | NA |
| 样式 | 指定是否应在间隔的起点产生切片。<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>如果将频率设置为月和样式设置为 EndOfInterval，切片上个月的最后一天产生。 如果样式设置为 StartOfInterval 上个月的第一天, 生成切片。<br/><br/>如果频率设置为天和样式设置为 EndOfInterval，在过去的一天中的小时生成切片。<br/><br/>如果频率设置为小时和样式设置为 EndOfInterval，切片生成末尾的小时。 例如，下午 1 点--下午 2 点期间的切片，切片在下午 2 点产生。 | 不 | EndOfInterval |
| anchorDateTime | 定义了时间计划程序用于计算数据集切片边界中的绝对位置。 <br/><br/>**注意︰**如果 AnchorDateTime 有频率比粒度更细的日期部分的更精细的部分将被忽略。 <br/><br/>例如，如果**时间间隔**为**每小时**(频率︰ 小时和间隔︰ 1) 和**AnchorDateTime**包含，**分和秒**，则 AnchorDateTime 的**分钟数和秒**的部分将被忽略。 | 不 | 0001 年 01 月 01 日 |
| 偏移量 | 依据移动的开始和结尾的所有数据集切片的时间跨度。 <br/><br/>**注意︰**如果指定了 anchorDateTime 和偏移量，则结果为组合 shift 键。 | 不 | NA |

### <a name="offset-example"></a>对方的示例

每天早上 6 而不是默认午夜切片的开始。 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

**频率**设置为**日期**和**时间间隔**设置为**1** （一天一次）︰ 如果您想要生成在 6 上午而不是在默认的时间切片︰ 上午 12。 请记住，此时间的 UTC 时间。 

## <a name="anchordatetime-example"></a>anchorDateTime 示例

在 2007 年开始的**示例︰** 23 小时数据集切片-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>偏移量/样式的示例

如果您需要在每月的基础上的数据集在特定日期和时间 （上午 8:00 在每月第三个上假设），使用**偏移**标记来设置日期和时间运行。 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>数据集策略

数据集定义中的**策略**部分定义的条件或数据集切片必须满足的条件。

### <a name="validation-policies"></a>验证策略

| 策略名称 | 说明 | 应用于 | 必填 | 默认 |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | 验证在**Azure 的 blob**数据满足的最小要求 （以兆字节为单位）。 | Azure 的斑点 | 不 | NA |
|minimumRows | 验证**SQL Azure 数据库**或**Azure 表**中的数据包含的最小行数。 | <ul><li>SQL azure 数据库</li><li>Azure 表</li></ul> | 不 | NA

#### <a name="examples"></a>示例

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>外部数据集

外部数据集是不由运行在数据工厂管线产生的那些。 如果数据集被标记为**外部**，可以将**ExternalData**策略定义为影响行为的数据集切片的可用性。 

除非 Azure 数据工厂生产数据集，则它应标记为**外部**。 此设置通常适用于管道中的第一个活动的输入除非正在使用活动或管道链。 

| 名称 | 说明 | 必填 | 默认值  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | 对给定的切片的外部数据的可用性检查的延迟时间。 例如，如果数据应能每小时，可以通过使用 dataDelay 延迟检查有可用的外部数据和相应扇区为就绪。<br/><br/>仅适用于当前时间。  例如，如果是现在下午 1:00，此值为 10 分钟，从下午 1:10 开始验证。<br/><br/>此设置不影响在过去切片 (片与切片结束时间 + dataDelay < 现在) 没有任何延迟处理。<br/><br/>大于 23:59 小时需要指定使用的 day.hours:minutes:seconds 格式的时间。 例如，要指定 24 小时内，请不要使用 24:00:00;相反，使用 1.00:00:00。 如果您使用 24:00:00，则将其视为 24 天 (24.00:00:00)。 1 一天 4 小时，指定 1:04:00:00。 | 不 | 0 |
| retryInterval | 请重试尝试之间失败下, 一次的等待时间。 适用于当前时间;如果以前尝试失败，我们等待这最后一次尝试很久以后。 <br/><br/>如果是现在下午 1:00，我们开始第一次尝试。 下次重试如果完成第一项验证检查的持续时间为 1 分钟和操作失败，则在 1:00 + 1 分钟 （持续时间） + 1 分钟 （重试间隔） = 1:02 pm。 <br/><br/>对于过去的切片，没有任何延迟。 重试立即反应。 | 不 | 00:01:00 （1 分钟） | 
| retryTimeout | 每次重试尝试超时。<br/><br/>如果此属性设置为 10 分钟，验证需要在 10 分钟内完成。 如果花费超过 10 分钟来执行验证，重试超时。<br/><br/>如果所有尝试验证都超时，该扇区标记为超时。 | 不 | 00:10:00 （10 分钟） |
| maximumRetry | 对外部数据的可用性检查次数。 允许的最大值是 10。 | 不 | 3 | 

## <a name="scoped-datasets"></a>指定了作用域的数据集
您可以创建作用于管线中使用**数据集**属性的数据集。 这些数据集只能通过该管线内的活动但不能通过其他管道中的活动。 下面的示例定义具有两个数据集-InputDataset rdc 和 OutputDataset-rdc-用于在管道内的管道︰  

> [AZURE.IMPORTANT] 指定了作用域的数据集仅支持一次性管道 (**pipelineMode**设置为**OneTime**)。 有关详细信息，请参阅[Onetime 管道](data-factory-scheduling-and-execution.md#onetime-pipeline)。

    {
        "name": "CopyPipeline-rdc",
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
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }