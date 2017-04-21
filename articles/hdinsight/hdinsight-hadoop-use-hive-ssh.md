<properties
   pageTitle="在 HDInsight (Hadoop) 中使用的配置单元外壳 |Microsoft Azure"
   description="了解如何使用基于 Linux 的 HDInsight 群集配置单元外壳。 您将学习如何连接到 HDInsight 群集使用 SSh，然后使用配置单元外壳程序以交互方式运行查询。"
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>使用 Hadoop 在 HDInsight 与 SSH 配置单元

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

在本文中，您将学习如何使用安全外壳协议 (SSH) 来连接到 Azure HDInsight 群集上 Hadoop，然后通过配置单元命令行界面 (CLI) 以交互方式提交配置单元查询。

> [AZURE.IMPORTANT] 在基于 Linux 的 HDInsight 群集上可用的配置单元命令时，您应该考虑使用 Beeline。 Beeline 是更新的客户端使用配置单元，并且包括 HDInsight 群集。 使用它的详细信息，请参阅[使用 Hadoop Beeline 与 HDInsight 中的配置单元](hdinsight-hadoop-use-hive-beeline.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* 基于 linux * 的 Hadoop，HDInsight 群集上。

* SSH 客户端。 Linux 和 Unix 中，Mac OS 应附带的 SSH 客户端。 Windows 用户必须下载客户端，如[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

##<a id="ssh"></a>用 SSH 连接

通过连接到的完全合格的域名称 (FQDN) 的 HDInsight 群集使用 SSH 命令。 FQDN 将使用的名称的群集，然后**。 azurehdinsight.net**。 例如，以下将连接到名为**myhdinsight**的群集︰

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**如果您提供 SSH 身份验证的证书密钥**的创建 HDInsight 群集时，您可能需要指定客户机系统上的私有密钥的位置︰

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**如果您提供的密码 SSH 身份验证**创建 HDInsight 群集时，您将需要提供密码提示时。

在 HDInsight 中使用 SSH 的详细信息，请参阅[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 和 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)。

###<a name="putty-windows-based-clients"></a>PuTTY （基于 Windows 的客户端）

Windows 不提供内置的 SSH 客户端。 我们建议使用**PuTTY**，这可以从[http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)下载。

使用 PuTTY 的详细信息，请参阅[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)。

##<a id="hive"></a>使用配置单元命令

2. 建立连接后，使用下面的命令启动配置单元 CLI:

        hive

3. 使用 CLI，输入以下语句，创建名为**log4jLogs** ，使用的样本数据的新表︰

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    这些语句执行以下操作︰

    * **除去表**-删除表和数据文件，以防表已存在。
    * **创建外部表**-在配置单元中创建一个新的外部表。 外部表只能在配置单元中存储的表定义。 将数据留在原来的位置。
    * **行格式**的说明配置单元设置数据的格式。 在这种情况下，由空格分隔每个日志中的字段。
    * **存储作为文本文件位置**-告诉配置单元数据的存储 （示例中的数据目录），并且它将存储为文本。
    * **选择**选择所有列**t4**其中包含值**[错误]**的行数。 随着三个包含此值的行，这应该返回值为**3** 。
    * **像 '%.log' INPUT__FILE__NAME** -告诉配置单元，我们只应从以结尾的文件返回的数据。 日志。 这将搜索限制到 sample.log 文件中包含的数据，并防止它与我们所定义的架构不匹配的数据文件从其他示例返回数据。

    > [AZURE.NOTE] 在预期基础数据更新的外部源，例如自动化的数据上载过程中，或者通过 MapReduce 的另一个操作，但却总是希望配置单元查询要使用的最新数据时，应使用外部表。
    >
    > 删除外部表 does**不**删除数据，仅表定义。

4. 使用下面的语句创建名为**errorLogs**的新内部表︰

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    这些语句执行以下操作︰

    * 如果不存在，则**创建表如果不存在**-创建一个表。 由于不使用**外部**关键字，则这种内部表，它存储在配置单元数据仓库和完全由配置单元。
    * **存储 AS ORC**的优化行纵栏式 (ORC) 格式存储的数据。 这是用于存储配置单元数据的高度优化和有效格式。
    * **插入覆盖...选择**-从**log4jLogs**表中包含**[错误]**，选择行，然后**errorLogs**表中插入数据。

    要验证仅行中列 t4 包含**[错误]**已存储到**errorLogs**表，请使用下面的语句从**errorLogs**返回的所有行︰

        SELECT * from errorLogs;

    三个数据的应返回的行，所有在列 t4 的包含**[错误]** 。

    > [AZURE.NOTE] 与外部表，除去内部的表将删除基础数据。

##<a id="summary"></a>摘要

如您所见，该配置单元命令提供了简便的方法来以交互方式运行配置单元查询 HDInsight 群集上，监视作业状态，检索输出。

##<a id="nextsteps"></a>下一步行动

HDInsight 中的配置单元的一般信息︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

如果您使用 Tez 配置单元，请参阅以下文档以供调试信息︰

* [在基于 Windows 的 HDInsight 上使用 Tez 用户界面](hdinsight-debug-tez-ui.md)

* [使用基于 Linux 的 HDInsight Ambari Tez 视图](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

