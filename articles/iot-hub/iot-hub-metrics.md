<properties
 pageTitle="IoT 中心诊断指标"
 description="Azure IoT 中心指标，从而使用户能够评估其资源的总体运行状况事件的概述"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>诊断标准简介

诊断指标提供在 Azure 订购有关 Azure 的资源的状态更好的数据。 指标可用于评估服务和连接到它的设备的整体健康状况。 面向用户的统计很重要，因为它可以帮助您看到这怎么回事 IoT 中心和帮助根源问题而无需联系 Azure 支持。

您可以启用从 Azure 门户的诊断指标。

## <a name="how-to-enable-diagnostic-metrics"></a>如何启用诊断指标

1. 创建 IoT 集线器。 您可以找到如何在[开始]创建 IoT 集线器说明[lnk-get-started]指南。

2. 打开 IoT 集线器刀片。 然后，单击**诊断**。

    ![][1]

3. 通过将状态设置为**在**Azure 存储帐户存储诊断数据来配置您的诊断程序。 检查**标准**，，然后按**保存**。 请注意，必须事先创建 Azure 存储帐户和存储您也会单独收取。 您还可以选择对事件集线器终结点发送您的诊断数据。

    ![][2]

4. 您已经设置了诊断程序后，回到**概述**IoT 集线器刀片。 衡量标准的信息填充刀片式服务器的**监视**部分中。 单击图表打开指标窗格位置可以查看 IoT 中心测量数据信息的汇总和编辑度量值显示在图表中所选内容。 您还可以配置警报基于度量的值。

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>指标和如何使用它们

IoT 中心提供多项指标，以便为您提供网络集线器和总与它相连的设备的运行状况概览。 您可以组合来自多个测量数据来绘制状态 IoT 中心的更大的画面信息。 下表描述每个 IoT 中枢跟踪，测量数据以及每个跃点计数与 IoT 中心的总体状态。

| 跃点计数 | 指标说明 | 指标的用途是什么 |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | 在所有设备上发送的消息数 | 概述在消息发送的数据 |
| d2c.telemetry.ingress.success | 插入集线器所有成功的消息数 | 成功的消息插入集线器的入口的概述 |
| c2d.commands.egress.complete.success | 所有命令消息由接收设备完成所有设备的数 | 放弃和拒绝的指标，以及概述了总体的 C2D 命令成功率 |
| c2d.commands.egress.abandon.success | 成功接收设备通过放弃所有设备中的所有消息数 | 如果消息被得到比预期更经常放弃将突出显示可能出现的问题 |
| c2d.commands.egress.reject.success | 已成功拒绝所有设备的接收设备的所有消息数 | 如果将得到比预期更经常拒绝邮件突出显示可能出现的问题 |
| devices.totalDevices | 平均值、 最小和最大值为注册到 IoT 集线器的设备数 | 注册到集线器的设备数 |
| devices.connectedDevices.allProtocol | 平均值、 最小和最大的并发连接的设备数 | 连接到集线器的设备数概览 |

## <a name="next-steps"></a>下一步行动

现在，您已看到诊断指标的概览，点击此链接以了解更多关于管理 Azure IoT 中心︰

- [操作监视][lnk-monitor]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
