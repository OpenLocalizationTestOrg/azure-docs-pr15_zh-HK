<properties
 pageTitle="开发人员指南 》 中的术语表 |Microsoft Azure"
 description="IoT 中心与相关的常用术语的术语表"
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

# <a name="glossary-of-iot-hub-terms"></a>IoT 集线器术语的词汇表

本文列出了一些常用的术语与 IoT 中心。

## <a name="advanced-message-queueing-protocol-amqp"></a>高级的消息队列协议 (AMQP)

[AMQP](https://www.amqp.org/)是消息传递 IoT 集线器支持用于与设备通信的协议之一。 有关 IoT 集线器支持的消息传递协议的详细信息，请参阅[发送和接收邮件使用 IoT 中枢](iot-hub-devguide-messaging.md)。

## <a name="cloud-to-device-c2d"></a>云至设备 (C2D)

通常用来指从 IoT 中心发送给已连接的设备的消息。 通常情况下，这些消息是指示设备采取某些行动的命令。 有关详细信息，请参阅[发送和接收邮件使用 IoT 中枢](iot-hub-devguide-messaging.md)。

## <a name="condition"></a>条件

所报告的设备应用程序是指设备状态信息，例如当前正在使用中，该连接方法。 设备还可以报告他们的能力。 您可以查询条件和使用设备两路输出的能力。

## <a name="data-point-message"></a>数据点消息

数据点消息是云至设备消息包含风速和温度的遥测数据。

## <a name="desired-properties"></a>所需的属性

在设备 twins 的上下文，所需的属性用于*报告属性*的结合同步设备配置或条件。 所需的属性只能设置由应用程序返回结束和观察到的设备应用程序。 

## <a name="device-to-cloud-d2c"></a>设备与云 (D2C)

通常用来指 IoT 集线器从已连接的设备发送的消息。 这些消息可能是[数据点](#data-point-message)或[交互式](#interactive-message)的消息。 有关详细信息，请参阅[发送和接收邮件使用 IoT 中枢](iot-hub-devguide-messaging.md)。

## <a name="device-identity-registry"></a>设备标识注册表

[设备标识注册表](iot-hub-devguide-identity-registry.md)是存储单个设备允许连接到集线器的信息 IoT 集线器的内置组件。

## <a name="device"></a>设备

在 IoT 上下文中，设备通常是小规模的独立计算设备，控制其他设备也收集数据。 例如一个设备可能是，环境监视设备或在温室中的浇水和通风系统的控制器。

## <a name="device-twin"></a>设备同步

[设备双子星](iot-hub-devguide-device-twins.md)是 IoT 集线器的物理设备的状态和配置设置的副本。 设备两路输出可用于管理物理设备的配置。

## <a name="direct-method"></a>直接方法

一种[直接的方法](iot-hub-devguide-direct-methods.md)是一种方法来触发方法通过调用 API IoT 集线器上的设备上执行。

## <a name="event-hub-compatible-endpoint"></a>事件中心兼容终结点

要读取设备到云消息发送到 IoT 中心，可以在网络集线器连接的终结点和使用事件中心兼容的任何方法来读取这些消息。 事件的中心兼容方法包括使用事件集线器 Sdk 和 Azure 流分析。

## <a name="field-gateway"></a>域的网关

一个域的网关使连接的设备不能直接连接到集线器 IoT 和通常部署本地与设备。 有关详细信息，请参阅[Azure IoT 中心是什么？](iot-hub-what-is-iot-hub.md)。

## <a name="job"></a>作业

您的解决方案后端可以使用作业来安排和跟踪活动上注册 IoT 集线器的设备一套。 活动包括更新设备所需的双子星属性、 更新设备双子星标记，以及调用直接方法。

## <a name="protocol-gateway"></a>协议网关

协议网关通常部署在云环境中，并提供到 IoT 集线器连接的设备的翻译服务的协议。 有关详细信息，请参阅[Azure IoT 中心是什么？](iot-hub-what-is-iot-hub.md)。

## <a name="interactive-message"></a>交互消息

交互消息是立刻触发一个操作的应用程序的后端云至设备信息。 例如，某个设备可能发送有关应自动登录到 CRM 系统的故障发出警报。

## <a name="iot-hub"></a>IoT 集线器

IoT 集线器是一完全托管的 Azure 服务，使数以百万计的 IoT 设备和解决方案后端之间的可靠和安全的双向通信。 有关详细信息，请参阅[Azure IoT 中心是什么？](iot-hub-what-is-iot-hub.md)。

## <a name="iot-suite"></a>IoT 套件

Azure IoT 套件一起打包多个 Azure 服务使用预配置的解决方案。 这些预配置的解决方案使您能够快速入门的端到端实现的常见 IoT 方案。 有关详细信息，请参阅[Azure IoT 套件是什么？](../iot-suite/iot-suite-overview.md)。

## <a name="job"></a>作业

IoT 中心中的[作业](iot-hub-devguide-jobs.md)，您可以执行操作，如固件升级跨多个设备连接到网络集线器。

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/)是消息传递 IoT 集线器支持用于与设备通信的协议之一。 有关 IoT 集线器支持的消息传递协议的详细信息，请参阅[发送和接收邮件使用 IoT 中枢](iot-hub-devguide-messaging.md)。

## <a name="reported-properties"></a>报告的属性

在设备 twins 的上下文中，报告的属性用于与*所需的属性*一起同步设备配置或条件。 报告的属性只可以通过设备应用程序设置，并可以读取和查询的应用程序的后端。

## <a name="tags"></a>标记

在 devcie twins 的上下文中，标记是设备元数据存储和检索应用程序的后端，JSON 文档的形式通过。 标记未显示在设备上的应用程序。

## <a name="system-properties"></a>系统属性

设备 twins 的上下文，在系统属性都是只读的并包含有关最后一个活动时间和连接状态等的设备使用情况的信息。