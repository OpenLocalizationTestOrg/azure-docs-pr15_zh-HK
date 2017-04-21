<properties
 pageTitle="开发人员指南-IoT 集线器 Sdk |Microsoft Azure"
 description="Azure IoT 中心开发指南 》 的信息和链接到各个 Azure IoT 中心设备和服务 Sdk。"
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>IoT 集线器 Sdk

## <a name="iot-hub-device-sdks"></a>IoT 集线器设备的 Sdk

Microsoft Azure IoT 设备 Sdk 包含代码，促进了建筑设备和应用程序将连接到并由 Azure IoT 中心服务。

下列 IoT 设备的 Sdk 是可以从 GitHub 下载︰

- [C 的 azure IoT 设备 SDK] [lnk-c-device-sdk]在 ANSI C 中 (C99) 编写可移植性和广泛的平台兼容性。
- [.NET 的 azure IoT 设备 SDK][lnk-dotnet-device-sdk]
- [Java 的 azure IoT 设备 SDK][lnk-java-device-sdk]
- [Azure IoT 设备 SDK 的 Node.js][lnk-node-device-sdk]
- [对于 Python 2.7 Microsoft Azure IoT 设备 SDK][lnk-python-device-sdk]

> [AZURE.NOTE] GitHub 资料库中自述文件，了解有关使用语言和特定于平台的包管理器在您的开发计算机上安装的二进制文件和依赖项的信息，请参阅。

## <a name="os-platforms-and-hardware-compatibility"></a>操作系统平台和硬件兼容性

有关与特定硬件设备的 SDK 兼容性的详细信息，请参阅[Azure 认证 IoT 设备目录][lnk-certified]。

## <a name="iot-hub-service-sdks"></a>IoT 中心服务 Sdk

Microsoft Azure IoT 服务 Sdk 包含代码，有利于构建交互的应用程序直接与 IoT 集线器设备和安全管理。

以下 IoT 服务 Sdk 可以用来从 GitHub 下载︰

- [.NET 的 azure IoT 服务 SDK][lnk-dotnet-service-sdk]
- [Azure IoT 服务 SDK 的 Node.js][lnk-node-service-sdk]
- [Java 的 azure IoT 服务 SDK][lnk-java-service-sdk]

> [AZURE.NOTE] GitHub 资料库中自述文件，了解有关使用语言和特定于平台的包管理器在您的开发计算机上安装的二进制文件和依赖项的信息，请参阅。

## <a name="azure-iot-gateway-sdk"></a>Azure IoT 网关 SDK

此 Azure IoT 网关 SDK 包含基础结构和模块创建 IoT 网关解决方案。 您可以扩展 SDK 来创建网关适合任何端到端方案。

您可以下载[Azure IoT 网关 SDK] [lnk-gateway-sdk]从 GitHub。

## <a name="online-api-reference-documentation"></a>在线的 API 参考文档

以下是 Azure IoT 设备、 服务和网关库联机 API 参考文档的链接的列表︰

- [(IoT).NET 物联网][lnk-dotnet-ref]
- [IoT 中心其余部分][lnk-rest-ref]
- [C 的 azure IoT 设备 SDK][lnk-c-ref]
- [Java 的 azure IoT 设备 SDK][lnk-java-ref]
- [Java 的 azure IoT 服务 SDK][lnk-java-service-ref]
- [Azure IoT 设备 SDK 的 Node.js][lnk-node-ref]
- [Azure IoT 服务 SDK 的 Node.js][lnk-node-service-ref]
- [Azure IoT 网关 SDK][lnk-gateway-ref]

## <a name="next-steps"></a>下一步行动

本 IoT 中心开发人员指南中的其他参考主题包括︰

- [IoT 集线器终结点][lnk-devguide-endpoints]
- [对于 twins、 方法和作业的查询语言][lnk-devguide-query]
- [配额和限制][lnk-devguide-quotas]
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md