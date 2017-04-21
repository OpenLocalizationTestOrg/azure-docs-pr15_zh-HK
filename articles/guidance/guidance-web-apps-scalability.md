<properties
   pageTitle="可扩展的 web 应用程序 |Azure 的参考体系结构 |Microsoft Azure"
   description="提高 web 应用程序运行在 Microsoft Azure 中的可扩展性。"
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>提高 web 应用程序中的可扩展性 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍了改进的可扩展性和性能对 Microsoft Azure 上运行的 web 应用中推荐的体系结构。 该体系结构基于[Azure 的参考体系结构︰ 基本的 web 应用程序][basic-web-app]。 建议和注意事项从这篇文章将应用于此的体系结构。

>[AZURE.NOTE] Azure 具有两种不同的部署模型︰ 资源管理器和经典。 本文使用资源管理器，Microsoft 建议为新的部署。

## <a name="architecture-diagram"></a>体系结构关系图

![[0]][0]

该体系结构具有以下组件︰

- **资源组**。 [资源组][resource-group]是 Azure 资源的逻辑容器。 

- ** [Web app] [app-service-web-app]**和**[API 应用程序][app-service-api-app]**。 典型的现代应用程序可能包括一个网站和一个或多个 rest 风格 web Api。 通过浏览器客户端通过 AJAX、 本机客户端应用程序或服务器端应用程序，可能会消耗 web API。 设计 web Api 的考虑因素，请参阅[API 设计指导][api-guidance]。    

- **WebJob**。 使用[Azure WebJobs] [webjobs]在后台运行长时间运行的任务。 按照日程安排，持续，或响应触发器，如一条消息放入队列中，可以运行 WebJobs。 WebJob 作为后台进程的应用程序服务的应用程序的上下文中运行。 

- **队列**中。 在此处所示的体系结构，应用程序队列后台任务通过将消息放到[Azure 队列存储][queue-storage]队列。 该消息触发 WebJob 中的函数。 

    或者，您可以使用服务总线队列。 为了进行比较，请参见[Azure 服务总线队列-比较和队列相对][queues-compared]。

- **高速缓存**。 在[Azure Redis 高速缓存]中存储半静态数据[azure-redis]。  

- **CDN**。 使用[Azure 内容交付网络][ azure-cdn] (CDN) 缓存公开内容，较低的滞后时间和内容的交付速度更快。

- **数据存储。** 使用[SQL Azure 数据库][sql-db]为关系数据。 对于非关系数据，请考虑 NoSQL 存储，如 Azure 表存储或[DocumentDB][documentdb]。

- **Azure 搜索**。 使用[Azure 搜索][azure-search]添加搜索功能，包括搜索建议、 模糊搜索和特定于语言的搜索。 尤其是在主数据存储要求严格的一致性，结合其他数据存储区中，通常将使用 azure 搜索。 在此方法中，您将其他数据存储区中，权威数据，放入 Azure 搜索的搜索索引。 此外可以使用 azure 搜索整合来自多个数据存储为单个搜索索引。  

- **电子邮件/SMS**。 如果您的应用程序需要发送电子邮件或短信，使用如 SendGrid 或 Twilio，而不是直接在应用程序中构建此功能的第三方服务。

## <a name="recommendations"></a>建议

### <a name="app-service-apps"></a>应用程序服务的应用程序 

我们建议作为单独的应用程序服务应用程序创建 web 应用程序和 web API。 这种设计允许您运行它们在单独的应用程序服务计划中，反过来让您独立扩展它们。 如果不需要该级别的可伸缩性，在第一次，可以将应用程序部署到相同的计划，并将它们移到单独的计划之后，如果需要。 （对于基本、 标准和特优计划中，付费的 VM 实例在计划中，不是每个应用程序。 请参见[应用程序服务定价][app-service-pricing])

如果您打算使用的*简单表*或*简单的 Api*的应用程序服务移动应用程序的功能，为此目的创建单独的应用程序服务应用程序。  这些功能依赖于一个特定的应用程序框架，可使他们。

### <a name="webjobs"></a>WebJobs

如果 WebJob 占用大量的资源，请考虑将其部署到一个单独的应用程序服务计划，为 WebJob 提供专用的实例内空的应用程序服务应用程序。 请参阅[后台作业指导][webjobs-guidance]。  

### <a name="cache"></a>高速缓存

您可以通过使用[Azure Redis 高速缓存]提高性能和可扩展性[azure-redis]缓存部分数据。 请考虑使用 Redis 高速缓存︰

- 半静态交易数据。

- 会话状态。

- HTML 输出。 这可以是有用的应用程序呈现复杂的 HTML 输出中。 

有关更多详细设计的缓存策略的指导，请参阅[缓存指导][caching-guidance]。

### <a name="cdn"></a>CDN 

使用[Azure CDN] [azure-cdn]缓存静态内容。 CDN 的主要优点是减少等待时间的用户，因为在靠近用户的地理位置是*边缘服务器*缓存的内容。 CDN 还可以减少对应用程序的负载，因为应用程序不处理该通讯。

- 如果您的应用程序主要包括静态页面，请考虑使用 CDN 缓存整个应用程序。 请参阅[使用 Azure 应用程序服务中的 Azure CDN][cdn-app-service]。

- 否则，将静态内容，如图像、 CSS 和 HTML 文件，到 Azure 存储并使用 CDN 缓存的那些文件。 请参阅[集成存储客户，与 CDN][cdn-storage-account]。

> [AZURE.NOTE] Azure CDN 无法提供要求身份验证的内容。

有关更详细的指导，请参阅[内容传递网络 (CDN) 指导][cdn-guidance]。 

### <a name="storage"></a>存储

现代应用程序经常处理大量的数据。 为了满足云，是一定要选择适当的存储类型。 以下是一些基准的建议。  有关更详细的指导，请参阅[Polyglot 持久性解决方案评估数据存储功能][polyglot-storage]。

您想要存储 | 示例 | 建议的存储
--- | --- | ---
文件 | 图像、 文档的 Pdf | Azure Blob 存储
键/值对 | 查找用户 ID 的用户配置文件数据 | Azure 表存储
短消息用于触发进一步的处理 | 订单请求 | Azure 队列存储、 服务总线队列或服务总线主题
非关系数据，使用一个灵活的架构，需要基本的查询 | 产品目录 | 文档数据库，如 Azure DocumentDB，MongoDB 或 Apache CouchDB 数据库
关系数据，需要更丰富的查询支持、 严格的架构，和/或强一致性 | 产品清单 | SQL azure 数据库

## <a name="scalability-considerations"></a>可伸缩性注意事项

### <a name="app-service-app"></a>应用程序服务的应用程序

如果您的解决方案中包含若干个应用程序服务应用程序，请考虑部署到单独的应用程序服务计划。 此方法使您能够扩展其独立，因为它们运行在单独的实例上。 横向扩展的详细信息，请参阅[可伸缩性注意事项][ basic-web-app-scalability] [基本的 web 应用程序体系结构]中的部分[basic-web-app]。

同样，考虑将 WebJob 放入其自己的计划，以便后台任务不在处理 HTTP 请求的同一实例上运行。  

### <a name="sql-database"></a>SQL 数据库

通过*分片*数据库增加 SQL 数据库的可伸缩性&mdash;，即水平分区数据库。 分片，可横向，横向扩展数据库使用[弹性数据库工具][sql-elastic]。 分片的潜在好处包括︰

- 更好的事务吞吐量。

- 通过将数据的一个子集，查询的运行得更快。 

### <a name="azure-search"></a>Azure 搜索

Azure 搜索中删除从主数据存储区中，执行复杂的数据搜索的开销，它可以扩展以处理负载。 [查询和索引工作负载在 Azure 搜索的缩放级别]，请参阅[azure-search-scaling]。

## <a name="security-considerations"></a>安全注意事项

### <a name="cross-origin-resource-sharing-cors"></a>跨来源的资源共享 (CORS)

如果您创建了一个网站和 web API 作为单独的应用程序，网站不能进行客户端的 AJAX 调用 API，除非您启用 CORS。 

> [AZURE.NOTE] 浏览器安全性防止 web 页到另一个域发出 AJAX 请求。 这种限制被称为同源策略，并防止恶意站点读取另一个网站中的 sentitive 数据。 CORS 是允许放宽同源策略，并拒绝其他人同时允许某些跨原始请求的服务器的 W3C 标准。

应用程序服务具有内置支持 CORS，而无需编写任何应用程序代码。 请参阅[使用 CORS 的 JavaScript API 应用程序使用][cors]。 将网站添加到允许来源的列表 API。 

### <a name="sql-database-encryption"></a>SQL 数据库加密

使用[透明数据加密][sql-encryption]如果您需要存放在数据库中的数据进行加密。 此功能执行实时加密和解密的整个数据库 （包括备份和事务日志文件），而无需更改应用程序。 加密 does 添加一定的延迟，因此最好单独的必须是安全的到其自己的数据库，并只为该数据库启用加密的数据。  

## <a name="next-steps"></a>下一步行动

- 为了获得高可用性，部署该应用程序在多个地区和使用[Azure 流量管理器][tm]的故障切换。 有关详细信息，请参阅[Azure 的参考体系结构︰ Web 应用程序提供高可用性][web-app-multi-region]。    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "在 Azure 中的改进的可扩展性的 web 应用程序"