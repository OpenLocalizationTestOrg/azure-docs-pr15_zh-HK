<properties
 pageTitle="比较 Azure IoT 集线器到集线器 Azure 事件 |Microsoft Azure"
 description="突出显示功能差异和用例的 Azure IoT 集线器和 Azure 事件集线器服务的比较。"
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Azure IoT 集线器和 Azure 事件集线器的比较

主要用例 IoT 集线器之一是收集设备的遥测。 鉴于此，IoT 集线器通常相当于[Azure 事件集线器][]。 IoT 集线器，类似事件集线器是事件处理服务，使到在大规模，云的事件和遥测入口具有低延迟和高可靠性。

但是，服务有很多差异，详见下表。

| 区域 | IoT 集线器 | 事件集线器 |
| ---- | ------- | ---------- |
| 通信模式 | 使设备到云，云至设备的消息。 | 只启用事件入口 （通常认为设备到云方案）。 |
| 设备协议支持 | 支持 MQTT AMQP、 AMQP 通过 Websocket 和 HTTP。 此外，IoT 中心配合[Azure IoT 协议网关][lnk-azure-protocol-gateway]，一个可自定义的协议网关实现，以支持自定义的协议。 | 支持 AMQP，AMQP 通过 Websocket 和 HTTP。 |
| 安全 | 提供了每个设备的标识和可撤销的访问控制。 请参阅[IoT 中心开发指南 》 的安全部分]。 | 提供事件集线器级[共享的访问策略][Event Hubs - security]，通过[出版商的策略]支持有限吊销[Event Hubs publisher policies]。 IoT 解决方案通常需要实现自定义解决方案以支持每个设备的凭据和反欺骗的措施。 |
| 操作监视 | 使 IoT 解决方案为订阅一套丰富的设备标识管理和连接事件，如单个设备身份验证错误，调节，和格式不正确的异常。 这些事件使您可以快速识别单个设备级别的连接问题。 | 公开只有聚合指标。 |
| 小数位数 | 支持数以百万计的同时连接的设备进行优化。 | 可以支持更有限的数量的同时连接，最多 5000 个 AMQP 连接，根据[Azure 服务总线配额][]。 另一方面，事件集线器可以指定发送每条消息的分区。 |
| 设备的 Sdk | 提供[设备的 Sdk] [Azure IoT Hub SDKs]的大量不同的平台和语言。 | 支持在.NET 和 c。此外提供了 AMQP 和 HTTP 发送接口。 |
| 文件上载 | 使 IoT 解决方案要上载文件从设备到云。 包含工作流集成和操作监视的调试支持类别的文件通知终结点。 | 使用声明检查图案手动请求从设备的文件和设备提供交易记录的存储密钥。 |

总之，即使唯一的用例是遥测设备到云入口 IoT 中心提供专门服务 IoT 设备的连接。 它将继续展开 IoT 特定的功能与这些方案的价值主张。 事件集线器用于事件入口处巨大比例，这两间数据中心和数据中心内的方案的上下文中。

不经常使用 IoT 中心和在同一解决方案 — 其中 IoT 中心处理设备到云的通信，以及事件集线器到实时处理引擎处理事件以后阶段入口事件集线器。

## <a name="next-steps"></a>下一步行动

若要了解有关规划 IoT 中心部署的详细信息，请参阅[缩放比例、 高可用性和灾难恢复][lnk-scaling]。

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]

[Azure 事件集线器]: ../event-hubs/event-hubs-what-is-event-hubs.md
[IoT 中心开发指南 》 的安全性部分]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure 服务总线配额]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
