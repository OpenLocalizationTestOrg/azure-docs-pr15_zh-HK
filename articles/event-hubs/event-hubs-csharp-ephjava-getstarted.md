<properties
    pageTitle="开始使用事件集线器上，形成 C# |Microsoft Azure"
    description="按照本教程中若要开始使用 Azure 事件集线器;在 C# 发送事件和接收它们使用 Java 使用 EventProcessorHost。"
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>开始使用事件集线器

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>介绍

事件集线器是一种服务，处理大量的连接的设备和应用程序的事件数据 （遥测）。 数据收集到事件集线器后，可以使用存储数据存储群集或转换使用实时分析功能提供程序。 这种大规模的事件收集和处理能力是包括互联网的东西 (IoT) 的现代应用程序体系结构的关键组成部分。

本教程展示如何使用 Azure 的传统门户网站创建事件中心。 它还演示您如何收集到 C# 中编写的控制台应用程序使用事件中心的消息，以及如何在并行使用 Java 事件处理器主机库中检索它们。

若要完成本教程，您需要︰

+ [Microsoft Visual Studio](http://visualstudio.com)

+ 活动的 Azure 帐户。 <br/>如果您没有，您可以在几分钟创建免费的帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank")。

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>运行应用程序

现在，您就可以运行应用程序了。

1.  运行**接收器**项目。

    ![][21]

2.  运行该**发件人**项目。

    ![][22]

## <a name="next-steps"></a>下一步行动

既然已经建立了有效的应用程序创建事件中心以及发送和接收数据，您可以将移动到以下情况︰

- 完整的[示例应用程序，它使用事件集线器][]。
- 该[事件处理事件集线器扩展][]示例。
- [事件集线器概述][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[扩展事件处理事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
