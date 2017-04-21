<properties
    pageTitle="Real-时间-统计在 Azure CDN |Microsoft Azure"
    description="交付给您的客户端的内容时，实时统计数据提供了 Azure CDN 的性能有关的实时数据。"
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
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>在 Microsoft Azure CDN 的实时统计

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>概述

本文介绍了在 Microsoft Azure CDN 的实时统计。  交付给您的客户端的内容时，此功能提供了实时的数据，如带宽、 高速缓存状态和对 CDN 配置文件的并发连接。 这样，在任何时候，包括投入的事件服务的运行状况的持续监控。

下图是可用的︰

* [带宽](#bandwidth)
* [状态代码](#status-codes)
* [高速缓存状态](#cache-statuses)
* [连接](#connections)


## <a name="accessing-real-time-stats"></a>访问实时统计

1. 在[Azure 门户网站](https://portal.azure.com)，浏览到您 CDN 的配置文件。

    ![CDN 配置文件刀片式服务器](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. 从 CDN 配置文件刀片式服务器，请单击**管理**按钮。

    ![CDN 配置文件刀片式服务器管理按钮](./media/cdn-real-time-stats/cdn-manage-btn.png)

    CDN 管理门户打开。

3. 悬停在**分析**选项卡，然后悬停在**实时统计**飞出。  单击**HTTP 大型对象**。

    ![CDN 管理门户](./media/cdn-real-time-stats/cdn-premium-portal.png)

    显示实时状态关系图。
    
每个关系图显示所选的时间范围，从开始加载页面时的实时统计。  关系图会自动更新每隔几秒钟。  **刷新图表**按钮 （如果存在） 将清除的图，在此之后它将只显示所选的数据。

## <a name="bandwidth"></a>带宽

![带宽图](./media/cdn-real-time-stats/cdn-bandwidth.png)

**带宽**图显示所选的时间范围内当前平台所使用的带宽量。 图形的阴影的部分表示带宽使用情况。 线图的正下方显示精确的当前使用的带宽量。

## <a name="status-codes"></a>状态代码

![状态代码图](./media/cdn-real-time-stats/cdn-status-codes.png)

**状态代码**图表明某些 HTTP 响应代码选定的时间范围内的发生频率。

> [AZURE.TIP]  每个 HTTP 状态代码选项的说明，请参阅[Azure CDN HTTP 状态代码](https://msdn.microsoft.com/library/mt759238.aspx)。

HTTP 状态代码的列表将显示关系图的正上方。 该列表指示每个状态代码，可以包括在折线图和每秒为该状态代码的当前数量。 默认情况下，这些关系图中的状态代码的每个显示的行。 但是，您可以选择仅监视具有特殊重要性，CDN 配置的状态代码。 若要执行此操作，检查所需的状态代码并清除所有其他选项，然后单击**刷新图表**。 

可以暂时隐藏特定状态代码记录的数据。  从直接关系图下方的图例，请单击您想要隐藏的状态代码。 将从关系图立即隐藏的状态代码。 再次单击该状态代码将导致再次显示该选项。

## <a name="cache-statuses"></a>高速缓存状态

![高速缓存状态关系图](./media/cdn-real-time-stats/cdn-cache-status.png)

**高速缓存状态**图表示某些类型的缓存状态的选定的时间范围内的发生频率。 

> [AZURE.TIP]  每个缓存状态代码选项的说明，请参阅[Azure CDN 缓存状态代码](https://msdn.microsoft.com/library/mt759237.aspx)。

缓存状态代码的列表将显示关系图的正上方。 该列表指示每个状态代码，可以包括在折线图和每秒为该状态代码的当前数量。 默认情况下，这些关系图中的状态代码的每个显示的行。 但是，您可以选择仅监视具有特殊重要性，CDN 配置的状态代码。 若要执行此操作，检查所需的状态代码并清除所有其他选项，然后单击**刷新图表**。 

可以暂时隐藏特定状态代码记录的数据。  从直接关系图下方的图例，请单击您想要隐藏的状态代码。 将从关系图立即隐藏的状态代码。 再次单击该状态代码将导致再次显示该选项。

## <a name="connections"></a>连接

![连接图](./media/cdn-real-time-stats/cdn-connections.png)

此图表显示了多少连接已建立到边缘服务器。 针对资产通过我们 CDN 产生连接的每个请求。

## <a name="next-steps"></a>下一步行动

- 获取与[在 Azure CDN 的实时警报](cdn-real-time-alerts.md)通知
- 使用[高级的 HTTP 报告](cdn-advanced-http-reports.md)深入
- 分析[使用模式](cdn-analyze-usage-patterns.md)

