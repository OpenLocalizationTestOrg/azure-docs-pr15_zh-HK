<properties
   pageTitle="MapReduce 和 SSH 连接使用 Hadoop 在 HDInsight |Microsoft Azure"
   description="了解如何使用 SSH 来运行在 HDInsight 上使用 Hadoop 的 MapReduce 作业。"
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>HDInsight 使用 SSH 在 Hadoop 使用 MapReduce

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

在本文中，您将学习如何使用安全外壳协议 (SSH) 连接到 Hadoop HDInsight 群集上，然后将 MapReduce 作业提交使用 Hadoop 命令。

> [AZURE.NOTE] 如果您已熟悉如何使用 Hadoop 的基于 Linux 的服务器，但您还不熟悉 HDInsight，请参阅[基于 Linux 的 HDInsight 提示](hdinsight-hadoop-linux-information.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* 基于 linux * 的 HDInsight (在 HDInsight 上的 Hadoop) 群集

* SSH 客户端。 Linux 和 Unix 中，Mac 操作系统应附带的 SSH 客户端。 Windows 用户必须下载客户端，如[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

##<a id="ssh"></a>用 SSH 连接

通过连接到的完全合格的域名称 (FQDN) 的 HDInsight 群集使用 SSH 命令。 FQDN 将跟群集使用的名称**。 azurehdinsight.net**。 例如，以下将连接到名为**myhdinsight**的群集︰

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**如果您提供 SSH 身份验证的证书密钥**的创建 HDInsight 群集时，您可能需要在客户端系统上，指定专用密钥的位置，例如︰

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**如果您提供的密码 SSH 身份验证**创建 HDInsight 群集时，您将需要提供密码提示时。

在 HDInsight 中使用 SSH 的详细信息，请参阅[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 和 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)。

###<a name="putty-windows-clients"></a>PuTTY （Windows 客户端）

Windows 不提供内置的 SSH 客户端。 我们建议使用**PuTTY**，这可以从[http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)下载。

使用 PuTTY 的详细信息，请参阅[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)。

##<a id="hadoop"></a>使用 Hadoop 命令

1. 您已连接到 HDInsight 群集后，使用下面的**Hadoop**命令启动 MapReduce 作业︰

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    这将启动**hadoop 的 mapreduce 的 examples.jar**文件中包含的**字数统计**类。 它使用**wasbs://example/data/gutenberg/davinci.txt**文档作为输入，并输出存储在**wasbs: / 示例/数据/WordCountOutput/**。

    > [AZURE.NOTE] 有关此 MapReduce 作业和示例数据的详细信息，请参阅[使用 MapReduce Hadoop 在 HDInsight 中](hdinsight-use-mapreduce.md)。

2. 因为它所处理的并且返回信息类似于以下在作业完成时，作业发出详细信息︰

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. 当作业完成时，请使用以下命令列出存储在**wasbs://example/data/WordCountOutput**的输出文件︰

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    此时应显示两个文件、 **_SUCCESS**和**部件-r-00000**。 该**部件-r-00000**文件包含此作业的输出。

    > [AZURE.NOTE] 一些 MapReduce 作业可能跨多个**部件-r-###**文件分割结果。 如果是，请使用 # # # 后缀以指示文件的顺序。

4. 若要查看的输出，请使用下面的命令︰

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    这将显示**wasbs://example/data/gutenberg/davinci.txt**文件和每个单词出现的次数中所包含的单词的列表。 以下是文件中将包含的数据的示例︰

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>摘要

如您所见，Hadoop 命令提供了一种简便方法 HDInsight 群集中运行 MapReduce 作业，然后查看作业输出。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 中的 MapReduce 作业有关的一般信息︰

* [在 HDInsight Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)
