<properties
    pageTitle="C-IoTHubClient 的 azure IoT 设备 SDK |Microsoft Azure"
    description="了解有关使用 C Azure IoT 设备 SDK 中的 IoTHubClient 库的详细信息"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT 设备 SDK C – 关于 IoTHubClient 的详细信息

本系列[第一篇文章](iot-hub-device-sdk-c-intro.md)介绍了**Microsoft Azure IoT 设备用于 c 语言的 SDK**。这篇文章解释了 SDK 中有两个结构层。 在底部是**IoTHubClient**库直接管理与 IoT 中心的通信。 此外，还有的**序列化程序**库的构建提供序列化服务。 在这篇文章中我们将**IoTHubClient**库上提供更多详细信息。

以前的文章描述了如何使用**IoTHubClient**库 IoT 集线器发送事件和接收消息。 本文通过介绍如何更精确地管理*时*发送和接收数据，向您介绍**较低级别的 Api**扩展该讨论。 我们还将介绍如何将属性附加到事件 （并从邮件中检索） 使用属性处理**IoTHubClient**库中的功能。 最后，我们将提供不同的方法来处理从 IoT 集线器接收的消息的其他解释。

文章最后介绍一些其他主题，包括更多有关设备的凭据以及如何更改**IoTHubClient**通过配置选项的行为。

我们将使用**IoTHubClient** SDK 示例来说明这些主题。 如果您想要继续下去，请参阅**iothub\_客户端\_示例\_http**和**iothub\_客户端\_示例\_amqp** C.一切以下各节介绍这些示例中演示的 Azure IoT 设备 SDK 中包含的应用程序。

可以在[Microsoft Azure IoT Sdk](https://github.com/Azure/azure-iot-sdks) GitHub 储存库中查找**Azure IoT 设备用于 c 语言的 SDK**并查看[C API 参考](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)中的 API 的详细信息。

## <a name="the-lower-level-apis"></a>较低级别的 Api

上一篇文章所述**IotHubClient**的上下文中的基本操作**iothub\_客户端\_示例\_amqp**应用程序。 例如，它解释了如何初始化使用此代码库。

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

它还描述了如何使用此函数调用的事件发送。

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

文章还介绍了如何通过注册一个回调函数来接收消息。

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

文章还介绍了如何释放资源使用如下所示的代码。

```
IoTHubClient_Destroy(iotHubClientHandle);
```

但是有这些 Api 的每个辅助函数︰

-   IoTHubClient\_LL\_CreateFromConnectionString

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_销毁


所有这些功能的 API 名称中包括"LL"。 不同的是，这些函数中的每个参数均与非 LL 与其相同。 但是，这些函数的行为是一种重要方式不同。

当您调用**IoTHubClient\_CreateFromConnectionString**，基础库创建一个新线程，在后台运行。 此线程发送事件，并从，IoT 集线器接收消息。 没有此类线程将创建使用"LL"Api 时。 后台线程的创建是给开发人员提供了便利。 您不必担心显式地将事件发送和接收消息 IoT 集线器，它会自动在后台。 与此相反的是，"LL"Api 为您提供显式控制与 IoT 中心的通信需要。

要更好地理解这一点，让我们看一个示例︰

当您调用**IoTHubClient\_SendEventAsync**，什么您实际上做将缓冲区中的事件。 当您调用创建后台线程**IoTHubClient\_CreateFromConnectionString**不断监视此缓冲区并将它所包含的任何数据发送到 IoT 集线器。 这是在主线程正在执行其他工作的同时在后台。

同样，您注册的邮件使用一个回调函数**IoTHubClient\_SetMessageCallback**，正在指示 SDK 具有后台线程消息的接收、 独立于主线程时调用的回调函数。

"LL"Api，请不要创建后台线程。 而新的 API 必须调用显式发送和接收来自 IoT 中心的数据。 在下面的示例说明了这一点。

**Iothub\_客户端\_示例\_http** SDK 中包含的应用程序演示的较低级别的 Api。 在该示例中，我们发送事件到 IoT 集线器与下面类似的代码︰

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

前三行创建邮件，和最后一个行发送该事件。 但是，如前所述，"发送"事件意味着只是将数据放到一个缓冲区。 没有传输到网络上时我们所说的**IoTHubClient\_LL\_SendEventAsync**。 为了真正进入到 IoT 中心的数据，则必须调用**IoTHubClient\_LL\_DoWork**，如下例所示︰

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

这段代码 (从**iothub\_客户端\_示例\_http**应用程序) 重复调用**IoTHubClient\_LL\_DoWork**。 每次**IoTHubClient\_LL\_DoWork**被调用，它从缓冲区将某些事件发送到 IoT 集线器，它检索队列的信息发送到设备。 后一种情况意味着，如果我们注册消息的回调函数，然后回调调用 （假定任何消息排队等候）。 我们将使用如下所示的代码注册回调函数︰

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

因此， **IoTHubClient\_LL\_DoWork**通常在循环中调用是每次调用、 IoT 集线器发送*一些*缓冲的事件和检索*下一个*消息排队的设备。 每次调用并不保证所有缓冲的事件发送或检索所有队列中的邮件。 如果您想要发送缓冲区中的所有事件，然后再继续进行其他处理可以将此循环替换代码如下所示︰

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

此代码调用**IoTHubClient\_LL\_DoWork**直到缓冲区中的所有事件已都发送到 IoT 集线器。 请注意这不还表示，已接收到所有排队的消息。 部分原因是检查"所有"的邮件，不是为确定一项操作。 如果您检索"全部"的邮件，但然后另一个发送到设备紧随其后？ 更好的方法来处理与程控超时。 例如，每次调用消息回调函数可以重置计时器。 然后，您可以编写逻辑来继续处理，如果，例如，没有消息已接收最后*X*秒。

当您正在完成的 ingressing 事件和接收邮件，请务必调用对应的函数来清理资源。

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

从根本上说是一套 Api 来发送和接收数据，另一组 Api，这些执行相同的操作，而无需后台线程与后台线程。 许多开发人员更倾向于非-LL Api，但当开发人员需要显式控制网络传输时，较低级别的 Api 非常有用。 例如，某些设备收集数据随时间和唯一入口事件按指定时间间隔 （例如，每隔一小时一次或每天一次）。 较低级别的 Api 使您能够以显式控制当您发送和接收来自 IoT 中心的数据。 其他人只会更喜欢低层 Api 提供的简单性。 一切事情都发生在主线程，而不是某些工作发生在后台。

选择，取决于哪种模型一定要使用的 Api 中保持一致。 如果通过调用启动**IoTHubClient\_LL\_CreateFromConnectionString**，请确保您只使用相应的低级 Api 的任何跟进工作︰

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_销毁

-   IoTHubClient\_LL\_DoWork

相反也是如此。 如果使用**IoTHubClient\_CreateFromConnectionString**，然后将非-LL Api 用于任何附加处理。

在用于 c 语言的 Azure IoT 设备 SDK，看到**iothub\_客户端\_示例\_http**有关的较低级别的 Api 的完整示例应用程序。 **Iothub\_客户端\_示例\_amqp**应用程序可以引用有关的非-LL Api 的完整示例。

## <a name="property-handling"></a>属性处理

到目前为止我们介绍发送数据，我们已经被指邮件的正文。 例如，请看这段代码︰

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

本示例将消息发送到 IoT 集线器带有文本"Hello World"。 但是，IoT 中心还允许附加到各个邮件的属性。 属性是可以附加到该邮件中的名称/值对。 例如，我们可以修改前面的代码中将属性附加到该消息︰

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

我们开始通过调用**IoTHubMessage\_属性**并向其传递消息的句柄。 我们重新获得了**映射\_处理**使我们能够开始添加属性的引用。 后者通过调用**映射\_AddOrUpdate**，其采用的参考地图\_句柄、 属性名称和属性值。 使用此 API，我们可以添加任意数量的属性，如我们喜欢。

当**事件集线器**从读取事件时，接收方可以枚举的属性，并检索及其相应的值。 例如，在.NET 中这可以通过访问[EventData 对象上的属性集合](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx)。

在前面的示例中，我们到我们给 IoT 中心发送事件附加属性。 属性还可以附加到从 IoT 集线器接收的消息。 如果我们想要从邮件中检索属性，我们可以在我们消息回调函数使用下列代码︰

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

对**IoTHubMessage\_属性**返回**映射\_处理**的引用。 我们然后将传递到该引用**映射\_GetInternals**以获取对名称/值对 （以及属性数） 的数组的引用。 此时，很简单地枚举的属性以获取我们所需的值。

您不需要在应用程序中使用属性。 但是，如果您需要设置这些事件或从邮件中检索记录， **IoTHubClient**库更加方便。

## <a name="message-handling"></a>消息处理

如前所述，当邮件到达 IoT 集线器从**IoTHubClient**库的响应通过调用注册的回调函数。 没有值得一些其他解释此函数的返回参数。 这里是摘录中的回调函数的**iothub\_客户端\_示例\_http**示例应用程序︰

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

请注意，返回类型是**IOTHUBMESSAGE\_处置\_结果**和在我们返回这个特例**IOTHUBMESSAGE\_接受**。 有我们可以从该函数返回其他值，更改**IoTHubClient**库对消息回调的反应如何。 下面是选项。

-   **IOTHUBMESSAGE\_接受**– 已成功处理该消息。 **IoTHubClient**库将不会调用回调函数再次使用相同的消息。

-   **IOTHUBMESSAGE\_拒绝**— 未处理该邮件，并没有这样做将来没有希望。 **IoTHubClient**库应该不会调用回调函数再次使用相同的消息。

-   **IOTHUBMESSAGE\_放弃**– 未成功地处理邮件，但**IoTHubClient**库应调用回调函数再次使用相同的消息。

对于前两个返回代码， **IoTHubClient**库发送消息到 IoT 集线器指出应从设备队列中删除并再次未送达邮件。 净效果是相同的 （从设备队列删除消息），但仍记录是否接受或拒绝该消息。  记录这种区别是对该邮件的发件人可以侦听的反馈，是否设备已接受或拒绝某个特定邮件了解有用。

在最后一种情况下消息还发送到 IoT 集线器，但则表示，应重新传送消息。 通常将放弃一条消息，如果遇到一些错误，但想要尝试再次处理该消息。 相反，拒绝消息是适当的遇到不可恢复的错误时 （或只是决定要不要处理该消息）。

在任何情况下，请注意不同的返回代码，以便您可以得出**IoTHubClient**库中所需的行为。

## <a name="alternate-device-credentials"></a>备用设备凭据

如先前所述，首先需要在**IoTHubClient**库的使用是获得**IOTHUB\_客户端\_处理**调用如下所示︰

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

参数与**IoTHubClient\_CreateFromConnectionString**是我们设备和一个参数，指示我们使用 IoT 中心与通信协议的连接字符串。 连接字符串具有如下格式︰

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

有四项在此字符串中的信息︰ IoT 中心名称、 IoT 集线器后缀、 设备 ID 和共享的访问键。 获取完全限定的域名称 (FQDN) IoT 集线器的 Azure 的门户网站中创建 IoT 集线器实例时 — — 这使您 IoT 中心名称 （FQDN 的第一部分） 以及 IoT 集线器后缀 （FQDN 的其余部分）。 注册您的设备具有 IoT 集线器 （如[以前的文章](iot-hub-device-sdk-c-intro.md)中所述） 时，获取设备 ID 和共享访问键。

**IoTHubClient\_CreateFromConnectionString**为您提供了一种方法来初始化库。 如果您愿意，您可以创建一个新**IOTHUB\_客户端\_处理**通过使用这些单个参数，而不是连接字符串。 这可以用下面的代码来实现︰

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

它能完成不同于**IoTHubClient\_CreateFromConnectionString**。

它看起来很明显，您可能需要使用**IoTHubClient\_CreateFromConnectionString**而不是初始化此详细方法。 但是，记住，IoT 中心注册设备时所获取的内容是设备 ID 和设备键 （而不是连接字符串）。 在[上一篇文章](iot-hub-device-sdk-c-intro.md)中引入了**设备管理器**SDK 工具使用**Azure IoT 服务 SDK**中的库设备 ID、 设备项和 IoT 中心主机名中创建连接字符串。 因此调用**IoTHubClient\_LL\_创建**可能更为可取，因为它保存生成的连接字符串的一步。 使用任何一种方法是非常方便的。

## <a name="configuration-options"></a>配置选项

到目前为止一切有关**IoTHubClient**库的工作的方式的说明反映了它的默认行为。 但是，有几个选项，您可以将更改库的工作原理。 这可通过利用**IoTHubClient\_LL\_SetOption** API。 请考虑以下示例︰

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

有几个常用的选项︰

-   **SetBatching**（布尔值） – 如果分批发送**，则返回 true**，则数据发送到 IoT 集线器。 如果是**false**，则邮件将单独发送。 默认值为**false**。 请注意， **SetBatching**选项仅适用于 HTTP 协议而不适用于 MQTT 或 AMQP 协议。

-   **超时**（无符号的整数） – 此值以毫秒为单位表示。 如果发送的 HTTP 请求和接收响应时间超过此时间，该连接将超时。

批处理选项非常重要。 默认情况下，库 ingresses 事件分别 (单个事件会传递给任何**IoTHubClient\_LL\_SendEventAsync**)。 批处理选项为**true**时，如果库收集尽可能多的事件会从缓冲区中 （直到 IoT 中心将接受的最大邮件大小）。  批事件发给 IoT 集线器中单个 HTTP 调用 （单个事件捆绑到 JSON 数组）。 启用批处理通常导致大的性能增益，因为您减少了网络往返。 由于您正在发送 HTTP 标头包含的事件批次的一套，而不一套邮件头，当每个单独的事件，它还可以显著降低带宽。 除非有特殊原因需要更改之外，通常您需要启用批处理。

## <a name="next-steps"></a>下一步行动

本文详细介绍了在**Azure IoT 设备 SDK 的 C** **IoTHubClient**库的行为。此信息，您应该有**IoTHubClient**库的功能更好地理解。 [下一篇文章中](iot-hub-device-sdk-c-serializer.md)提供的**序列化程序**库类似的详细信息。

有关 IoT 中心开发的详细信息，请参阅[IoT 集线器 Sdk][lnk-sdks]。

要进一步探索 IoT 中心的功能，请参阅︰

- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
