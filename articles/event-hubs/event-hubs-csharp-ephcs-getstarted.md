<properties
    pageTitle="开始使用事件集线器上，形成 C# |Microsoft Azure"
    description="按照与 C# 使用 Azure 事件集线器和使用 EventProcessorHost 开始本教程。"
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>开始使用事件集线器

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>介绍

事件集线器是一种服务，处理大量的连接的设备和应用程序的事件数据 （遥测）。 数据收集到事件集线器后，可以使用存储数据存储群集或转换使用实时分析功能提供程序。 这种大规模的事件收集和处理能力是包括互联网的东西 (IoT) 的现代应用程序体系结构的关键组成部分。

本教程展示如何使用 Azure 的传统门户网站创建事件中心。 它还演示您如何收集到 C# 中编写的控制台应用程序使用事件中心的消息，以及如何在并行使用 C#[事件处理器主机][]库中检索它们。

若要完成本教程，您需要︰

+ [Microsoft Visual Studio](http://visualstudio.com)

+ 活动的 Azure 帐户。 如果您没有，您可以在几分钟创建免费的帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/free/)。

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>运行应用程序

现在，您就可以运行应用程序了。

1. 从在 Visual Studio 中，打开您在前面创建的**接收方**项目。

2. 用鼠标右键单击**接收方**解决方案中，然后单击**添加**，然后单击**现有项目**。
 
3. 找到现有的 Sender.csproj 文件，然后双击它以将其添加到解决方案中。
 
4. 再次，用鼠标右键单击**接收方**解决方案，然后单击**属性**。 **接收器**属性页将显示。

5. 单击**启动项目**，然后单击**多个启动项目**按钮。 设置为**启动**的**发送者**和**接收者**项目**操作**框。

    ![][19]

6. 单击**项目依赖项**。 在**项目**框中，单击**发件人**。 在**依赖于**框中，确保选中**接收器**。

    ![][20]

7. 单击**确定**以关闭**属性**对话框。

1.  按 f5 键以运行**接收器**项目从在 Visual Studio，然后等待其启动的所有分区接收器。

    ![][21]

2.  **发件人**项目将自动运行。 按**enter 键**在控制台窗口中，并查看接收窗口中显示的事件。

    ![][22]

按**Ctrl + C**在**发件人**窗口结束发送方应用程序，然后按**Enter**接收窗口中关闭该应用程序。

## <a name="next-steps"></a>下一步行动

既然已经建立了有效的应用程序创建事件中心以及发送和接收数据，您可以将移动到以下情况︰

- 完整的[示例应用程序，它使用事件集线器][]。
- 该[事件处理事件集线器扩展][]示例。
- [事件集线器概述][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[事件处理器主机]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[扩展事件处理事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
