<properties
 pageTitle="开发人员指南 》 的消息 |Microsoft Azure"
 description="Azure IoT 中心开发人员指南-设备到云中和云至设备信息"
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

# <a name="send-and-receive-messages-with-iot-hub"></a>发送和接收邮件 IoT 集线器

## <a name="overview"></a>概述

IoT 中心提供以下消息传递的基元，与设备进行通信︰

- [设备为云][lnk-d2c]从设备到应用程序的后端。
- [云对设备][lnk-c2d]应用程序从后端 （*服务*或*云*）。

IoT 中心消息传递功能的核心属性是可靠性和耐用性的消息。 这些属性使连接中断在设备端，并加载事件处理上的云端中的峰值的复原能力。 IoT 中心实现了*至少一次*交货保证设备到云，云至设备的消息。

IoT 集线器支持多个[设备面向协议][ lnk-protocols] （例如 MQTT、 AMQP 和 HTTP）。 若要跨协议支持无缝的互操作性，IoT 中心定义[通用的消息格式][lnk-message-format]支持所有面向设备的协议。

IoT 中心公开[事件中心兼容终结点][lnk-compatible-endpoint]使后端应用程序可以读取设备到云所接收中心的消息。

### <a name="when-to-use"></a>何时使用

消息传递是 IoT 中心的核心功能。 您需要向您的后端，从移动设备发送消息或从您的后端向设备发送消息时，请使用此选项。

IoT 集线器和事件集线器服务的比较，请参见[比较的集线器 IoT 和事件集线器][lnk-compare]。

## <a name="device-to-cloud-messages"></a>设备与云的消息

您发送设备到云通过面向设备的终结点 （**/devices/ {deviceId} / 消息/事件**） 的邮件。 后端服务接收设备到云邮件通过[事件集线器]与兼容的面向服务的终结点 (**/messages/events**)[lnk-event-hubs]。 因此，您可以使用标准[事件集线器集成和 Sdk] [lnk-compatible-endpoint]接收设备到云的消息。

IoT 中心实现设备与云以一种类似于[集线器事件]消息[lnk-event-hubs]。 IoT 集线器设备到云消息非常类似事件集线器*事件*[服务总线]比[lnk-servicebus]*的消息*。

此实现具有以下含义︰

* 同样事件集线器事件设备到云消息是持久和 IoT 中心保留最多七天 ([设备到云的配置选项]，请参阅[lnk-d2c-configuration])。
* 跨一组固定的分区，则在创建时设置分区设备到云的消息 (请参阅[设备到云配置选项][lnk-d2c-configuration])。
* Analogously 事件集线器到阅读设备到云邮件客户端必须分区和检查点处理。 请参阅[事件集线器-使用事件][lnk-event-hubs-consuming-events]。
* 事件集线器与事件相似，设备与云消息最多只能包含 256 KB，并可以分批进行优化发送分组。 批次最多只能包含 256 KB 和最多 500 个消息。

有，但是，IoT 集线器设备到云消息和事件集线器之间的几个重要的差别︰

* [控制对 IoT 中心访问]所述[lnk-devguide-security]部分，IoT 中心允许每台设备的身份验证和访问控制。
* IoT 中心允许数以百万计的同时连接的设备 (请参阅[配额和限制][lnk-quotas])，而事件集线器仅限于 5000 AMQP 连接每个名称空间。
* IoT 集线器不允许任意分区使用**PartitionKey**。 根据其原始**deviceId**分区设备到云的消息。
* 横向 IoT 集线器是比缩放事件集线器略有不同。 有关详细信息，请参阅[缩放 IoT 中心][lnk-guidance-scale]。

> [AZURE.NOTE] 在所有情况下，无法替换事件集线器的 IoT 集线器。 例如，在某些事件处理计算，可能需要对另一个属性或字段的事件分析数据流之前重新分区。 在这种情况下，您可以使用事件中心分隔开来的流处理管道两个部分。 有关详细信息，请参阅*分区*在[Azure 事件集线器概述][lnk-eventhub-partitions]。

有关如何使用设备到云的消息，详细信息请参阅[IoT 集线器 Api 和 Sdk][lnk-sdks]。

> [AZURE.NOTE] 当使用 HTTP 发送设备到云，属性名称和值只能包含字母数字的 ASCII 字符，再加上``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``。

### <a name="non-telemetry-traffic"></a>非遥测通信

通常情况下，除了遥测数据点，设备还发送消息和请求的要求执行和应用程序的业务逻辑层的处理。 例如，关键警报必须触发特定操作中的后端或从后端发送命令到设备响应。

最佳的方法，来处理这种类型的消息的详细信息，请参阅[教程︰ 如何处理 IoT 集线器设备到云消息][lnk-d2c-tutorial]教程。

### <a name="device-to-cloud-configuration-options"></a>设备与云配置选项

IoT 中心公开下面的属性，使您能够控制设备与云消息。

* **分区数**。 将此属性设置在创建定义分区的设备到云事件摄入数量。
* **保留时间**。 此属性指定设备到云邮件的保留时间。 默认值为一天，但它可以增加到七天。

此外，analogously 的事件集线器 IoT 中心使您能够管理设备到云上的使用者组收到终结点。

您可以修改所有这些属性，以编程方式通过[IoT 中心资源提供程序 REST Api][lnk-resource-provider-apis]，或通过使用[Azure 的门户网站][lnk-management-portal]。

### <a name="anti-spoofing-properties"></a>反欺骗属性

避免欺骗在设备与云的邮件，IoT 集线器设备戳所有消息具有以下属性︰

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

前两个包含**deviceId**和原始设备，根据[设备标识属性]的**generationId** [lnk-device-properties]。

**ConnectionAuthMethod**属性包含一个 JSON 序列化的对象，具有以下属性︰

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>云至设备信息

发送通过面向服务的终结点 (**/messages/devicebound**) 的云至设备信息。 一个设备接收通过设备特定终结点 (**/devices/ {deviceId} / 消息/devicebound**)。

每个云至设备信息面向单个设备通过将**为**属性设置为**/devices/ {deviceId} / 消息/devicebound**。

>[AZURE.IMPORTANT] 每个设备队列保存最多 50 云至设备信息。 试图将多个消息发送到相同的设备将导致一个错误。

> [AZURE.NOTE] 当您发送云至设备信息时，属性名称和值只能包含字母数字的 ASCII 字符，再加上``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``。

### <a name="message-lifecycle"></a>消息生命周期

为了保证至少执行一次消息传递，IoT 中心仍然存在云至设备每个设备队列中的消息。 设备必须明确承认*完成*IoT 集线器，以便从队列中移除。 这样可以保证对连接和设备故障恢复能力。

下图显示了云至设备信息的生命周期状态图。

![云至设备信息生命周期][img-lifecycle]

当该服务发送消息时，则被视为*已排入队列*。 当设备要*接收*到一条消息，IoT 中心*锁*（的状态设置为**不可见**） 的消息允许其他线程在同一台设备开始接收其他的消息。 当某个设备线程完成消息的处理时，它*完成*消息通知 IoT 集线器。

设备还可以︰

- *拒绝*的消息，从而导致 IoT 集线器，以便将其设置为**Deadlettered**状态。 注意︰ 用 MQTT 连接的设备不能拒绝 C2D 的邮件。
- *放弃*消息，从而导致 IoT 集线器，以便将该消息放回队列的状态将设置为**已排入队列**。

一个线程可能会失败，而不通知 IoT 中心处理一条消息。 在这种情况下，邮件自动从**不可见**状态转变到**排队**状态*可见性 （或锁定） 超时*之后。 此超时的默认值为一分钟。

一条消息可以转移**排**和**不可见**之间状态顶多次数 IoT 集线器上的**最大传送计数**属性中指定的。 转换的次数之后, IoT 中心将消息的状态设置为**Deadlettered**。 同样，IoT 中心设置的状态邮件到**Deadlettered**后其过期时间 (请参阅[生存时间][lnk-ttl])。

有关云至设备信息的教程，请参见[教程︰ 如何发送 IoT 中枢云至设备信息][lnk-c2d-tutorial]。 不同的 Api 和 Sdk 参考主题公开云至设备功能，请参阅[IoT 集线器 Api 和 Sdk][lnk-sdks]。

> [AZURE.NOTE] 通常情况下，云至设备信息完成时邮件的损失不会影响应用程序逻辑。 例如，在本地存储中，成功地保持了消息内容或成功执行操作。 该消息还可能携带瞬态信息，其损失将不会影响应用程序的功能。 有时，对于长时间运行的任务，您可以完成后保存在本地存储区中的任务描述云至设备信息。 则可以向应用程序的后端，与一个或多个设备与云消息通知任务进程的各个阶段。

### <a name="message-expiration-time-to-live"></a>邮件到期时间 （生存时间）

每个云至设备信息有截止时间。 这一次的服务 （在**ExpiryTimeUtc**属性中），或通过 IoT 中心使用设置为 IoT 集线器属性指定的默认*生存时间*。 请参阅[云至设备配置选项][lnk-c2d-configuration]。

> [AZURE.NOTE] 利用邮件到期时间并避免的常见方法是将消息发送到断开连接的设备，设置短时间值的实时。 这种方法实现了维护设备的连接状态，同时更有效地与相同的结果。 在请求确认消息时，IoT 中心会通知您哪些设备是能够接收消息，以及哪些设备未处于联机状态，或者失败。

### <a name="message-feedback"></a>消息反馈

当您发送云至设备信息时，服务可以请求传递的每一封邮件反馈有关该消息的最终状态。

- 将**Ack**属性设置为**正**，如果 IoT 集线器将生成反馈信息，当且仅当云至设备信息达到**完成**状态。
- 将**Ack**属性设置为**负**，如果 IoT 集线器将生成反馈信息，当且仅当，云至设备信息到达**Deadlettered**状态。
- 如果将**Ack**属性设置为**完全**，则 IoT 中心生成反馈信息不论何种情况。

> [AZURE.NOTE] 如果**确认**已**满**，并且您没有收到反馈信息，这意味着反馈邮件已过期。 该服务无法知道原始邮件发生了什么变化。 在实践中，一项服务应确保过期之前，它可以处理反馈。 最长的到期时间为两天，以便有充足的时间来获取服务再次运行发生故障时。

[终结点]所述[lnk-endpoints]，IoT 中心作为消息传递通过面向服务的终结点 (**/messages/servicebound/feedback**) 的反馈。 接收反馈的语义是云至设备信息一样，具有相同[消息生命周期][lnk-lifecycle]。 只要可能，消息反馈分批处理在单个邮件中，使用以下格式︰

| 属性 | 说明 |
| -------- | ----------- |
| EnqueuedTime | 指示消息创建时的时间戳。 |
| 用户 Id | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

正文是 JSON 序列化数组的记录，每一个具有以下属性︰

| 属性 | 说明 |
| -------- | ----------- |
| EnqueuedTimeUtc | 时间戳指示消息的结果发生。 例如，设备已完成或邮件过期。 |
| OriginalMessageId | 此反馈信息与有关云至设备邮件的**邮件 Id** 。 |
| 状态代码 | 要求使用整数。 用于生成 IoT 集线器的反馈消息。 <br/> 0 = 成功 <br/> 1 = 邮件过期 <br/> 2 = 最大传递计数溢出 <br/> 3 = 消息被拒绝 |
| 说明 | **状态代码**的字符串值。 |
| DeviceId | **DeviceId**云至设备消息这条反馈信息与相关的目标设备。 |
| DeviceGenerationId | **DeviceGenerationId**的这条反馈信息与有关云至设备信息的目标设备。 |


>[AZURE.IMPORTANT] 该服务必须指定云至设备消息可将其反馈与原始邮件的**邮件 Id** 。

下面的示例显示反馈消息的正文。

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>云对设备配置选项

每个 IoT 中枢公开消息云至设备的以下配置选项。

| 属性 | 说明 | 范围和默认值 |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | 默认 TTL 云至设备信息。 | ISO_8601 到 2D （最小值为 1 分钟） 的时间间隔。 默认值︰ 1 小时。 |
| maxDeliveryCount | 对于云至设备每个设备队列传递最大计数。 | 1 到 100。 默认值︰ 10。 |
| feedback.ttlAsIso8601 | 服务绑定反馈消息的保留。 | ISO_8601 到 2D （最小值为 1 分钟） 的时间间隔。 默认值︰ 1 小时。 |
| feedback.maxDeliveryCount | 最大传递反馈队列计数。 | 1 到 100。 默认值︰ 100。 |

有关详细信息，请参阅[创建 IoT 集线器][lnk-portal]。

## <a name="read-device-to-cloud-messages"></a>读取设备到云的消息

IoT 中心公开您的后端服务读取接收网络集线器的设备到云消息的终结点。 终结点是事件中心兼容，它使您能够使用的任何机制事件集线器服务支持阅读邮件。

当您使用[.net 的 Azure 服务总线 SDK] [lnk-servicebus-sdk]或[事件集线器的事件处理器主机][lnk-eventprocessorhost]，可以使用任何 IoT 集线器连接字符串使用正确的权限。 作为事件中心的名称，然后使用**邮件中的事件**。

当您使用 Sdk （或产品集成） 不知道 IoT 集线器，则必须从 IoT 中心设置[Azure 门户]中检索事件中心兼容终结点和事件中心兼容名称[lnk-management-portal]:

1. 在 IoT 中心刀片式服务器，单击**消息**。
2. 在**设备与云设置**部分中，您可以找到以下值︰**事件中心兼容终结点**、**事件中心兼容名称**和**分区**。

    ![设备与云设置][img-eventhubcompatible]

> [AZURE.NOTE] 如果 SDK 需要**主机名**或**Namespace**值，从**事件中心兼容终结点**中删除方案。 例如，如果您事件中心兼容终结点是**sb://iothub-ns-myiothub-1234.servicebus.windows.net/**，**主机名**是**iothub ns-myiothub 1234.servicebus.windows.net**，而**Namespace** **iothub-ns-myiothub-1234年**。

然后可以使用任何具有连接到指定的事件中心的**ServiceConnect**权限的共享的访问安全策略。

如果您需要使用以前的信息生成事件集线器连接字符串，请使用下面的模式︰

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

以下是 Sdk 和集成，您可以使用与事件中心兼容 IoT 中心公开的终结点的列表︰

* [客户端 Java 事件集线器](https://github.com/hdinsight/eventhubs-client)
* [Apache 风暴 spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md)。 可以在 GitHub 上查看[spout 源](https://github.com/apache/storm/tree/master/external/storm-eventhubs)。
* [Apache 触发集成](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>参考主题︰

下列参考主题为您提供有关与 IoT 中心交换消息的详细信息。

## <a name="message-format"></a>邮件格式

IoT 中心消息组成︰

* 一套*系统属性*。 IoT 中心解释或设置的属性。 这一组是预先确定的。
* 一组*应用程序属性*。 可以定义应用程序的字符串属性和访问权限，而无需将消息体反序列的字典。 IoT 中心绝对不会修改这些属性。
* 不透明的二进制正文。

有关如何将消息编码在不同协议的详细信息，请参阅[IoT 集线器 Api 和 Sdk][lnk-sdks]。

下表列出了 IoT 中心消息中的系统属性的设置。

| 属性 | 说明 |
| -------- | ----------- |
| 邮件 Id | 邮件中，使用请求-应答模式用户可设置标识符。 格式︰ 区分大小写 （最多 128 个字符） 的字符串 ASCII 7 位的字母数字字符 + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`。 |
| 序列号 | 由 IoT 中心分配给每个云至设备信息数量 （每个设备队列是唯一的）。 |
| 自 | 在[云至设备]中指定的目标[lnk-c2d]的消息。 |
| ExpiryTimeUtc | 日期和时间的邮件到期时间。 |
| EnqueuedTime | 日期和时间 IoT 集线器接收消息。 |
| 都会 | 在响应消息通常包含的请求，在请求-应答模式的邮件 Id 字符串属性。 |
| 用户 Id | 用于指定消息的来源 ID。 当消息由 IoT 中心生成时，则将它设置为`{iot hub name}`。 |
| 肯定应答 | 一个反馈消息生成器。 此属性用于在云至设备信息请求 IoT 集线器来生成消息的消耗导致反馈消息由设备。 可能的值︰**无**（默认值）︰ 没有反馈信息生成的**正**︰ 如果消息已完成，收到反馈信息**负**︰ 如果邮件已过期 （或最大传递计数已达到） 而无需通过设备，或**完全**收到反馈信息︰ 正值和负值。 有关详细信息，请参阅[消息反馈][lnk-feedback]。 |
| ConnectionDeviceId | 设置 IoT 集线器设备到云的邮件 ID。 它包含**deviceId**设备发送的消息。 |
| ConnectionDeviceGenerationId | 设置 IoT 集线器设备到云的邮件 ID。 它包含**generationId** (根据[设备标识属性][lnk-device-properties]) 发送邮件的设备。 |
| ConnectionAuthMethod | IoT 集线器设备到云的消息设置身份验证方法。 此属性包含有关用于验证发送消息的设备的身份验证方法的信息。 有关详细信息，请参阅[设备至云反欺骗][lnk-antispoofing]。|

## <a name="communication-protocols"></a>通信协议

IoT 中心允许设备使用[MQTT][lnk-mqtt]，通过 Websocket， [AMQP]MQTT[lnk-amqp]，通过 Websocket 和 HTTP AMQP 端设备的通信协议。 下表提供了您选择的协议的高级别建议︰

| 协议 | 何时应选择此协议 |
| -------- | ------------------------------------ |
| MQTT <br> 通过 WebSocket MQTT     | 在不需要通过同一个 TLS 连接连接多个设备 （每个都有自己的每个设备凭据） 的所有设备上使用。 |
| AMQP <br> 通过 WebSocket AMQP    | 使用字段和云网关以利用多路复用技术在设备之间的连接。 |
| HTTP    | 用于不能支持其他协议的设备。 |

当您选择您的设备端通信的协议时，请考虑以下几点︰

* **云至设备模式**。 HTTP 没有实现服务器推送的有效方法。 在这种情况下，当您使用 HTTP，设备轮询 IoT 中心云至设备信息。 这种方法会降低效率的设备和 IoT 集线器。 在当前的 HTTP 原则，每台设备应轮询消息每隔 25 分钟或者更多。 另一方面，MQTT AMQP 时和支持服务器推送接收云至设备信息。 它们使即时推送的消息从 IoT 集线器到设备。 如果交货延迟问题，MQTT 或 AMQP 是使用的最佳协议。 对于很少连接设备，HTTP 也适用。
* **域的网关**。 当使用 MQTT 和 HTTP，您不能连接多个设备 （每个都有自己的每个设备凭据） 使用相同的 TLS 连接。 因此，对于[域的网关方案][lnk-azure-gateway-guidance]，这些协议是最理想的因为它们需要一个 TLS 连接的域的网关和 IoT 集线器之间的每个设备连接到的域的网关。
* **资源不足的设备**。 MQTT 和 HTTP 库具有更小的占地面积，比 AMQP 库。 在这种情况下，如果设备具有有限资源 （针对示例中，小于 1 MB RAM），这些协议可能是唯一可用的协议实现。
* **网络遍历**。 标准 AMQP 协议使用端口 5671，而 MQTT 侦听端口 8883，关闭对非 HTTP 协议的网络中可能会造成问题。 通过 Websocket MQTT，通过 Websocket 和 HTTP AMQP 是可用于此方案中使用。
* **有效载荷的大小**。 MQTT 和 AMQP 是一个二进制协议，这导致更简洁有效载荷比 HTTP。

> [AZURE.NOTE] 当使用 HTTP，每台设备应轮询云至设备信息每隔 25 分钟或者更多。 但是，在开发期间，却可经常比每隔 25 分钟轮询一次。

## <a name="port-numbers"></a>端口号

设备可以通信 IoT 中枢在 Azure 使用各种协议。 通常情况下，协议的选择是由该解决方案的特定需求驱动的。 下表列出了必须的设备能够使用特定协议打开出站端口︰

| 协议 | 输入端口 |
| -------- | ------- |
| MQTT     | 8883    |
| 通过 Websocket MQTT | 443    |
| AMQP     | 5671    |
| 通过 Websocket AMQP | 443    |
| HTTP     | 443     |
| LWM2M （设备管理） | 5684 |

一旦在 Azure 的区域创建 IoT 中心，中心将该集线器的生存期内保持相同的 IP 地址。 但是，如果 Microsoft 将 IoT 中心移动到不同的刻度单位维护质量的服务，然后它被分配新的 IP 地址。

## <a name="notes-on-mqtt-support"></a>MQTT 的支持说明

IoT 中心实现具有以下限制和特定行为的 MQTT v3.1.1 协议︰

  * **不支持 QoS 2**。 当设备客户端发布一条消息， **QoS 2**时，IoT 集线器关闭网络连接。 当设备客户端订阅的主题与**QoS 2**时，IoT 中心授予**SUBACK**数据包中的最大 QoS 级别 1。
  * **保留邮件不能永久存在**。 如果设备客户端将消息发布与保留标记设置为 1，添加 IoT 中心**x 选择保留**到该消息的应用程序属性。 在这种情况下，IoT 中枢不能持续保留邮件，但改为将其传递到后端应用程序。

有关详细信息，请参阅[支持 IoT 集线器 MQTT][lnk-devguide-mqtt]。

作为最后一个考虑因素，应检查[Azure IoT 协议网关][ lnk-azure-protocol-gateway] ，它使您能够部署直接与 IoT 集线器接口的高性能的自定义协议网关。 Azure IoT 协议网关使您可以自定义以适应 brownfield MQTT 部署的设备协议或其他自定义协议。 但是，这种方法确实需要您运行和操作的自定义协议网关。

## <a name="additional-reference-material"></a>其他参考资料

其他开发人员指南中的参考主题包含︰

- [终结点 IoT 中心][lnk-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。
- [带宽限制和配额][lnk-quotas]描述了适用于 IoT 中心服务和需要使用服务的限制行为的配额。
- [IoT 中心设备和服务 Sdk] [lnk-sdks]列出了各种语言的 Sdk 您开发设备和服务交互的应用程序具有 IoT 集线器时使用。
- [Twins、 方法和作业的查询语言][lnk-query]描述了您可以使用有关设备 twins、 方法和作业 IoT 集线器从检索信息的查询语言。
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]提供 MQTT 协议有关 IoT 集线器支持的详细信息。

## <a name="next-steps"></a>下一步行动

现在您已了解如何发送和接收邮件使用 IoT 中枢，您可能会感兴趣以下开发人员指南 》 的主题︰

- [从设备的文件上载][lnk-devguide-upload]
- [管理设备标识 IoT 中心][lnk-devguide-identities]
- [控制访问 IoT 集线器][lnk-devguide-security]
- [使用设备 twins 同步状态和配置][lnk-devguide-device-twins]
- [在设备上的直接方法调用][lnk-devguide-directmethods]
- [多台设备上的计划作业][lnk-devguide-jobs]

如果您想要尝试一些本文中介绍的概念，您可能会感兴趣下面 IoT 集线器教程︰

- [开始使用 Azure IoT 集线器][lnk-getstarted-tutorial]
- [如何发送 IoT 中枢云至设备信息][lnk-c2d-tutorial]
- [如何处理 IoT 集线器设备到云的消息][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
