<properties
    pageTitle="安装并使用基于 Linux 的 HDInsight (Hadoop) Giraph |Microsoft Azure"
    description="了解如何使用脚本操作的基于 Linux 的 HDInsight 群集上安装 Giraph。 脚本操作允许您自定义群集创建过程中，更改群集配置或安装服务和实用程序。"
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
    ms.date="10/17/2016"
    ms.author="larryfr"/>

# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>在 HDInsight Hadoop 群集上安装 Giraph，并使用 Giraph 来处理大规模的关系图

通过**脚本操作**自定义群集，您可以在任何类型的 Azure HDInsight 在 Hadoop 群集上安装 Giraph。

在本主题中，您将学习如何通过脚本操作安装 Giraph。 一旦您安装了 Giraph，您还将学习如何使用 Giraph 为最典型的应用程序，这是处理大规模的关系图。

> [AZURE.NOTE] 这篇文章中的信息是特定于基于 Linux 的 HDInsight 群集。 使用基于 Windows 群集的信息，请参阅[安装的 Giraph HDInsight Hadoop 群集 (Windows) 上](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>Giraph 是什么？

[Apache Giraph](http://giraph.apache.org/)允许您执行图形使用 Hadoop，处理，并可以使用 Azure HDInsight。 关系图的模型对象，如大型互联网，如网络上的路由器或上社交网络 （有时称为社交图） 的人之间的关系之间的连接之间的关系。 图形处理允许您为图中的对象之间的关系有关的原因如︰

- 根据您当前的关系识别潜在的朋友。
- 用于标识网络中的两台计算机之间最短的路线。
- 计算网页的网页排名。

> [AZURE.WARNING] 完全支持使用 HDInsight 群集提供组件和 Microsoft 支持将有助于确定和解决与这些组件相关的问题。
>
> 自定义组件，如 Giraph，接收商业上合理的支持，以帮助您进一步排查该问题。 这可能导致解决问题或要求您能够进行深入的专业技能，为该技术在其中找到的开放源代码技术可用的频道。 例如，有许多社区站点可以使用，如︰ [HDInsight 的 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight)、 [http://stackoverflow.com](http://stackoverflow.com)。 Apache 项目还有项目网站上[http://apache.org](http://apache.org)，例如︰ [Hadoop](http://hadoop.apache.org/)。

##<a name="what-the-script-does"></a>该脚本的执行

该脚本执行以下操作︰

* 安装到 Giraph`/usr/hdp/current/giraph`
* 复制`giraph-examples.jar`为群集默认存储 (WASB) 的文件︰`/example/jars/giraph-examples.jar`

## <a name="install"></a>安装 Giraph 使用脚本的操作

在 HDInsight 群集上安装 Giraph 的示例脚本可在以下位置。

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

本节说明如何通过使用 Azure 门户创建群集时使用的示例脚本。 

> [AZURE.NOTE] Azure PowerShell、 Azure CLI、 HDInsight.NET SDK 或 Azure 资源管理器模板还可应用脚本操作。 此外可以将脚本操作应用到已在运行的群集。 有关详细信息，请参阅[自定义 HDInsight 群集使用脚本的操作](hdinsight-hadoop-customize-cluster-linux.md)。

1. 开始创建群集使用的步骤中[创建 Linux 基于 HDInsight 群集](hdinsight-hadoop-create-linux-clusters-portal.md)，但未完成创建。

2. **可选配置**刀片式服务器，选择**脚本操作**，并提供以下信息︰

    * __名称__︰ 输入脚本操作的友好名称。
    * __脚本的 URI__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    * __头部__︰ 选中该选项
    * __工作人员__︰ 取消选中
    * __ZOOKEEPER__︰ 取消选中
    * __参数__︰ 将此字段保留为空

3. 底部的**脚本操作**，使用**选择**按钮以保存配置。 最后，使用**可选配置**刀片底部的**选择**按钮保存可选配置信息。

4. 继续创建群集，在[群集中创建 Linux 基于 HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md)所述。

## <a name="usegiraph"></a>如何在 HDInsight 中使用 Giraph？

完成后创建的群集，使用以下步骤运行 Giraph 中包含的 SimpleShortestPathsComputation 示例。 这将实现基本的<a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a>实现，寻找图中的对象之间的最短路径。

1. 连接到使用 SSH 的 HDInsight 群集︰

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    在 HDInsight 中使用 SSH 的详细信息，请参阅以下资源︰

    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

1. 使用以下方法来创建一个名为__tiny_graph.txt__的新文件︰

        nano tiny_graph.txt

    使用以下内容作为本文件的内容︰

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    此数据描述了有向图，使用的格式中的对象之间的关系 [源\_id、 源\_值，[[目标\_id]，[边缘\_值]，...]]。 每行表示之间的关系**源\_id**对象和一个或多个**目标\_id**对象。 **边缘\_值**（或粗细） 可以被认为是强度或**source_id**之间的连接的距离和**目标\_id**。

    绘制出、 并用作对象之间的距离的值 （或称重），上面的数据可能如下所示︰

    ![绘制圆行不同距离为 tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. 若要保存该文件，使用__Ctrl + X__， __Y__，然后最后__enter 键__接受的文件的名称。

3. 使用以下方法来将数据存储到主存储为 HDInsight 群集︰

        hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt

4. 运行 SimpleShortestPathsComputation 示例使用下面的命令。

         yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

    与此命令一起使用的参数如下表所述。

  	| 参数 | 它的作用 |
  	| --------- | ------------ |
  	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | 包含的示例 jar 文件。 |
  	| `org.apache.giraph.GiraphRunner` | 用来启动这些示例类。 |
  	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | 将运行该示例。 在这种情况下，它将计算 ID 1 和其他关系图中的所有 Id 之间的最短路径。 |
  	| `-ca mapred.job.tracker=headnodehost:9010` | 群集的 headnode。 |
  	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | 输入要用于输入数据的格式。 |
  	| `-vip /example/data/tiny_graph.txt` | 输入的数据文件。 |
  	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | 输出格式。 在这种情况下，ID 和值为纯文本。 |
  	| `-op /example/output/shortestpaths` | 输出位置。 |
  	| `-w 2` | 要使用的工作人员数目。 在此情况下，2。 |

    这些，与 Giraph 示例一起使用的其他参数的详细信息，请参阅[Giraph 快速入门](http://giraph.apache.org/quick_start.html)。

5. 完成作业后，将结果存储在__wasbs: / 示例/出/shotestpaths__目录。 所创建的文件将开始与__部件-m-__和结束与一个数字，指示第一、 第二、 等文件。 使用以下方法来查看输出︰

        hdfs dfs -text /example/output/shortestpaths/*

    输出应类似于以下内容︰

        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    示例是硬编码在开头使用 SimpleShortestPathComputation 对象 ID 为 1，并查找其他对象的最短路径。 因此应作为读取输出`destination_id distance`，其中距离是旅游过的对象 ID 1 和目标 ID 之间的边缘值 （或称重）

    直观显示这种情况，可以通过旅行 ID 1 和所有其他对象之间的最短路径验证结果。 请注意，ID 1 和 ID 4 之间的最短路径 5。 这是<span style="color:orange">ID 1 和 3</span>，然后<span style="color:red">ID 3 和 4</span>之间的总距离。

    ![用绘制之间的最短路径为圆形绘制的对象](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## <a name="next-steps"></a>下一步行动

- [安装和使用群集在 HDInsight 上的色调](hdinsight-hadoop-hue-linux.md)。 色调是一个 web 用户界面，便于创建、 运行和小猪和配置单元作业，保存以及您 HDInsight 浏览默认存储群集。

- [在 HDInsight 群集上安装 R](hdinsight-hadoop-r-scripts-linux.md)︰ 说明如何使用群集安装和 HDInsight Hadoop 群集上使用 R 的自定义。 R 是一种开源的语言和环境统计计算的。 它提供了数百个内置统计函数和自己相结合方面的功能和面向对象的编程的编程语言。 它还提供了丰富的图形功能。

- [在 HDInsight 群集上安装 Solr](hdinsight-hadoop-solr-install-linux.md)。 使用自定义群集在 HDInsight Hadoop 群集上安装 Solr。 Solr 允许您执行存储数据的功能强大的搜索操作。
