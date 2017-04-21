<properties
    pageTitle="使用 HDInsight 中的交互式配置单元 |Microsoft Azure"
    description="在 HDInsight 中学习如何使用交互式配置单元 （在 LLAP 配置单元）。"
    keywords=""
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
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>使用交互式配置单元中 HDInsight （预览）

交互式 （也配置单元 [实时的长时间和过程]( https://cwiki.apache.org/confluence/display/Hive/LLAP)）是一个新的 HDInsight[群集类型]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types)。  在内存缓存，更具交互性和更快地使配置单元查询允许交互式配置单元。 这一新功能使 HDInsight 世界上大多数的性能，灵活，并打开与内存中云上的大数据解决方案缓存 （使用配置单元和触发） 和高级分析通过深度集成与 R 服务。 

交互式配置单元群集与 Hadoop 群集不同。 它只包含配置单元服务。 

> [AZURE.NOTE] MapReduce、 小猪、 Sqoop、 Oozie 和其他服务不久将被删除从该群集类型。
在交互式配置单元群集配置单元服务才可通过 Ambari 配置单元视图、 Beeline 和配置单元 ODBC 访问。 通过配置单元控制台、 Templeton，Azure CLI 和 Azure PowerShell 无法访问它。 


 


## <a name="create-an-interactive-hive-cluster"></a>创建交互式配置单元群集

交互式配置单元群集仅支持基于 linux * 的群集上。 有关创建 HDInsight 群集的信息，请参阅[HDInsight 中的基于创建 Linux 的 Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)。


## <a name="execute-hive-from-interactive-hive"></a>从交互式配置单元执行配置单元

有不同的方式，您可以执行配置单元查询选项︰

- 运行配置单元使用 Ambari 配置单元视图

    有关使用配置单元视图的信息，请参阅[使用 Hadoop HDInsight 在配置单元视图]( hdinsight-hadoop-use-hive-ambari-view.md)。

- 运行配置单元使用 Beeline

    对使用 HDInsight Beeline 上的信息，请参阅[使用 Hadoop Beeline 与 HDInsight 中的配置单元](hdinsight-hadoop-use-hive-beeline.md)。

    使用 Beeline 从 headnode 或空的边缘节点。  建议从空边节点使用 Beeline。  创建空的 edgenode 与 HDInsight 群集的信息，请参阅[HDInsight 中的使用空白边缘节点](hdinsight-apps-use-edge-node.md)。

- 运行配置单元使用 ODBC 配置单元

    使用 ODBC 配置单元的信息，请参阅[连接到 Microsoft 配置单元的 ODBC 驱动程序使用 Hadoop 的 Excel](hdinsight-connect-excel-hive-odbc-driver.md)。

**若要查找的 JDBC 连接字符串︰**

1.  登录到 Ambari 使用下面的 URL: https://<ClusterName>。AzureHDInsight.net。
2.  单击左侧菜单中的**配置单元**。
3.  请单击突出显示图标可复制的 URL:

    ![HDInsight Hadoop LLAP JDBC 交互式配置单元](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>请参见
-   [在 HDInsight 中的创建 Linux 基于 Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)︰ 了解如何在 HDInsight 中创建交互式配置单元簇。
-   [使用 Hadoop Beeline 与 HDInsight 中的配置单元](hdinsight-hadoop-use-hive-beeline.md)︰ 了解如何使用 Beeline 提交配置单元查询。
-   [将 Excel 连接到 Hadoop 使用 Microsoft 配置单元的 ODBC 驱动程序](hdinsight-connect-excel-hive-odbc-driver.md)︰ 了解如何将 Excel 连接到的配置单元。
