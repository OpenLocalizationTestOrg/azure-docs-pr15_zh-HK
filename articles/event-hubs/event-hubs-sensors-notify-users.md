<properties 
   pageTitle="通知用户接收传感器或其他系统的数据 |Microsoft Azure"
   description="描述如何使用事件集线器通知传感器数据的用户。"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>通知用户接收传感器或其他系统的数据

假设您有一个应用程序监视实时数据或生成报告的计划。 如果您看一下这些实时图表或报告显示在其的网站，可能会看到一些不需要采取措施。 如果您需要为预警应对这些情况，而不是记住查看网站上的信赖？ 假设在温室中有放大光，需要立即知道是否指示灯熄灭。 做到这一点的一种方法是使用光传感器在温室中，排列要发送一封电子邮件，如果指示灯不亮。

![][1]

在另一种方案，假设运行宠物加盟设施供应的低库存水平必须提醒您。 例如，您可能排列要发送文本消息从 ERP 系统的狗食品仓库库存降到严重程度。 

![][2]

问题是如何在满足某些条件时，避开为签出静态报表时不获取关键信息。 如果您正在使用[Azure 事件中心][]或[Azure IoT 集线器][]接收来自[动态 AX][]的企业应用程序或设备的数据，您有几种方法来处理这些。 您可以在网站上查看它们、 对其进行分析、 可以存储它们，和可用于触发命令去做某事。 若要执行此操作，可以使用[Azure 网站][]、 [SQL Azure][]、 [HDInsight][]、 [Cortana 智能套件][]、 [IoT 套件][]、[逻辑的应用程序][]或[Azure 通知集线器][]等功能强大的工具。 但是有时您是希望将该数据发送给其他人用最少的系统开销。 若要向您展示如何去做的只是少量的代码，我们提供了一个新示例， [AppToNotifyUsers][]。 包含的选项为 (SMTP) 的电子邮件、 SMS 和电话。

## <a name="application-structure"></a>应用程序结构

在 C# 中，编写应用程序并在示例自述文件包含要修改、 构建和发布应用程序所需的所有信息。 以下各节提供了有关应用程序的高级概述。

我们开始有关键事件被推到 Azure 事件集线器或 IoT 中心的设想。 任何中心将进行操作，只要您有权访问它，并且知道连接字符串。

如果没有事件集线器或 IoT 中心，可以方便地设置测试平台上使用 Arduino 盾牌和 Raspberry Pi，[连接点](https://github.com/Azure/connectthedots)项目中的说明。 Arduino 防护板上的光传感器发送到[Azure 事件中心][](**ehdevices**)，通过 Pi 光线水平和[Azure 流分析](https://azure.microsoft.com/services/stream-analytics/)作业将推送通知到第二个事件中心 (**ehalerts**) 如果接收到的光线水平低于某一级别。

**AppToNotify**在启动时，它会读取一个配置文件 (App.config) 来获取接收通知事件中心的 URL 和凭证。 它然后生成连续监视，事件中心进入 – 只要有任何消息可以访问事件集线器或 IoT 中心和有效凭据的 URL，此事件集线器读卡器代码将连续读取即将推出的一个进程。 在启动期间，应用程序也读取 URL 和您想要使用，消息传递服务 （电子邮件、 SMS，电话） 和名称/地址的发件人和收件人列表中的凭据。

一旦事件中心监视器检测到一条消息，就会引发一个发送邮件使用的配置文件中指定的方法的过程。 请注意，它会发送它检测到的每条消息。 如果设置显示器点到事件集线器接收每秒 10 封邮件，发件人将十个电子邮件每秒十个 SMS 消息 / 秒，每秒十个电话每秒钟--10 个消息发送。 因此，请确保您监视只接收需要发送出去，不接收来自您的传感器或应用程序的所有原始数据事件中心的警报事件中心。

## <a name="applicability"></a>适用性

此示例中的代码只显示如何监视事件集线器，如何在这事件，您想要将此功能添加到您的应用程序调用外部消息传递服务。 请注意，此解决方案是 DIY，专注于开发人员的示例。 它不能解决企业需求如冗余，故障转移，重新启动后出现故障，等等。更多全面和生产解决方案，请参阅下列资源︰

- 使用连接器或使用[Azure 逻辑应用程序](../app-service-logic/app-service-logic-connectors-list.md)服务的推式通知。
- 使用[Azure 通知集线器](https://msdn.microsoft.com/library/azure/jj927170.aspx)，描述博客[广播到数以百万计的移动设备使用 Azure 通知集线器的推式通知](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs)。 

## <a name="next-steps"></a>下一步行动

它很容易创建一个简单的通知服务将电子邮件或文本消息发送给收件人，或调用它们，中继数据接收的事件集线器或 IoT 集线器。 若要部署解决方案通知取决于这些集线器接收到的数据的用户，请访问[AppToNotifyUsers][]。

有关这些集线器的详细信息，请参阅下列文章︰

- [Azure 事件集线器]
- [Azure IoT 集线器]
- 入门[教程事件集线器]。
- 完整的[示例应用程序，它使用事件集线器]。

若要部署解决方案，以通知用户基于这些集线器接收到的数据，请访问︰

- [AppToNotifyUsers][]

[事件集线器教程]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT 集线器]: https://azure.microsoft.com/services/iot-hub/
[Azure 事件集线器]: https://azure.microsoft.com/services/event-hubs/
[Azure 事件中心]: https://azure.microsoft.com/services/event-hubs/
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[动态 AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure 网站]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana 智能套件]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT 套件]: https://azure.microsoft.com/solutions/iot-suite/
[逻辑应用程序]: https://azure.microsoft.com/services/app-service/logic/
[Azure 通知集线器]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png