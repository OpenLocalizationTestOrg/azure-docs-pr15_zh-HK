<properties
    pageTitle="在基于 Linux 的 HDInsight 上运行 Hadoop MapReduce 样本 |Microsoft Azure"
    description="开始使用基于 Linux 的 HDInsight 使用 MapReduce 样本。 使用 SSH 连接到该群集，然后使用 Hadoop 命令来运行示例作业。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>在 HDInsight 中运行 Hadoop 示例

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

基于 linux * 的 HDInsight 群集提供一套可用于运行 Hadoop MapReduce 作业为自己熟悉的 MapReduce 样本。 在本文中，您了解有关可用的示例并引导运行其中的一些。

##<a name="prerequisites"></a>系统必备组件

- **Azure 订阅**︰[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)，请参阅

- **基于 linux * 的 HDInsight 群集**︰[开始使用 Hadoop 使用 Linux 上的 HDInsight 中的配置单元](hdinsight-hadoop-linux-tutorial-get-started.md)，请参阅

- **SSH 客户端**︰ 在 HDInsight 中使用 SSH 的信息，请参阅下列文章︰

    - [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>这些示例 ##

**位置**︰ 样本均位于位于**/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**的 HDInsight 群集

**内容**︰ 该存档文件中包含以下示例︰

- **aggregatewordcount**: 聚合基于地图/减少程序计算的输入文件中的单词
- **aggregatewordhist**: 聚合基于地图/减少程序计算直方图的输入文件中的单词
- **bbp**︰ 贝利-Borwein-但用于计算 Pi 的确切数字地图/减少程序
- **dbcount**︰ 计算用于日志存储在中的示例作业数据库
- **distbbp**︰ 使用 BBP 型公式来计算 Pi 的准确位的地图/减少程序
- **grep**︰ 地图/减少程序计数输入中的正则表达式匹配项
- **联接**︰ 影响联接的作业超过排序、 平均分区数据集
- **multifilewc**︰ 对多个文件中的单词进行计数的作业
- **pentomino**︰ 地图/减少平铺排列程序查找 pentomino 问题的解决方案
- **pi**︰ 地图/减少程序，估算 Pi 使用准蒙特卡罗方法
- **randomtextwriter**︰ 写入 10 GB 的每个节点的随机文本数据的地图/减少程序
- **randomwriter**︰ 写入 10 GB 的每个节点的随机数据的地图/减少程序
- **secondarysort**︰ 定义降低到次要排序示例
- **排序**︰ 排序随机写入器写入的数据的地图/减少程序
- **数独**︰ 独规划求解
- **teragen**︰ 生成 terasort 的数据
- **terasort**︰ 运行 terasort
- **teravalidate**︰ 检查 terasort 的结果
- **字数统计**︰ 统计输入文件中的单词的地图/减少程序
- **wordmean**︰ 地图/减少程序计算的输入文件中的单词的平均长度
- **wordmedian**︰ 计算的输入文件中的单词的中间长度的地图/减少程序
- **wordstandarddeviation**︰ 计算的输入文件中的单词长度的标准差的地图/减少程序

**源代码**︰ 这些示例的源代码包含在位于**/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**的 HDInsight 群集

> [AZURE.NOTE] `2.2.4.9-1`路径中是 HDInsight 群集中，Hortonworks 数据平台的版本，并且可以更改更新 HDInsight。

## <a name="how-to-run-the-samples"></a>如何运行示例 ##

1. 连接到 HDInsight 使用 SSH 在下列文章中所述︰

    - [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 从`username@#######:~$`提示，使用下面的命令列出了示例︰

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    从本文的前面部分生成示例的列表。

3. 使用下面的命令以获取有关特定示例的帮助。 在此情况下，**字数统计**示例︰

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    您应该收到以下消息︰

        Usage: wordcount <in> [<in>...] <out>

    这表示您可以提供几个输入的路径的源文档。 最终的路径是，在哪里存储输出 （在源文档中的单词数）。

4. 使用以下方法来计算笔记本的列奥纳多达芬奇，作为示例数据提供与您的群集中的所有单词︰

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    输入此作业是从**wasbs:///example/data/gutenberg/davinci.txt**中读取。

    本示例存储在输出**wasbs: / 示例/数据/davinciwordcount/**。

    > [AZURE.NOTE] 字数统计示例帮助文件中所述，您还可以指定多个输入的文件。 例如，`hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount`就会算 davinci.txt 和 ulysses.txt 中的单词。

5. 完成作业后，可以使用下面的命令以查看输出︰

        hdfs dfs -cat /example/data/davinciwordcount/*

    此连接作业，所产生的所有输出文件并显示它们。 对于本例中基本没有只有一个文件，但是如果有更多此命令将循环访问所有这些。

    输出内容类似于如下︰

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    每行代表一个单词和输入数据中发生的次数。

## <a name="sudoku"></a>数独

数独的示例已有些无用的使用说明;"包含在命令行上的谜题"。

[数独](https://en.wikipedia.org/wiki/Sudoku)是九 3 x 3 网格组成的拼图。 在网格中的某些单元格具有数字，其他人都是空白，而目标是要解决对空白单元。 上面的链接了解有关解决难题，但此示例的目的是解决对空白单元。 因此我们输入应以下列格式的文件︰

- 九的九个列的行

- 每个列可以包含任意数量或`?`（表示空白单元格）

- 由空格分隔单元格

还有某些方法来构造数独谜题;不能重复的列或行中的数字。 在构造正确的 HDInsight 群集上没有示例。 它位于**/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** ，内容如下︰

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] `2.2.4.9-1` HDInsight 群集进行更新时，可能会改变路径的一部分。

若要运行此独示例，请使用下面的命令︰

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

结果应类似于以下内容︰

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>Pi （π）

Pi 示例使用统计 (准蒙特卡罗) 方法来估计 pi 的值。 点随机放置在一个单元内方还范围内记录该正方形内圆，面积等于是概率圆 pi/4。 Pi 的值可以从 4R，其中 R 是在正方形内的点的总数在圆内的点数目的比率值估计。 使用点的样本更大，更好地估计是。

本例中的映射生成大量的随机在单位正方形内的点，然后计算在圆内的点的数目。

变径然后累积点由 mappers 计数，并估算公式 4R，其中 R 是计在正方形内的点的总数在圆内的点数目的比率从 pi 的值。

使用下面的命令来运行此示例。 这 10000000 样例使用 16 映射来估计 pi 的值︰

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

此返回值应该是类似于**3.14159155000000000000**。 对于引用，pi 的前 10 的小数位数是 3.1415926535。

##<a name="10gb-greysort"></a>10 GB Greysort

GraySort 是基准排序的度量标准是实现排序非常大量的数据，通常 100 TB 最低时的排序速率 （TB/分钟）。

此示例使用适度的 10 GB 的数据，以便它可以较快运行。 它使用 MapReduce 应用程序由 Owen O'Malley 和 Arun Murthy 率为 0.578 TB/最小值 (以分钟为单位 173 100 TB) 2009 年赢得年度通用 ("daytona") terabyte 排序准则。 这和其他排序基准的详细信息，请参阅[Sortbenchmark](http://sortbenchmark.org/)站点。

此示例使用三套 MapReduce 程序︰

- **TeraGen**︰ 生成要排序的数据行的 MapReduce 程序

- **TeraSort**︰ 对输入的数据进行取样，并使用 MapReduce 到总的订单对数据进行排序

    TeraSort 是标准的 MapReduce 的功能，但使用定义每个减少为主要范围的 N-1 取样键的排序的列表的自定义分区。 特别是，所有此类的键示例 [i-1] < = 键 < 发送示例 [i] 以减少 i。 这保证的输出降低 i 均小于输出的减少 i + 1。

- **TeraValidate**︰ 验证输出全局排序的 MapReduce 程序

    它将在输出目录中，创建一个映射每个文件和每个映射可确保每个键都小于或等于上一。 映射函数还生成记录的第一个和最后一个键的每个文件，并将归约函数可确保文件我的第一个键大于文件 i-1 最后一个键。 任何问题报告为减少输出顺序错乱的键。

使用以下步骤来生成数据，排序，然后对验证输出︰

1. 生成 10 GB 的数据将存储到 HDInsight 群集的默认存储在**wasbs: / / 数据/10 GB-排序的输入示例**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    `-Dmapred.map.tasks`告诉 Hadoop 多少个映射任务若要使用此作业。 最后两个参数指示作业创建必要的 10 GB 的数据以及存储在**wasbs: / / 数据/10 GB-排序的输入示例**。

2. 使用下面的命令对数据进行排序︰

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    `-Dmapred.reduce.tasks`告诉 Hadoop 多少减少用于该作业的任务。 最后两个参数是仅输入和输出数据的位置。

3. 使用以下方法来验证生成排序的数据︰

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>下一步行动 ##

从这篇文章中，您学习了如何运行的基于 Linux 的 HDInsight 群集中包含的示例。 关于使用的 HDInsight 的小猪，配置单元，然后 MapReduce 的教程，请参阅下列主题︰

* [使用 Hadoop HDInsight 上的小猪][hdinsight-use-pig]
* [使用 Hadoop HDInsight 上配置单元][hdinsight-use-hive]
* [在 HDInsight 上的 Hadoop 使用 MapReduce] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
