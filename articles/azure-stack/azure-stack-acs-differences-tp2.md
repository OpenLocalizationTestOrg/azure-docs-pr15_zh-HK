
<properties
    pageTitle="Azure 一致的存储︰ 差异和注意事项 |Microsoft Azure"
    description="了解从 Azure 存储和其他一致 Azure 存储部署方面的差异。"
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure 一致的存储︰ 差异和注意事项

Azure 一致的存储是一组存储在 Microsoft Azure 堆栈中的云服务。 Azure 一致的存储提供了斑点、 表、 队列和 Azure 一致语义的帐户管理功能。 这篇文章总结了使用 Azure 存储的已知一致 Azure 存储差异。 它还概述了部署 Microsoft Azure 堆栈的当前公开可用的预览版本时需要注意的其他事项。

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>已知的区别

此技术预览版本的一致性 Azure 存储并没有 100%功能与奇偶校验 Azure 存储所支持的 API 版本。 已知功能差异如下︰

-   某些存储帐户类型尚不可用，例如，标准\_RAGRS 和标准\_GRS。

-   特优\_可以调配 LRS 存储帐户。 但是，当前没有任何性能限制或保证。

-   Azure 文件功能尚不可用。

-   获取页面范围 API 不支持检索的页面 blob 的快照之间不同的页面。

-   获取页面范围 API 返回具有粒度的 4 KB 页面。

-   分区键并在 Azure 一致性实现中存储表行键分别超过 400 个字符 （即 800 个字节） 的大小。

-   在一致 Azure 存储 Blob 服务实现中的 blob 名称仅限于 880 个字符 （即 1760 个字节） 的大小。

-   报告提供了与在 Azure，具有相同的语义和单位相同的存储使用量米租户存储的使用率数据一致 Azure 存储实施。 但是，目前，存储交易记录使用计量器不包括 IaaS 的交易记录，并且数据传输使用计量器不区分到 Azure 堆栈区域的内部或外部网络通信的带宽使用情况。

-   存储的可管理性功能的作用域中存在某些差异。 例如，不能更改帐户的类型，或具有自定义的域。 此外，只有 API 级别一致性提供津贴\_LRS 存储帐户类型。

## <a name="deployment-considerations"></a>部署注意事项

-   **仅用于测试。** 不部署在生产环境中使用当前的 Microsoft Azure 堆栈技术预览版本一致 Azure 存储。 此版本仅用于评估目的在测试实验室环境中。

-   **性能**。 Azure 一致存储的 Microsoft Azure 堆栈技术预览版本不是完全的性能优化。

-   **可缩放性。** Azure 一致存储的 Microsoft Azure 堆栈技术预览版本未优化的可扩展性。

## <a name="version-support-considerations"></a>版本支持时的注意事项

此预览版本的 Azure 一致的存储支持以下版本︰

> Azure 存储客户端库︰ [Microsoft Azure 存储 6.x.NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure 存储数据服务︰ [2015年-04-05 REST API 版本](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure 存储管理服务︰ [2015年-05-01-预览](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015年-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>下一步行动

-   [Azure 一致存储的简介](azure-stack-storage-overview.md)
