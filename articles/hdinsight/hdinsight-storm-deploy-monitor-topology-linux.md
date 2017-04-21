<properties
   pageTitle="部署和管理基于 Linux 的 HDInsight 上的 Apache 风暴拓扑 |Microsoft Azure"
   description="了解如何部署、 监控和管理基于 Linux 的 HDInsight 上使用风暴仪表板的 Apache 风暴拓扑。 使用 Hadoop 用于 Visual Studio 工具。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>部署和管理基于 Linux 的 HDInsight 上的 Apache 风暴拓扑

在本文中，学习管理和监视运行在 HDInsight 群集上的基于 Linux 的风暴冲击拓扑结构的基础知识。

> [AZURE.IMPORTANT] 这篇文章中的步骤需要在 HDInsight 群集上基于 Linux 的风暴。 对部署和监控上的基于 Windows 的 HDInsight 拓扑的信息，请参阅[部署和管理基于 Windows 的 HDInsight 上的 Apache 风暴拓扑](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>系统必备组件

* **在 HDInsight 群集上的基于 Linux 的风暴**︰ 请参阅创建群集的步骤[开始使用 HDInsight 上的 Apache 风暴](hdinsight-apache-storm-tutorial-get-started-linux.md)

* **熟悉 SSH 和 SCP**︰ 在 HDInsight 中使用 SSH 和 SCP 的详细信息，请参阅以下︰

    * **Linux、 Unix 或 OS X 客户机**︰ 请[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 或 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows 客户端**︰ 请[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)

* **SCP 客户端**︰ 这所有 Linux 和 Unix 中，OS X 系统都提供。 对于 Windows 客户端，建议 PSCP，即可以从[PuTTY 下载页面](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

## <a name="start-a-storm-topology"></a>启动冲击拓扑

### <a name="using-ssh-and-the-storm-command"></a>使用 SSH 和风暴命令

1. 使用 SSH 连接到 HDInsight 群集。 更换**用户名**的 SSH 登录名称。 **群集名称**替换您的 HDInsight 群集名称︰

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    使用 SSH 连接到 HDInsight 群集的详细信息，请参阅以下文档︰
    
    * **Linux、 Unix 或 OS X 客户机**︰ 请[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 或 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows 客户端**︰ 请[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 使用下面的命令启动一个示例拓扑︰

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    这将启动群集上的字数统计的拓扑示例。 它将随机生成的句子并计数在句子中的每个单词的匹配项。

    > [AZURE.NOTE] 当提交到群集拓扑，必须先将复制使用之前包含群集的 jar 文件`storm`命令。 这可通过`scp`命令从客户端文件所在的位置。 例如，`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > 字数统计等和其他风暴初学者的示例中，已包含在群集上`/usr/hdp/current/storm-client/contrib/storm-starter/`。

### <a name="programmatically"></a>以编程方式

您可以以编程方式部署拓扑到 HDInsight 上的风暴与承载您的群集在 Nimbus 服务进行通信。 [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology)提供了一个示例演示如何部署和启动通过 Nimbus 服务拓扑的 Java 应用程序。

## <a name="monitor-and-manage-using-the-storm-command"></a>监视和管理使用的风暴命令

`storm`实用程序允许您从命令行运行拓扑处理。 以下是常用的命令的列表。 使用`storm -h`有关命令的完整列表。

### <a name="list-topologies"></a>列表中的拓扑

使用以下命令列出所有正在运行的拓扑︰

    storm list
    
这将返回类似于以下内容的信息︰

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>停用并重新激活

停用拓扑暂停它，直到它被终止或重新激活。 使用以下方法来停用并重新激活︰

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>取消一个正在运行的拓扑结构

风暴的拓扑中，启动后将继续运行，直到停止。 要停止一个拓扑，请使用下面的命令︰

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>重新平衡

重新平衡了拓扑结构允许修改拓扑结构的并行系统。 例如，如果已调整大小的群集中添加更多笔记，重新平衡将允许正在运行的拓扑结构，以便使用新的节点。

> [AZURE.WARNING] 首先重新平衡拓扑停用拓扑，然后重新工作人员均匀分布整个群集，然后最后返回到重新平衡发生之前的状态的拓扑结构。 因此如果拓扑处于活动状态，它将再次活动。 如果它被停用，它将保持已停用。

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>监视和管理使用风暴用户界面

风暴用户界面提供了一个 web 界面处理运行的拓扑，并将包括在 HDInsight 群集。 若要查看风暴 UI，使用 web 浏览器打开__https://CLUSTERNAME.azurehdinsight.net/stormui__，其中__群集名称__是该群集的名称。

> [AZURE.NOTE] 如果要求提供用户名和密码，输入群集管理员 （管理员） 和密码时使用创建群集。


### <a name="main-page"></a>主页面

冲击 UI 的主页面提供了以下信息︰

* **摘要的群集**︰ 群集风暴有关的基本信息。

* **拓扑摘要**︰ 运行拓扑结构的列表。 使用本部分中的链接以查看有关特定拓扑结构的详细信息。

* **主管摘要**︰ 风暴超级用户有关的信息。

* **Nimbus 配置**︰ Nimbus 群集配置。

### <a name="topology-summary"></a>拓扑摘要

从**拓扑摘要**部分选择一个链接显示拓扑结构有关的下列信息︰

* **拓扑摘要**︰ 拓扑的基本信息。

* **拓扑操作**︰ 为拓扑可以执行的管理操作。

  * **激活**︰ 继续处理已停用的拓扑结构。

  * **停用**︰ 暂停正在运行的拓扑结构。

  * **重新平衡**︰ 调整拓扑结构的并行性。 已更改群集中的节点数后，应该重新平衡运行拓扑。 这使拓扑结构调整并行度来弥补它在群集中的节点的增加或减少数。

    有关详细信息，请参阅<a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">了解风暴拓扑结构的并行性</a>。

  * **终止**︰ 在指定的超时时间后终止风暴拓扑。

* **拓扑统计**︰ 拓扑结构有关的统计信息。 使用**窗口**列中的链接来设置页面上剩余的条目的时间段。

* **Spouts**: spouts 使用的拓扑。 使用本部分中的链接以查看有关特定 spouts 的详细信息。

* **螺栓**︰ 螺栓使用的拓扑。 使用本部分中的链接以查看有关特定细节的详细信息。

* **拓扑配置**︰ 选定的拓扑配置。

### <a name="spout-and-bolt-summary"></a>管口和螺栓摘要

从**Spouts**或**螺栓**部分选择管口显示有关所选物料的以下信息︰

* **组件摘要**︰ 管口或螺栓的基本信息。

* **管口/螺栓统计**︰ 统计有关的管口或螺栓。 使用**窗口**列中的链接来设置页面上剩余的条目的时间段。

* **输入统计数据**（只螺栓）︰ 由螺栓使用输入流的信息。

* **输出的统计数字**︰ 流有关的信息由此发出 spout 或螺栓。

* **执行器**︰ 管口或螺栓的实例信息。 选择**端口**特定的执行器，以查看此实例生成的诊断信息的日志条目。

* **错误**︰ 此错误信息 spout 或螺栓。

## <a name="rest-api"></a>REST API，

风暴 UI 是基于 REST API，使您可以执行类似的管理和监视功能，通过使用 REST API。 可以使用 REST API 来创建用于管理和监视风暴拓扑的自定义工具。

有关详细信息，请参阅[REST API，风暴 UI](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html)。 以下信息是针对 REST API，使用 HDInsight 上的 Apache 风暴。

> [AZURE.IMPORTANT] 冲击 REST API，通过互联网，不公开，必须使用 HDInsight 群集的头节点到 SSH 隧道来访问。 有关创建和使用 SSH 隧道的信息，请参阅[使用 SSH 隧道 Ambari web 用户界面、 ResourceManager、 JobHistory、 NameNode，Oozie 和其他的 web 用户界面的访问](hdinsight-linux-ambari-ssh-tunnel.md)。

### <a name="base-uri"></a>基 URI

针对 REST API，对基于 Linux 的 HDInsight 群集的基 URI 是在头节点上可用**https://HEADNODEFQDN:8744/api/v1/**;但是，域名的头节点在群集创建过程中生成并不是静态的。

您可以以多种不同方式簇头节点找到完全合格的域名称 (FQDN):

* __从 SSH 会话__︰ 使用命令`headnode -f`从群集的 SSH 会话。

* __从 Ambari 网站__︰ 顶部的页上，选择__服务__，然后选择__风暴__。 从__摘要__选项卡，选择__风暴 UI 服务器__。 在页面的顶部将风暴 UI 和 REST API，运行在该节点的 FQDN。

* __从 Ambari REST API__︰ 使用命令`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"`来检索有关风暴 UI 和 REST API，运行在该节点的信息。 __密码__替换群集的管理员密码。 __群集名称__替换群集名称。 在响应中，"host_name"条目包含节点的 FQDN。

### <a name="authentication"></a>身份验证

对 REST API 的请求必须使用**基本身份验证**，因此您使用 HDInsight 群集管理员名称和密码。

> [AZURE.NOTE] 由于基本身份验证使用明文发送，您应该**始终**使用 HTTPS 来保护与群集的通信。

### <a name="return-values"></a>返回值

从 REST API 返回的信息可能只是从群集或群集相同 Azure 虚拟网络上的虚拟机中使用。 例如，返回 Zookeeper 服务器的完全合格的域名称 (FQDN) 不会从 Internet 进行访问。

## <a name="next-steps"></a>下一步行动

现在，您已经了解如何部署和监视拓扑通过风暴仪表板，学习如何[使用 Maven 基于 Java 开发的拓扑结构](hdinsight-storm-develop-java-topology.md)。

有关更多示例拓扑的列表，请参阅[风暴在 HDInsight 上的示例拓扑](hdinsight-storm-example-topology.md)。
