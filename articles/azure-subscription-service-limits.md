<properties
    pageTitle="订阅 Microsoft Azure 和服务限制、 配额和约束"
    description="提供常见的 Azure 订阅和服务限制、 配额和约束的列表。 这包括如何增加最大值限制的信息。"
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure 订阅和服务限制、 配额和约束

## <a name="overview"></a>概述

此文档指定一些最常见的 Microsoft Azure 限制。 这当前不包括所有的 Azure 服务。 随着时间的推移将扩展和更新，包含了多个平台的这些限制。

请[Azure 定价概述](https://azure.microsoft.com/pricing/)以了解有关 Azure 定价的详细信息，访问。 那里，您可以估计使用[价格计算器](https://azure.microsoft.com/pricing/calculator/)所需的成本或通过访问服务 (例如， [Windows 虚拟机](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)) 的定价详细信息页面。

> [AZURE.NOTE] 如果您想要提升高于**默认限制**的限制，则可以[打开免费在线客户支持请求](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)。 不能限制提升上面下面的表中的**最大限制**值。 如果没有**最大限制**列，然后指定的资源没有可调的限制。

## <a name="limits-and-the-azure-resource-manager"></a>限制和 Azure 的资源管理器

现可以组合到单个的 Azure 资源组中的多个 Azure 资源。 在使用资源组时，过去全球的限制成为托管在地区级别使用 Azure 资源管理器。 关于 Azure 资源组的详细信息，请参阅[Azure 资源管理器的概述](azure-resource-manager/resource-group-overview.md)。

在下面的限制，已添加一个新表以反映限制任何差异时使用 Azure 资源管理器。 例如，没有一个**订阅数限制**和**订阅数限制的 Azure 资源管理器**表。 当限制仅适用于这两种情形中时，它仅显示第一个表中。 除非另有说明，所有区域间是全局限制。

> [AZURE.NOTE] 值得强调的 Azure 资源组的资源配额是每个地区访问您的订阅，并不是每个订阅服务管理配额进行。 举一个例子，让我们使用核心配额。 如果您需要申请配额增加对内核的支持，您需要决定您想要使用哪个地区，并将 Azure 资源组核心配额的特定请求的金额和所需的区域的核心数量。 因此，如果您需要使用 30 西欧核心运行您的应用程序控制。特别是应请求 30 西部欧洲的核心。 但您将不能在任何其他区域中增加核心配额--只有西欧将有 30-核心配额。
<!-- -->
结果，可能会发现有用考虑决定需要将任何一个地区的工作负载的 Azure 资源组配额和请求部署考虑到其中每个区域中的量。 请参阅更多的帮助，发现当前配额的特定地区[部署问题进行故障排除](./resource-manager-common-deployment-errors.md)。

## <a name="service-specific-limits"></a>特定于服务的限制

- [活动目录](#active-directory-limits)
- [API 管理](#api-management-limits)
- [应用程序服务](#app-service-limits)
- [应用程序网关](#application-gateway-limits)
- [应用程序的见解](#application-insights-limits)
- [自动化](#automation-limits)
- [Azure 的 Redis 高速缓存](#azure-redis-cache-limits)
- [Azure 的远程应用程序](#azure-remoteapp-limits)
- [备份](#backup-limits)
- [批处理](#batch-limits)
- [BizTalk 服务](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [云服务](#cloud-services-limits)
- [数据工厂](#data-factory-limits)
- [数据湖分析](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [事件集线器](#event-hubs-limits)
- [IoT 集线器](#iot-hub-limits)
- [密钥存储库](#key-vault-limits)
- [介质服务](#media-services-limits)
- [移动服务](#mobile-engagement-limits)
- [移动服务](#mobile-services-limits)
- [监视](#monitoring-limits)
- [多因素身份验证](#multi-factor-authentication)
- [网络连接](#networking-limits)
- [通知中心服务](#notification-hub-service-limits)
- [操作建议](#operational-insights-limits)
- [资源组](#resource-group-limits)
- [计划程序](#scheduler-limits)
- [搜索](#search-limits)
- [服务总线](#service-bus-limits)
- [网站恢复](#site-recovery-limits)
- [SQL 数据库](#sql-database-limits)
- [存储](#storage-limits)
- [StorSimple 系统](#storsimple-system-limits)
- [流分析](#stream-analytics-limits)
- [订阅](#subscription-limits)
- [通信管理器](#traffic-manager-limits)
- [虚拟机](#virtual-machines-limits)
- [虚拟机比例设置](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>订阅数限制
#### <a name="subscription-limits"></a>订阅数限制
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>订阅数限制的 Azure 资源管理器

使用 Azure 资源管理器和 Azure 资源组时，将应用以下限制。 没有改变使用 Azure 资源管理器中的限制，下面未列出。 请参阅上表的那些限制。

有关处理资源管理器请求的限制的信息，请参阅[调节资源管理器的请求](resource-manager-request-limits.md)。

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>资源组的限制

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>虚拟机限制
#### <a name="virtual-machine-limits"></a>虚拟机限制
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>虚拟机限制的 Azure 资源管理器

使用 Azure 资源管理器和 Azure 资源组时，将应用以下限制。 没有改变使用 Azure 资源管理器中的限制，下面未列出。 请参阅上表的那些限制。

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>虚拟机的比例设置限制

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>网络限制

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>网络限制
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>应用程序网关限制

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>通信管理器限制

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS 限制

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>存储限制

帐户存储限制的详细信息，请参阅[Azure 存储可扩展性和性能目标](../articles/storage/storage-scalability-targets.md)。

#### <a name="storage-service-limits"></a>存储服务限制

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>虚拟机磁盘空间限制

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

有关其他详细信息，请参阅[虚拟机大小](../articles/virtual-machines/virtual-machines-linux-sizes.md)。

**标准的存储帐户**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**高级存储帐户**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>存储资源提供程序限制

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>云服务限制

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>应用程序服务限制
下面的应用程序服务限制包括限制 Web 应用程序、 移动应用程序、 API 的应用程序和应用程序逻辑。

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>计划程序限制

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>批次限制

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>BizTalk 服务限制
下表显示了 Azure Biztalk 服务的限制。

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>DocumentDB 限制

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

列出的带有星号 （*），[可通过联系 Azure 支持调整](./documentdb/documentdb-increase-limits.md)配额。

### <a name="mobile-engagement-limits"></a>移动服务限制

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>搜索限制

定价层确定容量和限制您的搜索服务。 层包括︰

- *免费*多租户服务，与其他 Azure 的订阅服务器，用于评估和小开发项目共享。
- *基本*为规模较小，与高度可用的查询工作负载的三个副本的生产工作负载提供专用计算资源。
- *（S1、 S2、 S3） S3 密度较高的标准*是较大的生产工作负载。 多层存在标准层内，以便您可以选择对特定方案的资源配置。

**每个订阅数限制**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**每个搜索服务的限制**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

其他限制，包括文档的大小，每秒、 键、 请求和响应，查询有关更详细信息请参阅[服务限制在 Azure 搜索](search/search-limits-quotas-capacity.md)。

### <a name="media-services-limits"></a>媒体服务限制

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN 限制

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>移动服务限制

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>监视的限制

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>通知中心服务限制

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>事件集线器限制

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>服务总线限制

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT 集线器限制

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>数据工厂限制

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>数据湖分析限制
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>限制了流分析
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>活动目录限制

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure 的远程应用程序限制

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple 系统限制

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>操作的见解限制

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>备份限制

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>网站恢复限制

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>应用程序的见解限制

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API 管理限制

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis 缓存限制

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>密钥存储区限制

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>多因素身份验证
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>自动化的限制
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>SQL 数据库限制

SQL 数据库的限制，请参阅[SQL 数据库的资源限制](sql-database/sql-database-resource-limits.md)。

## <a name="see-also"></a>请参见

[Azure 的限制和增加了解](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[虚拟机和 Azure 的云服务大小](virtual-machines/virtual-machines-linux-sizes.md)

[云服务的大小](cloud-services/cloud-services-sizes-specs.md)
