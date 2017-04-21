<properties 
    pageTitle="调用触发程序从 Azure 数据工厂" 
    description="了解如何调用在 Azure 数据工厂使用 MapReduce 活动触发程序。" 
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
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>调用数据工厂从触发程序
## <a name="introduction"></a>介绍
可用于 MapReduce 活动数据工厂管道在运行 HDInsight 触发群集上的触发程序。 在阅读本文之前使用该活动的详细信息请参阅[MapReduce 活动](data-factory-map-reduce.md)文章。 

## <a name="spark-sample-on-github"></a>在 GitHub 上触发示例
[触发的数据工厂在 GitHub 上的示例](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark)演示如何使用 MapReduce 活动调用触发程序。 触发程序只需将数据从一个 Azure Blob 容器复制到另一个。 

## <a name="data-factory-entities"></a>数据工厂实体
**触发的 ADF/src/ADFJsons**文件夹中包含文件的数据工厂实体 （链接的服务、 数据集、 管道）。  

在此示例中有两个**链接的服务**︰ Azure 存储和 Azure HDInsight。 在**StorageLinkedService.json**和 clusterUri、 用户名和密码在**HDInsightLinkedService.json**中的指定您 Azure 存储名称和密钥值。

在此示例中有两个**数据集**︰ **input.json**和**output.json**。 这些文件位于**数据集**文件夹中。  这些文件代表的 MapReduce 活动的输入和输出数据集

在**ADFJsons/管线**文件夹中找到示例管道。 检查管线要理解如何使用 MapReduce 活动调用触发程序。 

MapReduce 活动配置以调用**com.adf.sparklauncher.jar**在 Azure 存储 （在 StorageLinkedService.json 中指定） 中的**adflibs**容器中。 此程序的源代码中触发的 ADF/src/主/java/com/adf/文件夹并调用触发提交和运行作业的触发。 

> [AZURE.IMPORTANT] 
> 阅读自述文件[。TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt)为最新的和额外的信息，然后再使用此示例。 
>  
> 使用这种方法 HDInsight 触发群集来调用使用 MapReduce 活动触发程序。 不支持使用按需 HDInsight 群集。   


## <a name="see-also"></a>请参见
- [配置单元活动](data-factory-hive-activity.md)
- [猪的活动](data-factory-pig-activity.md)
- [MapReduce 活动](data-factory-map-reduce.md)
- [Hadoop 流活动](data-factory-hadoop-streaming-activity.md)
- [调用 R 脚本](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
