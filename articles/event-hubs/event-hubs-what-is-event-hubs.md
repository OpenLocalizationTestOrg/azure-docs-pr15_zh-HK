<properties
    pageTitle="什么是 Azure 事件集线器？ |Microsoft Azure"
    description="概述和说明的 Azure 事件集线器"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="sethm"/>

# <a name="what-is-azure-event-hubs"></a>什么是 Azure 事件集线器？

Azure 事件集线器是一种高度可扩展的数据入口服务，可以摄取数以百万计，每秒钟的事件，以便您可以处理和分析大量数据连接的设备和应用程序所产生。 事件集线器充当事件管道，"前门"，一旦数据被收集在一个事件集线器，它可以转换并存储使用任何实时分析提供程序或批处理存储适配器。 这样的事件使用者可以访问他们自己的日程安排上的事件，事件集线器将生产从这些事件中，消耗的事件流的分离。 有关详细信息和技术详细信息，请参阅[事件集线器概述](event-hubs-overview.md)。

## <a name="event-hubs-capabilities"></a>事件集线器功能

事件集线器是事件处理提供了事件和遥测处理在大规模，具有低延迟和高可靠性的服务。 此服务是非常有用的︰

- 应用程序规范
- 用户体验或工作流处理
- 互联网内容 (IoT) 方案

一些其他关键事件集线器功能包括移动应用程序中的跟踪行为、 通信信息从 web 场中，在控制台游戏或从工业计算机或连接的车辆收集遥测中游戏事件捕获。

## <a name="next-steps"></a>下一步行动

有关事件集线器的详细信息，请参阅下列主题。

- [事件集线器概述](event-hubs-overview.md)
- [事件集线器编程指南](event-hubs-programming-guide.md)
- [事件集线器可用性和支持常见问题](event-hubs-availability-and-support-faq.md)
- 入门[教程事件集线器][]
- 完整的[示例应用程序，它使用事件集线器][]

[事件集线器教程]: event-hubs-csharp-ephcs-getstarted.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
