<properties
    pageTitle="内存不足错误 (OOM)-配置单元设置 |Microsoft Azure"
    description="在 HDInsight 中的 Hadoop 修复内存不足错误 (OOM) 从配置单元查询。 客户情形是跨许多大型表的查询。"
    keywords="超出内存错误，OOM，配置单元设置"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>配置单元内存中的设置在 Azure HDInsight Hadoop 修复的内存 (OOM) 错误

常见的问题之一客户面临变出的内存 (OOM) 错误时使用配置单元。 本文介绍的客户方案和配置单元设置建议以解决此问题。

## <a name="scenario-hive-query-across-large-tables"></a>方案︰ 跨大表配置单元查询

客户已运行查询如下配置单元。

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

此查询的一些细微差别︰

* T1 是一个大表，表 1，有大量的字符串列类型的别名。
* 其他表就不大，但具有大量列。
* 所有表都加入，在某些情况下，使用多个列在表 1 和其他人。

当客户运行 MapReduce 配置单元使用 24 A3 群集节点上的查询时，查询将运行在 26 分钟的时间。 运行查询时使用 MapReduce 配置单元，客户注意到以下警告消息︰

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

查询完成执行在 26 分钟的时间，因为客户忽略这些警告，而是开始将精力集中在如何提高这进一步查询的性能。

客户商议[优化配置单元的 Hadoop HDInsight 中的查询](hdinsight-hadoop-optimize-hive-query.md)，并且决定使用 Tez 执行引擎。 一旦启用了 Tez 设置与运行相同查询查询运行 15 分钟，，然后出现以下错误︰

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

客户然后决定使用大思考一个更大的虚拟机的虚拟机 (即 D12) 将有更多堆空间。 即使这样，客户不断看到该错误消息。 客户达到到 HDInsight 团队中调试此问题的帮助。

## <a name="debug-the-out-of-memory-oom-error"></a>调试内存不足 (OOM) 错误

我们的支持和工程团队在一起找到导致错误的内存 (OOM) 的问题之一是[已知 Apache JIRA 中描述的问题](https://issues.apache.org/jira/browse/HIVE-8306)。 从 JIRA 中的描述︰

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

我们确认， **hive.auto.convert.join.noconditionaltask**确实通过查看配置单元 site.xml 文件下设置为**true** :

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

根据警告和 JIRA，我们的假设是地图加入 Java 堆空间 OOM 错误的原因。 因此我们发现深入了解此问题。

在[Hadoop Yarn 内存设置，在 HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)的博客文章中所述，执行引擎是的 Tez 使用的堆空间的使用时实际属于 Tez 容器。 请参阅下面描述的 Tez 容器内存的图像。

![Tez 容器内存图︰ 内存不足错误 OOM 配置单元](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


如所示的博客张贴内容，下面的两个内存设置定义容器内存堆︰ **hive.tez.container.size**和**hive.tez.java.opts**。 从我们的经验，OOM 异常并不表示容器的大小太小。 这意味着 Java 堆大小 (hive.tez.java.opts) 是太小。 因此只要看到 OOM，也可以尝试增加**hive.tez.java.opts**。 如果需要您可能必须增加**hive.tez.container.size**。 **Java.opts**设置应大约 80%的**container.size**。

> [AZURE.NOTE]  设置**hive.tez.java.opts**必须始终小于**hive.tez.container.size**。

由于 D12 机 28 GB 内存，我们决定使用的容器大小为 10GB (10240 MB) 和分配给 java.opts 的 80%。 这样做是在配置单元控制台上使用下面的设置︰

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

根据这些设置，该查询已成功运行在低于十分钟。

## <a name="conclusion-oom-errors-and-container-size"></a>结论︰ OOM 错误和容器大小

收到的 OOM 错误并不一定意味着容器的大小是太小。 相反，应配置内存设置，以便堆大小会增加，至少 80%的容器内存大小。
