<properties
 pageTitle="IoT 中心高可用性和灾难恢复 |Microsoft Azure"
 description="描述有助于构建高度可用 IoT 解决方案与灾难恢复能力的功能。"
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT 中心具有高可用性和灾难恢复能力

作为一个 Azure 服务 IoT 集线器提供高可用性 (HA) 在 Azure 区域级别，使用冗余，而无需任何额外的工作所需的解决方案。 此外，Azure 还提供大量的帮助如果需要生成与灾难恢复 (DR) 功能或跨地区的可用性解决方案的功能。 您必须设计和准备利用这些灾难恢复功能，如果您想要提供全局、 跨区域的设备或用户的高可用性解决方案。 [Azure 的业务连续性技术指南](../resiliency/resiliency-technical-guidance.md)介绍了在 Azure 用于业务连续性和灾难恢复的内置功能。 [灾难恢复和高可用性的 Azure 应用程序][]的纸张 Azure 应用程序以实现高可用性和灾难恢复战略提供了体系结构指南。

## <a name="azure-iot-hub-dr"></a>Azure IoT 中心灾难恢复
除了内地区 HA IoT 中心实现故障转移机制进行灾难恢复要求无需用户干预。 IoT 中心灾难恢复自启动和恢复时间目标 (RTO) 的 2-26 个小时，和下面的恢复点目标 (Rpo)。

| 功能 | RPO |
| ------------- | --- |
| 注册表和通信运营服务可用性 | CName 丢失 |
| 设备标识注册表中标识数据 | 0-5 分钟的数据丢失 |
| 设备与云的消息 | 将丢失所有未读的邮件 |
| 监视消息的操作 | 将丢失所有未读的邮件 |
| 云至设备信息 | 0-5 分钟的数据丢失 |
| 到设备云反馈队列 | 将丢失所有未读的邮件 |

## <a name="regional-failover-with-iot-hub"></a>IoT 中枢区域故障转移

超出范围的这篇文章，但目的是高可用性和灾难恢复，我们将考虑*区域故障转移*部署模型是完整地描述 IoT 解决方案中的部署拓扑结构。

在区域的故障切换模式中，解决方案后端运行在一个数据中心位置，主要是但 IoT 集线器和后端部署在故障转移的目的，另一个数据中心位置 IoT 中心主数据中心中发生停机或从设备到主数据中心的网络连接由于某种原因被中断的情况下。 每当不能到达主网关设备都使用辅助服务终结点。 具有跨区域故障转移能力，超出单个区域的高可用性，可以提高解决方案的可用性。

从较高层面，要 IoT 中枢实行区域故障转移模型，您将需要以下。

* **辅助 IoT 集线器并且设备路由逻辑**︰ 如果您的主区域中的服务中断，设备必须开始连接到您的辅助区域。 由于所涉及的大多数服务的状态感知特性，很常见的解决方案管理员可以触发间区域故障转移过程。 同时控制过程中，通信设备、 新的终结点的最好办法是让他们定期检查当前有效的终结点的*接待*服务。 接待服务可以是简单的 web 应用程序的复制并保持其可访问 （例如，使用[Azure 流量管理器][]） 使用 DNS 重定向技术。
* **标识注册表复制**--才能用，辅助 IoT 中心必须包含可以连接到解决方案中的所有设备标识。 该解决方案应保留的设备标识的 geo 复制备份并切换设备的当前终结点之前将它们上载到辅助 IoT 集线器。 IoT 集线器的设备标识导出功能将在此上下文中非常有用。 有关详细信息，请参阅[IoT 中心开发指南标识注册表][]。
* 所有必须迁移的状态和已创建辅助站点中的数据**合并逻辑**的主要地区再次可用时，回到主要的地区。 这主要是与设备身份和应用程序元数据，它必须与主 IoT 中心和主要区域中的所有其他应用程序特定商店合并。 若要简化这一步，通常建议使用幂等操作。 这将大大减少负面影响的事件，最终一致分布不仅从重复区域或无序传递事件。 此外，应用程序逻辑应旨在容许潜在的不一致或"略有"日期状态中。 这是由于要"修复"的系统花费的额外时间根据所需的恢复点目标 (RPO)。

## <a name="next-steps"></a>下一步行动

按照这些链接以了解有关 Azure IoT 中心︰

- [开始使用 IoT 集线器 （教程）][lnk-get-started]
- [什么是 Azure IoT 集线器？][]

[灾难恢复和 Azure 应用程序的高可用性]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure 的流量管理器]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT 中心开发人员指南-标识注册表]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[什么是 Azure IoT 集线器？]: iot-hub-what-is-iot-hub.md
