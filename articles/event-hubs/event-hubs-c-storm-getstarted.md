<properties
    pageTitle="开始使用 C 和 Apache 风暴事件集线器 |Microsoft Azure"
    description="按照本教程中若要开始使用 Azure 事件集线器;c 发送事件和接收它们 Apache 风暴群集中。"
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>开始使用事件集线器

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>介绍

事件集线器是可以摄入量数以百万计的具有高可扩展性的摄取系统每秒钟的事件，使应用程序可以处理和分析大量的数据产生的对连接的设备和应用程序。 一旦收集到事件集线器，可以转换并使用任何实时的分析提供者或存储群集存储数据。

有关详细信息，请参阅[事件集线器]。

在本教程中，您将学习如何到事件中心在 C 中使用一个控制台应用程序接收消息并在并行使用 Apache 风暴中检索它们。

若要完成本教程，您需要︰

+ C 开发环境。 对于本教程，我们将假定使用 Ubuntu 14.04 [Azure Linux 虚拟机](../virtual-machines/virtual-machines-linux-quick-create-cli.md)上的 gcc 栈。 将外部链接中提供的其他环境的说明。

+ Java 开发环境配置为运行[Maven](http://maven.apache.org/)。 对于本教程，我们将假定[Eclipse](https://www.eclipse.org/)。

+ 活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>运行应用程序

现在，您就可以运行应用程序了。

1.  日蚀式，从运行的**LogTopology**类，然后等待其启动的所有分区接收器。

2.  **发件人**信息，运行程序并查看接收窗口中显示的事件。

    ![][23]

> [AZURE.NOTE] 在本教程只风暴在本地模式下使用动向。 请参阅[HDInsight 风暴概述]和风暴部署和模式的详细信息的官方[Apache 风暴]文档。

## <a name="next-steps"></a>下一步行动

以下资源都可用于开发应用程序集成事件集线器和风暴。

- [分析与风暴和 HDInsight 传感器数据][]是完整方案教程，说明如何使用事件集线器、 占领和 HBase 摄取 Hadoop 群集中的传感器数据。
- [开发流数据处理应用程序与 SCP.NET 和 C# 在风暴和 HDInsight][]是如何编写使用 C# 的冲击管道的教程。

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[事件集线器概述]: event-hubs-overview.md

[Apache 的暴风雨]: https://storm.incubator.apache.org
[HDInsight 风暴概述]: ../hdinsight/hdinsight-storm-overview.md/
[分析与风暴和 HDInsight 传感器数据]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[开发流数据处理应用程序与 SCP.NET 和 C# 在风暴和 HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
