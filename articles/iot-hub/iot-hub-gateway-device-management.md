<properties
 pageTitle="启用 IoT 网关后的受控的设备 |Microsoft Azure"
 description="使用 IoT 网关的指导主题创建使用 IoT 网关 SDK 以及管理 IoT 集线器的设备。"
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>启用 IoT 网关后的受控的设备

## <a name="basic-device-isolation"></a>基本设备隔离

组织通常使用 IoT 网关来增加其 IoT 解决方案的总体安全性。 某些设备需要将数据发送到云中，但都不能保护自己免受 internet 上的威胁。 可以通过将它们与外部世界通过网关进行通信屏蔽来自外部线程这些设备。

网关位于一个安全的环境和开放的互联网之间的边框。 设备与网关，网关会沿将邮件传递到正确的云的目标。 网关对抗外部线程、 阻止未经授权的请求、 允许授权的入站通信，并转发到正确的设备的入站通信。

![][1]

您也可以将保护自己提供一层额外的安全的网关后面的设备。 在这种情况下只需保持 OS 修补对最新的漏洞，而不是更新的操作系统上的每个设备的网关。

## <a name="isolation-plus-intelligence"></a>隔离，再加上情报

加强的路由器是足够的网关，只是找出设备。 但是，IoT 解决方案通常要求一个网关，提供比简单地隔离设备的更多情报。 例如，您可能需要从云管理设备。 现在可以使用[LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki)，一种标准设备管理协议，云管理部分的解决方案。 但是，设备发送遥测使用 TCP/IP 的非已启用的协议。 此外，设备会生成大量的数据并且只想要上载的遥测子集筛选。 您可以构建一个包含了与两个不同的数据流的处理能够 IoT 网关的解决方案。 网关应︰

-   了解**遥测**、 筛选，并再将其上载到通过网关云。 网关不再是一个简单的路由器，只是转发设备和云之间的数据。

-   只是交换设备和云之间的**LWM2M 设备管理数据**。 网关不需要了解数据进入它，并仅需要确保数据获取设备和云之间来回传递。

下图阐释了此方案︰

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>解决方案︰ Azure IoT 设备管理和 IoT 网关 SDK 

公共预览版本的[Azure IoT 设备管理][lnk-device-management]和[Azure IoT 网关 SDK] beta 版支持此方案。 网关处理每个的数据流，如下所示︰

-   **遥测**︰ 您可以使用 IoT 网关 SDK 生成管线，识别、 筛选和将遥测数据发送到云中。 IoT 网关 SDK 提供了实现此管道代替开发人员部分的代码。 您可以[IoT 网关 SDK-入门]中发现 SDK 的体系结构的详细信息[lnk-gateway-get-started]教程。

-   **设备管理**︰ Azure 设备管理提供了设备，同时发送至设备的管理命令的云接口运行的 LWM2M 客户端。
    
    因为它不需要处理设备和 IoT 集线器之间交换的 LWM2M 数据，不需要任何特殊的逻辑上的网关。 您可以启用 internet 连接共享，许多现代操作系统中，网关来实现的 LWM2M 数据交换的功能。 您可以选择合适的操作系统，这种情况下，因为 IoT 网关 SDK 支持多种操作系统。 以下是操作系统的启用 internet 连接共享[Windows 10]和[Ubuntu]，两个多个支持上的说明。

下面的插图显示用来使这种情况下，使用[Azure IoT 设备管理]的高层次体系结构[lnk-device-management]和[Azure IoT 网关 SDK]。

![][3]

## <a name="next-steps"></a>下一步行动

若要了解有关如何使用 IoT 网关 SDK，请参阅下面的教程︰

- [开始使用 Linux IoT 网关 SDK-][lnk-gateway-get-started]
- [IoT 网关 SDK-与模拟设备使用 Linux 的发送设备与云消息][lnk-gateway-simulated]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure IoT 网关 SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md