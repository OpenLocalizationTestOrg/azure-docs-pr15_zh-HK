<properties
    pageTitle="在 HDInsight R 是什么？ 介绍到 R 服务器上的 HDInsight （预览） |Microsoft Azure"
    description="什么是 R HDInsight （预览），以及如何创建大数据分析的应用程序使用 R 服务器上的服务器。"
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>在 HDInsight R 服务器概述\(预览\)

与 Microsoft Azure HDInsight 特优，Microsoft R 服务器现在就可以作为一个选项 Azure 创建 HDInsight 群集时。 这一新功能提供了数据科学家，统计学家和 R 程序员可伸缩的、 按需访问分布在 HDInsight 分析的方法。

群集可以调整到项目和手头的任务和拆卸时就不再需要它们。 因为它们是 Azure HDInsight 的一部分，这些群集带有企业级 24/7 支持，99.9%正常运行时间 SLA 和灵活性与 Azure 的生态系统中的其他组件进行集成。

>[AZURE.NOTE] R 服务器所用语言为 HDInsight 津贴。 目前，HDInsight 高级功能仅适用于 Hadoop 和触发群集。 因此，当前可以使用 R 服务器只使用在 HDInsight 上的 Hadoop 并触发群集。 有关详细信息，请参阅[不同的服务级别和可与 HDInsight 的 Hadoop 组件是什么？](hdinsight-component-versioning.md)。

HDInsight R 服务器提供基于 R 的分析的最新功能加载到 Azure Blob 存储的大型数据集。 由于 R 服务器基于开源 R，构建基于 R 的应用程序可以利用任何 8000 + R 的开源软件包，以及 ScaleR，包含了与 R 服务器的 Microsoft 的大数据分析包中的例程。

高级群集的边缘节点提供了一个方便的位置连接到群集并运行 R 脚本。 边缘节点，您必须给核心边缘节点服务器运行 ScaleR 的并行化分布式的函数的选择。 您还可以选择运行它们在群集的节点之间使用 ScaleR 的 Hadoop 地图减少或触发计算上下文。

模型的预测，从分析结果可以下载以供内部使用 他们可以也被实施了其他地方在 Azure，如通过[Azure 机器学习 Studio](http://studio.azureml.net) [web 服务](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md)。

## <a name="get-started-with-r-on-hdinsight"></a>入门上 HDInsight R

HDInsight 群集中包含 R 服务器，您必须在使用 Azure 门户创建群集时使用高级选项创建 Hadoop 或触发群集。 这两种群集类型使用相同的配置，其中的 R 服务器群集的数据节点和边缘节点上为基于 R 服务器分析登录区域。 深入考察上创建群集，请参阅[入门 R 服务器 HDInsight 上](hdinsight-hadoop-r-server-get-started.md)。

## <a name="learn-about-data-storage-options"></a>了解数据存储选项

对于 HDInsight 群集的默认存储是 Blob 存储使用 HDFS 文件系统映射到 blob 容器。 这确保了任何数据上载到该群集存储器，或群集存储写入过程中分析，进行持久。 [AzCopy](../storage/storage-use-azcopy.md)实用程序可用于复制数据的 blob。

除了使用 Blob 存储，您必须与您的群集使用[Azure 数据湖存储](https://azure.microsoft.com/services/data-lake-store/)的选项。 如果您使用数据湖，然后可以对 HDFS 存储中使用 Blob 存储和数据湖。

在边缘节点上使用，还可以使用[Azure 文件](../storage/storage-how-to-use-files-linux.md)作为存储选项。 Azure 的文件，您可以装载到 Linux 文件系统在 Azure 存储中创建一个文件共享。 在 HDInsight 群集上的 R 服务器的数据存储选项的详细信息，请参阅[R HDInsight 群集上的服务器的存储选项](hdinsight-hadoop-r-server-storage.md)。

## <a name="access-r-server-on-the-cluster"></a>访问 R 服务器群集上

R 服务器创建群集后，您可以连接到 R 控制台上通过 SSH/PuTTY 群集的边缘节点。 如果您选择安装在边缘节点上的可选 RStudio 服务器 IDE，还可以连接到浏览器。 有关安装 RStudio 服务器的详细信息，请参阅[HDInsight 群集上安装 RStudio 服务器](hdinsight-hadoop-r-server-install-r-studio.md)。   

## <a name="develop-and-run-r-scripts"></a>开发和运行 R 脚本

R 脚本创建和运行可以使用的任何并行化和分布式例程除了 8000 + R 的开源包 ScaleR 库中。 一般情况下，R 服务器中边缘节点运行的脚本在 R 解释器中运行在该节点上。 例外情况是计算上下文设置为 Hadoop 地图减少 (RxHadoopMR) 或触发 (RxSpark) 调用 ScaleR 这些步骤作用。

在这些情况下，该函数以分散的方式跨运行这些数据 （任务） 中的节点群集与被引用的数据。 有关不同计算上下文选项的详细信息，请参阅[计算 R HDInsight 高级服务器的上下文选项](hdinsight-hadoop-r-server-compute-contexts.md)。

## <a name="operationalize-a-model"></a>实施一个模型

完成数据建模时，您可以实施模型进行预测的新数据在 Azure 和内部。 此过程称为评分。 下面是一些示例。

### <a name="score-in-hdinsight"></a>在 HDInsight 中的分数

若要得分在 HDInsight 中，编写调用您的模型进行预测，为新的数据文件已加载到您的存储帐户的 R 函数。 然后将预测保存回存储帐户。 您可以在边缘节点的群集或使用计划的作业运行日常的按需。  

### <a name="score-in-azure-machine-learning"></a>在 Azure 机器学习中的分数

若要通过使用 Azure 机器学习 web 服务得分，使用开源 Azure 机器学习 R 包称为[AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)发布作为 Azure 的 web 服务模型。 为方便起见，此包是预装在边缘节点上。 接下来，使用机器学习中的功能来创建用于 web 服务的用户界面，然后调用 web 服务所需的分数。

如果您选择此选项，您将需要将 ScaleR 模型的任何对象转换为等效的开源模型对象与 web 服务一起使用。 这可以通过使用 ScaleR 强制转换函数，如`as.randomForest()`ensemble 基于模型。


### <a name="score-on-premises"></a>分数部署

以得分内部创建模型后，可以序列化中 R 模型、 下载、 反序列化，然后将其用于评分新数据。 您可以通过使用[在 HDInsight 评分](#scoring-in-hdinsight)在前面部分中所述的方法或使用[DeployR](https://deployr.revolutionanalytics.com/)分数新数据。

## <a name="maintain-the-cluster"></a>维护群集

### <a name="install-and-maintain-r-packages"></a>安装和维护 R 包

由于 R 脚本中的大多数会那里将要求大部分使用 R 包边缘节点上。 要在边缘节点上安装其他 R 程序包，可以使用常规`install.packages()`方法在。

在大多数情况下，您不需要在数据节点上安装其他 R 包，如果只使用的例程从 ScaleR 库群集内的。 但是，您可能需要在数据节点上支持使用**rxExec**或**RxDataStep**执行的其他程序包。

在这些情况下，其他程序包，必须指定通过使用脚本操作之后创建群集。 有关详细信息，请参阅[创建带有 R 服务器 HDInsight 群集](hdinsight-hadoop-r-server-get-started.md)。   

### <a name="change-hadoop-map-reduce-memory-settings"></a>更改 Hadoop 地图减少内存设置

一个群集可以修改运行图减少作业时的 R 服务器可以使用的内存量。 若要修改群集，使用 Apache Ambari 用户界面可通过 Azure 门户刀片式服务器群集。 有关如何访问群集 Ambari 用户界面的说明，请参阅[管理 HDInsight 群集使用 Ambari Web 用户界面](hdinsight-hadoop-manage-ambari.md)。

还有可能更改，如下所示对**RxHadoopMR**的调用中使用 Hadoop 开关到 R 服务器可用的内存总量︰

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>扩展您的群集

一个现有的群集可以通过门户向上或向下扩展。 按比例，可以获得更大的处理任务，可能需要更多容量，或可以缩放回群集在空闲时。 有关如何扩展群集的说明，请参阅[管理 HDInsight 群集](hdinsight-administer-use-portal-linux.md)。

### <a name="maintain-the-system"></a>维护系统

在非工作时间要应用操作系统补丁程序和其他更新 HDInsight 群集中基本的 Linux 虚拟机上执行维护。 通常情况下，维护都在上午 3:30 （基于本地时间的 VM） 的每个星期一和星期四。 这样，它们不会影响群集的一次超过一季度会执行更新。  

因为头节点冗余，并不是所有的数据节点会受到影响，在此期间运行的任何作业可能会减慢。 他们应该仍然运行直到完成，但是。 除非需要群集重建发生灾难性故障，跨这些维护事件保留的任何自定义软件或您的本地数据。

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>了解如何为 R 服务器 HDInsight 群集上的 IDE 选项

HDInsight 高级群集的 Linux 边缘节点是基于 R 的分析的目标区域。 连接到群集后，您可以启动到 R 服务器控制台界面 Linux 命令提示符下键入**R** 。 如果在另一个窗口中，运行 R 脚本开发的文本编辑器和剪切并粘贴 R 控制台根据需要脚本的各个部分，使用控制台界面的功能得到增强。

R 脚本的开发一个更复杂的工具是基于 R 的 IDE 用于在桌面上，如微软最近宣布[R Visual Studio 工具](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx)(RTVS)。 这是一系列的[RStudio](https://www.rstudio.com/products/rstudio-server/)从台式机和服务器工具。 您还可以使用 Walware 的基于 Eclipse 的[StatET](http://www.walware.de/goto/statet)。

另一种方法是在 Linux 边缘节点本身上安装 IDE。  受欢迎的选择是[RStudio 服务器](https://www.rstudio.com/products/rstudio-server/)，它提供的远程客户端使用的基于浏览器的 IDE。 HDInsight 高级群集的边缘节点上安装 RStudio 服务器的开发和执行与 R 服务器群集上的 R 脚本提供完整的 IDE 体验。 它可以是有很大的 R 控制台比效率更高。  如果您想要使用 RStudio 服务器，请参阅[HDInsight 群集上安装 RStudio 服务器](hdinsight-hadoop-r-server-install-r-studio.md)。

## <a name="learn-about-pricing"></a>了解有关定价

到标准 HDInsight 群集的费用同样结构的 HDInsight 高级群集，R 服务器与相关联的费用。 它们基于整个名称、 数据和边缘节点，核心小时升级添加了特优的基础虚拟机的大小。 有关 HDInsight 最优定价，包括在公共预览和可用性的 30 天免费试用版，定价看到[HDInsight 定价](https://azure.microsoft.com/pricing/details/hdinsight/)。

## <a name="next-steps"></a>下一步行动

请按照下面的链接以了解有关如何使用 HDInsight 群集使用 R 服务器。

- [在 HDInsight R 服务器入门](hdinsight-hadoop-r-server-get-started.md)

- [RStudio 服务器添加到 HDInsight 津贴](hdinsight-hadoop-r-server-install-r-studio.md)

- [计算 R 服务器上下文的选项上 HDInsight （预览）](hdinsight-hadoop-r-server-compute-contexts.md)

- [R 服务器在 HDInsight 特优 azure 存储选项](hdinsight-hadoop-r-server-storage.md)
