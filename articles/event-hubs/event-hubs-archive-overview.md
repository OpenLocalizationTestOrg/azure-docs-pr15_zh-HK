<properties
    pageTitle="Azure 事件集线器归档文件 |Microsoft Azure"
    description="Azure 事件集线器存档功能的概述。"
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Azure 事件集线器存档

Azure 事件集线器存档使您能够自动提供流数据到您所选的 Blob 存储帐户您事件集线器中具有更大的灵活性，以指定的时间或您选择的间隔的大小。 设置存档是快速、 有无管理费来运行它，并带您事件集线器[吞吐量单位](event-hubs-overview.md#capacity-and-security)自动缩放。 事件集线器存档是流式数据加载到 Azure 的最简单方法，使您可以专注于数据处理，而不是数据捕获。

Azure 事件集线器存档使您能够处理在相同流的实时和基于批次的管线。 这使您可以构建解决方案，可以随着您的需求随着时间的推移。 无论构建批处理系统目前与未来的实时处理，提防还是您想要添加到现有实时解决方案有效的冷路径，事件集线器存档使流式处理数据更容易处理。

## <a name="how-event-hubs-archive-works"></a>事件集线器存档的工作原理

事件集线器是遥测入口，类似于分布式日志的时间保留持久缓冲区。 要缩放事件集线器中的键是[分区使用者模型](event-hubs-overview.md#partition-key)。 每个分区是一个独立的数据段和独立使用。 随着时间的推移这些数据变得陈旧，基于可配置的保留期间。 因此，给定的事件中心从未获取""太满。

事件的集线器存档使您可以指定您自己的 Azure Blob 存储帐户和用于存储存档的数据的容器。 此帐户可以是在同一个地区作为事件中心或在另一个区域中，将添加到事件集线器存档功能的灵活性。

存档的数据写入的[Apache Avro][]格式;小巧、 快速、 二进制格式使用内联架构提供丰富的数据结构。 在 Hadoop 的生态系统，也可以通过流分析和 Azure 数据工厂，广泛使用这种格式。 本文后面提供的有关使用 Avro 的详细信息。

### <a name="archive-windowing"></a>归档文件窗口

事件集线器存档允许您设置来控制归档的一个窗口。 此窗口是最小大小，与"第一个 wins 策略、"即是遇到的第一个触发器将导致存档操作时配置。 如果您有一个十五分钟/100 MB 存档窗口并发送 1 MB/s，大小窗口将触发之前的时间长度。 每个分区单独存档，并将已完成的块斑点时的档案，名为当遇到存档时间间隔的时间。 命名约定是，如下所示︰

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>缩放到吞吐量单元

事件集线器流量受[吞吐量单位](event-hubs-overview.md#capacity-and-security)。 一个吞吐量单位允许 1 MB/s 或 1000年每秒事件的入口和两次的出口量。 标准事件集线器可以获配 1-20 吞吐量单位，并通过配额增加[支持请求][]可以购买到更多。 使用率超过购买的吞吐量单位被遏制。 绕过吞吐量单位出口配额和保存如流分析或触发其他处理读者您外出，事件集线器归档文件直接从内部集线器事件存储，复制数据。

配置完成后，事件集线器档案会自动运行，只要您发送您的第一个事件。 在任何时候继续运行。 若要轻松地为您下游处理，以了解该进程正在运行，事件集线器写入空文件时没有数据。 这提供了一个可预测的节奏和可以送批处理器的标记。

## <a name="setting-up-event-hubs-archive"></a>设置事件集线器存档

可以在事件中心通过门户或 Azure 资源管理器的创建时配置事件集线器存档。 您只需通过单击**打开**按钮启用存档。 通过单击刀片式服务器的**容器**部分中配置的存储帐户和容器。 由于事件集线器存档存储与使用服务的服务身份验证，您不需要指定存储连接字符串。 资源选取器将自动选择您的存储帐户的资源的 URI。 如果使用 Azure 资源管理器，您必须作为字符串显式提供此 URI。

默认时间长度为 5 分钟。 最小值为 1，最大的 15 个。 **大小**窗口具有一系列的 10-500 MB。

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>将归档文件添加到现有的事件中心

归档文件可以配置事件集线器命名空间中的现有事件集线器上。 功能不可用在旧的消息传递或混合类型命名空间。 若要启用现有的事件集线器上的存档或更改您的存档设置，请单击您的名称空间来加载**基础**刀片式服务器，然后单击您要为其启用或更改存档设置事件集线器上。 最后，单击打开刀片式服务器的**属性**部分下面的图中所示。

![][2]

您还可以通过 Azure 资源管理器模板配置事件集线器存档。 有关更多信息，请参见[这篇文章](event-hubs-resource-manager-namespace-event-hub-enable-archive.md)。

## <a name="exploring-the-archive-and-working-with-avro"></a>浏览存档和使用 Avro

配置完成后，事件集线器存档文件中创建 Azure 存储帐户并在配置的时间窗口中提供的容器。 任何工具如[Azure 存储资源管理器][]中，可以查看这些文件。 您可以下载本地以处理这些文件。

通过集线器存档事件生成的文件具有以下 Avro 架构︰

![][3]

可以轻松地浏览 Avro 文件是使用从 Apache [Avro 工具][]jar。 下载此 jar 之后, 可以通过运行下面的命令查看特定的 Avro 文件的架构︰

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

此命令可返回

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

您还可以使用 Avro 工具转换为 JSON 格式的文件，并执行其他处理。

要执行更高级的处理，请下载并安装您选择的平台 Avro。 在撰写本文时，有实现供 C、 c + +、 C\#，Java、 NodeJS、 Perl、 PHP、 Python 和拼音。

Apache Avro [Java][]和[Python][]有完整的快速入门指南。 您还可以阅读[入门事件集线器存档](event-hubs-archive-python.md)文章。

## <a name="how-event-hubs-archive-is-charged"></a>如何收取事件集线器存档

事件集线器存档是按小时付费为同样测量吞吐量单位。 费用与吞吐量单位购买的命名空间的数目成正比。 随着吞吐量单位进行增加和减少，事件集线器存档将增加和减少提供匹配性能。 米一前一后发生。 事件集线器存档的费用是每小时单位吞吐量，50%的折扣在预览期间免费提供 0.10 美元。

事件集线器存档真正是最简单的方法将数据读入 Azure。 使用 Azure 数据湖、 Azure 数据工厂和 Azure HDInsight，您可以执行批处理和您选择的其他分析在您需要的任何比例上使用熟悉的工具和平台。

## <a name="next-steps"></a>下一步行动

您可以了解有关事件集线器通过访问以下链接︰

- 完整的[示例应用程序，它使用事件集线器][]。
- 该[事件处理事件集线器扩展][]示例。
- [事件集线器概述][]

[Apache Avro]: http://avro.apache.org/
[支持请求]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure 存储资源管理器]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Avro 工具]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[扩展事件处理事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3