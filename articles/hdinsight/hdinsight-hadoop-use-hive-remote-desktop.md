<properties
   pageTitle="在 HDInsight 中使用 Hadoop 配置单元和远程桌面 |Microsoft Azure"
   description="了解如何使用远程桌面连接到 HDInsight 的 Hadoop 群集，然后使用配置单元命令行界面来运行配置单元查询。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>使用 Hadoop 在 HDInsight 与远程桌面配置单元

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

在本文中，将了解如何使用远程桌面连接到一个 HDInsight 群集，然后使用配置单元的命令行界面 (CLI) 来运行配置单元查询。

> [AZURE.NOTE] 本文档不提供的示例中使用的 HiveQL 语句执行的操作的详细的说明。 有关此示例中使用的 HiveQL 的信息，请参阅[使用配置单元与 Hadoop HDInsight 上](hdinsight-use-hive.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* 基于 Windows 的 HDInsight (在 HDInsight 上的 Hadoop) 群集

* 运行 Windows 10、 窗口 8 或 Windows 7 的客户端计算机

##<a id="connect"></a>使用远程桌面连接

启用远程桌面 HDInsight 群集，然后按照下面的说明在[连接到 HDInsight 群集使用 RDP](hdinsight-administer-use-management-portal.md#rdp)连接到它。

##<a id="hive"></a>使用配置单元命令

当您已连接到 HDInsight 群集的桌面时，请使用以下步骤来处理的配置单元︰

1. 从 HDInsight 桌面， **Hadoop 命令行**的起点。

2. 输入以下命令以启动配置单元 CLI:

        %hive_home%\bin\hive

    当 CLI 已经启动时，您会看到配置单元 CLI 提示符︰ `hive>`。

3. 使用 CLI，输入以下语句，创建一个名为**log4jLogs** ，使用示例数据的新表︰

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    这些语句执行以下操作︰

    * **除去表**︰ 删除的表和数据文件，如果表已存在。

    * **创建外部表**︰ 在配置单元中创建一个新的外部表。 外部表将只表定义存储在配置单元 （将数据留在原来的位置）。

        > [AZURE.NOTE] 期望通过外部源 （如自动化的数据上载过程） 或另一个 MapReduce 操作，更新基础数据时，应使用外部表，但总要配置单元查询要使用的最新数据。
        >
        > 删除外部表 does**不**删除数据，仅表定义。

    * **行格式**︰ 告诉配置单元设置数据的格式。 在这种情况下，由空格分隔每个日志中的字段。

    * **存储作为文本文件位置**︰ 告诉配置单元数据的存储 （示例中的数据目录），它将存储为文本。

    * **选择**︰ 选择其中列**t4**包含**[错误]**的值的所有行的计数。 这应返回**3** ，因为有三行包含此值。

    * **像 '%.log' INPUT__FILE__NAME** -告诉配置单元，我们只应从以结尾的文件返回的数据。 日志。 这将搜索限制到 sample.log 文件中包含的数据，并防止它与我们所定义的架构不匹配的数据文件从其他示例返回数据。


4. 使用下面的语句创建名为**errorLogs**的新内部表︰

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    这些语句执行以下操作︰

    * **创建表如果不存在**︰ 如果不存在，则创建一个表。 由于不使用**外部**关键字，这是内部的表，它存储在配置单元数据仓库和完全由配置单元。

        > [AZURE.NOTE] 与**外部**表，删除内部表还会删除基础数据。

    * **存储 AS ORC**︰ 优化的行纵栏 (ORC) 格式存储的数据。 这是用于存储配置单元数据的高度优化和有效格式。

    * **插入覆盖...选择**: **log4jLogs**表中的包含**[错误]**，选择行，然后将数据插入到**errorLogs**表。

    要验证中列 t4 包含**[错误]**的行都存储到**errorLogs**表，请使用下面的语句从**errorLogs**返回的所有行︰

        SELECT * from errorLogs;

    三个数据的应返回的行，所有在列 t4 的包含**[错误]** 。

##<a id="summary"></a>摘要

正如您可以看到，配置单元命令提供了简便的方法来以交互方式运行配置单元查询 HDInsight 群集上，监视作业状态，检索输出。

##<a id="nextsteps"></a>下一步行动

HDInsight 中的配置单元有关的一般信息︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

如果您使用 Tez 配置单元，请参阅以下文档以供调试信息︰

* [在基于 Windows 的 HDInsight 上使用 Tez 用户界面](hdinsight-debug-tez-ui.md)

* [使用基于 Linux 的 HDInsight Ambari Tez 视图](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

