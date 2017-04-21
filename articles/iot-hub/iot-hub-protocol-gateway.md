<properties
   pageTitle="Azure IoT 协议网关 |Microsoft Azure"
   description="描述如何使用 Azure IoT 协议网关来扩展的功能和协议支持的 Azure IoT 集线器。"
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>支持更多协议 IoT 集线器

Azure IoT 中心本身支持通过 MQTT AMQP，以及 HTTP 协议的通讯。 在某些情况下，域的网关或设备可能无法使用其中一种标准协议，并需要协议适配。 在这种情况下，您可以使用自定义网关。 自定义网关可以通过桥接的通信量和 IoT 中心启用 IoT 集线器终结点的协议适配。 您可以使用自定义网关作为[Azure IoT 协议网关](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)启用协议适配 IoT 集线器。

## <a name="azure-iot-protocol-gateway"></a>Azure IoT 协议网关

Azure IoT 协议网关是专为高比例，双向 IoT 集线器的设备通信的协议适配的框架。 协议网关是通过特定协议接受设备连接传递组件。 它可以弥补通过 AMQP 1.0 到 IoT 中心的通信。 Azure IoT 协议网关是可用来提供灵活性添加各种协议和协议版本支持开放源代码软件项目。

您可以通过使用 Azure 云服务工作者角色高度可伸缩的方式部署 Azure 中的协议网关。 此外，协议网关可以部署在内部环境，如域的网关。

Azure IoT 协议网关包括 MQTT 协议适配器，您可以根据需要自定义 MQTT 协议行为。 因为 IoT 集线器 MQTT v3.1.1 协议提供内置的支持，仅应考虑使用 MQTT 协议适配器，如果有需要的自定义协议或其他功能的特定要求。

MQTT 适配器还演示用于构建其他协议的协议适配器的编程模型。 此外，Azure IoT 协议网关编程模型使您可以插入特殊的处理，如自定义身份验证、 消息转换、 压缩/解压缩或加密/解密 IoT 集线器设备之间的通信的自定义组件。

实现的灵活性，协议网关和 MQTT 实施开放源码软件项目中提供。 这使您可以根据需要自定义实现。

## <a name="next-steps"></a>下一步行动

Azure IoT 协议网关，以及如何使用，并将其部署为 IoT 解决方案的一部分有关的详细信息，请参阅︰

* [Azure IoT 协议网关存储库在 GitHub 上](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT 协议网关开发人员指南 》](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

若要了解有关规划 IoT 中心部署的详细信息，请参阅︰

- [与事件集线器][lnk-compare]
- [缩放、 高可用性和灾难恢复][lnk-scaling]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
