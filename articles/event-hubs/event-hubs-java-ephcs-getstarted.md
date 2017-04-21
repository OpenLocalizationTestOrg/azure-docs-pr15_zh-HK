<properties
    pageTitle="在 Java 中的事件集线器入门 |Microsoft Azure"
    description="按照本教程中若要开始使用 Azure 事件集线器;发送使用 Java 和 C# 使用 EventProcessorHost 在接收这些事件。"
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>开始使用事件集线器

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>介绍

事件集线器是可以摄入量数以百万计的具有高可扩展性的摄取系统每秒钟的事件，使应用程序可以处理和分析大量的数据产生的对连接的设备和应用程序。 一旦收集到事件集线器，可以转换并使用任何实时的分析提供者或存储群集存储数据。

有关详细信息，请参阅[事件集线器概述][]。

本教程介绍如何使用控制台应用程序在 Java 中，一个事件集线器到接收的邮件以及在并行使用 C#[事件处理器主机][]库中检索它们。

若要完成本教程，您需要︰

+ Java 开发环境。 对于本教程，我们将假定[Eclipse](https://www.eclipse.org/)。

+ [Microsoft Visual Studio](http://visualstudio.com)

+ 活动的 Azure 帐户。 <br/>如果您没有帐户，您可以在几分钟创建免费的帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 免费试用版</a>。

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>运行应用程序

现在，您就可以运行应用程序了。

1.  **接收器**项目运行 Visual Studio，然后等待其启动的所有分区的接收器。

    ![][21]

2.  运行该**发件人**项目。

    ![][22]

## <a name="next-steps"></a>下一步行动

既然已经建立了有效的应用程序创建事件中心以及发送和接收数据，您可以将移动到以下情况︰

- 完整的[示例应用程序，它使用事件集线器][]。
- 该[事件处理事件集线器扩展][]示例。

有关详细信息，请参见[Java 开发人员中心](/develop/java/)。

<!-- Images. -->
[21]: ./media/event-hubs-java-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-java-ephcs-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[事件处理器主机]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[扩展事件处理事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 