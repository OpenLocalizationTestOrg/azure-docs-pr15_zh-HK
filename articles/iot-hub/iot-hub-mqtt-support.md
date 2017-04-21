<properties
 pageTitle="IoT 集线器 MQTT 支持 |Microsoft Azure"
 description="MQTT 的描述 IoT 中心级支持"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT 集线器 MQTT 支持

IoT 集线器可以使与使用[MQTT v3.1.1] IoT 集线器设备端点通信设备[lnk-mqtt-org]端口 8883 或 MQTT v3.1.1 通过端口 443 上的 WebSocket 协议的协议。 IoT 中心要求所有设备通信使用 TLS/SSL 安全保护 （因此，IoT 集线器不支持不安全的连接通过端口 1883年）。

## <a name="connecting-to-iot-hub"></a>连接到集线器 IoT

设备可以使用 MQTT 协议连接到通过使用[Microsoft Azure IoT Sdk]中的库 IoT 中心[lnk-device-sdks]或使用 MQTT 协议直接。

## <a name="using-the-device-client-sdks"></a>使用设备客户端 Sdk

[设备客户端 Sdk] [lnk-device-sdks]支持的 MQTT 协议是可用于 Java，Node.js，C、 C# 和 Python。 设备客户端 Sdk 使用标准 IoT 集线器连接字符串建立到 IoT 集线器连接。 若要使用 MQTT 协议，必须到**MQTT**设置的客户端协议参数。 默认情况下，设备客户端 Sdk 连接到**CleanSession**与 IoT 中心标志设置为**0**和**QoS 1**用于 IoT 集线器的消息交换。

当设备连接到 IoT 集线器时，设备客户端 Sdk 提供了启用从 IoT 集线器接收消息并将消息发送到该设备的方法。

下表包含每个受支持的语言的代码示例的链接，并指定要用来建立到使用 MQTT 协议 IoT 集线器的连接参数。

| 语言                   | 协议参数        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | azure iot 设备 mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>从 AMQP 的设备应用程序迁移到 MQTT
如果您使用的[客户端设备的 Sdk][lnk-device-sdks]，从使用到 MQTT AMQP 模型切换需要更改客户端初始化中的协议参数，如上面所述。

当这样做，请务必检查以下各项︰

* AMQP 返回错误的许多条件，而 MQTT 将终止此连接。 因此您异常处理逻辑可能需要进行一些更改。
* MQTT 不支持*拒绝*操作时接收[C2D 消息][lnk-messaging]。 如果您的后端需要接收响应的设备应用程序时，请考虑使用[直接方法][lnk-methods]。

## <a name="using-the-mqtt-protocol-directly"></a>直接使用 MQTT 协议

如果某个设备不能使用客户端设备的 Sdk，它仍然可以连接到公用设备端点使用 MQTT 协议。 在**连接**数据包设备应使用以下值︰

- 对于**客户机 Id**字段中，请使用**deviceId**。 
- 对于**用户名**字段中，使用`{iothubhostname}/{device_id}`，其中 {iothubhostname} 是 IoT 集线器的完整 CName。

    例如，如果 IoT 中心的名称是**contoso.azure devices.net** ，并且您的设备的名称是**MyDevice01**，完整的**用户名**字段应包含`contoso.azure-devices.net/MyDevice01`。

- 对于**密码**字段中，请使用 SAS 令牌。 SAS 令牌格式是相同的 HTTP 和 AMQP 协议︰<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    有关如何生成 SAS 令牌的详细信息，请参阅[使用 IoT 中心安全令牌]的设备部分[lnk-sas-tokens]。
    
    在测试时，您还可以使用[设备浏览器][lnk-device-explorer]工具来快速生成一个 SAS 标记，您可以复制并粘贴到您自己的代码︰
    
    1. 转到设备资源管理器中的**管理**选项卡。
    2. 单击**标记 SAS** （右上角）。
    3. 在**SASTokenForm**，您的设备中**DeviceID**下拉列表的选择上。 设置您的**TTL**。
    4. 单击**生成**以创建您的令牌。
    
    生成的 SAS 标记具有这种结构︰   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`。

    若要使用**密码**字段使用 MQTT 连接此标记的部分︰   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`。

对于 MQTT 连接和断开连接的数据包、 IoT 中心问题可以帮助您解决连接问题的其他信息的**操作监视**信道上的事件。

### <a name="sending-messages-to-iot-hub"></a>将消息发送到 IoT 集线器

连接成功后，一个设备可以将消息发送到 IoT 中心使用`devices/{device_id}/messages/events/`或`devices/{device_id}/messages/events/{property_bag}`作为**主题名称**。 `{property_bag}`元素使设备中的 url 编码格式发送消息的其他属性。 例如︰

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] 这`{property_bag}`元素使用相同的编码与 HTTP 协议中的查询字符串。

设备客户端应用程序还可以使用`devices/{device_id}/messages/events/{property_bag}`作为**将主题名称**来定义*将邮件*作为遥测邮件转发。

IoT 集线器不支持 QoS 2 的消息。 如果设备客户端发布一条消息， **QoS 2**，IoT 集线器关闭网络连接。
IoT 中枢不能持续保留消息。 如果设备发送消息时使用的**保留**标志设置为 1，添加 IoT 中心**x 选择保留**到该消息的应用程序属性。 在这种情况下，而不是保持保留消息，IoT 集线器将它传递到后端应用程序。

### <a name="receiving-messages"></a>接收消息

若要从 IoT 集线器接收邮件，设备应该订阅使用`devices/{device_id}/messages/devicebound/#`作为**主题筛选器**。 多级通配符**#**主题筛选器仅用于允许设备接收在主题名中的其他属性。 IoT 集线器不允许使用**#**或**？** 通配符筛选的子主题。 IoT 中心不是一般用途的 pub sub 消息代理，因为它只支持有案可稽的主题名称以及主题筛选器。

请注意，该设备将无法接收任何邮件 IoT 集线器之前它已经成功地订阅了其设备特定的终结点，, 由`devices/{device_id}/messages/devicebound/#`主题筛选器。 在建立成功的订阅之后，设备将开始接收仅云至设备预订的时间后向其发送的邮件。 如果设备连接**CleanSession**标记设置为**0**时，将在不同会话之间保留预订。 在这种情况下，用**CleanSession 0**连接该设备下一次将收到完成已断开连接时发送给它的消息。 如果该设备使用**CleanSession**标记设置为**1** ，但它不会收到任何消息从 IoT 集线器直到它订阅其设备端点。

IoT 中心提供了邮件的**主题名称** `devices/{device_id}/messages/devicebound/`，或`devices/{device_id}/messages/devicebound/{property_bag}`如果没有任何消息属性。 `{property_bag}`包含消息属性的 url 编码键/值对。 只有应用程序属性和用户可设置系统属性 （如**邮件 Id**或**都会**） 包含在属性包中。 系统属性名称具有前缀**$**，应用程序属性使用原始属性名称没有前缀。

当设备客户端订阅的主题与**QoS 2**时，IoT 中心授予**SUBACK**数据包中的最大 QoS 级别 1。 在此之后，IoT 中心将把消息传送到使用 QoS 1 的设备。

### <a name="additional-considerations"></a>其他注意事项

作为最后一个考虑因素，如果您需要自定义的 MQTT 协议行为上的云端，您应该查看[Azure IoT 协议网关][ lnk-azure-protocol-gateway] ，它使您能够部署直接与 IoT 集线器接口的高性能的自定义协议网关。 Azure IoT 协议网关使您可以自定义以适应 brownfield MQTT 部署的设备协议或其他自定义协议。 但是，这种方法确实需要您运行和操作的自定义协议网关。

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅[注释 MQTT 支持][ lnk-mqtt-devguide] Azure IoT 中心开发人员指南 》 中。

若要了解有关 MQTT 协议的详细信息，请参阅[MQTT 文档][lnk-mqtt-docs]。

若要了解有关规划 IoT 中心部署的详细信息，请参阅︰

- [Azure 疏 IoT 设备目录][lnk-devices]
- [支持更多协议][lnk-protocols]
- [与事件集线器][lnk-compare]
- [缩放、 高可用性，以及灾难恢复][lnk-scaling]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
