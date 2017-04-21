 <properties
    pageTitle="介绍 Hadoop 的 Hadoop HDInsight 上的是什么？ |Microsoft Azure"
    description="在 HDInsight 上云中获取 Hadoop，分布式大数据处理和分析，以及 Hadoop 生态系统组成部分的简介。"
    keywords="大数据分析，hadoop，何谓 hadoop，hadoop 技术堆栈，hadoop 生态系统简介"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>在 Azure HDInsight 的 Hadoop 生态系统简介

 本文介绍 Hadoop 在 Azure HDInsight，它的生态系统和大数据。 了解有关 Hadoop 组件、 常用术语和大数据分析的方案。

## <a name="what-is-hadoop-on-hdinsight"></a>在 HDInsight 上的 Hadoop 是什么？

Hadoop 是指生态系统的开放源码软件是一个框架，用于分布式处理、 存储和分析大数据集的计算机群集上。 Azure HDInsight 使从**Hortonworks 数据平台 (HDP)**分发的 Hadoop 组件可以在云中，并部署和调配管理的群集具有高可靠性和可用性。  

Apache Hadoop 是大数据处理的原始的开源项目。 下面是开发相关的软件和实用程序视为 Hadoop 技术堆栈上，其中包括 Apache 配置单元、 Apache HBase、 Apache 触发和许多其他人的一部分。 有关详细信息，请参阅[HDInsight 中的 Hadoop 生态系统的概述](#overview)。

## <a name="what-is-big-data"></a>重要数据是什么？

大数据描述任何大量的数字信息，Twitter 送入工业设备，在网站上浏览的客户和购买信息的传感器信息中的文本。 大数据可以是历史 （即存储的数据） 或实时 （即直接从源传输）。 大数据正在收集和扩展以越来越快的速度，不断增加的量中各种不同的格式。

对于大数据提供可操作的智能或见解，必须收集相关数据，并提出正确的问题。 您还必须确保数据可以访问，清理、 分析，并提交以有用的方式。 这是有助于在 Hadoop 在 HDInsight 上的大数据分析。

## <a name="overview"></a>在 HDInsight 的 Hadoop 生态系统的概述

HDInsight 是在 Microsoft Azure 云分布大数据分析的快速膨胀 Apache Hadoop 技术堆栈。 它包括实现 Apache 触发，HBase、 风暴、 小猪、 配置单元、 Sqoop、 Oozie、 Ambari，等等。 HDInsight 还集成了电源 BI、 Excel、 SQL Server Analysis Services 和 SQL Server 报表服务的商业智能 (BI) 工具。

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop，HBase、 触发、 占领和自定义的群集

HDInsight 提供了 Apache Hadoop，触发，HBase 或电流冲击引起的群集配置。 或者，您可以[自定义群集使用脚本的操作](hdinsight-hadoop-customize-cluster-linux.md)。

* **Hadoop**: [HDFS](#hdfs)，和简单的[MapReduce](#mapreduce)编程模型来处理和分析数据，同时提供可靠的数据存储。

* **<a target="_blank" href="http://spark.apache.org/">Apache 触发</a>**︰ 一个支持内存中处理，以提高大数据分析的应用程序的性能的并行处理框架的 SQL，流数据，激励的工作原理和机器学习。 请参阅[概述︰ 什么是 Apache 触发中 HDInsight？](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**︰ 建立在大量的非结构化和半结构化数据的潜在数十亿行提供随机访问和强一致性的 Hadoop 使用 NoSQL 数据库时间数以百万计的列。 [HBase HDInsight 上的概述](hdinsight-hbase-overview.md)，请参阅。

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache 风暴</a>**︰ 分布式、 实时计算系统用于快速处理大数据的流。 作为托管群集在 HDInsight 中提供风暴。 请参阅[使用风暴和 Hadoop 分析实时传感器数据](hdinsight-storm-sensor-data-analysis.md)。

### <a name="example-customization-scripts"></a>示例自定义脚本

脚本操作是运行在群集资源调配，并可以用于在群集上安装其他组件的脚本。 对于基于 Linux 的群集中，这些都是 Bash 脚本。

HDInsight 团队提供了以下示例脚本︰

* [色调](hdinsight-hadoop-hue-linux.md)︰ 用于与群集进行交互的 web 应用程序的一组。 Linux 群集。

* [Giraph](hdinsight-hadoop-giraph-install-linux.md)︰ 图形处理对事物或人之间的关系进行建模。

* [R](hdinsight-hadoop-r-scripts-linux.md): 开放源码语言和统计计算，采用机器学习的环境。

* [Solr](hdinsight-hadoop-solr-install-linux.md): 企业级搜索平台，允许对数据进行全文搜索。

开发您自己的脚本操作的信息，请参阅[使用 HDInsight 脚本操作开发](hdinsight-hadoop-script-actions-linux.md)。


## <a name="what-are-the-hadoop-components-and-utilities"></a>Hadoop 组件和工具是什么？

以下组件和实用程序都包括在 HDInsight 群集。

* **[Ambari](#ambari)**︰ 群集资源调配、 管理、 监视和实用程序。

* **[Avro](#avro)**（Avro Microsoft.NET 库）︰ Microsoft.NET 环境的数据序列化。

* **[配置单元和 HCatalog](#hive)**︰ 结构化查询语言 (SQL) 的查询，和表和存储的管理层等。

* **[Mahout](#mahout)**︰ 为可扩展的机器学习的应用程序。

* **[MapReduce](#mapreduce)**︰ 旧的框架，用于 Hadoop 分布式处理和资源管理。 请参阅[YARN](#yarn)下, 一代的资源框架。

* **[Oozie](#oozie)**︰ 工作流管理。

* **[菲尼克斯](#phoenix)**︰ 对 HBase 关系数据库层。

* **[小猪](#pig)**︰ MapReduce 转换的简单脚本。

* **[Sqoop](#sqoop)**︰ 数据导入和导出。

* **[Tez](#tez)**︰ 允许数据密集型流程在比例上高效运行。

* **[YARN](#yarn)**: Hadoop 核心库和下一代的 MapReduce 软件框架的一部分。

* **[ZooKeeper](#zookeeper)**︰ 协调分布式系统中的进程。

> [AZURE.NOTE] 有关特定组件的信息和版本信息，请参阅[Hadoop 组件、 版本控制和服务产品的 HDInsight][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari 是资源调配、 管理和监控 Apache Hadoop 群集。 它包括一个直观的运算符工具集合和强大的一组 Api，这些隐藏的 Hadoop，简化的群集操作的复杂性。 基于 Linux 的 HDInsight 群集提供了 Ambari 的 web UI 和 Ambari REST API，而基于 Windows 群集提供了 REST API 的一个子集。 Ambari HDInsight 群集上的视图允许插件的用户界面功能。

请参阅[使用 Ambari 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari.md)(仅适用于 Linux)、 [HDInsight 使用 Ambari API 中的监视 Hadoop 群集](hdinsight-monitor-use-ambari-api.md)和<a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API 参考</a>。

### <a name="avro"></a>Avro （Avro Microsoft.NET 库）

Microsoft.NET 库的 Avro 实现 Microsoft.NET 环境的序列化 Apache Avro 压缩的二进制数据交换格式。 它使用<a target="_blank" href="http://www.json.org/">JavaScript 对象符号 (JSON)</a>来定义欠载而语言互操作性的语言无关架构，可以在另一个阅读意味着一种语言中序列化的数据。 格式的详细的信息可以在中找到 < 目标 = _"空白"href ="http://avro.apache.org/docs/current/spec.html"> Apache Avro 规范</a>。
Avro 文件格式支持分布式的 MapReduce 编程模型。 文件是"可拆分图形"，即搜寻文件中的任何位置，并从特定块开始读取。 若要了解具体步骤，请参阅[Avro 的 Microsoft.NET 库与序列化数据](hdinsight-dotnet-avro-serialization.md)。


### <a name="hdfs"></a>HDFS

Hadoop 分布式文件系统 (HDFS) 是一个分布式的文件系统，使用 MapReduce 和 YARN，Hadoop 生态系统的核心。 HDFS 是 HDInsight 上的 Hadoop 群集的标准文件系统。

### <a name="hive"></a>配置单元和 HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache 配置单元</a>是数据仓库软件建立在 Hadoop，您可以查询和管理分布式存储的大型数据集，通过使用一种称作 HiveQL 的类似于 SQL 的语言。 配置单元时，小猪，像是在 MapReduce 抽象。 运行时，配置单元将查询转换成一系列的 MapReduce 作业。 配置单元概念上更接近于关系数据库管理系统比猪，，因此适合用于更结构化的数据。 对于非结构化数据，小猪是更好的选择。 请参阅[使用 Hadoop HDInsight 在蜂巢](hdinsight-use-hive.md)。

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a>是用于向用户提供数据的关系视图的 Hadoop 的表和存储管理图层。 在 HCatalog，可以读取和写入文件，可以为其编写配置单元的 SerDe （序列化程序的反序列化程序） 任何格式。

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a>是机器的可伸缩学习算法在 Hadoop 上运行库。 机器学习应用程序使用的统计原则，教系统从数据中学习并使用过去的结果来确定未来的行为。 查看[生成的电影推荐使用 Hadoop 在 Mahout](hdinsight-mahout.md)。

### <a name="mapreduce"></a>MapReduce
MapReduce 是 Hadoop 的旧版软件框架编写批处理进程并行的大数据集的应用程序。 MapReduce 作业拆分大型数据集并将数据组织为处理的键 / 值对。

[YARN](#yarn) Hadoop 的新一代资源管理器和应用程序框架，也被称为 MapReduce 2.0。 在 YARN 运行 MapReduce 作业。

MapReduce 的详细信息，请参阅在 Hadoop Wiki <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> 。

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a>是管理 Hadoop 作业的工作流的协调系统。 它与 Hadoop 堆栈集成在一起，并用于 MapReduce、 小猪、 配置单元时，和 Sqoop 支持 Hadoop 作业。 它还用于计划作业系统，如 Java 程序或 shell 脚本所特有。 请参阅[使用 Hadoop 使用 Oozie](hdinsight-use-oozie.md)。

### <a name="phoenix"></a>菲尼克斯
<a  target="_blank" href="http://phoenix.apache.org/">Apache 菲尼克斯</a>是对 HBase 关系数据库图层。 Phoenix 包括允许用户查询和管理 SQL 表直接 JDBC 驱动程序。 菲尼克斯转换查询和本机 NoSQL API 调用-而不是使用 MapReduce-从而使 NoSQL 的存储区上更快地应用到其它语句。 请参阅[使用 Apache 菲尼克斯和团队使用 HBase 群集](hdinsight-hbase-phoenix-squirrel.md)。


### <a name="pig"></a>小猪
<a  target="_blank" href="http://pig.apache.org/">猪的 Apache</a>是允许您通过使用一种简单的脚本语言，称为猪的拉丁语在非常大的数据集上执行复杂的 MapReduce 转换的高层次平台。 小猪转化猪的拉丁脚本使其将运行 Hadoop 内。 您可以创建用户定义函数 (Udf) 来扩展猪的拉丁语。 请参阅[使用 Hadoop 使用猪](hdinsight-use-pig.md)。

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a>是传输大量数据之间 Hadoop 和关系数据库这种类型的 SQL 或其他结构化的数据存储，尽可能有效地工具。 请参阅[使用 Hadoop 使用 Sqoop](hdinsight-use-sqoop.md)。

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a>是在执行常规的数据处理的复杂、 非循环图的 Hadoop YARN 上构建应用程序框架。 它是允许数据密集型流程，如配置单元，按比例更高效地运行 MapReduce 框架的更灵活、 更强大后继。 请参阅["使用 Apache Tez 以提高性能"中使用配置单元和 HiveQL](hdinsight-use-hive.md#usetez)。

### <a name="yarn"></a>YARN
Apache YARN MapReduce （MapReduce 2.0 或 MRv2） 的下一代并且支持以外 MapReduce 批处理更大可伸缩性和实时处理的数据处理方案。 YARN 提供资源管理和分布式应用程序框架。 在 YARN 运行 MapReduce 作业。

要了解有关 YARN，查看<a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>。


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a>通过共享数据寄存器 (znodes) 的分层命名空间中大型分布式系统协调进程。 Znodes 包含少量的协调进程所需的元信息︰ 状态、 位置、 配置，等等。

## <a name="programming-languages-on-hdinsight"></a>在 HDInsight 的编程语言

HDInsight 群集-Hadoop，HBase 冲击，且触发群集-支持多种编程语言，但有些不默认安装的。 对于库、 模块或默认情况下未安装的软件包，使用脚本操作安装该组件。 请参阅[使用 HDInsight 脚本操作开发](hdinsight-hadoop-script-actions-linux.md)。

### <a name="default-programming-language-support"></a>默认编程语言支持

默认情况下，HDInsight 群集支持︰

* Java

* Python

可以安装其他语言，使用脚本操作︰[使用 HDInsight 脚本操作开发](hdinsight-hadoop-script-actions-linux.md)。

### <a name="java-virtual-machine-jvm-languages"></a>Java 虚拟机 (JVM) 的语言

可以运行许多非 Java 语言，使用 Java 虚拟机 (JVM);但是，运行这些语言的一些可能需要在群集上安装附加组件。

在 HDInsight 群集支持这些基于 JVM 的语言︰

* Clojure

* Jython (对于 Java 的 Python)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoop 特定语言

HDInsight 群集支持以下语言所特有的 Hadoop 生态系统︰

* 猪的拉丁语的猪的作业

* 配置单元和 SparkSQL HiveQL


## <a name="advantage"></a>云中的 Hadoop 的优点

作为 Azure 云生态系统的一部分，HDInsight 在 Hadoop 提供多种好处，其中包括︰

* 自动资源调配的 Hadoop 群集。 HDInsight 群集是比手动配置 Hadoop 群集创建容易得多。 有关详细信息，请参阅[HDInsight 中的规定 Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)。

* 先进的 Hadoop 组件。 有关详细信息，请参阅[Hadoop 组件、 版本控制和服务产品的 HDInsight][component-versioning]。

* 高可用性和可靠性的群集。 有关详细信息，请参阅[HDInsight 中的 Hadoop 群集的可用性和可靠性](hdinsight-high-availability-linux.md)。

* 效率高和经济数据存储使用 Azure Blob 存储，Hadoop 兼容选项。 详细信息，请参阅[HDInsight 中的 Hadoop 使用 Azure Blob 存储](hdinsight-hadoop-use-blob-storage.md)。

* 与其他 Azure 服务，包括[Web 应用程序](https://azure.microsoft.com/documentation/services/app-service/web/)和[SQL 数据库](https://azure.microsoft.com/documentation/services/sql-database/)的集成。

* 其他 VM 大小和类型的运行 HDInsight 群集。 请参阅[Hadoop 组件、 版本控制和服务产品的 HDInsight] [component-versioning]的详细信息。

* 群集的扩展。 群集扩展，使您可以更改正在运行的 HDInsight 群集中的节点数而无需删除或重新创建它。

* 虚拟的网络支持。 HDInsight 群集可以使用 Azure 虚拟网络用来支持隔离的云资源或链接与您的数据中心中的云资源的混合方案。

* 成本低廉的入门。 开始[免费试用版](https://azure.microsoft.com/free/)，或[HDInsight 定价的详细信息](https://azure.microsoft.com/pricing/details/hdinsight/)，请参阅。

若要阅读更多关于 Hadoop 在 HDInsight 上的优势，请参阅[HDInsight Azure 功能页][marketing-page]。

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight 标准和 HDInsight 特优

HDInsight 提供了在两个类别标准和特优的大数据云服务。 HDInsight 标准提供了企业级群集的组织可以使用来运行其大数据工作量。 HDInsight 高级生成，并提供高级分析和 HDInsight 群集的安全功能。 有关详细信息，请参阅[Azure HDInsight 津贴](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>了解更多关于大数据分析，Hadoop 和 HDInsight 的资源

基于这一简介 Hadoop 在云和大数据分析与下面的资源。


### <a name="hadoop-documentation-for-hdinsight"></a>HDInsight 的 Hadoop 文档

* [HDInsight 文档](https://azure.microsoft.com/documentation/services/hdinsight/)︰ 文档页中 Hadoop 在 Azure HDInsight 链接到文章、 视频和更多的资源。

* [开始使用 Hadoop 在 HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): HDInsight Hadoop 群集资源调配和运行查询的示例配置单元的快速启动教程。

* [开始使用 HDInsight 中触发](hdinsight-apache-spark-jupyter-spark-sql.md)︰ 快速启动教程，说明如何创建触发群集和运行交互式触发 SQL 查询。

* [在 HDInsight 上使用 R 服务器](hdinsight-hadoop-r-server-get-started.md)︰ 开始使用 R 服务器在 HDInsight 津贴。

* [条款 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md)︰ 了解如何通过在 Azure 的门户，Azure CLI 或 Azure PowerShell HDInsight Hadoop 群集配置。


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>︰ 了解更多有关 Apache Hadoop 软件库，用于分布式处理大型数据集允许跨群集的计算机的一个框架。

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>︰ 了解的体系结构和设计的 Hadoop 分布式文件系统，使用 Hadoop 应用程序的主存储系统。

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce 教程</a>︰ 了解更多有关编写 Hadoop 快速处理大量的数据并行计算节点的大型群集上的应用程序的编程框架。


### <a name="microsoft-business-intelligence"></a>Microsoft 商业智能

熟悉的商业智能 (BI) 工具-例如 Excel、 PowerPivot、 SQL Server Analysis Services，以及 SQL Server 报表服务的检索、 分析和报告数据与 HDInsight 集成使用电源查询外接程序或 Microsoft 配置单元 ODBC 驱动程序。

在大数据分析中，可以帮助这些 BI 工具︰

* [连接到电源查询使用 Hadoop 的 Excel](hdinsight-connect-excel-power-query.md)︰ 学习如何将 Excel 连接到 Azure 存储帐户存储 excel 使用 Query 电源与 HDInsight 群集关联的数据。 所需的 Windows 工作站。 使用基于 Windows 或 Linux 的群集。

* [将 Excel 连接到 Hadoop 使用 Microsoft 配置单元 ODBC 驱动程序](hdinsight-connect-excel-hive-odbc-driver.md)︰ 了解如何使用 Microsoft 的配置单元 ODBC 驱动程序的 HDInsight 从导入数据。 所需的 Windows 工作站。 使用基于 Windows 或 Linux 的群集。

* [微软云平台](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx)︰ 了解 Office 365 电源 BI、 下载 SQL Server 试用版，并设置 SharePoint Server 2013 和 SQL Server BI。

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)。

* [报表服务的 SQL Server](http://msdn.microsoft.com/library/ms159106.aspx)。




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
