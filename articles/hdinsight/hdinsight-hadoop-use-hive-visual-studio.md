<properties
   pageTitle="配置 Visual Studio 为单元使用 Hadoop 工具查询 |Microsoft Azure"
   description="了解如何使用 Hadoop 使用 Visual Studio 的 Hadoop 工具 HDInsight 中的配置单元。"
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>运行配置单元查询对 Visual Studio 中使用 HDInsight 工具

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

在本文中，您将学习如何提交到 Visual Studio 为使用 HDInsight 工具 HDInsight 群集配置单元查询。

> [AZURE.NOTE] 本文档不提供的示例中使用的 HiveQL 语句执行的操作的详细的说明。 有关此示例中使用的 HiveQL 的信息，请参阅[使用配置单元与 Hadoop HDInsight 上](hdinsight-use-hive.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要。

* Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 (Linux 或基于 Windows 的)

* Visual Studio （以下版本之一）︰

    Visual Studio 2013年社区/专业/高级/旗舰版与[更新 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 年 （社区/企业）

- Visual studio 的 HDInsight 工具。 有关安装和配置这些工具的信息，请参阅[开始使用 Visual Studio 的 Hadoop HDInsight 工具](hdinsight-hadoop-visual-studio-tools-get-started.md)。

##<a id="run"></a>运行配置单元查询对 Visual Studio 中使用 HDInsight 工具

1. 打开**Visual Studio**并选择**新建** > **项目** > **HDInsight** > **配置单元的应用程序**。 提供此项目的名称。

2. 打开此项目，请创建的**Script.hql**文件并粘贴下面的 HiveQL 语句中︰

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    这些语句执行以下操作︰

    * **除去表**︰ 删除的表和数据文件，如果表已存在。
    * **创建外部表**︰ 在配置单元中创建一个新的外部表。 外部表只能存储在配置单元 （将数据留在原来的位置） 的表定义。

        > [AZURE.NOTE] 期望通过外部源 （如自动化的数据上载过程） 或另一个 MapReduce 操作，更新基础数据时，应使用外部表，但总要配置单元查询要使用的最新数据。
        >
        > 删除外部表 does**不**删除数据，仅表定义。

    * **行格式**︰ 告诉配置单元设置数据的格式。 在这种情况下，由空格分隔每个日志中的字段。
    * **存储作为文本文件位置**︰ 告诉配置单元数据的存储 （示例中的数据目录），它将存储为文本。
    * **选择**︰ 选择其中列**t4**包含**[错误]**的值的所有行的计数。 这应返回**3** ，因为有三行包含此值。
    * **像 '%.log' INPUT__FILE__NAME** -告诉配置单元，我们只应从以结尾的文件返回的数据。 日志。 这将搜索限制到 sample.log 文件中包含的数据，并防止它与我们所定义的架构不匹配的数据文件从其他示例返回数据。

3. 从工具栏上，选择您要使用此查询， **HDInsight 群集**，然后选择运行这些语句作为使用 WebHCat 配置单元作业**提交到 WebHCat** 。 您还可以提交作业使用的__HiveServer2 通过执行__按钮，如果群集版本 HiveServer2。 **配置单元作业摘要**并显示有关正在运行的作业的信息。 **刷新**链接用于刷新作业信息，直到将**作业状态**更改为**已完成**。

4. 使用**作业输出**链接可查看此作业的输出。 它应该显示`[ERROR] 3`，它是由 SELECT 语句返回的值。

5. 此外可以运行配置单元查询而无需创建一个项目。 使用**服务器资源管理器中**，展开**Azure** > **HDInsight**，HDInsight 服务器上，用鼠标右键单击，然后选择**编写配置单元的查询**。

6. 在**temp.hql**文档中显示，添加下面的 HiveQL 语句︰

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    这些语句执行以下操作︰

    * **创建表如果不存在**︰ 如果不存在，则创建一个表。 由于不使用**外部**关键字，这是内部的表，它存储在配置单元数据仓库和完全由配置单元。

        > [AZURE.NOTE] 与**外部**表，删除内部表还会删除基础数据。

    * **存储 AS ORC**︰ 优化的行纵栏 (ORC) 格式存储的数据。 这是用于存储配置单元数据的高度优化和有效格式。
    * **插入覆盖...选择**: **log4jLogs**表中的包含**[错误]**，选择行，然后将数据插入到**errorLogs**表。

7. 从工具栏上，选择**提交**运行作业。 **作业状态**用于确定作业已成功完成。

8. 要验证作业已完成，并创建一个新表，请使用**服务器资源管理器中**，展开**Azure** > **HDInsight** > HDInsight 群集 >**配置单元数据库**> 和**默认值**。 您应该看到**errorLogs**表和**log4jLogs** 。

##<a id="summary"></a>摘要

如您所见，Visual Studio 的 HDInsight 工具提供了简便的方法，对 HDInsight 群集运行配置单元查询，监视作业状态，并检索输出。

##<a id="nextsteps"></a>下一步行动

HDInsight 中的配置单元有关的一般信息︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

对于 Visual Studio HDInsight 工具的详细信息︰

* [快速入门 Visual Studio HDInsight 工具](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
