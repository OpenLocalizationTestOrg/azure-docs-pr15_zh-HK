<properties
   pageTitle="优化配置单元中 HDInsight 加快执行查询 |Microsoft Azure"
   description="了解如何在 HDInsight 的 Hadoop 为优化配置单元查询。"
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>在 HDInsight 的 Hadoop 为优化配置单元查询

默认情况下，不进行性能优化 Hadoop 群集。 本文介绍了几种最常见的配置单元的性能优化方法可以应用于我们的查询。

##<a name="scale-out-worker-nodes"></a>辅助节点扩展

增加的工作人员在群集中的节点数可以利用更多的 mappers 和变径并行运行。 有两种方法可以增加在 HDInsight 中的向外扩展︰

- 在调配资源时，您可以指定使用 Azure 门户、 Azure PowerShell 或跨平台命令行界面辅助节点的数目。  有关详细信息，请参阅[设置 HDInsight 群集](hdinsight-provision-clusters.md)。 下面的屏幕显示工作人员在 Azure 门户节点配置︰

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- 在运行时，您还可以群集扩张而不用重新创建一个。 如下所示。
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

由 HDInsight 支持的不同虚拟机上的更多详细信息，请参阅[HDInsight 定价](https://azure.microsoft.com/pricing/details/hdinsight/)。

##<a name="enable-tez"></a>启用 Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/)是 MapReduce 引擎替代执行引擎︰

![tez_1][image-hdi-optimize-hive-tez_1]


Tez 是更快，因为︰

- 作为单个作业 MapReduce 引擎中执行定向非循环图 (DAG) 时，表示 DAG 要求每个组的 mappers 应遵循一套变径。 这将导致多个 MapReduce 作业，来为每个配置单元查询旋转。 Tez 不具有此类约束，并且可以作为一个任务，这样就减少了作业启动开销处理复杂 DAG。
- **避免不必要的写入**由于正在旋转的 MapReduce 引擎中相同的配置单元查询的多个作业，每个作业的输出写入中间数据 HDFS。 由于 Tez 最小化作业的每个配置单元查询就可以避免不必要的写入数。
- **最小启动延迟**Tez 是更好地能够最小化启动延迟减少的 mappers 启动所需的数量，还可以提高整个优化。
- **重复使用的容器**只要可能 Tez 就能够重复使用集装箱，减少由于容器启动的延迟。
- **连续的优化技术**传统上在编译阶段进行优化。 但是有关所输入的详细信息，允许在运行时更好的优化。 Tez 使用连续优化技术，这使它可以优化计划进一步分解的运行阶段。

有关这些概念的详细信息，请单击[此处](http://hortonworks.com/hadoop/tez/)

您可以进行任何配置单元查询 Tez 由前缀查询与下面的设置启用︰

    set hive.execution.engine=tez;

对于基于 Windows 的 HDInsight 群集，必须在规定的时间启用 Tez。 以下是与 Tez 启用配置 Hadoop 群集的示例 Azure PowerShell 脚本︰

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] 基于 linux * 的 HDInsight 群集有 Tez，默认情况下启用。
    

## <a name="hive-partitioning"></a>配置单元划分

I/O 操作是运行配置单元查询的主要性能瓶颈。 如果可以减少需要读取的数据量，可以提高性能。 默认情况下，配置单元查询扫描配置单元中的所有表。 正如表扫描，但是对于那些只需要扫描的少量数据 （例如查询与筛选） 的查询，这将创建不必要的开销，这非常适合于查询。 配置单元分区允许配置单元查询访问只需要配置单元表中的数据的量。

分区配置单元是通过重组到新目录的原始数据与每个分区有其自己的目录的分区由用户实现。 下图说明了按列*年*分区配置单元表。 每年创建一个新目录。

![分区][image-hdi-optimize-hive-partitioning_1]

分区的一些注意事项︰

- **不要不不足的分区**的分区上只有几个值的列可能会导致极少的分区。 例如，分区在性别上将只创建两个分区 （男性和女性） 创建，因此只能减少滞后时间按最大值的一半。

- **过不分区**-在另一个极端，具有一个唯一的值 (如用户 id) 的列上创建一个分区将导致多个分区，因为它将不得不处理大量的目录上群集 namenode 导致大量的压力。

- **避免数据倾斜**的明智地选择您的分区键，以使所有分区大小相等。 一个示例分区上*状态*可能导致的下加利福尼亚州是几乎 30 记录数倍，由于人口中的差异的佛蒙特。

若要创建分区表，请使用*分区的*子句︰

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

创建已分区的表后，您可以创建静态分区或动态分区。

- **静态分区**意味着相应的目录中有的已经 sharded 数据，并可以要求配置单元分区手动基于目录的位置。 以下代码片段所示。

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **动态分区**，则意味着您想配置单元会自动为您创建分区。 因为我们已经从临时表创建分区表，我们需要做的所有是插入到已分区表的数据，如下所示︰

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

有关详细信息，请参阅[分区表](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables)。

##<a name="use-the-orcfile-format"></a>使用 ORCFile 的格式

配置单元支持不同的文件格式。 例如︰

- **文本**︰ 这是默认的文件格式，大多数情况下的合作
- **Avro**︰ 适用于互操作性方案
- **ORC/Parquet**︰ 最适用于性能

ORC （优化行纵栏式） 格式是一种高效的方法来存储配置单元数据。 与其他格式对比，ORC 具有以下优点︰

- 对于复杂类型，包括日期时间和复杂和半结构化类型的支持
- 高达 70%压缩
- 索引每隔 10000 行允许跳过行
- 在运行时执行明显下降

要启用 ORC 格式，首先应通过用子句*作为 ORC 的存储*创建一个表︰

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

接下来，您插入数据到 ORC 表从临时表。 例如︰

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

您可以阅读更多有关 ORC 格式[这里](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC)。

##<a name="vectorization"></a>向量化

向量化允许配置单元来处理一批 1024年行而一次处理一行不是在一起。 这意味着更快地完成简单的操作由于内部代码较少需要运行。

若要启用向量化前缀配置单元查询下列设置︰

    set hive.vectorized.execution.enabled = true;

有关详细信息，请参阅[Vectorized 查询执行](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution)。


##<a name="other-optimization-methods"></a>其他的优化方法

有更多的优化方法可以考虑，例如︰

- **配置单元桶︰**一种允许群集或分段大套的数据来优化查询性能。
- **加入优化︰**的配置单元的查询执行计划以提高连接的效率并减少用户提示需要优化。 有关详细信息，请参见[联接优化](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization)。
- **提高变径**

##<a id="nextsteps"></a>下一步行动
在本文中，您学习了几种常见的配置单元查询优化方法。 若要了解详细信息，请参阅下列文章︰

- [在 HDInsight 中使用 Apache 配置单元](hdinsight-use-hive.md)
- [使用 HDInsight 中的配置单元分析航班延迟数据](hdinsight-analyze-flight-delay-data.md)
- [Twitter 使用分析数据中 HDInsight 的配置单元](hdinsight-analyze-twitter-data.md)
- [分析传感器数据在 Hadoop 在 HDInsight 上使用该配置单元查询控制台](hdinsight-hive-analyze-sensor-data.md)
- [使用 HDInsight 配置单元来分析网站的日志](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
