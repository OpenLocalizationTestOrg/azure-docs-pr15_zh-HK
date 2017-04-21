<properties
    pageTitle="控制从缓存的查询字符串的请求行为的 Verizon 的 Azure CDN 特优 |Microsoft Azure"
    description="Azure CDN 查询字符串缓存的控件包含查询字符串时缓存文件的方式。"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>控制缓存行为的 CDN 请求与查询字符串-高级

> [AZURE.SELECTOR]
- [标准](cdn-query-string.md)
- [从 Verizon 的 azure CDN 津贴](cdn-query-string-premium.md)

##<a name="overview"></a>概述

查询字符串缓存的控件包含查询字符串时缓存文件的方式。

> [AZURE.IMPORTANT] 标准和最优 CDN 产品提供相同的查询字符串缓存功能，但用户界面不同。  此文档介绍了**Azure CDN 津贴从 Verizon**的接口。  查询字符串与**标准从 Akamai 的 CDN Azure**和**标准从 Verizon 的 CDN Azure**的缓存，请参阅[的 CDN 请求与查询字符串控制缓存行为](cdn-query-string.md)。

提供了三种模式︰

- **标准缓存**︰ 这是默认的模式。  CDN 边缘节点将传递查询字符串从请求者到原点上第一次请求缓存该资产。  所有后续请求该资产提供从边缘节点将忽略查询字符串，直到缓存的资产到期。
- **无缓存**︰ 在此模式下，使用查询字符串的请求将不被缓存在 CDN 边缘节点。  边缘节点直接从源检索该资产，并将它传递给每个请求的请求。
- **唯一缓存**︰ 这种模式将作为其自己的缓存的唯一资产处理每个请求的查询字符串。  例如，从原点的*foo.ashx?q=bar*请求的响应将在边缘节点缓存，为后续高速缓存使用的同一查询字符串返回。  *Foo.ashx?q=somethingelse*请求将作为与自己的时间独立资产活的缓存。

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>更改查询字符串缓存特优 CDN 配置文件的设置

1. 从 CDN 配置文件刀片式服务器，请单击**管理**按钮。

    ![CDN 配置文件刀片式服务器管理按钮](./media/cdn-query-string-premium/cdn-manage-btn.png)

    CDN 管理门户打开。

2. 悬停在**HTTP 大**选项卡，然后悬停在**缓存设置**飞出。  单击**缓存查询字符串**。

    将显示查询字符串缓存选项。

    ![CDN 缓存选项的查询字符串](./media/cdn-query-string-premium/cdn-query-string.png)

3. 您的选择后，单击**更新**按钮。


> [AZURE.IMPORTANT] 设置更改可能不会立即，因为要花时间传播通过 CDN 的登记。  <b>Azure CDN Verizon 从</b>配置文件中，传播通常会在 90 分钟内完成，但在某些情况下可能需要更长的时间。
