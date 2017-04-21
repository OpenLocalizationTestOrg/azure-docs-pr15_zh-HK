<properties
    pageTitle="Azure CDN 概述 |Microsoft Azure"
    description="了解什么是 Azure 内容传递网络 (CDN) 以及如何使用它来缓存通过提供高带宽内容 blob 和静态内容。"
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Azure 内容传递网络 (CDN) 的概述

> [AZURE.NOTE] 此文档介绍了 Azure 内容传递网络 (CDN) 是什么，它的工作原理，以及每个 Azure CDN 产品的功能。  如果您想要跳过此信息，请直接转到教程，说明如何创建一个 CDN 的终结点，请参阅[使用 Azure CDN](cdn-create-new-endpoint.md)。  如果您想要查看的当前 CDN 节点位置的列表，请参阅[Azure CDN POP 位置](cdn-pop-locations.md)。

Azure 内容传递网络 (CDN) 缓存静态 web 内容在策略性地放置位置向用户传递内容提供最大吞吐量。  CDN 为开发人员提供全球解决方案提供高带宽内容缓存为全球的物理节点上的内容。 

使用缓存的 web 站点资产 CDN 的优点包括︰

- 更好的性能和用户体验对于最终用户，特别是在使用应用程序的多个回路所需加载内容。
- 扩展到更大的产品发布活动开始像处理瞬时高负载。
- 通过将用户请求并提供从边缘服务器的内容，更少的通信量发送到原点。


## <a name="how-it-works"></a>它的工作原理

![CDN 概述](./media/cdn-overview/cdn-overview.png)

1. (Alice) 的用户请求文件 （也称为资产） 使用 URL 与特殊的域名，如`<endpointname>.azureedge.net`。  DNS 将请求路由到最佳的执行点的存在 (POP) 的位置。  通常这是地理上最接近用户的弹出窗口。

2. 如果 POP 中的边缘服务器在其缓存中没有该文件，则边缘服务器请求文件从原点。  来源可以是 Azure Web 应用程序、 Azure 云服务、 Azure 存储帐户或任何可公开访问的 web 服务器。

3. 原点至边缘服务器，包括可选描述该文件的生存时间 (TTL) 的 HTTP 标头中返回的文件。

4. 边缘服务器缓存该文件并将文件恢复到原始请求者 （小红）。  TTL 过期之前，文件仍保留在边缘服务器缓存。  如果原点并未指定 TTL 设置，默认的 TTL 为七天。

5. 其他用户可能然后请求相同的文件使用此相同的 URL，并还可能被定向到该相同的 POP。

6. 如果该文件的 TTL 没有到期，边缘服务器会从缓存中返回该文件。  这导致了更快、 响应性更强的用户体验。


## <a name="azure-cdn-features"></a>Azure CDN 功能

有三种 Azure CDN 产品︰**标准从 Akamai 的 CDN Azure**、**标准从 Verizon 的 CDN Azure**和**Verizon 从 Azure CDN 津贴**。  下表列出了与每个产品提供的功能。

|       | Akamai 的标准 | Verizon 标准 | 特优 Verizon |
|-------|-----------------|------------------|-----------------|
| 方便地集成与 Azure 服务如[存储](cdn-create-a-storage-account-with-cdn.md)、[云服务](cdn-cloud-service-with-cdn.md)、 [Web 应用程序](../app-service-web/cdn-websites-with-cdn.md)和[媒体服务](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| 通过[REST API](https://msdn.microsoft.com/library/mt634456.aspx)、 [.NET](./cdn-app-dev-net.md)， [Node.js](./cdn-app-dev-node.md)或[PowerShell](./cdn-manage-powershell.md)的管理。 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| HTTPS 支持 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| 负载平衡 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015)保护 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| IPv4/IPv6 双栈 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [自定义域名支持](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [查询字符串缓存](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Geo 筛选](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [快速清除](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [资产预加载](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [核分析](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [HTTP/2 支持](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [高级的 HTTP 报表](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [实时统计](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [实时警报](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [可自定义的、 基于规则的内容传递引擎](cdn-rules-engine.md) | | | **& #x 2713;** |
| 缓存/头设置 （使用[规则引擎](cdn-rules-engine.md)）  | | | **& #x 2713;** |
| URL 重定向/重写 （使用[规则引擎](cdn-rules-engine.md)） | | | **& #x 2713;** |
| 移动设备规则 （使用[规则引擎](cdn-rules-engine.md)）  | | | **& #x 2713;** |

>[AZURE.TIP] 是否有您想要在 Azure CDN 中看到的功能？  请[向我们提供反馈](https://feedback.azure.com/forums/169397-cdn)！ 

## <a name="next-steps"></a>下一步行动

若要开始使用 CDN，请参阅[使用 Azure CDN](./cdn-create-new-endpoint.md)。

如果您是现有 CDN 客户，现在可以管理您的 CDN 端点通过[Microsoft Azure 门户](https://portal.azure.com)或使用[PowerShell](cdn-manage-powershell.md)。

要查看操作中的 CDN，签出[我们生成 2016年会话的视频](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/)。

了解如何自动化与[.NET](./cdn-app-dev-net.md)或[Node.js](./cdn-app-dev-node.md)Azure CDN。

定价信息，请参阅[CDN 定价](https://azure.microsoft.com/pricing/details/cdn/)。
