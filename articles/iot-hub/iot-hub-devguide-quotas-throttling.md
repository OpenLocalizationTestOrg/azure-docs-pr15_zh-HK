<properties
 pageTitle="开发人员指南 》 的配额和限制 |Microsoft Azure"
 description="Azure IoT 中心开发人员指南-配额应用于 IoT 集线器和预期的调整行为的描述"
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

# <a name="reference---quotas-and-throttling"></a>引用的配额和限制

## <a name="quotas-and-throttling"></a>配额和限制

每个 Azure 订阅可以有最多 10 个 IoT 集线器。

每个 IoT 中心配有一定数量的特定 SKU 中单位 (有关详细信息，请参阅[Azure IoT 集线器定价][lnk-pricing])。 SKU 和单位数确定可以发送邮件的最大每日的配额。

SKU 还决定限制 IoT 中心强制执行所有操作的限制。

## <a name="operation-throttles"></a>操作限制

操作限制为从速度限制在每分钟范围应用，旨在避免滥用。 IoT 中心试图避免返回错误，只要有可能，但是它开始返回例外，如果与限制冲突的时间太长。

以下是强制限制的列表。 值是指单独的中心。

| 限制 | 每个中心值 |
| -------- | ------------- |
| 标识注册表操作 （创建、 检索、 列表、 更新、 删除） | 5000/min/单位 （S3) <br/> 100/min/单位 （S1 和 S2）。 |
| 设备连接 | 6000/秒/单位 （S3)，120/秒/单位 （S2)，12/秒/单位 （S1)。 <br/>100 秒的最小值。 <br/> 例如，S1 的两个单元的 2\*12 = 24/秒，但您有至少 100/秒跨您的单位。 有九个 S1 单位，有 108/秒 (9\*12) 在您的单位。 |
| 发送设备到云 | 6000/秒/单位 （S3)，120/秒/单位 （S2)，12/秒/单位 （S1)。 <br/>100 秒的最小值。 <br/> 例如，S1 的两个单元的 2\*12 = 24/秒，但您有至少 100/秒跨您的单位。 有九个 S1 单位，有 108/秒 (9\*12) 在您的单位。 |
| 云向设备发送 | 5000/min/单位 （S3)，100/min/单位 （S1 和 S2）。 |
| 云对设备接收 | 50000/min/单位 （S3)，1000年/min/单位 （S1 和 S2）。 |
| 文件上载操作 | 5000 文件上载 （用于 S1 和 S2） 通知/min/单位 （S3)、 100 的文件上载通知/min/单位。 <br/> 10000 SAS Uri 可以出 Azure 存储帐户一次。<br/> 10 个 SAS Uri/设备可出一次。 | 

请务必明确*设备连接*油门控制的新设备连接建立 IoT 集线器，并不同时连接的设备的最大数量的比率。 油门取决于为中心调配的单位数。

例如，如果您购买一个 S1 单元，可以每秒 100 个连接限制。 这意味着，若要将 100000 设备连接，花费至少 1000 秒 （大约 16 分钟）。 但是，您可以有多个同时连接的设备与您拥有的设备设备标识注册表中注册。

IoT 中心深入讨论限制的行为，请参阅博客文章[IoT 中枢调节和您][lnk-throttle-blog]。

>[AZURE.NOTE] 在任何给定时间，则可能要增加配额或限制限制增加 IoT 中心调配的单位数。

>[AZURE.IMPORTANT] 标识注册表操作供运行时使用在设备管理和资源调配方案。 通过[导入和导出作业]支持读取或更新大量的设备标识[lnk-importexport]。

## <a name="next-steps"></a>下一步行动

本 IoT 中心开发人员指南中的其他参考主题包括︰

- [IoT 集线器终结点][lnk-devguide-endpoints]
- [对于 twins、 方法和作业的查询语言][lnk-devguide-query]
- [IoT 集线器 MQTT 支持][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md