<properties 
    pageTitle="数据转换︰ 过程和转换数据 |Microsoft Azure" 
    description="了解如何转换数据或使用 Hadoop，机器学习或 Azure 数据湖分析 Azure 数据工厂中的流程数据。" 
    keywords="数据转换、 处理数据转换数据，转换活动"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>转换数据在 Azure 数据工厂
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>概述 
本文介绍了 Azure 数据工厂，您可以使用转换和处理为预测和见解的原始数据中的数据转换活动。 如 Azure HDInsight 群集或 Azure 批处理计算环境中执行的转换活动。 它提供每个转换活动的详细信息的文章的链接。
 
数据工厂支持以下数据转换活动可添加到[管线](data-factory-create-pipelines.md)或者单独或链与另一个活动。

> [AZURE.NOTE] 有关使用的分步说明，请参阅[创建管线配置单元转换与](data-factory-build-your-first-pipeline.md)文章的演练。  

## <a name="hdinsight-hive-activity"></a>HDInsight 配置单元活动
数据工厂管线中的 HDInsight 配置单元活动执行您自己的配置单元查询或按需基于 Windows/Linux HDInsight 群集。 有关此活动的详细信息请参阅[配置单元活动](data-factory-hive-activity.md)文章。 

## <a name="hdinsight-pig-activity"></a>HDInsight 小猪活动
数据工厂管线中的 HDInsight 豚活动执行自己的猪的查询或按需基于 Windows/Linux HDInsight 群集。 有关此活动的详细信息请参阅[豚活动](data-factory-pig-activity.md)文章。 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce 活动
数据工厂管线中的 HDInsight MapReduce 活动执行 MapReduce 在您自己计算机上的程序或按需基于 Windows/Linux HDInsight 群集。 有关此活动的详细信息请参阅[MapReduce 活动](data-factory-map-reduce.md)文章。

您可以使用 MapReduce 活动 HDInsight 触发群集上运行触发程序。 详细信息，请参阅[从 Azure 数据工厂调用触发程序](data-factory-spark.md)。

## <a name="hdinsight-streaming-activity"></a>HDInsight 流活动
数据工厂管线中的 HDInsight 流活动执行 Hadoop 流在您自己计算机上的程序或按需基于 Windows/Linux HDInsight 群集。 有关此活动的详细信息，请参阅[HDInsight 流活动](data-factory-hadoop-streaming-activity.md)。

## <a name="machine-learning-activities"></a>机器学习活动
Azure 数据工厂使您能够轻松地创建使用预测分析的已发布的 Azure 机器学习 web 服务的管道。 在 Azure 数据工厂管道中使用[批处理执行活动](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity)，您可以调用批处理中的数据进行预测的机器学习 web 服务。

随着时间的推移评分的实验机器学习中的预测模型需要重新训练使用新输入的数据集。 您已完成再培训之后，要与 retrained 的机器学习模型更新计分的 web 服务。 [更新资源活动](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity)可用于更新 web 服务与新培训模型。  

这些机器学习活动有关的详细信息，请参阅[使用机器学习的活动](data-factory-azure-ml-batch-execution-activity.md)。 

## <a name="stored-procedure-activity"></a>存储的过程活动
可以在数据工厂管道中使用 SQL Server 存储过程活动来调用一个下面的数据存储区中的存储的过程︰ Azure SQL 数据库，SQL Azure 数据仓库在企业中的 SQL Server 数据库或 Azure VM。 有关详细信息请参阅[存储过程活动](data-factory-stored-proc-activity.md)文章。  

## <a name="data-lake-analytics-u-sql-activity"></a>湖泊分析 U-SQL 活动数据
数据湖分析 U SQL 活动 Azure 数据湖分析群集上运行 U SQL 脚本。 有关详细信息请参阅[数据分析 U SQL 活动](data-factory-usql-activity.md)文章。 

## <a name="net-custom-activity"></a>.NET 的自定义活动
如果您需要转换数据以一种不受数据工厂，可以使用您自己的数据处理逻辑中创建自定义活动，并在管道中使用的活动。 您可以配置要使用 Azure 批服务或 Azure HDInsight 群集运行的自定义.NET 活动。 有关详细信息请参阅[使用自定义活动](data-factory-use-custom-activities.md)文章。 

您可以创建自定义活动与 R 安装 HDInsight 群集上运行 R 脚本。 请参阅[运行 R 脚本使用 Azure 数据工厂](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)。 

## <a name="compute-environments"></a>计算环境
创建链接的服务的计算环境，并将链接的服务定义转换活动时。 有两种类型的数据工厂支持的计算环境。 

1. **按需**︰ 在这种情况下，通过数据工厂完全托管计算环境。 它会自动创建由数据工厂服务，则在提交给流程数据和当作业完成时删除作业之前。 您可以配置和控制作业执行、 群集管理和引导操作按需计算环境的精确设置。 
2. **将您自己**︰ 在这种情况下，您可以注册自己的计算环境 （例如 HDInsight 群集） 作为链接服务数据工厂中。 由您管理计算环境和数据工厂服务使用它来执行活动。 

请参阅[计算链接服务](data-factory-compute-linked-services.md)文章，以了解有关计算数据工厂支持的服务。 


## <a name="summary"></a>摘要
Azure 数据工厂支持以下数据转换活动和计算环境的活动。 可以添加到管线或者单独或链与另一个活动的转换活动。

数据转换活动 |  计算环境 
:----------------------- | :--------------------
[配置单元](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[小猪](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop 流](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[机器学习活动︰ 批处理执行和更新资源](data-factory-azure-ml-batch-execution-activity.md) | Azure 的 VM 
[存储的过程](data-factory-stored-proc-activity.md) | SQL azure、 SQL Azure 数据仓库或 SQL Server |
[数据分析湖 U-SQL](data-factory-usql-activity.md) | Azure 数据湖分析 
[最低](data-factory-use-custom-activities.md) | HDInsight [Hadoop] 或 Azure 的批处理
   

