<properties
    pageTitle="控制缓存的查询字符串的请求行为的 Azure CDN |Microsoft Azure"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>控制缓存行为的 CDN 请求与查询字符串

> [AZURE.SELECTOR]
- [标准](cdn-query-string.md)
- [从 Verizon 的 azure CDN 津贴](cdn-query-string-premium.md)

##<a name="overview"></a>概述

查询字符串缓存的控件包含查询字符串时缓存文件的方式。

> [AZURE.IMPORTANT] 标准和最优 CDN 产品提供相同的查询字符串缓存功能，但用户界面不同。  本文介绍**标准从 Akamai 的 CDN Azure**和**标准从 Verizon 的 CDN Azure**的界面。  特优**Azure CDN Verizon 从**查询字符串的缓存，请参阅[控制与查询字符串-完美的 CDN 请求缓存行为](cdn-query-string-premium.md)。

提供了三种模式︰

- **忽略查询字符串**︰ 这是默认的模式。  CDN 边缘节点将传递查询字符串从请求者到原点上第一次请求缓存该资产。  所有后续请求该资产提供从边缘节点将忽略查询字符串，直到缓存的资产到期。
- **绕过缓存与查询字符串的 URL**︰ 在此模式下，使用查询字符串的请求将不被缓存在 CDN 边缘节点。  边缘节点直接从源检索该资产，并将它传递给每个请求的请求。
- **缓存每个唯一的 URL**︰ 这种模式将作为其自己的缓存的唯一资产处理每个请求的查询字符串。  例如，从原点的*foo.ashx?q=bar*请求的响应将在边缘节点缓存，为后续高速缓存使用的同一查询字符串返回。  *Foo.ashx?q=somethingelse*请求将作为与自己的时间独立资产活的缓存。

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>更改查询字符串缓存标准 CDN 配置文件的设置

1. 从 CDN 配置文件刀片式服务器，单击您要管理的 CDN 终结点。

    ![CDN 配置文件刀片式服务器终结点](./media/cdn-query-string/cdn-endpoints.png)

    CDN 端点刀片式服务器打开。

2. 单击**配置**按钮。

    ![CDN 配置文件刀片式服务器管理按钮](./media/cdn-query-string/cdn-config-btn.png)

    CDN 配置刀片式服务器打开。

3. 从**缓存行为的查询字符串**下拉列表中选择的设置。

    ![CDN 缓存选项的查询字符串](./media/cdn-query-string/cdn-query-string.png)

4. 您的选择后，单击**保存**按钮。

> [AZURE.IMPORTANT] 设置更改可能不会立即，因为要花时间传播通过 CDN 的登记。  对于<b>Azure CDN Akamai 从</b>配置文件中，传播通常在一分钟内完成。  <b>Azure CDN Verizon 从</b>配置文件中，传播通常会在 90 分钟内完成，但在某些情况下可能需要更长的时间。
