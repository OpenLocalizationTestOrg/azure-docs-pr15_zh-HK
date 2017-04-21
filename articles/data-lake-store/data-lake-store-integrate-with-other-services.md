<properties
   pageTitle="与其他 Azure 服务集成数据湖存储 |Microsoft Azure"
   description="了解如何与其他 Azure 服务集成的数据湖存储区"
   documentationCenter=""
   services="data-lake-store"
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="integrating-data-lake-store-with-other-azure-services"></a>与其他 Azure 服务集成数据湖存储区

Azure 数据湖商店可以与其他 Azure 服务配合使用，以使更广泛的方案。 下文列出数据湖存储可与集成的服务。

## <a name="use-data-lake-store-with-azure-hdinsight"></a>使用数据湖库使用 Azure HDInsight

您可以设置数据湖存储区用作兼容 HDFS 存储[Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)群集。 对于此版本，在 Windows 和 Linux，Hadoop 和风暴群集可以使用数据湖商店只作为额外的存储空间。 此类群集仍为默认存储使用 Azure 存储 (WASB)。 但是，对于在 Windows 和 Linux HBase 群集，可以作为默认存储和 / 或附加存储空间，使用数据湖存储区。

有关如何配置 HDInsight 群集数据湖存储区的说明，请参阅︰

* [提供与数据湖存储区使用 Azure 门户 HDInsight 群集](data-lake-store-hdinsight-hadoop-use-portal.md)
* [提供与数据使用 Azure PowerShell 的湖商店 HDInsight 群集](data-lake-store-hdinsight-hadoop-use-powershell.md)

**喜欢的视频？** 请按照下面的链接以观看视频说明如何使用 HDInsight 群集数据湖存储区。

* [创建有权访问数据湖商店 HDInsight 群集](https://mix.office.com/watch/l93xri2yhtp2)
* 群集设置之后，[数据湖存储区使用配置单元和猪的脚本中访问数据](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>使用数据湖库使用 Azure 数据湖分析

[Azure 数据湖分析功能](../data-lake-analytics/data-lake-analytics-overview.md)使您能够处理大数据在云规模较大。 动态调配资源，并允许您执行万亿字节或甚至千兆兆字节的数据可存储在多种支持的数据源，其中一个被数据湖商店上的分析。 数据湖分析精心优化 Azure 数据湖商店-大数据的工作负载提供最高级别的性能、 吞吐量和为您的并行化处理。

有关如何使用数据湖存储数据湖分析说明，请参阅[入门使用数据湖存储数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)。

**喜欢的视频？** 请按照下面的链接以观看视频说明如何使用 HDInsight 群集数据湖存储区。

* [连接到 Azure 数据湖库的 Azure 数据湖分析](https://mix.office.com/watch/qwji0dc9rx9k)
* [通过数据湖分析访问 Azure 数据湖商店](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>使用数据湖库使用 Azure 数据工厂

您可以使用[Azure 数据工厂](https://azure.microsoft.com/services/data-factory/)摄取 Azure 表、 SQL Azure 数据库、 Azure SQL 数据仓库，Azure 存储 Blob 和内部数据库中的数据。 Azure 数据工厂作为头等公民在 Azure 的生态系统中，用于协调对 Azure 数据湖存储来自这些源的数据的接收。

有关如何与数据湖商店使用 Azure 数据工厂的说明，请参阅[将数据传入和传出数据湖存储区使用数据工厂](../data-factory/data-factory-azure-datalake-connector.md)。

**再次的视频 ！** 请参阅[使用 Azure 数据工厂的 Azure 数据湖存储区的数据业务流程](https://mix.office.com/watch/1oa7le7t2u4ka)。 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Azure 存储 Blob 中的数据复制到数据湖存储

Azure 数据湖商店提供了一个命令行工具，AdlCopy，使您能够从 Azure Blob 存储的数据复制到数据湖存储帐户。 有关详细信息，请参阅[复制 Azure 存储 Blob 数据湖存储区中的数据](data-lake-store-copy-data-azure-storage-blob.md)。

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>SQL Azure 数据库和数据湖存储之间复制数据

Apache Sqoop 可用于导入和导出 SQL Azure 数据库和数据湖存储之间的数据。 有关详细信息，请参阅[数据湖存储和使用 Sqoop 的 SQL Azure 数据库之间复制数据](data-lake-store-data-transfer-sql-sqoop.md)。

**观看这段视频**[使用 Apache Sqoop 将 Azure 数据湖存储关系数据源之间的数据移动](https://mix.office.com/watch/1butcdjxmu114)。

## <a name="use-data-lake-store-with-stream-analytics"></a>使用数据湖库与流分析

可以使用作为一个输出的数据湖存储来存储数据传输使用 Azure 流分析。 有关详细信息，请参阅[数据湖商店到 Azure 存储 Blob 中的流数据使用 Azure 流分析](data-lake-store-stream-analytics.md)。

## <a name="use-data-lake-store-with-power-bi"></a>使用数据湖库与电源 BI

可以使用电源 BI 数据导入数据湖存储帐户来分析和可视化数据。 有关详细信息，请参阅[数据湖存储区中的分析数据使用双电源](data-lake-store-power-bi.md)。

## <a name="use-data-lake-store-with-data-catalog"></a>使用数据湖库数据目录

您可以到 Azure 数据目录，以使数据在整个组织内可发现注册数据湖存储区中的数据。 有关详细信息请参阅[注册数据湖在 Azure 数据目录中的存储区](data-lake-store-with-data-catalog.md)。


## <a name="see-also"></a>请参见

- [Azure 湖存储数据的概览](data-lake-store-overview.md)
- [学习如何使用数据湖存储区使用门户](data-lake-store-get-started-portal.md)
- [学习如何使用数据使用 PowerShell 的湖泊存储区](data-lake-store-get-started-powershell.md)  
