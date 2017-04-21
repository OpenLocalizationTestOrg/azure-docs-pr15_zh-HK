<properties 
    pageTitle="创建/调度管道、 数据工厂中的活动链接 |Microsoft Azure" 
    description="了解如何在移动和转换数据的 Azure 数据工厂创建数据管道。 创建数据驱动的工作流中，生成已准备好要使用的信息。" 
    keywords="数据管道，数据驱动的工作流"
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

# <a name="pipelines-and-activities-in-azure-data-factory"></a>管线和 Azure 数据工厂中的活动
这篇文章可以帮助您了解管线和 Azure 数据工厂中的活动，并使用它们来构建端到端数据驱动的工作流的数据移动和数据处理方案。  

> [AZURE.NOTE] 本文假定您已通过[Azure 数据工厂简介](data-factory-introduction.md)。 如果您没有举办-上体验与创建的数据工厂，通过[构建您的第一个数据工厂](data-factory-build-your-first-pipeline.md)教程将帮助您理解这篇文章更好。  

## <a name="what-is-a-data-pipeline"></a>什么是数据管道？
**管线**是活动的一组逻辑相关**活动**。 它用于为单位执行任务组活动。 为了更好地理解管线，您需要首先了解活动。 

## <a name="what-is-an-activity"></a>活动是什么？
活动定义要对数据执行的操作。 每个活动采用零个或多个[数据集](data-factory-create-datasets.md)作为输入，并生成一个或多个数据集作为输出。 

例如，您可以使用复制活动协调的数据从一个数据存储区复制到另一个数据存储区。 同样，您可以使用 HDInsight 配置单元活动 Azure HDInsight 群集来转换您的数据上运行配置单元查询。 Azure 数据工厂提供了大量的[数据转换](data-factory-data-transformation-activities.md)和[移动数据](data-factory-data-movement-activities.md)的活动。 您还可以选择创建自定义的.NET 活动运行您自己的代码。 

## <a name="sample-copy-pipeline"></a>示例复制管道
在下面的示例管线中，没有一个活动类型的**活动**部分的**副本**。 在此示例中，[复制活动](data-factory-data-movement-activities.md)将数据从 Azure Blob 存储到 SQL Azure 数据库。 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

请注意以下几点︰

- 在活动部分是其**类型**设置为**复制**只能有一个活动。
- 活动的输入设置为**InputDataset** ，输出为活动设置为**OutputDataset**。
- **TypeProperties**部分中， **BlobSource**指定为源类型并将**SqlSink**指定为接收器类型。

完成创建此管线的演练，请参见[教程︰ Blob 存储中的数据复制到 SQL 数据库](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。 

## <a name="sample-transformation-pipeline"></a>示例转换管道
在下面的示例管线中，没有一个活动类型的**活动**部分的**HDInsightHive** 。 在此示例中， [HDInsight 配置单元活动](data-factory-hive-activity.md)从 Azure Blob 存储的数据转换通过运行在 Azure HDInsight Hadoop 群集配置单元脚本文件。 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
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
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

请注意以下几点︰ 

- 在活动部分中，则只能有一个活动的**类型**设置为**HDInsightHive**。
- 在 Azure 存储帐户 （由 scriptLinkedService，称为**AzureStorageLinkedService**），并在容器**adfgetstarted****脚本**文件夹中存储的配置单元脚本文件**partitionweblogs.hql**。
- **定义**部分用于指定传递给配置单元脚本以配置单元配置值的运行时设置 (例如 ${hiveconf: inputtable}，{hiveconf:partitionedtable} $)。

完成创建此管线的演练，请参见[教程︰ 创建用于处理数据使用 Hadoop 群集您第一个渠道](data-factory-build-your-first-pipeline.md)。 

## <a name="chaining-activities"></a>活动链接
如果管道中有多个活动，活动的输出不是输入另一活动，这些活动可能会并行运行如果输入的数据切片的活动已准备就绪。 

通过让其他活动的输入数据集作为一个活动的输出数据集，可以链接两个活动。 活动可以是同一管道中或在不同的管道。 仅当第一个成功完成后将执行第二个活动。 

例如，请考虑以下情况︰
 
1.  管线 P1 有活动 A1 要求外部输入数据集 D1，并产生**输出**数据集**D2**。
2.  P2 管道具有活动 A2，需要**输入**从数据集中**D2**，并生成输出数据集 D3。
 
在这种情况下，活动 A1 运行时外部数据可用，并达到预定的可用性频率。  该活动 A2 时从 D2 计划的片变为可用，并且达到预定的可用性频率运行。 如果在其中一个数据集 D2 中的扇区时出错，A2 不运行为该片，直到连接器可用。

关系图视图中︰

![串联两个管道中的活动](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

这两个活动在同一管道中的关系图视图︰ 

![链接同一管线中的活动](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

有关详细信息，请参阅[计划和执行](#chaining-activities)。 

## <a name="scheduling-and-execution"></a>安排和执行
到目前为止已理解管线和活动是什么。 您也查看了如何将他们定义和一个高层次的 Azure 数据工厂中的活动视图。 现在让我们看他们如何得到执行。 

管线是只能在其开始时间和结束时间之间的活动。 它不执行之前的开始时间或结束时间之后。 如果管道已暂停，它未执行 irrespective of 其开始和结束时间。 为管道运行，它应暂停。 事实上，它不是获取执行管道。 它是在管道中获得执行的活动。 但是他们这样做总体的管道上下文中。 

请参阅[计划和执行](data-factory-scheduling-and-execution.md)计划和执行在 Azure 数据工厂中的工作方式的理解。

## <a name="create-pipelines"></a>创建管线
Azure 数据工厂提供了各种机制，以编写和部署 （其中又包含在其中的一个或多个活动） 的管道。 

### <a name="using-azure-portal"></a>使用 Azure 门户
您可以使用在 Azure 门户数据工厂编辑器创建管线。 端到端的演练，请参阅[入门 Azure 数据工厂 （数据工厂编辑器）](data-factory-build-your-first-pipeline-using-editor.md) 。 

### <a name="using-visual-studio"></a>使用 Visual Studio 
可以使用 Visual Studio 以创作并部署到 Azure 数据工厂的管道。 端到端的演练，请参阅[入门 Azure 数据工厂 (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) 。 

### <a name="using-azure-powershell"></a>使用 Azure PowerShell
您可以使用 Azure PowerShell 在 Azure 数据工厂创建管道。 说，您在 c:\DPWikisample.json 的文件中定义的管道 JSON。 您可以将其上载到 Azure 数据工厂实例，如下面的示例所示︰

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

使用管线创建数据工厂的端到端演练，请参阅[入门 Azure 数据工厂 (Azure PowerShell)](data-factory-build-your-first-pipeline-using-powershell.md) 。 

### <a name="using-net-sdk"></a>使用.NET SDK
您可以创建和部署.NET SDK 通过管道太。 这种机制可以用于以编程方式创建管线。 有关详细信息，请参阅[创建、 管理和监控数据工厂以编程方式](data-factory-create-data-factories-programmatically.md)。 


### <a name="using-azure-resource-manager-template"></a>使用 Azure 资源管理器模板
您可以创建和部署使用 Azure 资源管理器模板的管道。 有关详细信息，请参阅[入门 Azure 数据工厂 （Azure 资源管理器）](data-factory-build-your-first-pipeline-using-arm.md)。 

### <a name="using-rest-api"></a>使用 REST API，
您可以创建和部署过使用 REST Api 的管道。 这种机制可以用于以编程方式创建管线。 有关详细信息，请参阅[创建或更新管线](https://msdn.microsoft.com/library/azure/dn906741.aspx)。 


## <a name="monitor-and-manage-pipelines"></a>监视和管理管道  
管线部署之后，您可以管理和监视您的管线、 切片和运行。 阅读更多有关此处︰[监视和管理管道](data-factory-monitor-manage-pipelines.md)。


## <a name="pipeline-json"></a>JSON 管线   
让我们深入了解一下上一个管道以 JSON 格式的定义方式。 管线的一般结构如下所示︰

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

**动作**区域可以在其中定义的一个或多个活动。 每个活动都有顶级结构如下︰

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

以下表格描述了在活动和管线 JSON 定义内的属性︰

标记 | 说明 | 必填
--- | ----------- | --------
名称 | 该活动或管道的名称。 指定一个名称，表示操作该活动或管道配置为执行<br/><ul><li>最大字符数︰ 260</li><li>必须以字母数字或下划线 (_) 开头</li><li>不允许以下字符:"。"，"+"、"?""/"、"<"、">"，"*"，"%"、"&"":"，"\\"</li></ul> | 是的
说明 | 文本描述的活动或管道的用途是什么 | 是的
类型 | 指定活动的类型。 请参阅[数据移动活动](data-factory-data-movement-activities.md)和[数据转换活动](data-factory-data-transformation-activities.md)的文章，为不同类型的活动。 | 是的
输入 | 输入的表中使用的活动<br/><br/>单输入的表<br/>"输入": [{"name":"inputtable1"}]，<br/><br/>两个输入的表 <br/>"输入": [{"name":"inputtable1"}，{"name":"inputtable2"}]， | 是的
输出 | 输出表，由 activity.// 一个输出表<br/>"输出": [{"name":"outputtable1"}]，<br/><br/>两个输出表<br/>"输出": [{"name":"outputtable1"}，{"name":"outputtable2"}]， | 是的
linkedServiceName | 使用该活动的链接服务的名称。 <br/><br/>活动可能要求您指定链接的服务链接到所需的计算环境。 | HDInsight 活动和 Azure 机器学习评分活动的批次是 <br/><br/>否，所有其他人
typeProperties | TypeProperties 部分中的属性取决于活动的类型。 | 不
策略 | 影响该活动的运行时行为的策略。 如果没有指定，将使用默认策略。 | 不
启动 | 开始日期时间的管道。 必须为[ISO 格式](http://en.wikipedia.org/wiki/ISO_8601)。 例如︰ 2014年-10-14T16:32:41Z。 <br/><br/>也可以指定本地时间，例如东部时间。 下面是一个示例:"2016年-02-27T06:00:00**-05:00**"，即 6 上午预计<br/><br/>开始和结束属性共同指定管道的有效期间。 在此活动期间，只与输出切片将生成。 | 不<br/><br/>如果指定 end 属性的值，则必须指定启动属性的值。<br/><br/>开始和结束时间可以同时为空以创建管线。 您必须指定两个值来设置要运行的管线活动期。 如果不指定开始时间和结束时间在创建管道时，您可以设置它们以后使用一组 AzureRmDataFactoryPipelineActivePeriod cmdlet。
结束 | 结束日期-时间的管道。 如果指定必须为 ISO 格式。 例如︰ 2014年-10-14T17:32:41Z <br/><br/>也可以指定本地时间，例如东部时间。 下面是一个示例:"2016年-02-27T06:00:00**-05:00**"，即 6 上午预计<br/><br/>要无限期地运行管道，请指定 9999-09-09 end 属性的值。 | 不 <br/><br/>如果您指定启动属性的值，则必须指定 end 属性的值。<br/><br/>对**启动**属性的说明，请参阅。
isPaused | 如果设置为 true 管道不会运行。 默认值为 false。 您可以使用此属性来启用或禁用。 | 不 
计划程序 | "计划程序"属性用于定义活动所需的安排。 其子属性是[数据集中的可用性属性](data-factory-create-datasets.md#Availability)中的相同。 | 不 |   
| pipelineMode | 针对管道运行调度的方法。 允许值为︰ 计划 （默认），一次性。<br/><br/>'安排' 指示管道运行在其活动周期 （开始和结束时间） 根据指定的时间间隔。 一次性表明，管道只运行一次。 一次创建的一次性管线不能修改更新当前。 有关一次性设置的详细信息，请参阅[Onetime 管道](data-factory-scheduling-and-execution.md#onetime-pipeline)。 | 不 | 
| expirationTime | 创建管线有效，并应保持调配后一段时间的持续时间。 如果它没有任何活动，失败，或者挂起运行过程中，管道自动删除过期时间到达。 | 不 | 
| 数据集 | 用于定义在管道中的活动数据集的列表。 此属性可用于定义特定于该管道而不定义在数据工厂内的数据集。 在此管线中定义的数据集只能通过该管线和不能共享。 [作用域的数据集](data-factory-create-datasets.md#scoped-datasets)的详细信息，请参见| 不 |  
 

### <a name="policies"></a>策略
策略影响运行时行为的活动，特别是，当处理表中的切片。 下表提供了详细信息。

属性 | 允许的值 | 默认值 | 说明
-------- | ----------- | -------------- | ---------------
并发性 | 整数 <br/><br/>最大值︰ 10 | 1 | 并发执行的活动的数目。<br/><br/>它确定可能发生不同的片上并行活动次数。 例如，如果活动需要经过大量的可用数据，具有更大的并发值加快数据处理。 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | 确定正在处理的数据切片的排序。<br/><br/>例如，如果您有 2 切片 （一个发生在下午 4 点和另一个在下午 5 点），并都挂起执行。 如果设置为 NewestFirst executionPriorityOrder，首先处理下午 5 片。 同样如果将设置为 OldestFIrst executionPriorityORder，被处理切片在下午 4 点。 
重试 | 整数<br/><br/>最大值可以是 10 | 3 | 数据处理的扇区标记为失败之前重试的次数。 数据切片的活动执行重试次数不超过指定的重试计数。 重试失败之后尽快执行。
超时 | 时间跨度 | 00:00:00 | 活动超时值。 示例︰ 00:10:00 （隐含超时 10 分钟）<br/><br/>如果值未指定或为 0，则将超时是无限的。<br/><br/>如果一个片上的数据处理时间超过了超时值，取消，并且系统会尝试重试处理。 重试的次数取决于重试属性。 当发生超时，则将状态设置为超时。
延迟 | 时间跨度 | 00:00:00 | 指定数据处理切片开始之前的延迟。<br/><br/>数据切片的活动的执行开始后的延迟时间是超出预期的执行时间。<br/><br/>示例︰ 00:10:00 （隐含的 10 分钟的延迟）
longRetry | 整数<br/><br/>最大值︰ 10 | 1 | 前片执行失败尝试长重试的次数。<br/><br/>longRetry 尝试是由 longRetryInterval 分布的。 因此，如果您需要指定重试尝试之间的时间，使用 longRetry。 如果指定了重试和 longRetry，每个 longRetry 尝试包括重试次数和尝试的最大数目是重试 * longRetry。<br/><br/>例如，如果活动策略中有以下设置︰<br/>重试︰ 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>假定只有一个扇区来执行 （等待状态），活动执行失败每次。 最初将 3 连续执行尝试。 在每次尝试后切片状态将重试。 前 3 个尝试通过之后，则该切片状态将为 LongRetry。<br/><br/>后一个小时 （即 longRetryInteval 的值），将需要另一套 3 连续执行尝试。 在此之后，切片状态会失败并会尝试进行任何更多的重试次数。 因此整体 6 尝试进行。<br/><br/>如果任何执行成功，该切片状态将为已准备好，并没有更多的重试尝试。<br/><br/>longRetry 可能采用依赖数据到达非确定的时间或总体环境是片状下进行的数据处理的情况。 在这种情况下，这样做，重试后另一个可能没有帮助，这样的时间间隔后的时间中所需的输出结果。<br/><br/>需要注意的一点︰ 不设置为 longRetry 或 longRetryInterval 的高值。 通常情况下，较高的值表示其他系统性的问题。 
longRetryInterval | 时间跨度 | 00:00:00 | 长时间的重试尝试之间的延迟 


## <a name="next-steps"></a>下一步行动

- 了解[调度和执行在 Azure 数据工厂](data-factory-scheduling-and-execution.md)。  
- 阅读有关的[数据移动](data-factory-data-movement-activities.md)和[数据转换功能](data-factory-data-transformation-activities.md)在 Azure 数据工厂
- 了解[管理和监视在 Azure 数据工厂](data-factory-monitor-manage-pipelines.md)。
- [生成和部署第一管线](data-factory-build-your-first-pipeline.md)。 
