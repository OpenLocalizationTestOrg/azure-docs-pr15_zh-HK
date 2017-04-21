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
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure 的政府数据库

##  <a name="sql-database"></a>SQL 数据库

请参阅<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx">Microsoft SQL 数据库引擎的安全中心</a>和[Azure SQL 数据库公共文档](https://azure.microsoft.com/documentation/services/sql-database/)的附加元数据可见性配置，和保护最佳做法指南。

### <a name="variations"></a>变体

SQL V12 数据库是在 Azure 政府正式提供。

在 Azure 政府中的 SQL Azure 服务器的地址是不同的︰

服务类型|Azure 公共|Azure 的政府
---|---|---
SQL 数据库|*。 database.windows.net|*。 database.usgovcloudapi.net

### <a name="considerations"></a>注意事项

以下信息标识 Azure SQL Azure 政府边界︰

| 允许调节控制的数据 | 不允许使用的管制控制数据 |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 所有的数据存储和处理 Microsoft Azure SQL 可以包含 Azure 政府管控数据。 您必须使用 Azure 政府管控数据数据传输数据库工具。 | SQL azure 的元数据不能包含控制导出数据。 此元数据包括创建和维护您的存储产品时输入的所有配置数据。  不要在以下字段中输入管制控制数据︰ 数据库名称、 订阅名称、 资源组、 服务器名称、 服务器管理员登录，部署的名称、 资源名称、 资源标记

## <a name="azure-redis-cache"></a>Azure 的 Redis 高速缓存

有关此服务以及如何使用它的详细信息，请参阅[Redis Azure 的高速缓存公共文档](https://azure.microsoft.com/documentation/services/redis-cache/)。

### <a name="variations"></a>变体

用于访问和管理 Azure Redis Azure 政府中缓存的 Url 是不同的︰

服务类型|Azure 公共|Azure 的政府
---|---|---
高速缓存端点|*。 redis.cache.windows.net|*。 redis.cache.usgovcloudapi.net
Azure 门户|https://portal.azure.com|https://portal.azure.us

>[AZURE.NOTE] 所有的脚本和代码需要考虑适当的终结点和环境。 有关详细信息，请参阅[如何连接到 Azure 政府云](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud)。


### <a name="considerations"></a>注意事项

以下信息标识为 Azure Redis 缓存 Azure 政府边界︰

| 允许调节控制的数据 | 不允许使用的管制控制数据 |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 所有的数据存储，并处理在 Azure Redis 缓存可以包含 Azure 政府管控数据。 | Azure Redis 缓存元数据不能包含控制导出数据。 不要在以下字段中输入管制控制数据︰ 缓存名称、 VNET 名称、 订阅名称、 资源组、 资源标记、 Redis 属性。  

##  <a name="next-steps"></a>下一步行动

为补充信息和更新订阅<a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure 的政府博客。</a>
