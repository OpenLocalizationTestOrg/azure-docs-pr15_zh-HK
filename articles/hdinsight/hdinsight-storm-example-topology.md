<properties
 pageTitle="Apache 风暴拓扑示例在 HDInsight |Microsoft Azure"
 description="创建和经 HDInsight 包括基本 C# 和 Java 拓扑结构，并使用事件集线器上的 Apache 风暴冲击拓扑示例的列表。"
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
 ms.date="08/23/2016"
 ms.author="larryfr"/>

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>示例风暴 toplogies 和 HDInsight 上的 Apache 风暴的组件

以下是创建和维护由 Microsoft 使用 Apache 风暴在 HDInsight 上的示例列表。 这些示例涉及各种主题，从创建基本的 C# 和 Java 拓扑等事件集线器、 DocumentDB、 电源 BI、 SQL 数据库、 HDInsight，和 Azure 存储 HBase Azure 服务与合作。 一些示例还演示如何使用非-Azure 或甚至非 Microsoft 技术，那么 SignalR 和 Socket.IO 等

| 说明                                                                                             | 演示                                         | 语言/框架         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [从 Apache 风暴写到 Azure 数据湖存储区](hdinsight-storm-write-data-lake-store.md) | Azure 数据湖存储区写入 | Java |
| [源事件中心 Spout 和螺栓](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | 源事件中心管口和螺栓 | Java |
| [在 HDInsight 上的 Apache 风暴为开发基于 Java 的拓扑][5797064f]                                 | Maven                                                | Java                       |
| [在使用 Visual Studio 的 HDInsight 上的 Apache 风暴为开发 C# 拓扑][16fce2d1]                     | Visual Studio 的 HDInsight 工具                    | C# Java                   |
| [在 C# 风暴拓扑中创建多个数据流][ec5a4064]                                         | 多个流                                     | C#                         |
| [确定在 HDInsight 上的风暴与 Twitter 趋势主题][3c86c7c8]                                   | Trident                                              | Java Trident              |
| [HDInsight (C#) 上的风暴与 Azure 事件集线器从流程事件][844d1d81]                                | 事件集线器                                           | C# 和 Java                |
| [从 Azure 事件集线器上 HDInsight (Java) 的冲击与流程事件](hdinsight-storm-develop-java-event-hub-topology.md) | 事件集线器 | Java |
| [使用电源 Bi 可视化数据从风暴拓扑][94d15238]                              | 双电源                                             | C#                         |
| [分析与风暴和 HBase 中 HDInsight 传感器数据][ab894747]                                     | 事件集线器，HBase，Socket.IO，Web 仪表板          | C#，Java，JavaScript HTML |
| [处理事件集线器在 HDInsight 上使用风暴从车辆传感器数据][246ee964]                        | 事件集线器，DocumentDb，Azure 存储 Blob (WASB)    | C# Java                   |
| [提取、 转换和加载 (ETL) 从 Azure 事件集线器到 HBase，在 HDInsight 上使用风暴][b4b68194] | 事件集线器 HBase                                    | C#                         |
| [使用 HDInsight 上的风暴从 Azure 服务模板 C# 风暴拓扑结构项目][ce0c02a2]  | 事件集线器，DocumentDb，SQL 数据库，HBase，那么 SignalR | C# Java                   |
| [用于读取在 HDInsight 上使用风暴 Azure 事件集线器可扩展性基准测试][d6c540e3]           | 消息吞吐量，事件集线器，SQL 数据库         | C# Java                   |
| [将使用 HDInsight 上的风暴和 HBase 事件关联](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Python 中使用 HDInsight 上的风暴](hdinsight-storm-develop-python-topology.md) | 与 Java 和 Clojure 的 Python 组件基于风暴拓扑 | Python |

## <a name="next-steps"></a>下一步行动

* [在 HDInsight 上的 Apache 风暴开始][2b8c3488]

* [了解如何部署和管理与在 HDInsight 上的风暴冲击拓扑][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "了解如何 HDInsight 群集上创建有暴风雨，风暴仪表板用于部署的示例拓扑。"
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "了解如何部署和管理基于 web 的风暴仪表板和风暴 UI 或 HDInsight 工具使用 Visual Studio 的拓扑。"
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "了解如何使用 HDInsight 工具 Visual Studio 创建 C# 风暴拓扑。"
  [5797064f]: hdinsight-storm-develop-java-topology.md "了解如何在 Java 中，使用 Maven，通过创建一个基本的 wordcount 拓扑结构中创建风暴拓扑。"
  [94d15238]: hdinsight-storm-power-bi-topology.md "演示如何写入数据电源 BI C# 拓扑中，然后从数据创建的图表和仪表板。"
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "演示如何执行字数统计，在 C# 中实现的一个基本风暴拓扑。这也说明如何创建一个 C# 拓扑中的多个数据流。"
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "了解如何从读取和写入数据与风暴 Azure 事件集线器上 HDInsight。"
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "了解如何在 HDInsight 上使用 Apache 风暴处理从 Azure 事件集线器的传感器数据，直观地显示它使用 D3.js，并 （可选） 将其存储到 HBase。"
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "在 Twitter 上学习如何使用 Trident 创建风暴拓扑确定趋势 （基于 hashtags，） 的主题。"
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "了解如何使用一个风暴拓扑从 Azure 事件集线器，读取消息的数据引用从 Azure DocumentDB 阅读文档并将数据保存到 Azure 存储。"
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "几种拓扑结构来演示吞吐量从 Azure 事件集线器读取并存储到 SQL 数据库在 HDInsight 上使用 Apache 风暴时。"
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "了解如何从 Azure 事件集线器，聚合读取数据和转换数据，然后将其存储到 HBase HDInsight 上。"
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "此项目包含模板 spouts、 螺栓和拓扑与事件集线器、 DocumentDB 和 SQL 数据库等各种 Azure 服务进行交互。"
 
