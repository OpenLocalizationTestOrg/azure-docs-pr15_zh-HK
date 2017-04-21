<properties
 pageTitle="开发人员指南-设备标识注册表 |Microsoft Azure"
 description="Azure IoT 中心开发人员指南-设备标识注册表和如何使用它来管理您的设备的说明"
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

# <a name="manage-device-identities-in-iot-hub"></a>管理设备标识 IoT 中心

## <a name="overview"></a>概述

每个 IoT 集线器已存储的信息都可以连接到集线器的设备设备标识注册表。 一个设备可以连接到中心之前，必须有该设备的集线器设备标识注册表中的项。 此外必须进行与中心根据设备标识注册表中存储的凭据进行身份验证设备。

从较高层次，设备标识注册表是支持其余的设备标识资源的集合。 向注册表中添加一个条目，IoT 中心如队列，其中包含云至设备信息传输的数据服务中创建一组的每个设备资源。

### <a name="when-to-use"></a>何时使用

当您需要提供的设备连接到 IoT 集线器和需要来控制每个设备访问您中心面向设备的端点时，请使用设备标识注册表。

> [AZURE.NOTE] 设备标识注册表中不包含任何特定于应用程序的元数据。

## <a name="device-identity-registry-operations"></a>设备标识注册表操作

IoT 集线器设备标识注册表公开以下操作︰

* 创建设备标识
* 更新设备标识
* 检索设备 id 的标识
* 删除设备标识
* 列出最多 1000 个标识
* 导出到 Azure blob 存储所有标识
* 从 Azure blob 存储导入标识

所有这些操作均可使用开放式并发，作为中指定[RFC7232][lnk-rfc7232]。

> [AZURE.IMPORTANT] 检索所有集线器标识注册表中的标识的唯一办法是使用[导出][lnk-export]功能。

IoT 集线器设备标识注册表︰

- 不包含任何应用程序的元数据。
- 可以通过使用键**deviceId**访问类似于字典中。
- 不支持表达的查询。

IoT 解决方案通常有单独的特定于解决方案的存储库，其中包含特定于应用程序的元数据。 例如，智能建筑解决方案中的特定于解决方案的存储将记录温度传感器部署在该房间。

> [AZURE.IMPORTANT] 为设备管理和资源调配操作只能使用设备标识注册表。 高吞吐量操作在运行时不应依赖于设备标识注册表中执行的操作。 例如，发送一个命令之前检查设备的连接状态不是受支持的模式。 请务必检查[节流率][ lnk-quotas] [设备检测信号]设备标识注册表，以及[lnk-guidance-heartbeat]模式。

## <a name="disable-devices"></a>禁用设备

您可以通过更新注册表中标识的**状态**属性禁用设备。 通常情况下，在两个方案中，使用此属性︰

- 在配置业务流程过程。 有关详细信息，请参阅[设备置备][lnk-guidance-provisioning]。
- 如果出于任何原因，您考虑的设备遭到破坏或已成为未经授权。

## <a name="import-and-export-device-identities"></a>导入和导出设备标识

您可以导出中大容量设备标识 IoT 中心标识注册表中，通过使用异步操作[IoT 中心资源提供方终结点]上的[lnk-endpoints]。 导出是使用客户提供的 blob 容器来保存从标识注册表读取设备标识数据的长时间运行作业。

您可以使用导入中大容量设备标识到 IoT 中心标识注册表， [IoT 中心资源提供方终结点]上的异步操作[lnk-endpoints]。 导入是使用客户提供 blob 容器中的数据写入设备标识注册表设备标识数据的长时间运行作业。

- 有关导入和导出的 Api 的详细信息，请参阅[IoT 中心资源提供程序 REST Api][lnk-resource-provider-apis]。
- 要了解更多有关运行导入和导出作业，请参阅[批量管理 IoT 集线器设备标识的][lnk-bulk-identity]。

## <a name="device-provisioning"></a>设备资源调配

给定的 IoT 解决方案存储设备数据取决于该解决方案的特定需求。 但是，至少，解决方案必须存储设备标识和身份验证密钥。 Azure IoT 中心包括可存储为每个设备，如 Id、 验证密钥和状态代码的值标识注册表。 一个解决方案可以使用 Azure 表存储，Azure blob 存储或 Azure DocumentDB 等其他 Azure 服务来存储任何其他设备的数据。

*设备置备*是将初始设备数据添加到您的解决方案中的存储过程。 若要启用新设备连接到网络集线器，必须向 IoT 中心标识注册表中添加新的设备 ID 和密钥。 作为设置过程的一部分，您可能需要初始化其他解决方案存储中特定于设备的数据。

## <a name="device-heartbeat"></a>设备检测信号

IoT 中心标识注册表包含名为**connectionState**的字段。 您只应在开发过程中使用**connectionState**字段和调试，IoT 解决方案应该不会查询字段 （例如，检查设备是否已连接来决定是否发送云至设备信息或 SMS） 运行时。

如果 IoT 解决方案需要知道如果设备已连接 （在运行时，或更准确，不是**connectionState**属性提供），您的解决方案应实现*信号模式*。

在检测信号模式中，设备会发送设备到云的消息至少一次每隔固定的时间 （例如，每隔一小时至少一次） 量。 这意味着即使设备中没有要发送的任何数据，则它仍发送空设备到云消息 （通常具有的属性，将其标识为心跳）。 在服务方面，解决方案维护与接收到的每个设备，最后一次心跳的地图，并假定如果没有在预期的时间内接收检测信号消息没有设备有问题。

一个更复杂的实现可能包括[操作监视]的信息[lnk-devguide-opmon]确定试图连接或通信，但故障设备。 在实现时的心跳模式，一定要检查[IoT 集线器配额和限制][lnk-quotas]。

> [AZURE.NOTE] 如果 IoT 解决方案需要设备的连接状态，只是为了确定是否发送云至设备信息和消息不会广播到设备的大集，更简单模式考虑是使用短的到期时间。 此实现与维护设备连接状态注册表使用的检测信号模式，同时又大大增加了效率相同的结果。 它也可能是，通过请求消息确认 IoT 集线器的设备都能接收消息并不处于联机状态或正在失败的通知。

## <a name="reference-topics"></a>参考主题︰

下列参考主题为您提供了有关 devcie 标识注册表的详细信息。

## <a name="device-identity-properties"></a>设备标识属性

设备标识表示为具有以下属性的 JSON 文档。

| 属性 | 选项 | 说明 |
| -------- | ------- | ----------- |
| deviceId | 要求，在更新只读 | ASCII 7 位的字母数字字符区分大小写的字符串 （最多 128 个字符） + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`。 |
| generationId | 所需的只读 | 中心生成的、 区分大小写的最多 128 个字符长的字符串。 这用于区分具有相同**deviceId**，设备时已被删除并重新创建。 |
| etag | 所需的只读 | 一个字符串，表示为设备的标识，按照[RFC7232]弱 etag[lnk-rfc7232]。|
| 执行授权 | 可选 | 复合对象包含身份验证信息和安全资料。 |
| auth.symkey | 可选 | 一个复合对象，该对象包含一个主要和辅助键，以 base64 格式存储。 |
| 状态 | 必填 | 访问指示器。 可以**启用**或**禁用**。 如果**启用**，此设备将允许连接。 如果**禁用**该设备不能访问任何面向设备的终结点。 |
| statusReason | 可选 | 存储设备标识状态的原因 128 字符长的字符串。 允许所有的 utf-8 字符。 |
| statusUpdateTime | 只读的 | 临时的指示器，显示的日期和时间的最后的状态更新。 |
| connectionState | 只读的 | 指示连接状态字段︰**连接**或**断开连接**。 此字段表示 IoT 集线器设备连接状态的视图。 **重要提示**︰ 此字段应仅用于开发/调试目的。 仅使用 MQTT 或 AMQP 设备更新的连接状态。 此外，它基于协议级 ping （MQTT ping 或 AMQP ping），而且它可以用最大延迟时间只有 5 分钟的时间。 出于这些原因，可以有误报，例如设备报告为连接，但是，实际上断开连接。 |
| connectionStateUpdatedTime | 只读的 | 已更新时间指示器，显示的日期和最后一次连接状态。 |
| lastActivityTime  | 只读的 | 瞬时的指示器，显示的日期和最后一次设备连接、 接收，或者发送一条消息。 |

> [AZURE.NOTE] 连接状态只能表示 IoT 中心查看连接的状态。 对这种状态的更新可能会延迟，具体取决于网络情况和配置。

## <a name="additional-reference-material"></a>其他参考资料

其他开发人员指南中的参考主题包含︰

- [终结点 IoT 中心][lnk-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。
- [带宽限制和配额][lnk-quotas]描述了适用于 IoT 中心服务和需要使用服务的限制行为的配额。
- [IoT 中心设备和服务 Sdk] [lnk-sdks]列出了各种语言的 Sdk 您开发设备和服务交互的应用程序具有 IoT 集线器时使用。
- [Twins、 方法和作业的查询语言][lnk-query]描述了您可以使用有关设备 twins、 方法和作业 IoT 集线器从检索信息的查询语言。
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]提供 MQTT 协议有关 IoT 集线器支持的详细信息。

## <a name="next-steps"></a>下一步行动

现在，您已经学习了如何使用 IoT 集线器设备标识注册表，您可能会感兴趣以下开发人员指南 》 的主题︰

- [控制访问 IoT 集线器][lnk-devguide-security]
- [使用设备 twins 同步状态和配置][lnk-devguide-device-twins]
- [在设备上的直接方法调用][lnk-devguide-directmethods]
- [多台设备上的计划作业][lnk-devguide-jobs]

如果您想要尝试一些本文中介绍的概念，您可能会感兴趣以下 IoT 集线器教程︰

- [开始使用 Azure IoT 集线器][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md