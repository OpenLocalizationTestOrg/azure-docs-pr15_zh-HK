<properties
    pageTitle="开始使用 C 和 C# 中的事件集线器 |Microsoft Azure"
    description="按照本教程中若要开始使用 Azure 事件集线器;在 C 发送事件和接收 C# 使用 EventProcessorHost 中的折边。"
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>开始使用事件集线器

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>介绍

事件集线器是可以摄入量数以百万计的具有高可扩展性的摄取系统每秒钟的事件，使应用程序可以处理和分析大量的数据产生的对连接的设备和应用程序。 一旦收集到事件集线器，可以转换并使用任何实时的分析提供者或存储群集存储数据。

有关详细信息，请参阅[事件集线器概述][]。

在本教程中，您将学习如何到事件中心在 C 中使用一个控制台应用程序接收消息并在并行使用 C#[事件处理器主机][]库中检索它们。

若要完成本教程，您需要︰

+ C 开发环境。 对于本教程，我们将假定使用 Ubuntu 14.04 [Azure Linux 虚拟机](../virtual-machines/virtual-machines-linux-quick-create-cli.md)上的 gcc 栈。 将外部链接中提供的其他环境的说明。

+ 对于 Windows 的 Microsoft Visual Studio 速成版

+ 活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>运行应用程序

现在，您就可以运行应用程序了。

1.  运行**接收器**项目从在 Visual Studio，然后等待其启动的所有分区接收器。

    ![][21]

2.  **发件人**信息，运行程序并查看接收窗口中显示的事件。

    ![][24]

## <a name="next-steps"></a>下一步行动

既然已经建立了有效的应用程序创建事件中心以及发送和接收数据，您可以将移动到以下情况︰

- 完整的[示例应用程序，它使用事件集线器][]。
- 该[事件处理事件集线器扩展][]示例。
- [事件集线器概述][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[事件处理器主机]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[扩展事件处理事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
