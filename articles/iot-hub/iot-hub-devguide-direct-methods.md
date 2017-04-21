<properties
 pageTitle="开发人员指南 》 的直接方法 |Microsoft Azure"
 description="Azure IoT 中心开发指南的使用直接方法调用代码在您的设备上"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>直接在上调用方法的设备 （预览）

## <a name="overview"></a>概述

IoT 中心为您提供了设备从云上的方法调用的能力。 方法表示与类似于 HTTP 调用的设备交互的请求-回复，无论成功或失败 （紧跟用户指定的超时） 以使用户知道呼叫的状态。 这可用于不同取决于是否能够作出响应，例如 SMS 唤醒发送到设备，如果设备为离线 (SMS 更高成本的方法调用比) 设备的直接操作过程的情况。

您可以看作方法直接对设备的远程过程调用。 可能从云调用的设备上已经实现的方法。 如果云试图对设备没有定义该方法的调用一个方法，该方法调用将失败。

每个设备的方法的目标是一台设备。 [作业][lnk-devguide-jobs]提供一种方法来调用方法在多个设备，并断开连接的设备的方法调用进行排队。

与**服务连接**IoT 集线器上的权限的任何人都可能调用设备上的方法。

### <a name="when-to-use"></a>何时使用

设备的方法是类似于[云至设备信息][ lnk-devguide-messages] ，同时启用云后端将信息传递到设备上，但它们的基本方法不同。 从概念上讲，方法是同步并不持久，而云至设备信息是 48 小时耐久性与异步。

方法按照请求-响应模式，并不是持久的。 在被命令的设备时，缺乏持久性提供两个直接的好处︰

- **方法执行的即时反馈**意味着没有必要来管理请求和应答之间的相关性。
- **更高的吞吐量**意味着因为 IoT 中心没有提供任何持续性可以更快地执行操作。 IoT 中心允许更多单位比云至设备信息的方法调用。

云至设备信息不一定命令到设备，但相反表示云服务传递信息的一些，以使它能够在其方便的时候，拿起设备和设备可能会或可能不响应。 云至设备信息具有更长的时间超时时间 （最多 48 小时） 方法时过期快得多。

使用设备和作业计划在设备上的一个命令调用上的直接命令调用设备的方法。

## <a name="method-lifecycle"></a>方法生命周期

方法在设备上实现，并可能需要正确实例化的方法有效负载中的零个或多个输入。 通过面向服务的 URI 的直接方法调用 (`{iot hub}/twins/{device id}/methods/`)。 一个设备接收到特定于设备的 MQTT 主题的直接方法 (`$iothub/methods/POST/{method name}/`)。 我们将来可能会在其他设备端网络协议支持的方法。

> [AZURE.NOTE] 在调用方法直接在设备上的时，属性名称和值只能包含可打印美国 ASCII 字母，以外任何在下列一组︰ ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``。

方法是同步的并且是成功或失败后的超时时间 (默认︰ 30 秒，可设置设置为 3600 秒)。 方法是非常有用的交互作用当且仅当设备已联机并接收命令，如从电话灯打开设备位置。 在这些情况下，您希望看到立即的成功或失败，所以云服务可以尽可能快地处理结果。 设备可能会返回该方法中，由于某些邮件正文，但它并不是必需这样做的方法。 没有订购任何保证或方法调用任何并发语义。

设备方法调用是只有 HTTP 中的云端，和仅 MQTT 从设备端。

## <a name="reference-topics"></a>参考主题︰

下列参考主题为您提供了有关使用直接方法的详细信息。

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>调用后端应用程序中的直接方法

### <a name="method-invocation"></a>方法调用

在设备上的直接方法调用是构成 HTTP 调用︰

- 特定于设备*的 URI* (`{iot hub}/twins/{device id}/methods/`)
- POST*方法*
- *标头*包含授权、 请求 ID、 内容类型和内容编码
- 透明 JSON*正文*以下面的格式︰

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  超时是以秒为单位。 如果未设置超时，则它默认为 30 秒。
  
### <a name="response"></a>响应

后端接收到响应，其中包括︰

- *HTTP 状态代码*，它使用来自 IoT 中心，包括当前未连接的设备出现 404 错误的错误
- *标头*包含 etag，请求 ID、 内容类型和内容编码
- JSON*正文*以下面的格式︰

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   同时`status`，`body`是提供的设备，用来响应与设备的状态代码和/或描述。

## <a name="handle-a-direct-method-on-a-devcie"></a>句柄上 devcie 的直接方法

### <a name="method-invocation"></a>方法调用

MQTT 主题上的直接方法请求接收设备︰`$iothub/methods/POST/{method name}/?$rid={request id}`

该设备接收到的主体是以下面的格式︰

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

方法请求是 QoS 0。

### <a name="response"></a>响应

设备会发送响应`$iothub/methods/res/{status}/?$rid={request id}`，其中︰

 - `status`属性是自备设备的方法的执行状态。
 - `$rid`属性是从方法调用从 IoT 中心接收到的请求 ID。

正文由设备设置，并可将任何状态。

## <a name="additional-reference-material"></a>其他参考资料

其他开发人员指南中的参考主题包含︰

- [终结点 IoT 中心][lnk-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。
- [带宽限制和配额][lnk-quotas]描述了适用于 IoT 中心服务和需要使用服务的限制行为的配额。
- [IoT 中心设备和服务 Sdk] [lnk-sdks]列出了各种语言的 Sdk 您开发设备和服务交互的应用程序具有 IoT 集线器时使用。
- [Twins、 方法和作业的查询语言][lnk-query]描述了您可以使用有关设备 twins、 方法和作业 IoT 集线器从检索信息的查询语言。
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]提供 MQTT 协议有关 IoT 集线器支持的详细信息。

## <a name="next-steps"></a>下一步行动

现在，您已经学习了如何使用直接的方法，您可能感兴趣的以下开发人员指南 》 的主题︰

- [多台设备上的计划作业][lnk-devguide-jobs]

如果您想要尝试一些本文中介绍的概念，您可能会感兴趣以下 IoT 集线器教程︰

- [使用云至设备的方法][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
