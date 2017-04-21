<properties
    pageTitle="发行说明，了解 Hadoop 组件在 Azure HDInsight |Microsoft Azure"
    description="新的发行说明和 Azure HDInsight 的 Hadoop 组件的版本。 Hadoop，Apache 风暴和 HBase 中获得开发提示和详细信息。"
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Hadoop 组件在 Azure HDInsight 上的发行说明

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>10/26/2016年发行上 HDInsight R 服务器的注意事项

- R HDInsight 群集资源调配服务器已经进行了改进。
- HDInsight R 服务器现作为常规 HDInsight"R 服务器"群集类型，不再作为单独的 HDInsight 应用程序安装。 作为 R 服务器群集部署的一部分现在调配的边缘节点和 R 服务器二进制文件。 这提高了速度和可靠性的资源调配。 相应地更新 R 服务器的定价模型。
- R 服务器群集类型价格现在基于标准层价格加上 R 服务器附加费的价格。 特优层现在将保留可用的高级功能用于在不同的群集类型，而不会用于 R 服务器群集类型。 此更改不会影响 R 服务器的有效定价，它只会更改费用清单中的显示方式。 所有现有的 R 服务器群集将继续工作，ARM 模板将继续工作直到否决通知。 **建议虽然以更新脚本的部署使用 ARM 的新模板。**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>08/30/2016年发行上 HDInsight R 服务器的注意事项

使用此版本部署的基于 Linux 的 HDInsight 群集的完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成   |Ambari 生成 |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

对于基于 Windows 的 HDInsight 群集使用此版本部署完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>08/17/2016年发行上 HDInsight R 服务器的注意事项

- R 服务器 8.0.5 – 主要发行版本的 bug 修复。 [R 服务器发行说明](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes)以获取详细信息，请参阅。 
- 边缘节点 –[此 R 包](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)上的 AzureML 软件包使 R 模型发布，Azure ML web 服务中使用。  请参阅["概述的 R 服务器在 HDInsight"](hdinsight-hadoop-r-server-overview.md)文章以获取详细信息的["实施模型"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model)一节。
- Linux 的[top 100 最受欢迎的 R 包](https://github.com/metacran/cranlogs)– 这些 Linux 现在都预安装包的依赖项的依赖项。  
- 要使用 CRAN repo 时到数据节点添加 R 包选项。 请参阅["安装 R 包"](hdinsight-hadoop-r-server-get-started.md#install-r-packages)部分我们["开始使用 HDInsight R 服务器"](hdinsight-hadoop-r-server-get-started.md)以获取详细信息。
- 改进的 R 服务器资源调配创建群集时的可靠性。


## <a name="notes-for-08012016-release-of-hdinsight"></a>HDInsight 08/01/2016年发布注意事项

使用此版本部署的基于 Linux 的 HDInsight 群集的完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成   |Ambari 生成 |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

对于基于 Windows 的 HDInsight 群集使用此版本部署完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如触发，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 对 HDInsight 3.4 群集更改 | 以下配置单元配置的默认值更改为更好的性能 <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| 服务    | 所有| N/A|
| 此版本中包括下列修复程序 | 配置单元 13632、 HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| 服务    | 所有| N/A

## <a name="notes-for-07142016-release-of-hdinsight"></a>07/14/2016年版本的 HDInsight 的注释

使用此版本部署的基于 Linux 的 HDInsight 群集的完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成   |Ambari 生成 |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

对于基于 Windows 的 HDInsight 群集使用此版本部署完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>HDInsight 07/07/2016年发布注意事项

使用此版本部署的基于 Linux 的 HDInsight 群集的完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成   |
|----|----------------------|----|------------|
|3.2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

对于基于 Windows 的 HDInsight 群集使用此版本部署完整版本号︰

|HDI |HDI 群集版本   |HDP |HDP 生成     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如触发，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [IntelliJ 的 HDInsight 工具](hdinsight-apache-spark-intellij-tool-plugin.md) | IntelliJ 知道插件 HDInsight 触发群集现在与集成 Azure Toolkit 为 IntelliJ。 它支持 Azure SDK v2.9.1，最新的 Java Sdk，并包括从独立 IntelliJ 的 HDInsight 插件的所有功能。| 工具    | 触发| N/A|
| [日蚀式的 HDInsight 工具](hdinsight-apache-spark-eclipse-tool-plugin.md) | 日蚀式的 azure Toolkit 现在支持 HDInsight 触发群集。 它实现了以下功能。 <ul><li>创建并在 Scala 和 Java 轻松地编写触发应用程序，与第一类创作支持 IntelliSense，自动套用格式，错误检查，等等。</li><li>测试本地触发应用程序。</li><li>将作业提交到 HDInsight 触发群集和检索结果。</li><li>登录到 Azure 和访问与 Azure 订阅相关的所有触发群集。</li><li>导航 HDInsight 触发群集中的所有关联的存储资源。</li></ul>| 工具    | 触发| N/A

从本发行版开始，我们还改变了基于 Linux 的 HDInsight 群集的来宾 OS 修补策略。 新策略的目标是极大地减少由于修补的重新启动次数。 新策略将在 Linux 群集的每个星期一或星期四上午 12 UTC 采用交错的方式在任何给定的群集中的节点之间开始继续修补程序虚拟机 (Vm)。 但是，任何给定的 VM 将只重新启动最大一次由于来宾 OS 修补程序每隔 30 天。 此外，对于新创建的群集的第一个重新启动操作不会很快从群集创建日期的 30 天内。

>[AZURE.NOTE] 这些更改将只应用于新创建的群集等于或大于此发行版本中。

## <a name="notes-for-06062016-release-of-hdinsight"></a>06/06/2016年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

|HDP    |HDI 版本    |触发版本  |Ambari 生成号    |HDP 内部版本号|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如触发，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 在 HDInsight 上的触发是上市 | 这一版本会改进了可用性、 可扩展性和生产率以打开源在 HDInsight 上的 Apache 触发。 <ul><li>业界领先的可用性为 99.9%，这使它适合于要求苛刻的企业工作负载的 SLA。</li><li>使用 Azure 数据湖存储的可扩展的存储层。</li><li>每个阶段的数据探索和开发的生产效率工具。 Jupyter 笔记本与自定义触发内核启用交互式数据研究、 与双面板电源 BI、 Tableau 和 Qlik 等集成非常适合快速数据共享和连续报告、 IntelliJ 插件是可靠的选择，从长远来看代码的项目开发和调试。</li></ul>| 服务    | 触发| N/A|
| IntelliJ 的 HDInsight 工具 | 这是 HDInsight 触发群集的一个 IntelliJ 的想法的插件。 它实现了以下功能。<ul><li>创建并在 Scala 和 Java 轻松地编写触发应用程序，与第一类创作支持 IntelliSense，自动套用格式，错误检查，等等。</li><li>测试本地触发应用程序。</li><li>将作业提交到 HDInsight 触发群集和检索结果。</li><li>登录到 Azure 和访问与 Azure 订阅相关的所有触发群集。</li><li>导航 HDInsight 触发群集中的所有关联的存储资源。</li><li>所有的作业历史记录和 HDInsight 触发群集的作业信息导航。</li><li>调试从您的桌面计算机远程触发作业。</li></ul>| 工具    | 触发| N/A

## <a name="notes-for-05132016-release-of-hdinsight"></a>对 HDInsight 05/13/2016年发布说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如触发，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 引起大家对版本更新和其他错误修复 | 此版本 1.6.1，更新 HDInsight 群集中的触发版本和修复其他 bug| 服务    | 触发| N/A

## <a name="notes-for-04112016-release-of-hdinsight"></a>04/11/2016年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-不变)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 自定义 metastore 升级问题的 HDI 3.4 | 如果您使用自定义的 metastore，以前曾在较低版本的另一个 HDInsight 群集，群集创建就会失败。 这是因为现在已修复的升级脚本错误| 创建群集    | 所有 | N/A
| 晚故障恢复 | 通过晚提交任何作业提供作业状态的恢复能力 | 可靠性 | 在 Linux 上的触发| N/A
| Jupyter 内容 HA | 提供保存和加载 Jupyter 笔记本内容与群集相关的存储帐户的能力。 有关详细信息，请参阅[Jupyter 笔记本的可用的内核](hdinsight-apache-spark-jupyter-notebook-kernels.md)。| 笔记本 | 在 Linux 上的触发| N/A
| HiveContext Jupter 笔记本中删除 | 使用`%%sql`而不是魔术`%%hive`魔术。 SqlContext 相当于 hiveContext。 有关详细信息，请参见[内核可用于 Jupyter 笔记本](hdinsight-apache-spark-jupyter-notebook-kernels.md)| 笔记本    | 在 Linux 上的触发群集| N/A
| 触发的旧版的 deprecation | 删除旧版本 1.3.1 触发将从服务上 5/31 | 服务 | 在 Windows 上的触发群集 | N/A

## <a name="notes-for-03292016-release-of-hdinsight"></a>03/29/2016年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7-保持不变)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8-保持不变)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7-保持不变)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 添加的 HDInsight 3.4 版本和更新所有 HDInsight 群集 HDP 版本 | 此发行版中，我们添加了 HDInsight v3.4 （基于 HDP 2.4），也已更新了其他 HDP 版本。 HDP 2.4 发行说明是可用[在此处](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)，可以版本 HDInsight 的详细信息找到[这里](hdinsight-component-versioning.md)。| 服务    | 所有的 Linux 群集| N/A
| HDInsight 高级 | HDInsight 现已推出两个类别的标准和高级。 HDInsight 高级当前在预览，并仅适用 Hadoop，触发群集在 Linux 上。 有关详细信息，请参阅[此处](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)。| 服务    | Hadoop 并在 Linux 上的触发| N/A
| Microsoft R 服务器 | HDInsight 高级提供 Microsoft R 服务器可以包含与在 Linux 上的 Hadoop 和触发群集。 有关详细信息请参阅[概述的 R 服务器 HDInsight 上](hdinsight-hadoop-r-server-overview.md)。| 服务    | Hadoop 并在 Linux 上的触发| N/A
| 触发 1.6.0 | HDInsight 3.4 群集现在包括触发 1.6.0| 服务    | 在 Linux 上的触发群集| N/A
| Jupyter 笔记本的增强功能 | 通过触发群集可用的 Jupyter 笔记本现在提供其他触发内核。 它们还包括改进功能，例如使用 %%魔术、 自动可视化和与 Python 可视化效果库 （如 matplotlib) 的集成。 有关详细信息，请参阅[Jupyter 笔记本的可用的内核](hdinsight-apache-spark-jupyter-notebook-kernels.md)。 | 服务 | 在 Linux 上的触发群集 | N/A

## <a name="notes-for-03222016-release-of-hdinsight"></a>03/22/2016年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7-保持不变)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8-保持不变)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7-保持不变)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 对于所有的 HDInsight 群集更新的 HDInsight 版本 | 此发行版中，我们已更新了所有 HDInsight 群集的 HDInsight 版本| 服务    | 所有| N/A


## <a name="notes-for-03102016-release-of-hdinsight"></a>03/10/2016年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5-保持不变)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 对于所有的 HDInsight 群集更新的 HDInsight 版本 | 此发行版中，我们已更新了所有 HDInsight 群集的 HDInsight 版本| 服务    | 所有| N/A

## <a name="notes-for-01272016-release-of-hdinsight"></a>对 HDInsight 01/27/2016年发行说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5-保持不变)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 对于所有的 HDInsight 群集更新的 HDInsight 版本 | 此发行版中，我们已更新了所有 HDInsight 群集的 HDInsight 版本| 服务    | 所有| N/A

## <a name="notes-for-12022015-release-of-hdinsight"></a>12/02/2015年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34-保持不变)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34-保持不变)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 添加的 HDInsight 3.3 版本和更新所有 HDInsight 群集 HDP 版本 | 此发行版中，我们添加了 HDInsight v3.3 （基于 HDP 2.3），也已更新了其他 HDP 版本。 HDP 2.3 发行说明是可用[在此处](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)，可以版本 HDInsight 的详细信息找到[这里](hdinsight-component-versioning.md)。| 服务    | 所有| N/A

## <a name="notes-for-11302015-release-of-hdinsight"></a>HDInsight 11/30/2015年发布注意事项

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 所有 HDInsight 群集和 HDP （Windows 和 Linux） HDInsight 3.2 群集版本更新的 HDInsight 版本 | 此版本中，HDInsight 和 HDP 的版本已经更新， | 服务    | 所有| N/A


## <a name="notes-for-10272015-release-of-hdinsight"></a>附注 10/27/2015 HDInsight 版

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795-保持不变)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117-保持不变)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374-保持不变)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 更新的 HDInsight 版本的所有 HDInsight 群集 （Windows 和 Linux） | 此版本中，HDInsight 和 HDP 的版本已经更新， | 服务    | 所有| N/A
| 固定的窗口触发的 Jupyter 群集使用大写字母群集 | 必须以大写字母表示指定的 DNS 名称的群集必须由于来源请求检查 Jupyter 笔记本的问题。 此修复程序将 Jupyter 的配置的 DNS 名称更改为小写字母。 | 服务    | HDInsight 触发 (Windows)| N/A


## <a name="notes-for-10202015-release-of-hdinsight"></a>附注 10/20/2015 HDInsight 版

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 默认 HDP 版本更改为 HDP 2.2 | HDInsight Windows 群集的默认版本将变为 HDP 2.2。 HDInsight 版本 3.2 (HDP 2.2) 以来在正式提供 2 月 2015年。 此更改仅翻转默认群集版本，当尚未使用 Azure 的门户，PowerShell 的 cmdlet 或 SDK 的群集资源调配时进行明确的选择。 | 服务    | 所有| N/A                  |
|在虚拟网络中单个群集中部署多个 HDInsight 在 Linux 上的 VM 名称格式的更改 | 在此发行版中增加了部署在单个虚拟网络中的多个 HDInsight Linux 群集的支持。 作为此过程中，群集中的虚拟机名称的格式已从 headnode\*，workernode\*和 zookeepernode\*对 hn\*，wn\*，和 zk\*分别。 它不是建议的做法，因为这可能会有更改虚拟机名称的格式采取直接依赖项。 请使用在本地计算机或 Ambari Api"主机名-f"确定的主机，并映射到主机的组件的列表。 您可以找到更多的信息，在[https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md)和[https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md)。 | 服务 | 在 Linux 上的 HDInsight 群集 | N/A |
| 配置更改 | 对于 HDInsight 3.1 群集现在启用以下配置︰ <ul><li>tez.yarn.ats.enabled 和 yarn.log.server.url。 这使得应用时间线服务器和日志服务器能够提供出日志。</li></ul>对于 HDInsight 3.2 群集已修改以下配置︰ <ul><li>mapreduce.fileoutputcommitter.algorithm.version 设置为 2。 这使 FileOutputCommitter 的 V2 版本的使用。</li></ul> | 服务 | 所有 | N/A |


## <a name="notes-for-09092015-release-of-hdinsight"></a>09/09/2015年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334-保持不变)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012-保持不变)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 对于所有的 HDInsight 群集更新的 HDInsight 版本 | 此版本中，HDInsight 版本已更新 | 服务    | 所有| N/A                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>HDInsight 07/31/2015年发布注意事项

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334-保持不变)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012-保持不变)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 解决触发群集节点重新成像的工作流 | 修复一个 bug，导致触发后重建映像而不是恢复到群集节点 | 服务    | 触发| N/A                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>HDInsight 07/31/2015年发布注意事项

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334-保持不变)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012-保持不变)
* SDK 1.5.8

此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 对于所有的 HDInsight 群集更新的 HDInsight 版本 | 此版本中，HDInsight 版本已更新 | 服务    | 所有| N/A                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>HDInsight 07/07/2015年发布注意事项

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334-保持不变)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


此版本包含了下面的更新。

| 标题                                           | 说明                                          | 受影响的区域 （例如，服务、 组件或 SDK） | 群集类型 （例如，Hadoop，HBase 或冲击） | JIRA （如果适用） |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 对于 HDInsight 3.2 群集更新的 HDP 版本 | 此版本中，HDInsight 3.2 部署 HDP 2.2.6.1-0012 | 服务    | 所有                                                 | N/A                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>对 HDInsight 06/26/2015年发布说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334-保持不变)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>对于 HDInsight 3.2 群集更新的 HDP 版本</td>
<td>此版本中，HDInsight 3.2 部署 HDP 2.2.6.1</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>对 HDInsight 06/18/2015年发布说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>打开其他 HTTPS 端口</td>
<td>云服务立即打开 5 端口 8001 至 8005 例如在群集上 在 https://<clustername>.azurehdinsight.net:8001/。 这些 url 的请求进行身份验证为 443 端口使用相同的基本身份验证密码机制。 这些端口将绑定到同一端口上活动的 headnode。 可以使用脚本操作进行侦听这些端口上的 headnode 和路由群集外部的客户服务。</td>
<td>云服务</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>对于 HDInsight 3.2 间歇性 MapReduce 无序播放问题</td>
<td>有时任务失败所导致的大型群集中地图减少无序播放罕见、 间歇性竞争条件的修补程序。 有关详细信息，请参阅<a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> 。</td>
<td>Hadoop 核心</td>
<td>所有</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>

<tr>
<td>将移动到新的 Azure Java SDK HDInsight 3.2 2.2</td>
<td>WASB 驱动程序所使用的 Java 移动到最新版本的 Azure SDK。 最新 SDK 包含几个修补程序，可在 https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt 相同的发行说明。</td>
<td>Hadoop 核心</td>
<td>所有</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>

<tr>
<td>移至 HDP 2.1.15 HDInsight 3.1 群集</td>
<td>Hortonworks 版本的发行说明位于<a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">此处</a>。</td>
<td>HDP</td>
<td>所有</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>06/04/2015 发行版的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003-保持不变)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>对于风暴群集 502 错误的网关错误修复</td>
<td>此版本中修复了 bug 影响作业提交导致在重新启动后已关闭该网站的 API。</td>
<td>服务</td>
<td>风暴</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>06/01/2015年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003-保持不变))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800-保持不变)
* SDK 1.5.8


此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>各种缺陷修补程序</td>
<td>此版本中修复了 bug 与群集资源调配。</td>
<td>服务</td>
<td>所有群集类型</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>对 HDInsight 05/27/2015年发布说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* 其他群集版本和 SDK 不会为此发行版的一部分部署。


此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>HDP 2.2 更新</td>
<td>此版本的 HDInsight 3.2 包含 HDP 2.2.6，并带给 HDInsight 的几个重要的 bug 修复。 完整的发行说明位于<a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 发行说明，</a>。</td>
<td>HDP</td>
<td>所有群集类型</td>
<td>N/A</td>
</tr>

<tr>
<td>更改为默认 Yarn 容器内存配置</td>
<td>在此更新时，默认可用内存到 YARN 容器 （yarn.nodemanager.resource.memory mb 和 yarn.scheduler.maximum-分配-mb） 启动的节点管理器中，增至 5632 MB。 以前这是减至 4608 MB，但基于各种作业运行，新值必须提供更好的可靠性和性能对大多数作业，因此是更好的默认值。 通常，如果为您关键依赖关系对此内存配置，请在创建群集时显式地设置它。</td>
<td>HDP</td>
<td>所有群集类型</td>
<td>N/A</td>
</tr>

<tr>
<td>HBase 和风暴群集的默认配置奇偶校验</td>
<td>此更新将恢复 Hbase 和风暴 YARN 配置的相同值用作 Hadoop 群集的群集。 这是奇偶校验跨所有群集类型。</td>
<td>HDP</td>
<td>HBase 风暴</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>对 HDInsight 05/20/2015年发布说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>SCP.NET EventHub 支持</td>
<td>HDInsight 冲击的更新的群集软件包给 SCP.NET 带来的新功能。 您现在可以访问到拓扑生成器中新更加轻松地使用 EventHubSpout 或 Java Spouts 的 Api。 您必须更新您的 SCP.NET 客户端 SDK 合同已更新为使用新的群集。 有关详细信息 （包括 bug 修复） 新 Api、 用法和发行说明，请参阅 SCP.NET nuget 程序包中包含的自述文件。</td>
<td>与刀具</td>
<td>暴 HDInsight 3.2 群集</td>
<td>N/A</td>
</tr>

<tr>
<td>JDBC 驱动程序更新</td>
<td>对 sqljdbc_4.1.5605.100 中的受支持的 SQL Server 版本更新驱动程序。</td>
<td>Metastore</td>
<td>所有</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>04/27/2015年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329-保持不变)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>解决 DLL 依赖项</td>
<td>删除在单元测试框架 HDInsight 依赖项。</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>争用条件的 bug 修复</td>
<td>群集创建请求现在等待 PUT 请求将被接受之前状态轮询</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>04/14/2015年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329-保持不变)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>Tez 错误修复</td>
<td>Apache TEZ 2214 和 TEZ 1923 修补程序包含在此版本中的 HDI 3.2。 这些特别需要某些配置单元查询 Tez 上需要无序播放大量的数据。
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>04/06/2015 发行版的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795-保持不变)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117-保持不变)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329-保持不变)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>HDInsight.NET SDK 1.5.6</td>
<td>要在 Linux 上的 HDInsight 中删除某些内部类的更新。</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Avro 库 1.5.6</td>
<td>添加方法<b>GetAllKnownTypes</b> <b>KnownTypeAttribute</b> 。 固定的 NullReferenceException GetAllKnownTypes 方法为 null 类型时。</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>错误修复</td>
<td>对服务的各种缺陷修补程序</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>04/01/2015年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>启用/禁用远程桌面凭据通过.NET SDK 的 Windows 群集上的能力</td>
<td>用于启用或禁用 Windows 群集上的 RDP 凭据的编程支持。</td>
<td>SDK</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>启用在群集上的远程桌面的凭据时他们正在调配的能力</td>
<td>启用远程桌面的凭据，因为正在创建群集编程支持。 这将移除第一次配置群集，然后启用远程桌面的两步过程。</td>
<td>SDK</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>已升级到 2.7.8 的 Python</td>
<td>升级后的 Python HDInsight 群集到 Python 2.7.8，其中包含一些重要的安全修补程序 HDInsight 版本 2.1、 3.0、 3.1 和 3.2</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>YARN 配置更改</td>
<td>更改的 YARN 配置 yarn.resourcemanager.max 完成的应用程序到 1000 3.1 和 3.2 HDInsight 版本的所有群集类型。 此值仅控制 YARN UI 中已完成的应用程序的列表。 若要获取有关应用程序在用户界面上显示的应用程序列表之前提交的信息，您可以直接转到历史记录服务器。</td>
<td>YARN</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>HBase 群集中的节点的大小调整</td>
<td>HBase 群集现在允许调整大小的节点 （向上和向下） HDInsight 版本 3.1 和 3.2</td>
<td>服务</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>JDBC 升级</td>
<td>HDInsight 3.2 版本的情况下，SQL JDBC 驱动程序升级到版本 sqljdbc_4.0.2206.100。 此版本包含重要的安全增强功能。</td>
<td>HDP</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>JVM 配置更新</td>
<td>更新的 JVM 配置 networkaddress.cache.ttl 为 300 秒从 HDInsight 版本 3.1 和 3.2-1 的默认值。 此配置值控制成功从名称服务查找的缓存策略。 这可以修复与增大和缩小 HBase 群集相关的 bug。</td>
<td>服务</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>升级到 Azure 存储 Java 2.0 的 SDK</td>
<td>HDInsight 3.2 版本升级 Java 使用 Azure 存储 SDK 的最新版本。 这对当前的 0.6.0 包含几个重要的 bug 修补程序版本。</td>
<td>HDP</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>升级至 Apache WASB 源代码</td>
<td>HDInsight 3.2 现在使用 Apache Hadoop 从 WASB 文件系统驱动程序代码的最新版本。 进行此更改后，WASB 驱动程序现在打包成单独的 jar。 这是纯粹的包装更改并不包含对 WASB 驱动程序行为的任何更改。 该 JAR 文件的名称是 hadoop 的 azure-2.6.0.jar。</td>
<td>HDP</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>Jar 文件的名称更新在 HDInsight 3.2</td>
<td>HDInsight 3.2 版本到此更新包含几个缺陷修补程序，并已升级了几个内部 jar 打包为 HDP 的一部分。 请注意，这些 JAR 文件内部 HDP 包而不是针对客户的应用程序直接使用。 应用程序应该打包他们自己版本的 Jar，以便对 HDP 内部 Jar 的升级不会中断客户的应用程序。</td>
<td>HDP</td>
<td>所有</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>03/03/2015年版本的 HDInsight 的注释

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351-保持不变)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097-保持不变)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290-保持不变)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340-保持不变)
* SDK 1.5.0 （不变）

此版本包含以下更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA</th>
</tr>


<tr>
<td>可靠性的改进</td>
<td>我们做了修复程序允许使用创建群集之间日益增长的负载的伸缩性更好的服务。</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>对 HDInsight 02/18/2015年发布说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351-保持不变)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097-保持不变)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290-保持不变)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* SDK 1.5.0

此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>HDInsight 3.2 群集</td>
<td>Hadoop 2.6/HDP2.2 与 HDInsight 3.2 群集不可。 它包含所有的开源组件的主要更新。 有关详细信息，请参阅 HDInsight 和<a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 发行说明</a>中的新增功能。</td>
<td>开放源代码软件</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>HDinsight 在 Linux （预览）</td>
<td>可以在 Ubuntu Linux 上运行部署群集。 有关更多详细信息，请参阅在 Linux 上的 HDInsight 开始。</td>
<td>服务</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>暴风雨一般可用性</td>
<td>Apache 风暴群集是普遍适用的。 有关更多详细信息，请参阅开始使用 HDInsight 中的风暴。</td>
<td>服务</td>
<td>风暴</td>
<td>N/A</td>
</tr>

<tr>
<td>虚拟机大小</td>
<td>Azure 的 HDInsight 有多个虚拟机的类型和大小。 HDInsight 可以利用 A2 到 A7 大小生成用于常规;D 系列节点采用固态驱动器 (SSDs) 和 60%更快的处理器;和 A8 和 A9 大小具有无限带宽的快速网络连接支持。</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>群集的扩展</td>
<td>可以更改正在运行的 HDInsight 群集的数据节点数而无需删除或重新创建它。 目前，只有 Hadoop 查询和 Apache 风暴的群集类型具备这一能力，但对群集类型是很快按照 Apache HBase 支持。 有关详细信息，请参阅管理 HDInsight 群集。</td>
<td>服务</td>
<td>Hadoop 风暴</td>
<td>N/A</td>
</tr>

<tr>
<td>Visual Studio 工具</td>
<td>除了完成刀具的 Apache 风暴的 Apache 配置单元在 Visual Studio 工具已更新，包括语句完成、 本地验证和改进了调试支持。 有关详细信息，请参阅入门 HDInsight Hadoop 工具 Visual Studio。</td>
<td>刀具</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>DocumentDB 的 Hadoop 连接器</td>
<td>与 DocumentDB 的 Hadoop 连接器，您可以执行复杂的聚合、 分析和操作架构灵活的 JSON 文档存储在 DocumentDB 集合或数据库帐户间通过。 有关教程，说明如何和详细信息，请参阅使用 DocumentDB 和 HDInsight 的运行 Hadoop 作业。</td>
<td>服务</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>错误修复</td>
<td>我们做了各种次要的 bug 修补程序 HDInsight 服务。 我们不期望任何面向客户的行为更改。</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>对 02/06/2015 HDInsight 版本的说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351-保持不变)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097-保持不变)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK N/A

此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>错误修复</td>
<td>我们做了各种次要的 bug 修补程序 HDInsight 服务。 我们不期望任何面向客户的行为更改。</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP 2.1 维护更新</td>
<td>HDInsight 3.1 更新部署 HDP 2.1.10.0。 有关详细信息，请参阅<a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">发行说明 HDP 2.1.10</a>。 </td>
<td>开放源代码软件</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP 二进制更新</td>
<td>HBase 哪些文件已更新名称中有几个 JAR 文件。 这些 JAR 文件使用内部 HBase，因此并不期望客户具有相关性，这些 JAR 文件的名称。 其中包括︰
<ul>
<li>./lib/jetty-6.1.26.hwx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/jetty-util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>开放源代码软件</td>
<td>HBase</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>对 HDInsight 的 1/29/2015年发行说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351-保持不变)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097-保持不变)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196-保持不变)
* SDK N/A

此版本包含以下更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>受影响的区域 （例如，服务、 组件或 SDK）</p></th>
<th>群集类型 （例如，Hadoop，HBase 或冲击）</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>

<td>错误修复</td>
<td>我们做了 Azure 升级过程改进的 HDInsight 群集的可靠性的几个重要的 bug 修复。</td>
<td>服务</td>
<td>所有</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>对 HDInsight 的 1/5/2015年发行说明

HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351-保持不变)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097-保持不变)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196-保持不变)


此版本包含了下面的更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>组件</th>
<th>群集类型</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>对于 Twitter 趋势分析和基于 Mahout 的影片建议示例</td>
<td><p>在此版本中，HDInsight 查询控制台有两个附加的示例︰</p>

<p><b>Twitter 趋势分析</b><br>
公共 Api 提供的类似 Twitter 的网站是有用的分析和了解流行趋势的数据源。 在本教程中，学习如何使用配置单元获取发送包含特定词大部分 tweets 的 Twitter 用户的列表。 </p>

<p><b>Mahout 影片建议</b><br>
Apache Mahout 是 Apache Hadoop 机器学习库。 Mahout 包含用于处理数据 （如筛选、 分类和群集） 的算法。 在本教程中，使用一个推荐引擎生成影片建议根据您的朋友已经了解的电影。</p></td>
<td>查询控制台</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>更改配置单元配置为默认值︰ hive.auto.convert.join.noconditionaltask.size</td>
<td><p>此大小配置将应用于自动转换的映射连接。 值表示可以转换为适合在内存中的哈希映射表的大小的总和。 在以前的版本中，此值增加 10 MB 的默认值为 128 MB。 但是，新 128 MB 的值导致作业失败原因由于内存不足。 此版本将恢复到 10 MB 的默认值。 客户仍可以选择重写此值在群集创建过程中指定其查询和表大小。 有关此设置和如何重写它的详细信息，请参阅 Hortonworks 文档中的<a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">优化自动联接转换</a>。 </p></td>
<td>配置单元</td>
<td>Hadoop Hbase</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>附注 12/23/2014 HDInsight 版

是 HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.10.420.1246783 （HDP 版本保持不变）
* HDInsight 3.0.6.420.1246783 （HDP 版本保持不变）
* HDInsight 3.1.1.420.1246783 （HDP 版本保持不变）

此版本包含以下更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>组件</th>
<th>群集类型</th>
<th>JIRA （如果适用）</th>
</tr>


<tr>
<td>由于负载过大的间歇性群集创建故障</td>
<td><p>在群集创建过程中下载 HDP 包的改进的算法使更可靠的处理负载过大引起的故障。</p></td>
<td>服务</td>
<td>Hadoop，Hbase，风暴</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>附注 12/18/2014 HDInsight 版

此版本包含以下组件更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>组件</th>
<th>群集类型</th>
<th>JIRA （如果适用）</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">自定义群集常规 Avalability</a></td>
<td><p>自定义项提供自定义 Azure HDInsight 群集与 Apache Hadoop 生态系统中可用的项目的能力。 使用此新功能，可以进行试验，并将 Hadoop 项目部署到 Azure HDInsight。 通过**脚本操作**功能，可以通过使用自定义脚本以任意的方式修改 Hadoop 群集时启用此选项。 此自定义出现在所有类型的 HDInsight 群集包括 Hadoop，HBase 和风暴上。 为了演示此功能的强大，我们已经记录安装常用<a href = "hdinsight-hadoop-spark-install.md" target="_blank">触发</a>、 <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>， <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>和<a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a>模块的过程。 此版本还将添加客户指定 Azure 的门户网站，通过其自定义脚本操作的功能提供了指导原则和最佳做法有关如何生成自定义脚本操作使用的帮助器方法，并提供了有关如何测试脚本的操作指南。 </p></td>
<td>常规功能的可用性</td>
<td>所有</td>
<td>N/A</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>对于 12/05/2014 发行版的 HDInsight 注释

是 HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK n/A

此版本包含以下组件更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>组件</th>
<th>群集类型</th>
<th>JIRA （如果适用）</th>
</tr>

<tr>
<td>Bug 修复︰ 将大量的分区添加到表中配置单元的 DDL 时间歇性错误 </td>
<td><p>如果配置单元表中添加大量的分区时出错间歇性连接配置单元 metastore 数据库，配置单元 DDL 可能会失败。 发生此故障时，将配置单元错误日志中看到以下语句︰ </p><p>"错误 [主要]: ql.驱动程序 (SessionState.java:printError(547))-失败︰ 执行错误，从 org.apache.hadoop.hive.ql.exec.DDLTask 返回代码 1。 MetaException (message:java.lang.RuntimeException: commitTransaction 曾但 openTransactionCalls = 0。 这可能表明有不均衡的对 openTransaction/commitTransaction）"</p></td>
<td>配置单元</td>
<td>Hadoop Hbase</td>
<td>配置单元 482 （这是内部 JIRA，因此它不能从外部引用。 在此注明引用。）</td>
</tr>

<tr>
<td>修复 bug︰ 在 HDInsight 查询控制台中偶尔挂起</td>
<td>这种情况下，WebHCat 启动程序作业的 WebHCat 日志中可以看到下面的语句︰ <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper |org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException)︰ 无法加载历史记录文件 {wasb url 历史记录文件}"</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>配置单元 482 （这是内部 JIRA，因此它不能从外部引用。 在此注明引用。）</td>
</tr>

<tr>
<td>修复 bug︰ 在 Hbase 查询延迟偶尔图文场</td>
<td>如果发生这种情况，用户会注意到 3 秒的偶然峰值在 Hbase 查询延迟。 </td>
<td>网关 HDInsight 群集</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP JAR 文件名称更改</td>
<td>HDI 为群集版本 3.0 中，那里几个对所安装的 HDP 的内部 JAR 文件的更改。 jetty 6.1.26.jar 已经替换为 jetty 6.1.26.hwx.jar。 jetty-实用工具-6.1.26.jar 已替换 jetty-实用工具-6.1.26.hwx.jar。 这些更改将应用于 Hadoop Mahout、 WebHCat，Oozie 的项目。</td>
<td>Hadoop，Mahout，WebHCat Oozie</td>
<td>Hadoop HBase</td>
<td>N/A</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>HDInsight 11/21/2014年发布注意事项

是 HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.9.382.1169709 （11/14/2014年没有更改）
* HDInsight 3.0.5.382.1169709 （11/14/2014年没有更改）
* HDInsight 3.1.1.382.1169709 （11/14/2014年没有更改）
* HDINsight SDK 1.4.0 版

此版本包含以下组件更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>组件</th>
<th>群集类型</th>
<th>JIRA （如果适用）</th>
</tr>

<tr>
<td>访问应用程序日志</td>
<td>若要以编程方式枚举已在群集运行的应用程序和下载相关的特定于应用程序或特定于容器的日志来帮助调试问题的应用程序的能力。</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>在 IHdInsightClient.DeleteCluster 中指定区域名称的能力 </td>
<td>Azure HDInsight SDK 提供了使用**DeleteCluster**时，指定的区域名称的能力。 这有助于允许客户在不同地区有两个具有相同名称的资源，并一度无法删除其中的一个。</td>
<td>SDK</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>**ClusterDetails**对象返回一个**DeploymentID**字段表示群集的唯一标识符。 它保证在群集创建尝试使用相同的名称是唯一的。</td>
<td>SDK</td>
<td>所有</td>
<td>N/A</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>HDInsight 11/14/2014年发布注意事项

是 HDInsight 群集使用此版本部署的完整版本号︰

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

此版本包含以下新功能、 组件更新和 bug 修复。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>组件</th>
<th>群集类型</th>
<th>JIRA （如果适用）</th>
</tr>

<tr>
<td>将操作脚本保存 （预览）</td>
<td>可用于修改 Hadoop 群集自定义特征的预览群集以任意的方式使用自定义脚本。 使用此功能，用户可以试验和部署到 Azure HDInsight 群集可从 Apache Hadoop 生态系统的项目。 此自定义功能是提供对所有类型的 HDInsight 群集，包括 Hadoop，HBase 和风暴。</td>
<td>新功能</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>Azure 网站和存储日志分析的预生成的作业</td>
<td>HDInsight 查询控制台有一个入门的库，支持解决方案，您的数据或示例数据。
<p>**解决方案的工作在您的数据**︰<br>
我们制作了一些最常见的数据分析方案来创建您自己的解决方案提供一个起始点的作业。 与作业中，可以使用您的数据以查看它如何工作。 然后准备就绪后，可使用您已经学会创建的解决方案，仿效预建的作业。</p>
<p>**解决方案的工作示例数据**︰<br>
了解如何使用 HDInsight 来通过依次完成一些基本的方案 （如 web 日志和传感器数据分析）。 您将了解如何使用 HDInsight 来分析此类数据以及如何可以对此数据连接的其他应用程序和服务。 通过连接到 Microsoft Excel 中可视化数据提供的能力的这种方法的一个示例。</p></td>
<td>查询控制台</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>在 Templeton 内存泄漏修复</td>
<td>受影响的客户进行了长时间运行的群集或已提交作业的数百的 Templeton 存在内存泄漏请求已解决第二个。 显示为 Templeton 5xx 错误和解决方法的问题是重新启动该服务。 不再需要此替代方法。</td>
<td>Templeton</td>
<td>所有</td>
<td>https://issues.apache.org/jira/browse/HADOOP-11248</td>
</tr>
</table>
<br>


**注意**︰ 为了演示由自定义群集提供的新功能，使用脚本操作将触发和 R 模块安装在群集上的过程已记录了。 有关详细信息，请参阅︰

* [安装和使用 HDInsight 群集上触发 1.0](hdinsight-hadoop-spark-install.md)
* [上安装和使用 R HDInsight Hadoop 群集](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>HDInsight 11/07/2014年发布注意事项

使用此版本部署的 HDInsight 群集的完整版号为︰

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

此版本包含以下组件更新。

<table border="1">
<tr>
<th>标题</th>
<th>说明</th>
<th>组件</th>
<th>群集类型</th>
<th>JIRA （如果适用）</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>此发行版基于在 Hortonworks 数据平台 (HDP) 2.1.7。 有关详细信息，请参阅<a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">发行说明，了解 HDP 2.1.7</a>。</td>
<td>HDP</td>
<td>所有</td>
<td>N/A</td>
</tr>

<tr>
<td>YARN 日程表服务器</td>
<td>默认情况下已启用 YARN 日程表服务器 （也称为泛型应用程序历史记录）。 时间线服务器提供有关已完成的应用程序 （如应用程序 ID、 应用程序名称、 应用程序状态、 应用程序提交时间和应用程序完成的时间） 的一般信息。

可以通过访问 URI http://headnodehost:8188 或运行 YARN 命令从 head 节点检索此应用程序的信息︰ yarn 应用程序 – – appStates 列出所有。

此信息还可远程尽管 REST API，在 https://{ClusterDnsName}。 azurehdinsight.net/ws/v1/applicationhistory/。

有关更多详细信息，请参阅<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN 日程表服务器</a>。</td>
<td>服务 YARN</td>
<td>Hadoop HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>群集部署 ID</td>
<td>从 SDK 版本 1.3.3.1.5426.29232，用户可以访问每个群集，发出 HDInsight 的唯一 ID。 这使客户能够重复使用 DNS 名称时会确定群集的唯一实例之间创建或删除方案。</td>
<td>SDK</td>
<td>所有</td>
<td>N/A</td>
</tr>
</table>
<br>

**注意**︰ 无法完整的版本号从显示在门户或通过 SDK 或通过 Windows PowerShell 返回 bug 是否已修复此发行版中。

## <a name="notes-for-10152014-release"></a>10/15/2014年发布笔记

该修复程序版本中 Templeton 影响 Templeton 大量用户修复内存泄漏。 在某些情况下，很大程度执行 Templeton 用户将看到错误显示为 500 错误代码，因为请求者没有足够的内存来运行。 为解决此问题的解决方法是重新启动 Templeton 服务。 此问题已得到解决。


## <a name="notes-for-1072014-release"></a>10/7/2014年发布笔记

* 当使用 Ambari 端点"https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}"， *host_name*字段返回完全限定的域名称 (FQDN) 的节点，而不是只使用主机名。 例如，而不是返回"**headnode0**"，得到 FQDN"**headnode0。 {ClusterDNS}.azurehdinsight.net**"。 此更改才能促进的方案 （如 HBase 和 Hadoop） 的多个群集类型可以部署一个虚拟网络中的位置。 发生这种情况，例如，当使用 Hadoop 作为后端平台 HBase。

* 我们提供 HDInsight 群集的默认部署新的内存设置。 以前的默认内存设置没有充分考虑为部署的 CPU 内核的数量的指南。 这些新的内存设置应提供更好的默认值 （根据 Hortonworks 的建议）。 要更改它们，请参阅 SDK 参考文档中有关更改群集配置。 所使用的默认值是 4 CPU 核心 （8 容器） HDInsight 群集的新内存设置是下表中列出。 （以前的发行版中使用的值也将提供 parenthetically。）

<table border="1">
<tr><th>组件</th><th>内存分配</th></tr>
<tr><td> yarn.scheduler.minimum 分配</td><td>768 MB (以前为 512 MB)</td></tr>
<tr><td> yarn.scheduler.maximum 分配</td><td>6144 MB （不变）</td></tr>
<tr><td>yarn.nodemanager.resource.memory</td><td>6144 MB （不变）</td></tr>
<tr><td>mapreduce.map.memory</td><td>768 MB (以前为 512 MB)</td></tr>
<tr><td>mapreduce.map.java.opts</td><td>选择 =-Xmx512m (以前-Xmx410m)</td></tr>
<tr><td>mapreduce.reduce.memory</td><td>1536 MB (以前 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.java.opts</td><td>选择 =-Xmx1024m (以前-Xmx819m)</td></tr>
<tr><td>yarn.app.mapreduce.am.resource</td><td>768 MB (以前 1024 MB)</td></tr>
<tr><td>yarn.app.mapreduce.am.command</td><td>选择 =-Xmx512m (以前-Xmx819m)</td></tr>
<tr><td>mapreduce.task.io.sort</td><td>256 MB (以前为 200 MB)</td></tr>
<tr><td>tez.am.resource.memory</td><td>1536 MB （不变）</td></tr>

</table><br>

关于 YARN 和 MapReduce 由 HDInsight Hortonworks 数据平台上的所使用的内存配置设置的详细信息，请参阅[确定 HDP 内存配置设置](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html)。 Hortonworks 还提供了一种工具来计算正确的内存设置。

有关 Azure PowerShell 和 HDInsight SDK 错误消息:"*群集没有配置为 HTTP 服务访问*":

* 此错误是群集的一个已知的[兼容性问题](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight)，可能会导致 HDInsight SDK 或 Azure PowerShell 的版本和版本中的差异。 在 8 月 15 或更高版本中创建的群集都支持到虚拟网络的新资源调配能力。 但是使用较早版本的 HDInsight SDK 或 Azure PowerShell 没有正确地解释这种能力。 其结果是一些作业提交操作失败。 如果您使用 HDInsight SDK Api 或 Azure PowerShell cmdlet （**使用 AzureRmHDInsightCluster**或**调用 AzureRmHDInsightHiveJob**） 提交作业，这些操作可能会失败并显示错误消息"*群集<clustername>没有配置 HTTP 服务访问*。" 或者 （具体取决于操作），您可能会收到其他错误消息，如"*无法连接到群集*"。

* 最新版本的 HDInsight SDK 和 Azure PowerShell 中解析这些兼容性问题。 我们建议正在 HDInsight SDK 更新到版本 1.3.1.6 或更高版本并且 Azure PowerShell 工具版本 0.8.8 或更高版本。 您可以访问到最新的 HDInsight SDK 从[](http://nuget.codeplex.com/wikipage?title=Getting%20Started)以及如何[安装和配置 Azure PowerShell](../powershell-install-configure.md)的 Azure PowerShell 工具。



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>对 9/12/2014 HDInsight 3.1 版本的说明

* 此发行版基于在 Hortonworks 数据平台 (HDP) 2.1.5。 有关此版本中修复的 bug 的列表，在 Hortonworks 网站上查看[此版本中的固定](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html)页。
* 在小猪库文件夹"avro-mapred-1.7.4.jar"文件已更改为"avro-mapred-1.7.4-hadoop2.jar。" 此文件的内容包含次要 bug 修复的非换行。 建议客户不要进行直接依赖项时，为了避免中断重命名文件的 JAR 文件的名称。


## <a name="notes-for-8212014-release"></a>8/21/2014年发布笔记

* 我们将添加下面的 WebHCat 配置 (配置单元 7155)，其设置 Templeton 控制器作业的默认内存限制为 1 GB。 （以前的默认值是 512 MB）。

     templeton.mapper.memory.mb （= 1024年）

    * 此更改解决了某些配置单元查询已运行由于低内存限制的下述错误:"容器正在运行超出物理内存的限制。"
    * 若要还原到旧的默认值，您可以配置将该值设置为 512 到 Azure PowerShell 创建群集时使用下面的命令︰

        添加 AzureRmHDInsightConfigValues-核心@{"templeton.mapper.memory.mb"="512";}


* Zookeeper 角色的主机名已更改为*zookeeper*。 这将影响群集名称解析，但不会影响其余部分的外部 Api。 如果必须使用*zookeepernode*的主机名称的组件，您需要更新它们以使用新的名称。 三个 zookeeper 节点的新名称是︰
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase 版本支持列表将会更新。 HBase 工作负荷的生产支持仅 HDInsight 版本 3.1 (HBase 0.98 版本)。 版本 3.0 （这是可用于预览） 不支持移动向前。

## <a name="notes-about-clusters-created-prior-to-8152014"></a>有关群集创建在 8/15/2014 的说明

Azure PowerShell 或 HDInsight SDK 错误消息，"群集<clustername>未配置的 HTTP 服务访问"(或根据操作，其他错误等消息:"无法连接到群集") 可能会遇到由于 Azure PowerShell 或 HDInsight SDK 和群集之间的版本差异。 在 8 月 15 或更高版本中创建的群集都支持到虚拟网络的新资源调配能力。 此功能不正确地解释使用较早版本的 Azure PowerShell 或 HDInsight SDK，这将导致故障的作业提交操作。 如果您使用 （例如，使用 AzureRmHDInsightCluster 或调用 AzureRmHDInsightHiveJob） 的 HDInsight SDK Api 或 Azure PowerShell cmdlet 提交作业，这些操作可能会失败与所描述的错误信息之一。

最新版本的 HDInsight SDK 和 Azure PowerShell 中解析这些兼容性问题。 我们建议正在 HDInsight SDK 更新到版本 1.3.1.6 或更高版本并且 Azure PowerShell 工具版本 0.8.8 或更高版本。 您可以访问到最新的 HDInsight SDK [NuGet]从[nuget-link]。 您可以通过使用[Microsoft Web 平台安装程序]访问 Azure PowerShell 工具[webpi-link]。


## <a name="notes-for-7282014-release"></a>7/28/2014年发布笔记

* **HDInsight 在新的地区**︰ 我们到这三个地区展开 HDInsight 地理位置是否存在。 HDInsight 的客户可以在这些区域中创建群集︰
    * 东亚
    * 美国中北部
    * 美国中南部
* 正在从 Azure 门户删除 HDInsight 版本 1.6 （HDP 1.1 和 Hadoop 1.0.3） 和 HDInsight （HDP1.3 和 Hadoop 1.2） 2.1 版。 您可以继续使用 Azure PowerShell cmdlet，[新建 AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx)或使用[HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx)来创建这些版本的 Hadoop 群集。 请[HDInsight 组件版本控制](hdinsight-component-versioning.md)页面的详细信息，参阅。
* Hortonworks 数据平台 (HDP) 在此版本中的更改︰

<table border="1">
<tr><th>HDP</th><th>更改</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>任何更改</td></tr>
<tr><td>HDP 2.0 / 3.0 HDI</td><td>任何更改</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>zookeeper:"3.4.5.2.1.3.0-1948"->"3.4.5.2.1.3.2-0002"</td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>6/24/2014年发布笔记

此版本包含 HDInsight 服务的增强功能︰

* **HDP 2.1 可用性**︰ HDInsight 3.1 （其中包含 HDP 2.1） 是普遍适用，是新的群集的默认版本。
* **HBase – Azure 门户的改进**︰ 我们正在进行 HBase 群集可在预览。 您可以创建只需几次单击门户 HBase 群集。 

使用 HBase，可以多种实时工作负载生成 HDInsight，在从处理大型数据集添加到服务存储从数以百万计的终点传感器和遥测数据的交互式网站。 下一步将是在 Hadoop 作业，这些工作负荷数据进行分析，在 HDInsight 通过 Azure PowerShell 和配置单元群集仪表板中做到这一点。

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout 预装上 HDInsight 3.1

 [Mahout](http://hortonworks.com/hadoop/mahout/)是 HDInsight 3.1 Hadoop 群集上预装的所以您可以运行 Mahout 作业，而不需要附加的群集配置。 例如，您可以将远程到 Hadoop 群集使用远程桌面协议 (RDP) 并没有额外的步骤，您可以运行以下照会世界 Mahout 命令︰

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

有关此过程的更完整说明，请参阅在 Apache Mahout 网站上的[Breiman 示例](https://mahout.apache.org/users/classification/breiman-example.html)文档。


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>配置单元查询可以使用 HDInsight 3.1 中的 Tez

配置单元 0.13 是可用 HDInsight 3.1 所示，它可以运行查询使用 Tez，可以用于实际性能改进。
Tez 不是默认的配置单元查询启用。 若要使用它，必须自愿参加。 通过运行下面的代码段，您可以启用 Tez:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

由于传递的标准基准测试中，Hortonworks 已经发布配置单元查询性能增强，Tez 的细目。 有关详细信息，请参阅[基准 Apache 配置单元 13 企业 Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/)。

有关使用 Tez 配置单元的详细信息，请参阅[配置单元在 Tez 上](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez)。

###<a name="global-availability"></a>在全球范围提供
HDInsight 在 Hadoop 2.2 版本，Microsoft 已 HDInsight 提供在所有主要地区 Azure 不可。 具体来说，西欧洲和东南亚中心已联机。 这使客户能够在很近的数据中心和类似的法规遵从性要求的区域可能找到群集。


###<a name="dos--donts-between-cluster-versions"></a>该做和别群集版本之间

**Oozie metastores 与 HDInsight 3.1 群集一起使用并且不向后的兼容 HDInsight 2.1 群集，并且不能使用这个旧版本**。

与 HDInsight 2.1 群集，使用 HDInsight 3.1 群集部署自定义 Oozie metastore 数据库不能重新使用。 这是这种情况即使 metastore 来自 HDInsight 2.1 群集。 因为获取升级的 metastore 架构，因此不再兼容 HDInsight 2.1 群集所需的 metastore 与 HDInsight 3.1 群集，使用时不支持此方案。 任何尝试重新使用已使用 HDInsight 3.1 群集使用 Oozie metastore 将呈现 HDInsight 2.1 群集毫无用处。

**无法在群集共享 Oozie metastores。**

Oozie metastores 附加到特定群集，并不能在群集共享。

###<a name="breaking-changes"></a>重大更改

**前缀语法**︰ 只有"wasbs: / /"HDInsight 3.1 和 3.0 群集中支持的语法。 较早"asv: / /"HDInsight 2.1 和 1.6 的群集中支持的语法，但它不支持 HDInsight 3.1 或 3.0 的群集中。 这意味着，任何显式使用的作业提交到 HDInsight 3.1 或 3.0 群集"asv: / /"语法将失败。 "Wasbs: / /"应改为使用语法。 此外，作业提交到任何 HDInsight 3.1 或创建包含对资源使用的显式引用现有 metastore 的 3.0 群集"asv: / /"语法将失败。 这些 metastores 需要重新创建使用"wasbs: / /"地址资源的语法。


**端口**︰ 使用 HDInsight 服务的端口已更改。 正在使用的端口号是 Windows 操作系统的临时端口范围内。 从预定义临时短期协议基于互联网通信范围自动分配端口。 超出此范围，以避免遇到与在头节点上运行的服务所使用的端口也可能会发生的冲突是的新的一套允许 Hortonworks 数据平台 (HDP) 服务端口号。 新的端口号不应导致任何重大更改。 使用的数字如下︰

 **1.6 (HDP 1.1) HDInsight**
<table border="1">
<tr><th>名称</th><th>值</th></tr>
<tr><td>dfs.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>dfs.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>dfs.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>dfs.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>dfs.secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.task.tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 和 3.0 (HDP 2.1 和 2.0)**
<table border="1">
<tr><th>名称</th><th>值</th></tr>
<tr><td>dfs.namenode.http 地址</td><td>namenodehost:30070</td></tr>
<tr><td>dfs.namenode.https 地址</td><td>headnodehost:30470</td></tr>
<tr><td>dfs.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>dfs.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>dfs.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>dfs.namenode.secondary.http 地址</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.webapp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>依赖项

在 HDInsight 中添加了以下相关 3.x 版 (HDP2.x):

* guice servlet
* optiq 核心
* javax.inject
* 激活
* jsr305
* geronimo-jaspic_1.0_spec
* 7 月为 slf4j
* java xmlbuilder
* ant
* 一些编译器
* jdo api
* 一些 math3
* paranamer
* jaxb 实现
* stringtemplate
* eigenbase xom
* 广东 servlet
* 一些执行
* jaxb api
* jetty 全部服务器
* janino
* xercesImpl
* optiq avatica
* jta
* eigenbase 属性
* groovy 全部
* hamcrest 核心
* 邮件
* linq4j
* jpam
* 广东客户端
* aopalliance
* geronimo-annotation_1.0_spec
* ant 启动器
* 广东 guice
* xml 的 api
* stax api
* asm-常规
* asm 树
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni 所有
* 速度
* jettison
* 漂亮 java
* jetty 全部
* 一些 dbcp

在 HDInsight 中不再存在以下依存关系 3.x 版 (HDP2.x):

* jdeb
* kfs
* sqlline
* 常青藤
* aspectjrt
* json
* 核心
* jdo2 api
* avro mapred
* datanucleus 增高器
* jsp
* 一些日志记录 api
* 一些数学
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* 不错

###<a name="version-changes"></a>版本更改

以下版本更改之间 HDInsight 2.x (HDP1.x) 和 HDInsight 3.x 版 (HDP2.x):

* 指标的核心:"2.1.2"->"3.0.0"
* derbynet:"10.4.2.0"->"10.10.1.1"
* datanucleus: [rdbms 的 3.0.8]-> [rdbms-3.2.9]
* jasper 编译器:"5.5.12"->"5.5.23"
* log4j: ["1.2.15"、 1.2.16.]-> [1.2.16.，1.2.17.]
* derbyclient:"10.4.2.0"->"10.10.1.1"
* httpcore:"4.2.4"->"4.2.5"
* hsqldb:"1.8.0.10"->"2.0.0"
* jets3t:"0.6.1"->"0.9.0"
* protobuf java: '2.4.1'-'2.5.0' >
* 比赛:"10.4.2.0"->"10.10.1.1"
* jasper: [运行时-5.5.12]-> [运行时-5.5.23]
* 一些后台程序: '1.0.1'-'1.0.13' >
* datanucleus 核心:"3.0.9"->"3.2.10"
* datanucleus-api jdo:"3.0.7"->"3.2.6"
* zookeeper:"3.4.5.1.3.9.0-01320"->"3.4.5.2.1.3.0-1948"
* bonecp:"0.7.1.RELEASE"-> [
* 0.8.0.RELEASE]


### <a name="drivers"></a>驱动程序
SQL Server 的 Java 数据库 Connnectivity (JDBC) 驱动程序由 HDInsight 在内部使用，不用于外部操作。 如果您想要使用开放式数据库连接(ODBC) (ODBC) 连接到 HDInsight，请使用 Microsoft 配置单元的 ODBC 驱动程序。 有关详细信息，请参阅[将 Excel 连接到 Microsoft 配置单元 ODBC 驱动程序的 HDInsight](hdinsight-connect-excel-hive-odbc-driver.md)。


### <a name="bug-fixes"></a>错误修复

此发行版中，我们已刷新以下 HDInsight 版本的几个修补程序︰

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Hortonworks 发行说明

发行说明，了解 Hortonworks 数据平台 (HDPs) 所使用的 HDInsight 版本群集可在以下位置︰

* HDInsight 3.1 版使用 Hadoop 分布，基于[Hortonworks 数据平台 2.1.7][hdp-2-1-7]。 这是默认 Hadoop 群集创建在后 11/7/2014年使用 Azure 的门户。 基于[Hortonworks 数据平台 2.1.1] 11/7/2014年前创建的 HDInsight 3.1 群集[hdp-2-1-1]

* HDInsight 版本 3.0 使用 Hadoop 分布，基于[Hortonworks 数据平台 2.0][hdp-2-0-8]。

* HDInsight 2.1 版使用 Hadoop 分布，基于[Hortonworks 数据平台 1.3][hdp-1-3-0]。

* HDInsight 1.6 版使用 Hadoop 分布，基于[Hortonworks 数据平台 1.1][hdp-1-1-0]。

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
