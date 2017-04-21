<properties
   pageTitle="部署和管理 Apache 风暴拓扑上 HDInsight |Microsoft Azure"
   description="了解如何部署、 监视和管理在 HDInsight 上使用风暴仪表板的 Apache 风暴拓扑。 使用 Hadoop 用于 Visual Studio 工具。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>部署和管理基于 Windows 的 HDInsight 上的 Apache 风暴拓扑

风暴面板允许您轻松地部署和使用 web 浏览器 HDInsight 群集运行 Apache 风暴拓扑。 您可以使用仪表板来监视和管理正在运行的拓扑结构。 如果使用 Visual Studio 时，Visual Studio 的 HDInsight 工具提供了在 Visual Studio 中的类似功能。

风暴的仪表板和 HDInsight 工具中的风暴功能依赖于风暴 REST API，可用于创建自己的监视和管理解决方案。

> [AZURE.IMPORTANT] 本文档中的步骤要求 HDInsight 群集上的基于 Windows 的风暴。 使用基于 Linux 的群集的信息，请参阅[部署和管理基于 Linux 的 HDInsight 上的 Apache 风暴拓扑](hdinsight-storm-deploy-monitor-topology-linux.md)

##<a name="prerequisites"></a>系统必备组件

* **在 HDInsight 上的 Apache 风暴**-请参阅创建群集的步骤<a href="../hdinsight-storm-getting-started/" target="_blank">开始使用 HDInsight 上的 Apache 风暴</a>

* **风暴的仪表板**︰ 支持 HTML5 的现代 web 浏览器

* **Visual Studio**的 Azure SDK 2.5.1 版或更高版本和 Visual Studio 的 HDInsight 工具。 请参阅<a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">开始使用 Visual Studio 的 HDInsight 工具</a>来安装和配置 Visual Studio 的 HDInsight 工具。

    下面的 Visual Studio 版本之一︰

    * Visual Studio 2012 与<a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">更新 4</a>

    * Visual Studio 2013 年<a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">更新 4</a>或<a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio 2013年社区</a>

    * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>

    > [AZURE.NOTE] 目前用于 Visual Studio 的 HDInsight 工具仅支持风暴 HDInsight 群集 3.2 版上。

##<a name="storm-dashboard"></a>风暴的仪表板

风暴的仪表板是风暴群集上可用的 web 页。 URL 是**https://&lt;群集名称 >.azurehdinsight.net/**，其中**群集名称**是您在 HDInsight 群集上风暴的名称。

从风暴面板的顶部，选择**提交拓扑**。 按照运行示例拓扑也可以上载和运行您创建的拓扑结构页上的说明。

![提交拓扑结构页][storm-dashboard-submit]

###<a name="storm-ui"></a>UI 的暴风雨

从风暴面板中，选择**风暴 UI**的链接。 这将显示有关该群集，以及任何正在运行的拓扑信息。

![风暴的 ui][storm-dashboard-ui]

> [AZURE.NOTE] 某些版本的 Internet Explorer 中，您可能会发现风暴 UI 不会刷新后您第一次访问它。 例如，它可能不会显示新的拓扑提交，或您以前停用它时，它可显示为活动拓扑。 Microsoft 已意识到此问题，使用的解决方案。

####<a name="main-page"></a>主页面

冲击 UI 的主页面提供了以下信息︰

* **摘要的群集**︰ 群集风暴有关的基本信息。

* **拓扑摘要**︰ 运行拓扑结构的列表。 使用本部分中的链接以查看有关特定拓扑结构的详细信息。

* **主管摘要**︰ 风暴超级用户有关的信息。

* **Nimbus 配置**︰ Nimbus 群集配置。

####<a name="topology-summary"></a>拓扑摘要

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

####<a name="spout-and-bolt-summary"></a>管口和螺栓摘要

从**Spouts**或**螺栓**部分选择管口显示有关所选物料的以下信息︰

* **组件摘要**︰ 管口或螺栓的基本信息。

* **管口/螺栓统计**︰ 统计有关的管口或螺栓。 使用**窗口**列中的链接来设置页面上剩余的条目的时间段。

* **输入统计数据**（只螺栓）︰ 由螺栓使用输入流的信息。

* **输出的统计数字**︰ 流有关的信息由此发出 spout 或螺栓。

* **执行器**︰ 管口或螺栓的实例信息。 选择**端口**特定的执行器，以查看此实例生成的诊断信息的日志条目。

* **错误**︰ 此错误信息 spout 或螺栓。

##<a name="hdinsight-tools-for-visual-studio"></a>Visual Studio 的 HDInsight 工具

HDInsight 工具可以用于提交到风暴群集的 C# 或混合拓扑。 下列步骤将使用一个示例应用程序。 有关使用 HDInsight 工具创建您自己的拓扑信息，请参见[使用 Visual Studio 的 HDInsight 工具开发 C# 拓扑](hdinsight-storm-develop-csharp-visual-studio-topology.md)。

使用以下步骤来将一个示例部署到您在 HDInsight 群集上的风暴，然后查看和管理拓扑。

1. 如果有 HDInsight 工具的最新版本的 Visual Studio 安装，请参阅<a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">开始使用 Visual Studio 的 HDInsight 工具</a>。

2. 打开 Visual Studio 中，选择**文件** > **New** > **项目**。

3. 在**新建项目**对话框中，展开**安装** > **模板**，然后选择**HDInsight**。 从模板列表中，选择**风暴示例**。 底部的对话框中，键入应用程序的名称。

    ![图像](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

1. 在**解决方案资源管理器**中用鼠标右键单击该项目，然后选择**提交到 HDInsight 上的冲击**。

    > [AZURE.NOTE] 如果出现提示，请输入 Azure 订阅的登录凭据。 如果您有多个订阅，登录到包含您在 HDInsight 群集上的冲击。

2. 从**风暴群集**下拉列表中，选择您在 HDInsight 群集上的风暴，然后选择**提交**。 您可以监视是否提交是成功的通过使用**输出**窗口。

3. 已成功提交拓扑结构，应显示为群集的**风暴拓扑**。 选择从列表以查看有关正在运行的拓扑信息的拓扑结构。

    ![visual studio 监视器](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

    > [AZURE.NOTE] 您还可以查看从**服务器资源管理器**的**电流冲击拓扑**通过展开**Azure** > **HDInsight**，然后右键单击 HDInsight 群集上的风暴并选择**视图风暴拓扑**。

    选择形状的 spouts 或螺栓以查看有关这些组件的信息。 对于每个选定的项将打开一个新窗口。
    
    > [AZURE.NOTE] 拓扑的名称是拓扑的类名称 (在这种情况下， `HelloWord`，) 追加时间戳。

4. 从**拓扑摘要**视图中，选择**取消**停止拓扑。

    > [AZURE.NOTE] 风暴的拓扑继续运行，直到它们停止或群集已被删除。

##<a name="rest-api"></a>REST API，

风暴 UI 是基于 REST API，使您可以执行类似的管理和监视功能，通过使用 REST API。 可以使用 REST API 来创建用于管理和监视风暴拓扑的自定义工具。

有关详细信息，请参阅[REST API，风暴 UI](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md)。 以下信息是针对 REST API，使用 HDInsight 上的 Apache 风暴。

###<a name="base-uri"></a>基 URI

针对 REST API，HDInsight 群集上的基 URI 是**https://&lt;群集名称 >.azurehdinsight.net/stormui/api/v1/**，其中**群集名称**是您在 HDInsight 群集上风暴的名称。

###<a name="authentication"></a>身份验证

对 REST API 的请求必须使用**基本身份验证**，因此您使用 HDInsight 群集管理员名称和密码。

> [AZURE.NOTE] 由于基本身份验证使用明文发送，您应该**始终**使用 HTTPS 来保护与群集的通信。

###<a name="return-values"></a>返回值

从 REST API 返回的信息可能只是从群集或群集相同 Azure 虚拟网络上的虚拟机中使用。 例如，返回 Zookeeper 服务器的完全合格的域名称 (FQDN) 不会从 Internet 进行访问。

##<a name="next-steps"></a>下一步行动

现在，您已经了解如何部署和监视拓扑通过风暴仪表板，学习如何︰

* [开发对 Visual Studio 中使用 HDInsight 工具 C# 拓扑](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [开发使用 Maven 的基于 Java 的拓扑](hdinsight-storm-develop-java-topology.md)

有关更多示例拓扑的列表，请参阅[风暴在 HDInsight 上的示例拓扑](hdinsight-storm-example-topology.md)。

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
