<properties
     pageTitle="使用 Azure 的门户配置文件上载 |Microsoft Azure"
     description="简要介绍了如何将配置文件上载使用 Azure 门户"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>配置文件上传使用 Azure 门户

## <a name="file-upload"></a>文件上载

使用[文件上载功能 IoT 中心][lnk-upload]，您必须先将 Azure 存储帐户关联与网络集线器。 选择要显示的 IoT 集线器被修改的文件上载属性列表的**文件上载**设置。

![][13]

**存储容器**︰ 使用 Azure 的门户网站中您当前的 Azure 订阅 IoT 中心相关联的 Azure 存储帐户选择一个 blob 容器。 如有必要，您可以在**容器**刀片式服务器上的**存储帐户**刀片和 blob 容器创建 Azure 存储帐户。 IoT 集线器与此 blob 容器上载文件时要使用的设备的写入权限将自动生成 SAS 的 Uri。

![][14]

**上载的文件的接收通知**︰ 启用或禁用通过切换文件上载通知。

**SAS TTL**︰ 此设置是--生存时间的 SAS Uri 返回到设备 IoT 集线器。 默认情况下设置为一小时，但可以自定义为其他值使用的滑块。

**设置默认 TTL 文件通知**︰-生存时间的文件上载通知之前，它已过期。 默认设置为一天，但可以自定义为其他值使用的滑块。

**文件通知传递的最大数量**︰ 数次 IoT 中心尝试传递文件上载通知。 默认设置为 10，但可以自定义为其他值使用的滑块。

![][15]

## <a name="next-steps"></a>下一步行动

IoT 中心的文件上载功能的详细信息，请参阅[将文件从设备上传][lnk-upload]的开发人员指南中。

按照这些链接以了解更多关于管理 Azure IoT 中心︰

- [批量管理 IoT 设备][lnk-bulk]
- [使用情况指标][lnk-metrics]
- [操作监视][lnk-monitor]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]
- [设置安全 IoT 解决方案从零开始][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md