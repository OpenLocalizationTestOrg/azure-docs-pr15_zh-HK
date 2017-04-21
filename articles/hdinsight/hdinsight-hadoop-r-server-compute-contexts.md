<properties
   pageTitle="计算 R 服务器在 HDInsight （预览） 上下文选项 |Microsoft Azure"
   description="了解有关不同计算上下文选项供用户与 R 服务器 HDInsight （预览）"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>计算 R 服务器上下文的选项上 HDInsight （预览）

在 Azure HDInsight （预览） Microsoft R 服务器提供了最新的基于 R 的分析功能。 它将使用存储在 HDFS 中您[Azure Blob](../storage/storage-introduction.md "Azure Blob 存储")的存储帐户或本地的 Linux 文件系统的容器中的数据。 由于 R 服务器基于开源 R，生成基于 R 的应用程序可以利用任何 8000 + R 的开源软件包。 他们还可以利用[ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "旋转分析 ScaleR")，包含了与 R 服务器的 Microsoft 的大数据分析包中的例程。  

高级群集的边缘节点，可方便地连接到群集并运行 R 脚本。 边缘节点，您必须给核心边缘节点服务器运行 ScaleR 的并行化分布式的函数的选择。 您还可以选择运行它们在群集的节点之间使用 ScaleR 的 Hadoop 地图减少或触发计算上下文。

## <a name="compute-contexts-for-an-edge-node"></a>计算边节点的上下文

一般情况下，R 为 R 服务器运行在边缘节点上运行脚本内 R 解释器在该节点上。 例外情况是那些调用 ScaleR 函数的步骤。 取决于您如何设置 ScaleR 计算上下文计算环境中运行的 ScaleR 调用。  从边缘节点运行 R 脚本时，计算上下文的可能的值为本地顺序 （本地），本地并行 (localpar)、 图减少，并触发。

仅在执行 rxExec 调用的方式不同的本地和 localpar 选项。 这两个执行其他 rx 函数调用以并行的方式跨所有可用的内核除非另有指定通过使用 ScaleR numCoresToUse 选项，例如 rxOptions(numCoresToUse=6)。 下面总结了各种计算上下文选项

| 计算上下文  | 如何设置                      | 执行上下文                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| 本地连续 | rxSetComputeContext('local')    | 边缘节点服务器，除 rxExec 之外的并行化的执行跨内核调用可被执行的顺序 |
| 本地并行   | rxSetComputeContext('localpar') | 并行化的执行跨内核的边缘节点服务器                                 |
| 触发            | RxSpark()                       | 并行分布式的执行通过触发各个节点的 HDI 群集      |
| 缩小图       | RxHadoopMR()                    | 并行分布式的执行通过地图减少跨节点的 HDI 群集 |


假定您想并行化的执行性能的目的，然后有三个选项。 选择哪个选项取决于所做的分析工作和大小的特性和数据的位置。

## <a name="guidelines-for-deciding-on-a-compute-context"></a>在计算上下文决定的准则

目前，没有告诉您该计算要使用的上下文的公式。 有，但是，一些可以帮助您做出正确的选择，或至少帮助您缩小您的选择之前运行基准, 的指导原则。 这些指导原则包括︰

1.  本地的 Linux 文件系统是 HDFS 比速度更快。
2.  如果数据是本地的并且处于 XDF，重复的分析会更快。
3.  我们最好还是以流式传输少量数据的文本文件数据源;如果数据量较大，将其转换为 XDF 之前分析。
4.  复制或流式处理分析的边缘节点数据的开销变得难以控制的非常大量的数据。
5.  触发是比映射在 Hadoop 分析减少的速度快。

由于这些原则，有一些通用的规则用于计算上下文选择︰

### <a name="local"></a>本地

- 如果要分析的数据量小，且不需要重复的分析，然后将其流式处理到分析例程直接并使用本地或 localpar。
- 如果要分析的数据量小或中等大小，并需要反复的分析，然后将其复制到本地文件系统，将其导入到 XDF，并通过本地或 localpar 对其进行分析。

### <a name="hadoop-spark"></a>Hadoop 触发

- 如果要分析的数据量很大，然后将它导入 XDF 在 HDFS 中 （除非存储问题），并对其进行分析，通过触发。

### <a name="hadoop-map-reduce"></a>Hadoop 地图减少

- 使用仅在您遇到问题是不可战胜的触发计算上下文使用，因为通常会变慢。  

## <a name="inline-help-on-rxsetcomputecontext"></a>在 rxSetComputeContext 上的内联帮助

有关详细信息和示例的 ScaleR 计算上下文，请参见内联帮助 R 上的 rxSetComputeContext 方法，例如︰

    > ?rxSetComputeContext

也可以指"ScaleR 分布式计算指南"即[R 服务器 MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R 服务器在 MSDN")库中可用。


## <a name="next-steps"></a>下一步行动

在本文中，您学习了如何创建一个新的 HDInsight 群集包含 R 服务器。 您还学习了使用 R 控制台的 SSH 会话的基本知识。 现在您可以阅读下面的文章，发现 HDInsight R 服务器所使用的其他方法︰

- [Hadoop 的 R 服务器的概述](hdinsight-hadoop-r-server-overview.md)
- [入门 Hadoop R 服务器](hdinsight-hadoop-r-server-get-started.md)
- [RStudio 服务器添加到 HDInsight 津贴](hdinsight-hadoop-r-server-install-r-studio.md)
- [R 服务器在 HDInsight 特优 azure 存储选项](hdinsight-hadoop-r-server-storage.md)
