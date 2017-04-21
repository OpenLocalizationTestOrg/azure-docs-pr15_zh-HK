<properties
    pageTitle="在基于 Linux 的 HDInsight R 安装 |Microsoft Azure"
    description="了解如何安装和使用 R 自定义基于 Linux 的 Hadoop 群集。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>上安装和使用 R HDInsight Hadoop 群集

通过使用**脚本操作**群集的自定义项，可以在任何类型的 HDInsight 在 Hadoop 群集上安装 R。 这使得数据科学家和分析家使用 R 来部署功能强大的 MapReduce/YARN 编程框架来处理大量的数据在 HDInsight 中所部署的 Hadoop 群集上。

> [AZURE.IMPORTANT] 为 HDInsight 提供[特优层](https://azure.microsoft.com/pricing/details/hdinsight/)包括 R 服务器作为 HDInsight 群集的一部分。 这允许使用 MapReduce 和触发运行分布式的计算 R 脚本。 有关详细信息，请参阅[开始使用 R 服务器 HDInsight 上](hdinsight-hadoop-r-server-get-started.md)。 


## <a name="what-is-r"></a>R 是什么？

<a href="http://www.r-project.org/" target="_blank">用于统计计算的 R 项目</a>是开放源代码语言和环境的统计计算。 R 提供数百个生成中的统计函数和自己相结合方面的功能和面向对象的编程的编程语言。 它还提供了丰富的图形功能。 R 是大多数专业统计学家和科学家在各种不同的字段中首选的编程环境。

R 脚本可以在 HDInsight 中使用脚本操作时创建安装 R 环境自定义的 Hadoop 群集上运行。 R 是兼容与 Azure Blob 存储 (WASB)，以便可以在 HDInsight 上使用 R 处理其中所存储的数据。

## <a name="what-the-script-does"></a>该脚本的执行

使用 HDInsight 群集上安装 R 的脚本操作将安装以下的 Ubuntu 包提供了基本的 R 安装︰

* [r 基](http://packages.ubuntu.com/precise/r-base)︰ 基 GNU R 包
* [适用于开发人员 r 基](http://packages.ubuntu.com/precise/r-base-dev)︰ 辅助 GNU R 包

此外安装下面的 RHadoop 包，其中 HDFS MapReduce 与提供的集成︰

* [rmr2](https://github.com/RevolutionAnalytics/rmr2)︰ 允许 R 开发人员使用 Hadoop MapReduce
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs)︰ 允许 R 开发人员使用 Hadoop HDFS (WASB 的 HDInsight)

此外，下面的 R 包的安装︰

| R 包 | 它提供了 |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | 低级别 R 到 Java 接口。 |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | R 和 c + + 集成。 |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | 序列化/反序列化 JSON R 对象 |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | 对于按位运算整数向量上的功能。 |
| [摘要](https://cran.r-project.org/web/packages/digest/index.html) | 创建加密哈希值的 R 对象的摘要。 |
| [功能](https://cran.r-project.org/web/packages/functional/index.html) | .柯里、 撰写中和其他高阶函数 |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | 灵活地重新构建和聚合数据。 |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | 简单、 一致的普通字符串操作的包装。 |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | 用于拆分、 应用以及组合的数据的工具。 |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | 用于移动窗口统计，GIF，Base64，台湾 AUC 工具等。 |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | 近似匹配的字符串，字符串的距离函数。 |

## <a name="install-r-using-script-actions"></a>安装使用脚本操作 R

下面的脚本操作使用 HDInsight 群集上安装 R。 https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    
本节提供有关如何使用脚本创建新群集使用 Azure 门户时说明。 

> [AZURE.NOTE] Azure PowerShell、 Azure CLI、 HDInsight.NET SDK 或 Azure 资源管理器模板还可应用脚本操作。 此外可以将脚本操作应用到已在运行的群集。 有关详细信息，请参阅[自定义 HDInsight 群集使用脚本的操作](hdinsight-hadoop-customize-cluster-linux.md)。

1. 开始调配通过步骤在[配置 Linux 基于 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md#portal)中，群集，但未完成资源调配。

2. **可选配置**刀片式服务器，选择**脚本操作**，并提供以下信息︰

    * __名称__︰ 输入脚本操作的友好名称。
    * __脚本的 URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __头部__︰ 选中该选项
    * __工作人员__︰ 选中该选项
    * __ZOOKEEPER__︰ 选中此选项可在 Zookeeper 节点上安装。
    * __参数__︰ 将此字段保留为空

3. 底部的**脚本操作**，使用**选择**按钮以保存配置。 最后，使用**可选配置**刀片底部的**选择**按钮保存可选配置信息。

4. 继续[提供 Linux 基于 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md#portal)中所述配置群集。

## <a name="run-r-scripts"></a>运行 R 脚本

设置完群集后，通过以下步骤，使用 R 执行 MapReduce 的群集操作。

1. 连接到使用 SSH 的 HDInsight 群集︰

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    在 HDInsight 中使用 SSH 的详细信息，请参阅以下资源︰

    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 从`username@hn0-CLUSTERNAME:~$`提示下，输入以下命令以启动一个交互式的 R 会话︰

        R

3. 输入下面的 R 程序。 这生成编号 1 至 100，然后将其乘以 2。

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    第一行调用 RHadoop 库 rmr2，用于 MapReduce 操作。

    第二行生成 1-100 之间的值，然后将它们存储到 Hadoop 文件系统使用`to.dfs`。

    第三行创建 MapReduce 过程中使用 rmr2，所提供的功能，并开始处理。 您应该看到滚过处理开始时的几行。

4. 接下来，使用以下方法来查看 MapReduce 输出存储到临时路径︰

        print(calc())

    这应该是类似于`/tmp/file5f615d870ad2`。 若要查看实际的输出，请使用以下方法︰

        print(from.dfs(calc))

    输出应如下所示︰

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. 对退出 R，输入以下命令︰

        q()


## <a name="next-steps"></a>下一步行动

- [安装和使用群集在 HDInsight 上的色调](hdinsight-hadoop-hue-linux.md)。 色调是一个 web 用户界面，便于创建、 运行和小猪和配置单元作业，保存以及您 HDInsight 浏览默认存储群集。

- [在 HDInsight 群集上安装 Giraph](hdinsight-hadoop-giraph-install.md)。 使用自定义群集在 HDInsight Hadoop 群集上安装 Giraph。 Giraph 允许您执行使用 Hadoop，图形处理和使用 Azure HDInsight。

- [在 HDInsight 群集上安装 Solr](hdinsight-hadoop-solr-install.md)。 使用自定义群集在 HDInsight Hadoop 群集上安装 Solr。 Solr 允许您执行存储数据的功能强大的搜索操作。

- [安装 HDInsight 群集上的色调](hdinsight-hadoop-hue-linux.md)。 使用自定义群集安装 HDInsight Hadoop 群集上的色调。 色调是一套用于与 Hadoop 群集进行交互的 Web 应用程序。

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
