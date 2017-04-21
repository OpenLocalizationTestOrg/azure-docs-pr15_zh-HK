<properties
   pageTitle="MapReduce 和 Hadoop 在 HDInsight 中使用的远程桌面 |Microsoft Azure"
   description="了解如何使用远程桌面连接到 HDInsight 上的 Hadoop 并运行 MapReduce 作业。"
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>在与远程桌面的 HDInsight 上的 Hadoop 使用 MapReduce

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

在本文中，您将学习如何通过使用远程桌面连接到 HDInsight 群集上 Hadoop 并使用 Hadoop 命令运行 MapReduce 作业。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* 基于 Windows 的 HDInsight (在 HDInsight 上的 Hadoop) 群集

* 运行 Windows 10、 Windows 8 或 Windows 7 的客户端计算机

##<a id="connect"></a>使用远程桌面连接

启用远程桌面 HDInsight 群集，然后按照下面的说明在[连接到 HDInsight 群集使用 RDP](hdinsight-administer-use-management-portal.md#rdp)连接到它。

##<a id="hadoop"></a>使用 Hadoop 命令

当连接到 HDInsight 群集的桌面时，使用下列步骤使用 Hadoop 命令运行 MapReduce 作业︰

1. 从 HDInsight 桌面， **Hadoop 命令行**的起点。 这将打开一个新的命令提示符中**c:\apps\dist\hadoop-&lt;版本号码 >**目录。

    > [AZURE.NOTE] Hadoop 更新更改版本号。 **HADOOP_HOME**环境变量可以用于查找路径。 例如，`cd %HADOOP_HOME%`目录变为 Hadoop 目录中，而无需知道的版本编号。

2. 若要使用**Hadoop**命令运行示例 MapReduce 作业，请使用下面的命令︰

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    这将启动**hadoop 的 mapreduce examples.jar**文件在当前目录中包含的**字数统计**类。 它使用**wasbs://example/data/gutenberg/davinci.txt**文档作为输入，并输出存储在︰ **wasbs: / 示例/数据/WordCountOutput/**。

    > [AZURE.NOTE] 有关此 MapReduce 作业和示例数据的详细信息，请参阅<a href="hdinsight-use-mapreduce.md">使用 MapReduce HDInsight Hadoop 中</a>。

2. 当它被处理，并且返回信息类似于以下作业完成时，作业发出详细信息︰

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. 在作业完成时，使用以下命令列出存储在**wasbs://example/data/WordCountOutput**的输出文件︰

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    此时应显示两个文件、 **_SUCCESS**和**部件-r-00000**。 该**部件-r-00000**文件包含此作业的输出。

    > [AZURE.NOTE] 一些 MapReduce 作业可能跨多个**部件-r-###**文件分割结果。 如果是，请使用 # # # 后缀以指示文件的顺序。

4. 若要查看的输出，请使用下面的命令︰

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    这将显示在**wasbs://example/data/gutenberg/davinci.txt**文件中，每个单词出现的次数以及所包含的单词的列表。 以下是文件中将包含的数据的示例︰

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>摘要

如您所见，Hadoop 命令提供了一种简便方法在 HDInsight 群集上运行 MapReduce 作业，然后查看作业输出。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 中的 MapReduce 作业有关的一般信息︰

* [在 HDInsight Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)
