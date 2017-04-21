<properties
    pageTitle="Azure 政府文档 |Microsoft Azure"
    description="这为 Azure 政府开发应用程序提供功能和指导的比较"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/13/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-storage"></a>政府 azure 存储

##  <a name="azure-storage"></a>Azure 存储

有关此服务以及如何使用它的详细信息，请参阅[Azure 存储公用文档](https://azure.microsoft.com/documentation/services/storage/)。

### <a name="variations"></a>变体

政府 Azure 存储帐户的 Url 是不同的︰

服务类型|Azure 公共|Azure 的政府
---|---|---
Blob 存储|*。 blob.core.windows.net|*。 blob.core.usgovcloudapi.net
队列存储|*。 queue.core.windows.net|*。 queue.core.usgovcloudapi.net
表存储|*。 table.core.windows.net| *。 table.core.usgovcloudapi.net

>[AZURE.NOTE] 所有的脚本和代码必须考虑的适当的终结点。  请参阅[配置 Azure 存储连接字符串](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint)。 

Api 的详细信息，请参阅<a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">云存储帐户构造函数</a>。

要使用这些重载中的终结点后缀为 core.usgovcloudapi.net 

### <a name="considerations"></a>注意事项

以下信息标识 Azure 政府边界 Azure 存储︰

| 允许调节控制的数据 | 不允许使用的管制控制数据 |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 输入，数据存储和处理在 Azure 存储产品都可包含控制导出数据。 静态身份验证器，如密码和 Azure 平台组件访问智能卡 Pin。 用于管理 Azure 平台组件的证书的私钥。 其他安全性信息/机密信息，例如证书、 加密密钥、 主密钥和 Azure 服务中存储的存储密钥。 | Azure 存储元数据不能包含控制导出数据。 此元数据包括创建和维护您的存储产品时输入的所有配置数据。  不要在以下字段输入受管制/控制数据︰ 资源组、 部署名称、 资源名称、 资源标记  

##  <a name="premium-storage"></a>高级存储

有关此服务以及如何使用它的详细信息，请参阅[高级存储︰ Azure 虚拟机工作负载的高性能存储](../storage/storage-premium-storage.md)。

###  <a name="variations"></a>变体

高级存储是在 USGov 弗吉尼亚州正式提供。 这包括 DS 系列虚拟机。 

### <a name="considerations"></a>注意事项

上述同一存储数据注意事项适用于高级存储帐户。 

##  <a name="next-steps"></a>下一步行动

为补充信息和更新订阅<a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure 的政府博客。</a>
