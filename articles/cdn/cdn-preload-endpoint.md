<properties
    pageTitle="预加载 Azure CDN 端点上的资源 |Microsoft Azure"
    description="了解如何预加载缓存的内容，对 CDN 终结点。"
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

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>预加载 Azure CDN 终结点上的资产

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

默认情况下，资产根据请求第一次缓存。 这意味着从每个地区的第一个请求可能需要较长时间，因为边缘服务器不会内容缓存，将需要请求转发到源服务器。 预加载内容可以避免此第一个命中的滞后时间。

除了提供更好的客户体验，预加载缓存的资产还可以减少的源服务器上的网络通信。

> [AZURE.NOTE] 预加载的资产可用于大型事件或也可以同时供大量用户，如新的电影版本或软件更新的内容。

本教程将指导您完成预加载所有 Azure CDN 边缘节点缓存的内容。

## <a name="walkthrough"></a>演练

1. 在[Azure 门户](https://portal.azure.com)中，浏览到 CDN 配置文件包含您想要预加载的终结点。  打开刀片式服务器的配置文件。

2. 单击列表中的终结点。  打开刀片式服务器终结点。

3. 从 CDN 端点刀片式服务器，请单击载入按钮。

    ![CDN 端点刀片式服务器](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    打开刀片式服务器负载。

    ![CDN 负载刀片式服务器](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. 输入您要加载的每个资产的完整路径 (例如， `/pictures/kitten.png`) 在**路径**文本框。

    > [AZURE.TIP] 输入文本以使您可以生成多个资源的列表后，会出现更多的**路径**文本框。  您可以通过单击省略号 （...） 按钮，从列表中删除资产。
    >
    > 路径必须是相对 URL 符合下面的[正则表达式](https://msdn.microsoft.com/library/az24scfc.aspx)︰ `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`。  每个资产必须有自己的路径。  没有预加载资产通配符功能。

    ![（load） 按钮](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. 单击**载入**按钮。

    ![（load） 按钮](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] 没有每分钟每个配置文件 CDN 的 10 个加载请求的限制。

## <a name="see-also"></a>请参见
- [清除的 Azure CDN 终结点](cdn-purge-endpoint.md)
- [Azure CDN REST API 参考-清除或预先加载某个终结点](https://msdn.microsoft.com/library/mt634451.aspx)
