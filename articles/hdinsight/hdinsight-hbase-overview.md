<properties
    pageTitle="HDInsight 在 HBase 是什么？ |Microsoft Azure"
    description="HDInsight 在 Apache HBase 的简介，Hadoop 使用 NoSQL 数据库构建。 了解有关使用案例和 HBase 与其他 Hadoop 群集。"
    keywords="bigtable，nosql，hbase 是什么"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>什么是 HDInsight 在 HBase: NoSQL 数据库的 Hadoop 提供类似 BigTable 的能力

Apache HBase 是一个开源，NoSQL 数据库建立在 Hadoop 并仿效 Google BigTable。 HBase 提供随机访问和大量的非结构化和 semistructured 数据按列家族组织的 schemaless 数据库中的强一致性。

数据存储在一个表中，行，行中的数据按列族。 HBase 是一个意义上说，列或在其中存储的数据类型均不需要定义在使用它们之前的 schemaless 数据库。 开放源代码线性扩展以应对 petabytes 的数千个节点上的数据。 它可以依赖数据冗余、 批处理和 Hadoop 生态系统中的分布式应用程序提供的其他功能。

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>HBase 是如何实现在 Azure HDInsight 的？

HDInsight HBase 被作为集成到 Azure 环境管理群集。 群集配置数据将直接存储在 Azure Blob 存储，它提供低延迟和提高的弹性性能和成本选择中。 这使客户能够构建处理大型数据集，若要生成传感器和遥测数据存储的终点，数以百万计的服务并分析此数据与 Hadoop 作业的交互式网站。 HBase 和 Hadoop 是良好开端 Azure; 对于大数据项目特别是，它们可以使实时应用程序来处理大型数据集。

HDInsight 实现利用 HBase 提供自动分片表，用于读取和写入操作，并自动故障切换的强一致性的向外扩展体系结构。 通过在内存中缓存读取和写入流的高吞吐量的情况下提高性能。 网络虚拟资源调配还为 HDInsight HBase 提供。 有关详细信息，请参阅[设置 HDInsight 群集的 Azure 虚拟网络上] [hbase-provision-vnet]。

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>在 HDInsight HBase 中如何管理数据？

可以通过使用 HBase 中管理数据`create`， `get`， `put`，和`scan`HBase 外壳命令。 通过将数据写入到数据库`put`并使用读取`get`。 `scan`命令用于从多个表中的行中获取数据。 也可以使用 HBase C# api，该 API 提供了一个客户端库在 HBase REST API 管理数据。 HBase 数据库还可以通过使用配置单元进行查询。 这些编程模型的介绍，请参阅[开始使用 HBase Hadoop 在 HDInsight 与][hbase-get-started]。 协处理器也是可用的允许该主机数据库的节点中的数据处理。


## <a name="scenarios-use-cases-for-hbase"></a>方案︰ 使用 HBase 案例
创建规范的用例的 BigTable 的 （和引申，HBase） 是 web 搜索。 搜索引擎构建索引映射到包含这些 web 页的条款。 但有许多 HBase 是适合于其他使用情形 — — 其中有多个在这一节明细化。

- 键 / 值存储

    HBase 可以用作键 / 值存储区，并且适用于管理信息系统。 Facebook 为其邮件系统使用 HBase，它非常适合于存储和管理互联网通信。 WebTable 使用 HBase 搜索和管理网页中提取的表。

- 传感器数据

    HBase 是用于捕获增量从各种来源收集的数据。 这包括社会分析，时间序列的趋势和计数器，及时的交互式控制板和管理审核日志系统。 示例包括彭博美国交易商终端和打开时间序列数据库 (OpenTSDB)，它可以存储并提供对度量标准收集的有关服务器系统的运行状况。

- 实时查询

    [菲尼克斯](http://phoenix.apache.org/)是 Apache HBase 的 SQL 查询引擎。 访问作为 JDBC 驱动程序，并允许查询和使用 SQL 管理 HBase 表。

- HBase 作为平台

    应用程序可以通过将其作为数据存储运行在 HBase。 例如，菲尼克斯、 OpenTSDB、 Kiji 和土卫六。 应用程序还可以将集成使用 HBase。 示例包括配置单元、 小猪，Solr、 风暴、 Flume，Impala、 姿、 Ganglia，加钻。


##<a name="next-steps"></a>下一步行动

- [开始使用 Hadoop 在 HDInsight 中使用 HBase][hbase-get-started]
- [在 Azure 虚拟网络提供 HDInsight 群集] [hbase-provision-vnet]
- [在 HDInsight 中配置 HBase 复制](hdinsight-hbase-geo-replication.md)
- [分析与 HBase 中 HDInsight 的 Twitter 观点][hbase-twitter-sentiment]
- [使用 Maven 构建的 Java 应用程序使用 HBase HDInsight (Hadoop)][hbase-build-java-maven]

##<a name="see-also"></a>请参见

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable︰ 用于结构化数据的分布式的存储系统](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
