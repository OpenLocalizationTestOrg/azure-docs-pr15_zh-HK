<properties
 pageTitle="开发人员指南 》 的文件上传 |Microsoft Azure"
 description="Azure IoT 中心开发指南 》 的上传文件从设备到 IoT 集线器"
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

# <a name="upload-files-from-a-device"></a>从设备的文件上载

## <a name="overview"></a>概述

详见[IoT 集线器终结点][lnk-endpoints]的文章中，设备可以启动文件上载，通过发送一个通知通过面向设备的终结点 (**/devices/ {deviceId} 文件 /**)。  当设备通知 IoT 集线器的上载完成时，IoT 中心生成文件上载通知，您可以以消息的形式接收通过面向服务的终结点 (**/messages/servicebound/filenotifications**)。

而不是通过自身 IoT 集线器的经纪消息，IoT 中心改为作为与关联的 Azure 存储帐户的调度。 设备请求 IoT 中心，它是特定于该设备想要上载的文件的存储标记。 该设备使用 SAS URI 将文件上载到存储，并上载完成时，设备会发送完成通知 IoT 集线器。 IoT 中心验证已上载该文件，然后将文件上载通知添加到新的面向服务的文件通知消息的终结点。

从设备的文件上载到 IoT 中心之前，您必须配置您的中枢通过[将 Azure 存储相关联][lnk-associate-storage]到该帐户。

您的设备可以[初始化上载][ lnk-initialize] ，然后[通知 IoT 中心][lnk-notify]上载完成时。 （可选） 当设备通知 IoT 集线器上载完毕，该服务可以生成[通知消息][lnk-service-notification]。

### <a name="when-to-use"></a>何时使用

当您需要将文件从设备到您的后端服务，而不是通过 IoT 中心发送消息时，请使用此 IoT 中心功能。

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>IoT 中心相关联的 Azure 存储帐户

若要使用的文件上载功能，您必须先链接到 IoT 集线器 Azure 存储帐户。 你可以通过[Azure 的门户网站][lnk-management-portal]，或通过[IoT 中心资源提供程序 REST Api]以编程方式[lnk-resource-provider-apis]。 一旦您 Azure 存储帐户关联 IoT 集线器，服务会返回 SAS URI 到设备设备初始化文件上载请求。

> [AZURE.NOTE] [Azure IoT 集线器 Sdk] [lnk-sdks]自动处理检索 SAS URI、 上传文件，并通知 IoT 中心已完成上载。

## <a name="initialize-a-file-upload"></a>初始化文件上载

IoT 中心有专门设备请求要上载的文件存储的 SAS URI 的终结点。 设备启动文件上载过程通过向 IoT 中心发送公告`{iot hub}.azure-devices.net/devices/{deviceId}/files`以下的 JSON 正文︰

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT 中心返回以下设备用来上载文件︰

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>不建议使用︰ 初始化文件上载与获取

> [AZURE.NOTE] 本部分介绍如何从 IoT 集线器接收 SAS URI 的对立的功能。 请使用上面所述的 POST 方法。

IoT 中心有两个其他终结点来支持文件上传，一个用于获取用于存储，另一个 SAS URI，通知 IoT 中心已完成上载。 设备启动文件上载过程通过向 IoT 中心发送 GET `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`。 中心对要上载的文件和相关性 ID 用于完成上载后返回 SAS URI 特定。

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>通知已完成的文件上载 IoT 集线器

该设备负责将文件上载到存储使用 Azure 存储 Sdk。 完成上载后，设备会发送 POST 到 IoT 集线器`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications`以下的 JSON 正文︰

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

值`isSuccess`是一个 boolean 类型的值，表示是否已成功上载文件。 状态代码为`statusCode`是到存储，文件上载的状态和`statusDescription`对应于`statusCode`。

## <a name="reference-topics"></a>参考主题︰

下列参考主题为您提供了有关上传文件从设备的详细信息。

## <a name="file-upload-notifications"></a>文件上载通知

当设备上载的文件并通知 IoT 集线器的上载完成时，该服务还可生成一条通知消息，其中包含的文件的文件名和存储位置。

[终结点]所述[lnk-endpoints]，IoT 集线器可提供通过面向服务的终结点 (**/messages/servicebound/fileuploadnotifications**) 作为邮件发送的文件上载通知。 文件上载通知的接收语义云至设备信息一样，具有相同[消息生命周期][lnk-lifecycle]。 从文件上载通知端点检索每个消息是带有以下属性的 JSON 记录︰

| 属性 | 说明 |
| -------- | ----------- |
| EnqueuedTimeUtc | 指示创建通知时的时间戳。 |
| DeviceId | **DeviceId**其上载文件的设备。 |
| BlobUri | 上载的文件的 URI。 |
| BlobName | 上载的文件的名称。 |
| LastUpdatedTime | 指示上次更新该文件的时间戳。 |
| BlobSizeInBytes | 上载文件的大小。 |

本**示例**。 这是示例邮件正文的文件上载通知。

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>文件上载通知配置选项

每个 IoT 中枢公开文件上载通知以下配置选项︰

| 属性 | 说明 | 范围和默认值 |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | 控制是否将文件上载通知写入文件通知终结点。 | 布尔值。 默认值︰ True。 |
| **fileNotifications.ttlAsIso8601** | 文件上载通知的默认 TTL 值。 | ISO_8601 的间隔最多 48 H （最小值为 1 分钟）。 默认值︰ 1 小时。 |
| **fileNotifications.lockDuration** | 文件上载通知队列锁持续时间。 | 5 到 300 秒 （最短的 5 秒）。 默认值︰ 60 秒。 |
| **fileNotifications.maxDeliveryCount** | 最大传送计数的文件上载通知队列。 | 1 到 100。 默认值︰ 100。 |

## <a name="additional-reference-material"></a>其他参考资料

其他开发人员指南中的参考主题包含︰

- [终结点 IoT 中心][lnk-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。
- [带宽限制和配额][lnk-quotas]描述了适用于 IoT 中心服务和需要使用服务的限制行为的配额。
- [IoT 中心设备和服务 Sdk] [lnk-sdks]列出了各种语言的 Sdk 您开发设备和服务交互的应用程序具有 IoT 集线器时使用。
- [Twins、 方法和作业的查询语言][lnk-query]描述了您可以使用有关设备 twins、 方法和作业 IoT 集线器从检索信息的查询语言。
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]提供 MQTT 协议有关 IoT 集线器支持的详细信息。

## <a name="next-steps"></a>下一步行动

现在您已了解如何上载使用 IoT 集线器的设备中的文件，您可以关注在下列主题中开发人员指南 》:

- [管理设备标识 IoT 中心][lnk-devguide-identities]
- [控制访问 IoT 集线器][lnk-devguide-security]
- [使用设备 twins 同步状态和配置][lnk-devguide-device-twins]
- [在设备上的直接方法调用][lnk-devguide-directmethods]
- [多台设备上的计划作业][lnk-devguide-jobs]

如果您想要尝试一些本文中介绍的概念，您可能会感兴趣以下 IoT 集线器教程︰

- [如何将文件从设备到 IoT 中枢云上载][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
