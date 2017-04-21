<properties
    pageTitle="通过压缩文件在 Azure CDN 中的提高性能 |Microsoft Azure"
    description="了解如何提高文件传输速度和增加页面装载性能压缩 Azure CDN 中的文件。"
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

# <a name="improve-performance-by-compressing-files"></a>通过压缩文件来提高性能

压缩是一种简单而有效的方法，以提高文件传输速度和增加页面装载性能从服务器发送之前减小文件大小。 它降低了带宽成本，并为您的用户提供的响应能力更强的体验。

有两种方法可以启用压缩︰

- 您可以在源服务器，此时 CDN 将经过压缩的文件和将压缩的文件传递给请求它们的客户端上启用压缩。
- 您可以直接在 CDN 边缘服务器，此时 CDN 将压缩的文件和为其提供服务给最终用户，即使它们未压缩的原始服务器上的压缩。

> [AZURE.IMPORTANT] CDN 配置更改需要一些时间来在网络间传播。  对于<b>Azure CDN Akamai 从</b>配置文件中，传播通常在下一分钟内就完成。  对于<b>Azure CDN Verizon 从</b>配置文件中，您通常会看到所做的更改在 90 分钟内应用。  如果这是首次设置了压缩 CDN 端点，则应考虑等待 1-2 个小时，以确保设置传播到之前进行故障排除的 Pop 的压缩

## <a name="enabling-compression"></a>启用压缩

> [AZURE.NOTE] 标准和最优 CDN 层提供相同的压缩功能，但用户界面不同。  有关标准和最优 CDN 层之间的差异的详细信息，请参阅[Azure CDN 概述](cdn-overview.md)。

### <a name="standard-tier"></a>标准层

> [AZURE.NOTE] 本节适用于**标准从 Verizon 的 CDN Azure**及**标准从 Akamai 的 CDN Azure**的配置文件。

1. 从 CDN 配置文件刀片式服务器，单击您要管理的 CDN 终结点。

    ![CDN 配置文件刀片式服务器终结点](./media/cdn-file-compression/cdn-endpoints.png)

    CDN 端点刀片式服务器打开。

2. 单击**配置**按钮。

    ![CDN 配置文件刀片式服务器管理按钮](./media/cdn-file-compression/cdn-config-btn.png)

    CDN 配置刀片式服务器打开。

3. 启用**压缩**。

    ![CDN 压缩选项](./media/cdn-file-compression/cdn-compress-standard.png)

4. 使用默认的类型，或修改通过删除或添加的文件类型的列表。
    
    > [AZURE.TIP] 虽然有可能，建议不要将压缩应用于压缩的格式，例如 ZIP、 MP3、 MP4、 JPG 等。
    
5. 所做的更改后，单击**保存**按钮。

### <a name="premium-tier"></a>特优层

> [AZURE.NOTE] 本节适用于**从 Verizon 的 Azure CDN 高级**配置文件中。

1. 从 CDN 配置文件刀片式服务器，请单击**管理**按钮。

    ![CDN 配置文件刀片式服务器管理按钮](./media/cdn-file-compression/cdn-manage-btn.png)

    CDN 管理门户打开。

2. 悬停在**HTTP 大**选项卡，然后悬停在**缓存设置**飞出。  单击**压缩**。

    显示压缩选项。

    ![文件压缩](./media/cdn-file-compression/cdn-compress-files.png)

3. 通过单击**启用压缩**单选按钮启用压缩。  请输入您想要压缩为一个逗号分隔的列表 （无空格） 在**文件类型**文本框的 MIME 类型。
        
    > [AZURE.TIP] 虽然有可能，建议不要将压缩应用于压缩的格式，例如 ZIP、 MP3、 MP4、 JPG 等。 

4. 完成后您的更改，请单击**更新**按钮。


## <a name="compression-rules"></a>压缩的规则

这些表描述了每个方案的 Azure CDN 压缩行为。

> [AZURE.IMPORTANT] **从 Verizon 的 Azure CDN** （标准和特优），对符合条件的文件被压缩。  要想有资格获得压缩，文件必须︰
>
> - 为大于 128 个字节。
> - 小于 1 MB。
> 
> **Akamai 从 Azure CDN**，所有文件都是适合压缩。
>
> 对于所有的 Azure CDN 产品，文件必须已[配置为压缩](#enabling-compression)的 MIME 类型。
>
> **从 Verizon 的 azure CDN**配置文件 （标准和特优） 支持**gzip**、**地道**，或**bzip2**编码。  **Akamai 从 azure CDN**配置文件仅支持**gzip**编码。
>
> **Akamai 从 azure CDN**终结点始终请求的来源，而不考虑客户端请求**gzip**编码文件。

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>禁用压缩或文件不可压缩

|客户端请求的格式 （通过接受编码标头）|缓存的文件格式|CDN 响应到客户端|备注|
|----------------|-----------|------------|-----|
|压缩|压缩|压缩|   |
|压缩|未压缩|未压缩|    | 
|压缩|不缓存|压缩或未压缩|取决于原始响应|
|未压缩|压缩|未压缩|    |
|未压缩|未压缩|未压缩|    |   
|未压缩|不缓存|未压缩|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>启用压缩和文件有资格的压缩

|客户端请求的格式 （通过接受编码标头）|缓存的文件格式|CDN 响应到客户端|备注|
|----------------|-----------|------------|-----|
|压缩|压缩|压缩|受支持的格式之间的 CDN transcodes|
|压缩|未压缩|压缩|CDN 进行压缩|
|压缩|不缓存|压缩|如果原点返回未压缩，CDN 执行压缩。  **从 Verizon 的 azure CDN**会将未压缩的文件在首次请求传递，然后压缩和高速缓存文件中的后续请求。  文件`Cache-Control: no-cache`标头将永远不会被压缩。 
|未压缩|压缩|未压缩|CDN 执行解压缩|
|未压缩|未压缩|未压缩|     |  
|未压缩|不缓存|未压缩|     |

## <a name="media-services-cdn-compression"></a>媒体服务 CDN 压缩

媒体服务 CDN 启用流终结点时，默认情况下，为以下内容类型启用压缩︰ sstr-应用程序/vnd.ms + xml、 application/dash+xml,application/vnd.apple.mpegurl、 应用程序/f4m + xml。 您不能启用/禁用压缩，从而提供所述类型使用 Azure 的门户。  

## <a name="see-also"></a>请参见
- [故障排除 CDN 文件压缩](cdn-troubleshoot-compression.md)    
