<properties
   pageTitle="使用 Azure 存储 Blob 的 azure 数据湖商店比较 |Microsoft Azure"
   description="使用 Azure 存储 Blob 的 azure 数据湖商店比较"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>比较 Azure 数据湖商店和 Azure Blob 存储

在这篇文章表总结了 Azure 数据湖商店和 Azure Blob 存储之间沿大数据处理的一些关键方面的差异。 Azure Blob 存储是一种通用、 可扩展对象存储为多种存储方案设计。 Azure 数据湖商店是专为大数据分析工作负荷超大规模存储库。

|    | Azure 数据湖存储区  | Azure Blob 存储  |
|----|-----------------------|--------------------|
| 目的  | 对于大数据分析的工作负载优化的存储                                                                          | 常规用途的对象存储用于多种存储方案                                                                                |
| 使用案例                                   | 批处理，交互式、 如流分析和机器学习数据日志文件，IoT 数据，请单击流、 大型数据集 | 任何类型文本或二进制数据，例如，应用程序的后端、 备份数据、 流式和常规用途数据的介质存储 |
| 主要概念                                | 数据湖存储帐户包含文件夹，其中又包含数据存储为文件 | 存储帐户具有容器，又有在窗体中的 blob 数据 |
| 结构 | 分层文件系统                                                                                                    | 具有平面命名空间的对象存储区  |
| API   | 通过 HTTPS 的 REST API | 通过 HTTP/HTTPS 的 REST API                                                                                                                            |
| 服务器端 API                             | [WebHDFS 兼容 REST API，](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure Blob 存储 REST API，](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Hadoop 文件系统客户端                   | 是的                                                                                                                         | 是的                                                                                                                                                 |
| 数据操作-身份验证            | 基于[Azure Active Directory 标识](../active-directory/active-directory-authentication-scenarios.md) | 基于共享机密的[帐户访问键](../storage/storage-create-storage-account.md#manage-your-storage-account)和[共享访问签名密钥](../storage/storage-dotnet-shared-access-signature-part-1.md)。                                                                       |
| 数据操作的身份验证协议     | OAuth 2.0。 调用必须包含颁发的 Azure Active Directory 有效 JWT （JSON Web 标记）                     | 基于散列的消息验证代码 (HMAC)。 调用必须通过 HTTP 请求的一部分包含一个 Base64 编码 sha-256 哈希。 |
| 数据操作的授权               | POSIX 的访问控制列表 (Acl)。  可以将 Acl 基于 Azure 活动目录标识设置文件和文件夹级别。 | 为帐户级别授权 — 使用[帐户访问键](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>有关帐户、 容器或 blob 授权-使用[共享访问签名密钥](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| 数据操作的审核                    | 可用。 请参阅[此处](data-lake-store-diagnostic-logs.md)的信息。                                                                                                                   | 可用                                                                                                                                           |
| 存放时加密数据                     | 透明的服务器端 （即将提供）<ul><li>使用服务管理密钥</li><li>客户管理的键在 Azure KeyVault</li></ul>| <ul><li>透明的服务器端</li> <ul><li>使用服务管理密钥</li><li>客户管理的键在 Azure KeyVault （即将提供）</li></ul><li>客户端-端加密</li></ul> |
| 管理操作 （如创建帐户） | [基于角色的访问控制](../active-directory/role-based-access-control-what-is.md)(RBAC) 由 Azure 帐户管理                                                                       | [基于角色的访问控制](../active-directory/role-based-access-control-what-is.md)(RBAC) 由 Azure 帐户管理                                                                                               |
| 开发 Sdk                              | .NET 中，Java，Node.js                                                                                                         | .Net 中，Java，Python Node.js，c + +，拼音                                                                                                              |
| 分析工作负荷性能              | 优化并行分析的工作负载的性能。 高吞吐量和 IOPS。                                           | 不适于分析工作负荷                                                                                                               |
| 大小限制                                 | 帐户的大小、 文件大小或文件的数量没有限制                                                                   | 特定的限制有案可稽[此处](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Geo 冗余                              | 本地冗余 （一个 Azure 的区域中的数据的多个副本）                                                             | 本地冗余 (LRS)、 全局冗余 (GRS)、 读取访问全局冗余 (RA-GRS)。 请参阅[下面](../storage/storage-redundancy.md)的详细信息 |
| 服务状态                               | 公共预览                                                                                                              | 上市                                                                                                                                 |
| 区域的可用性  | 请参阅[此处](https://azure.microsoft.com/regions/#services)| 请参阅[此处](https://azure.microsoft.com/regions/#services) |
| 价格                                       |     查看[定价](https://azure.microsoft.com/pricing/details/data-lake-store/)| 查看[定价](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>下一步行动

- [Azure 湖存储数据的概览](data-lake-store-overview.md)
- [学习如何使用数据湖存储区](data-lake-store-get-started-portal.md)



