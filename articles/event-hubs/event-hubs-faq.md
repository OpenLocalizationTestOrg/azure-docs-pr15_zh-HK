<properties 
    pageTitle="事件集线器常见问题解答 (FAQ) |Microsoft Azure"
    description="事件集线器的常见问题解答。"
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>事件集线器的常见问题解答

事件集线器提供大规模的进气口、 持久性和高吞吐量的数据源中的数据事件的处理和/或数以百万计的设备。 当配合服务总线队列和主题，事件集线器启用[Internet 内容 (IoT)](https://azure.microsoft.com/services/iot-hub/)方案持久性命令和控制的部署。

本文讨论定价信息，并回答了一些有关事件集线器的常见问题︰

## <a name="pricing-information"></a>定价信息

有关事件集线器定价的完整信息，请参阅[事件集线器定价的详细信息](https://azure.microsoft.com/pricing/details/event-hubs/)。

## <a name="how-are-event-hubs-ingress-events-calculated"></a>事件集线器入口事件是如何计算的？

每个事件发送给事件中心可以算是收费的消息。 *入站事件*被定义为小于或等于 64 KB 的数据单位。 小于或等于 64 KB 大小的任何事件被认为是一个收费的事件。 如果大于 64 KB 的事件，根据事件的大小为 64 KB 的倍数计算收费事件数。 例如，8 KB 事件发送到事件中心记帐为一个事件，但 96 KB 的邮件发送到该事件的中心两个事件作为记帐。

事件从事件中心，作为消耗以及管理操作和控件调用，如检查点、 不算作收费入口事件，但累到吞吐量单位补助。

## <a name="what-are-event-hubs-throughput-units"></a>事件集线器吞吐量单位有哪些？

明确选择事件集线器吞吐量单位，通过 Azure 门户或事件集线器资源管理器模板。 吞吐量单位适用于事件集线器命名空间中的所有事件集线器和吞吐量的每个单位就没有命名空间的以下功能︰

- 上移到 1 MB 每秒的入站事件 （事件发送到事件中心），但没有超过 1000年的入站事件、 管理操作或控件 API 调用每秒。

- 最多 2 MB / 秒的出站事件 （事件从事件集线器消耗）。

- 84 GB 的事件存储 （足够默认 24 小时保留期间）。

根据在给定时间内所选的单位数小时，计费事件集线器吞吐量单位。

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>如何强制事件集线器吞吐量单位限制？

如果总入站吞吐量或在命名空间中的所有事件集线器之间总入口事件速率超过了聚合吞吐量单位补助，发件人将控制和接收错误指示超过了入口配额。

如果总出口吞吐量或在命名空间中的所有事件集线器之间的事件总出口速率超过了聚合吞吐量单位经费，接收器被限制和接收错误指示超过了出口配额。 这样，没有发件人可能会导致事件消耗降低，也可以接收方阻止事件发送到事件中心分别实施入口和出口配额。

请注意，吞吐量单位选择独立的事件集线器的分区数。 而每个分区提供 1 MB （最大值 1000年每秒钟的事件），第二个入口和第二个出口每 2 MB 的最大吞吐量，自己的分区没有固定的费用。 费用是用于事件集线器命名空间中的所有事件集线器上的聚合的吞吐量单位。 使用此模式，您可以创建足够的分区，以支持他们的系统，预计最大负荷不会招致任何吞吐量的单位费用，直到系统事件的负载实际要求较高的吞吐量值，而无需更改的结构和系统的体系结构与系统上的负载增加。

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>有没有可以选择的吞吐量单位数限制？

没有默认的配额是每个名称空间的 20 个吞吐量单位。 通过建档的支持票，可以请求更大的吞吐量单位配额。 超过 20 吞吐量单位限制，捆绑有 20 到 100 吞吐量单位。 请注意，使用 20 多个吞吐量单位删除不建档的支持票的情况下更改吞吐量单位数的能力。

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>有保留 24 小时以上的事件集线器事件的费用吗？

事件集线器标准层允许邮件保留时间超过 24 小时，最多 30 天的期间。 如果存储事件总数的大小超过选定的吞吐量单位 (每单位吞吐量 84 GB) 的可用存储容量，大小超过补助负责发布 Azure Blob 存储速率。 每个吞吐量单元中的存储补助涵盖所有存储成本保留期内的 24 小时内 （默认值） 即使吞吐量单位最大的入口补助到用完。

## <a name="what-is-the-maximum-retention-period"></a>什么是最大保持期？

事件集线器标准层目前支持的最大保留期为 7 天。 注意事件集线器不用作永久数据存储区。 保留期间大于 24 小时供的方案，它是方便地将一个事件流重放到同一个系统中;例如，若要进行训练或验证新的机器学习模型上的现有数据。

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>事件集线器存储大小计算和收取的如何？

总大小的所有存储的事件，包括事件头或磁盘存储结构上任何内部开销中所有事件集线器，被指在一天。 在一天结束时，将计算峰值存储大小。 根据所选择的那一天 （每个吞吐量单位提供 84 GB 的补助） 的吞吐量单位的最小数目计算每日存储津贴。 如果总大小超过了计算每天可用存储容量，使用 Azure Blob 存储率 （按**本地的冗余存储**速率） 记帐时间过多的存储。

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>可以使用单个 AMQP 连接来发送和接收事件集线器和服务总线队列/主题？

是的只要所有事件集线器、 队列和主题都都在同一个命名空间中。 在这种情况下，您可以经济高效和高可扩展性的方式实现双向的、 具有连接到很多设备，而延迟。

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>具有的连接费用不应用于事件集线器？

对于发件人，只在使用 AMQP 协议时将应用连接费用。 没有发送使用 HTTP，无论有多少个系统或设备发送事件的连接费用。 如果您计划使用 AMQP （例如，以实现更高效的事件流，或使 IoT 命令和控制方案中的双向通讯），请参阅关于什么构成一个具有的连接和它们如何计量信息的[服务总线定价信息](https://azure.microsoft.com/pricing/details/service-bus/)页。

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>事件集线器基本和标准层之间的区别是什么？

事件集线器标准层提供远超事件集线器基本和一些竞争对手的系统中可用的功能。 这些功能包括超过 24 小时，并且能够使用单个 AMQP 连接将命令发送到数量庞大的设备与存取时间延迟，以及从这些设备的遥测发送到事件集线器的保留期。 功能列表，请参阅[事件集线器定价的详细信息](https://azure.microsoft.com/pricing/details/event-hubs/)。

## <a name="geographic-availability"></a>地理的可用性

事件集线器位于以下地区︰

|地区|地区|
|---|---|
|美国|美国中部、 东部美国、 东亚美国 2、 美国中南部，西美国|
|欧洲|北欧西欧|
|亚太|东亚东南亚|
|日本|日本东，西日本地区|
|巴西|巴西南部|
|澳大利亚|澳大利亚东部，澳大利亚东南部|

## <a name="support-and-sla"></a>支持和 SLA

通过[社区论坛](https://social.msdn.microsoft.com/forums/azure/home)提供了事件集线器的技术支持。 免费提供记帐和订阅管理支持。

若要了解有关我们的 SLA 的详细信息，请参阅[服务级别协议](https://azure.microsoft.com/support/legal/sla/)页。

## <a name="next-steps"></a>下一步行动

若要了解有关事件集线器的详细信息，请参阅下列文章︰

- [事件集线器概述][]。
- 完整的[示例应用程序，它使用事件集线器][]。

[事件集线器概述]: event-hubs-overview.md
[示例应用程序使用事件集线器]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
