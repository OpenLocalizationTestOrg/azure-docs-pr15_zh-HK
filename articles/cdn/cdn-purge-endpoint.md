<properties
    pageTitle="清除的 Azure CDN 终结点 |Microsoft Azure"
    description="了解如何从一个 CDN 终结点清除所有缓存的内容。"
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

# <a name="purge-an-azure-cdn-endpoint"></a>清除的 Azure CDN 终结点

## <a name="overview"></a>概述

资产的生存时间 (TTL) 过期之前，azure CDN 边缘节点将缓存的资产。  资产的 TTL 过期，当客户端请求从边缘节点的资产后，边节点将资产以处理客户端请求更新的新副本中检索和存储刷新缓存。

有时您可能希望从所有边缘节点清除缓存的内容，让他们全部检索新更新的资产。  这可能是因为更新到您的 web 应用程序，或快速更新资产包含不正确的信息。

> [AZURE.TIP] 请注意，只有清除清除上 CDN 边缘服务器缓存的内容。  所有下游的缓存，例如代理服务器和本地浏览器的缓存，仍可能保留文件的缓存的副本。  设置文件的生存时间时这一点至关重要。  您可以强制下游客户端每次更新时，为其提供一个唯一的名称，或利用[查询字符串缓存](cdn-query-string.md)请求文件的最新版本。  

本教程将带领您通过从某个终结点的所有边缘节点清除资产。

## <a name="walkthrough"></a>演练

1. 在[Azure 门户](https://portal.azure.com)，浏览到包含您想要清除该终结点的 CDN 配置文件。

2. 从 CDN 配置文件刀片式服务器，请单击清除按钮。

    ![CDN 配置文件刀片式服务器](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    打开清除刀片式服务器。

    ![CDN 清除刀片式服务器](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. 在清除刀片式服务器，选择您想要从 URL 下拉列表中清除的服务地址。

    ![清除窗体](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] 此外可以通过单击 CDN 端点刀片式服务器上的**清除**按钮清除刀片式服务器。  在这种情况下， **URL**字段将预先填充该特定终结点的服务地址。

4. 选择您要清除的边缘节点中的哪些资产。  如果要清除所有的资产，请单击**全部清除**复选框。  否则，请键入您想要清除每个资产的完整路径 (例如， `/pictures/kitten.png`) 在**路径**文本框。

    > [AZURE.TIP] 输入文本以使您可以生成多个资源的列表后，会出现更多的**路径**文本框。  您可以通过单击省略号 （...） 按钮，从列表中删除资产。
    >
    > 路径必须是相对 URL 符合下面的[正则表达式](https://msdn.microsoft.com/library/az24scfc.aspx)︰ `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`。  **从 Verizon 的 Azure CDN** （标准和特优），为星号 (\*) 可能用作通配符 (例如， `/music/*`)。  **Akamai 从 Azure CDN**与不允许使用通配符和**全部清除**。
    
5. 单击**清除**按钮。

    ![清除按钮](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] 清除请求需要 2-3 分钟的时间来处理与**Verizon 从 Azure CDN** （标准和特优），并使用**Azure CDN Akamai 从**大约 7 分钟。  Azure CDN 的在任何给定时间清除请求并发 50 的限制。 

## <a name="see-also"></a>请参见
- [预加载 Azure CDN 终结点上的资产](cdn-preload-endpoint.md)
- [Azure CDN REST API 参考-清除或预先加载某个终结点](https://msdn.microsoft.com/library/mt634451.aspx)
