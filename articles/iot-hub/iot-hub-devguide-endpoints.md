<properties
 pageTitle="开发人员指南-IoT 集线器终结点 |Microsoft Azure"
 description="Azure IoT 中心开发人员指南-IoT 中心端点引用信息"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="reference---iot-hub-endpoints"></a>引用-IoT 集线器终结点

## <a name="list-of-iot-hub-endpoints"></a>IoT 集线器终结点的列表

Azure IoT 集线器是公开各种参与者对其功能的多租户服务。 下图显示各个端点 IoT 中心公开。

![IoT 集线器终结点][img-endpoints]

以下是终结点的说明︰

* **资源提供程序**。 IoT 中心资源提供程序公开[Azure 资源管理器][lnk-arm]使 Azure 订阅所有者能够创建和删除 IoT 集线器和更新 IoT 集线器属性的接口。 IoT 集线器属性控制[中心级安全策略][lnk-accesscontrol]，而不是设备级的访问控制和云至设备和设备到云消息的功能选项。 IoT 中心资源提供程序还可以导出[设备标识][lnk-importexport]。
* **设备标识管理**。 每个 IoT 中枢公开 HTTP REST 端点管理设备标识一组 （创建、 检索、 更新和删除）。 [设备标识][lnk-device-identities]用于设备的身份验证和访问控制。
* **设备两路输出管理**。 每个 IoT 中枢公开一组用于查询和更新[设备 twins]的面向服务的 HTTP REST 端点的[lnk-twins]（更新标记和属性）。
* **作业管理**。 每个 IoT 中枢公开一组面向服务的 HTTP REST 端点，可以查询和管理[作业]的[lnk-jobs]。
* **设备端点**。 每个设备设备标识注册表设置，IoT 中心公开一套设备可用于发送和接收消息的终结点︰
    - *发送设备到云的消息*。 使用此端点[将设备与云消息发送]到[lnk-d2c]。
    - *接收云至设备信息*。 设备使用此端点接收目标的[云至设备信息][lnk-c2d]。
    - *启动文件上传*。 设备使用此终结点[将文件上载]到 IoT 集线器接收 Azure 存储 SA URI[lnk-upload]。
    - *检索和更新双子星的属性*。 设备使用此端点来访问其[设备 twin][lnk-twins]的属性。
    - *直接方法接收的请求*。 设备使用此终结点侦听[直接方法][lnk-methods]的请求。

    这些终结点公开使用[MQTT v3.1.1][lnk-mqtt]，HTTP 1.1 和[AMQP 1.0] [lnk-amqp]协议。 请注意，AMQP 也可通过[Websocket] [lnk-websockets]端口 443 上。
    
    Twins 的和方法的 endpoins 是适合于仅使用[MQTT v3.1.1][lnk-mqtt]。

* **服务终结点**。 每个 IoT 中枢公开一组您应用程序的后端可以使用与您的设备进行通信的终结点。 这些终结点将当前只公开使用[AMQP] [lnk-amqp]协议，除了公开通过 HTTP 1.1 的方法调用端点。
    - *接收设备到云消息*。 此端点是[Azure 事件集线器]与兼容[lnk-event-hubs]。 后端服务可以使用它来阅读所有[设备到云消息][lnk-d2c]由设备发送。
    - *云向设备发送和接收送达回执*。 这些终结点允许您将后端应用程序发送可靠[云至设备信息][lnk-c2d]，并接收相应的交货或到期确认。
    - *接收文件的通知*。 此消息的终结点使您可以在您的设备已成功上载文件时接收通知。 
    - *直接方法调用*。 此终结点允许后端服务调用[直接方法][lnk-methods]的设备上。

[IoT 集线器 Api 和 Sdk] [lnk-sdks]介绍了各种方法来访问这些终结点。

最后，请务必注意 IoT 集线器的所有终结点使用[TLS] [lnk-tls]未加密保护通道上曾经公开协议，并没有终结点。

## <a name="field-gateways"></a>域的网关

在 IoT 解决方案中，一个*域的网关*位于您的设备和 IoT 中心端点之间。 它是通常位于靠近您的设备。 您的设备通过使用设备支持的协议直接与域的网关进行通信。 域的网关连接到 IoT 集线器终结点使用 IoT 集线器支持的协议。 高度专门的硬件或运行软件，实现了网关所针对的端到端方案的低功耗计算机可以是域的网关。

您可以使用[Azure IoT 网关 SDK] [lnk-gateway-sdk]实现的域的网关。 此 SDK 提供了特定功能，如多路传输到 IoT 集线器从同一个连接到多个设备通信的能力。

## <a name="next-steps"></a>下一步行动

本 IoT 中心开发人员指南中的其他参考主题包括︰

- [对于 twins、 方法和作业的查询语言][lnk-devguide-query]
- [配额和限制][lnk-devguide-quotas]
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md