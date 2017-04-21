<properties
 pageTitle="开发人员指南主题 IoT 集线器 |Microsoft Azure"
 description="Azure IoT 中心开发者指南，包括 IoT 中心端点、 安全、 设备标识注册表、 设备管理和消息"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT 中心开发指南

Azure IoT 集线器是完全托管的服务，可帮助使数以百万计的 IoT 设备之间的可靠和安全双向通信和应用程序后结束。

Azure IoT 中心为您提供︰

* 安全通信使用每个设备的安全凭据和访问控制。
* 可靠设备到云，云至设备超大规模的消息传送。
* 简单设备与设备库为最受欢迎的语言和平台的连接性。

此 IoT 中心开发指南包括了下列文章︰

- [发送和接收邮件使用 IoT 中枢][devguide-messaging]描述 IoT 中心公开的消息传递功能 （设备到云中和云至设备）。 文章还包括主题，如消息格式有关的信息和支持的通信协议以及他们使用的端口号。
- [将文件从设备上传][devguide-upload]介绍了如何将从设备的文件上载。 文章还包括 uplaod 流程可以发送有关通知等主题的信息。
- [管理设备标识 IoT 中心][devguide-identities]描述了每个 IoT 中枢设备标识注册表存储的信息，以及如何访问并对其进行修改。
- [控制访问 IoT 中心][devguide-security]描述用来授予访问 IoT 集线器功能这两个设备和云组件的安全模型。 文章介绍了有关使用标记和 X.509 证书，您可以授予的权限的详细信息。
- [使用设备 twins 同步状态和配置][devguide-device-twins]描述*设备孪生*概念和它公开如其双子星与同步设备的功能。 文章包括信息设备双子星中存储的数据。
- [在设备上的直接方法调用][devguide-directmethods]描述的直接方法，了解如何调用在后端应用程序的设备上的方法并处理您的设备上的直接方法的生命周期。
- [计划在多个设备上的作业][devguide-jobs]介绍了如何可以安排多个设备上的作业。 本文介绍如何将提交执行任务执行直接的方法，使用 devcie twin devcie 更新的作业。 它还介绍了如何查询作业的状态。
- [引用-IoT 集线器终结点][devguide-endpoints]描述各个端点的每个 IoT 中枢公开运行时和管理操作。 本文还介绍如何使用字段网关以启用某些设备连接到集线器 IoT 端点。
- [引用-twins、 方法和作业的查询语言][devguide-query]描述使有关设备 twins 和作业网络集线器从检索信息的查询语言。
- [引用的配额和限制][devguide-quotas]总结了 IoT 中心服务和您可能会看到当您超过配额限制行为设置配额。
- [引用的设备和服务 Sdk] [devguide-sdks]列表可以使用 Sdk 开发设备和服务交互的应用程序使用 IoT 中枢。 文章包括在线的 API 文档的链接。
- [引用-IoT 集线器 MQTT 支持][devguide-mqtt]提供了有关如何 IoT 集线器支持的 MQTT 协议的详细的信息。 文章描述内置到 Sdk 的 MQTT 协议的支持，并提供直接使用的 MQTT 协议信息。
- [术语表][devguide-glossary]常用 IoT 中心相关的术语的列表。



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

