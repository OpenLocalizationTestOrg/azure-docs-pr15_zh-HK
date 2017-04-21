<properties 
    pageTitle="介绍 DocumentDB，JSON 数据库 |Microsoft Azure" 
    description="请查阅 Azure DocumentDB，NoSQL JSON 数据库。 此文档数据库生成大数据，灵活的可伸缩性和高可用性。" 
    keywords="json 数据库、 文档数据库"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>介绍 DocumentDB: JSON NoSQL 数据库

##<a name="what-is-documentdb"></a>DocumentDB 是什么？

DocumentDB 是为快速、 可预知的性能、 高可用性、 弹性缩放、 全局通讯组和易用性开发构建一个完全托管的 NoSQL 数据库服务。 为免架构 NoSQL 数据库，DocumentDB 提供了丰富且熟悉 SQL 查询功能具有一致的低延迟的 JSON 数据-确保您读取的 99%都提供低于 10 毫秒和您写入的 99%通过提供低于 15 毫秒。 这些独特优势使 DocumentDB 很好适合 web、 移动游戏、 IoT 和许多其他应用程序无缝扩展和全球复制所需。

## <a name="how-can-i-learn-about-documentdb"></a>如何可以了解有关 DocumentDB？ 

若要了解有关 DocumentDB 和在操作中看到它的快速方法是按照这三个步骤︰ 

1. 观察两分钟[是 DocumentDB？](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/)视频，其中介绍了使用 DocumentDB 的好处。
2. 观察三分钟[在 Azure 上创建的 DocumentDB](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/)视频，它着重强调了如何开始使用 DocumentDB 使用 Azure 门户。
3. 请访问[查询运动场](http://www.documentdb.com/sql/demo)，可以遍历不同的活动，以了解 DocumentDB 中提供的丰富的查询功能。 然后，头上沙盒选项卡并运行您自己的自定义 SQL 查询并 DocumentDB 试验。

然后，返回到这篇文章，在其中我们将深入了解更深。  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>DocumentDB 提供哪些功能和主要功能？  

Azure DocumentDB 提供的以下关键的功能和好处︰

-   **弹性可伸缩的吞吐量和存储︰**轻松地扩大或缩小您的 DocumentDB JSON 数据库以满足应用程序的需要。 数据存储在可预测的低延迟的固态磁盘 (SSD)。 DocumentDB 支持存储调用集合，可以扩展到几乎无限制的存储大小和调配的吞吐量的 JSON 数据的容器。 您可以弹性 DocumentDB 可预知的性能与无缝扩展随着您的应用程序。 

-   **多区域复制︰**DocumentDB 以透明方式将数据复制到您已经与您的 DocumentDB 帐户使您能够开发需要同时提供一致性、 可用性和性能，所有相应的保证之间的权衡全局访问数据的应用程序的所有区域。 DocumentDB 提供了与多主页 Api，透明区域的故障切换和吞吐量和存储弹性在全球范围内扩展的能力。 了解更多在[分发全局与 DocumentDB 的数据](documentdb-distribute-data-globally.md)。

-   **使用熟悉的 SQL 语法的特殊查询︰**存储在 DocumentDB 中的异类 JSON 文档，这些文档通过熟悉的 SQL 语法查询。 DocumentDB 利用免费的高并发，锁、 日志结构化的索引技术来自动编制索引的文档中的所有内容。 这样丰富的实时查询，而无需指定架构提示、 辅助索引或视图。 了解更多[查询 DocumentDB](documentdb-sql-query.md)中。 

-   **数据库中的 JavaScript 执行︰**将应用程序逻辑表示为存储的过程、 触发器和用户定义函数 (Udf) 使用标准 JavaScript。 这使您的应用程序逻辑，以在数据上操作而无需担心应用程序和数据库架构之间的不匹配。 DocumentDB 提供了完整事务执行的 JavaScript 应用程序直接在数据库引擎内部的逻辑。 JavaScript 深度集成允许在执行插入、 替换、 删除和选择操作从 JavaScript 程序中的作为独立的事务。 了解在[DocumentDB 服务器端编程](documentdb-programming.md)的更多信息。

-   **可调式的一致性级别︰**从四个定义良好的一致性级别中选择，以实现一致性和性能之间的最佳平衡。 有关查询和读取的操作，DocumentDB 提供了四种不同的一致性级别︰ 强、 包围失效会话并最终。 这些细致、 明确定义的一致性级别允许您进行声音之间的折衷一致性、 可用性和滞后时间。 了解更多使用[要最大化可用性和 DocumentDB 的性能的一致性级别](documentdb-consistency-levels.md)。

-   **完全管理︰**无需管理数据库和计算机资源。 作为全面管理 Microsoft Azure 服务，不要需要管理虚拟机、 部署和配置软件、 管理比例，或处理复杂数据层升级。 每个数据库自动备份并针对区域故障保护。 可以轻松地添加 DocumentDB 帐户和调配容量，您可以根据需要使您可以专注于您的应用程序，而不是操作和管理您的数据库。 

-   **被设计打开︰**通过使用现有的技能和工具快速启动。 针对 DocumentDB 编程简单、 稳固，并不需要采用新的工具或遵循自定义扩展到 JSON 或 JavaScript。 您可以访问所有数据库功能，包括 CRUD、 查询和处理简单的 rest 风格的 HTTP 接口上的 JavaScript。 DocumentDB 同时提供高价值在它们上面的数据库功能涵盖现有的格式、 语言和标准。

-   **自动索引︰**默认情况下，DocumentDB[自动索引](documentdb-indexing.md)数据库中的所有文档和不期望或要求任何架构或辅助索引的创建。 不希望索引的所有内容吗？ 别担心，您可以[选择在 JSON 文件中的路径不](documentdb-indexing-policies.md)太。

##<a name="data-management"></a>DocumentDB 是如何管理数据的？

Azure DocumentDB 管理 JSON 数据通过完善定义的数据库资源。 这些资源的高可用性复制，并且是由其逻辑的 URI 唯一可寻址的。 DocumentDB 提供的简单 HTTP 基于 RESTful 编程模型的所有资源。 

DocumentDB 数据库帐户是可以访问的 Azure DocumentDB 唯一的命名空间。 您可以创建一个数据库帐户之前，您必须 Azure 的订阅，可以访问各种 Azure 服务。 

建模在 DocumentDB 中的所有资源并将其存储为 JSON 文档中时。 资源的管理的项目，哪些是 JSON 文档包含元数据，以及作为源的项的集合。 组的项都包含在其各自的供给。

下图显示了 DocumentDB 资源之间的关系︰

![DocumentDB，一个 JSON NoSQL 数据库中的资源之间的层次关系][1] 

数据库帐户包含一套数据库，每个包含多个集合，其中每个可以包含存储的过程、 触发器、 Udf、 文档和相关的附件。 数据库也有关联的用户，每个都有一组的权限来访问各种其他收藏集、 存储的过程、 触发器、 Udf、 文档或附件。 虽然数据库、 用户、 权限和集合是已知的架构与系统定义的资源-文档、 存储的过程、 触发器、 Udf 和附件包含任意、 用户定义的 JSON 内容。  

##<a name="develop"></a>如何开发与 DocumentDB 的应用程序？

Azure DocumentDB 公开通过 REST API，可以调用任何语言能够发出 HTTP/HTTPS 请求的资源。 此外，DocumentDB 提供了几种常用语言的编程库。 这些库简化了使用 Azure DocumentDB 通过处理的详细信息，如地址缓存、 异常管理、 自动重试次数等诸多方面。 库是当前可用于以下语言和平台︰  

下载 | 文档
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET 库](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.js 库](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java 库](http://azure.github.io/azure-documentdb-java/)
[JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript 库](http://azure.github.io/azure-documentdb-js/)
n/a | [服务器端 JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python 库](http://azure.github.io/azure-documentdb-python/)

超越基本创建、 读取、 更新和删除操作、 DocumentDB 为检索 JSON 文档提供了丰富的 SQL 查询接口和服务器端支持事务执行的 JavaScript 应用程序逻辑。 查询和脚本执行接口可通过所有平台库以及 REST Api。 

### <a name="sql-query"></a>SQL 查询
Azure 的 DocumentDB 支持查询的文档中使用的 SQL 语言，以 JavaScript 类型系统以及提供支持的表达式的关系、 分层的和空间查询为基础。 DocumentDB 查询语言是一个简单而功能强大的界面，连接查询的 JSON 文档。 语言支持 ANSI SQL 语法的一个子集，并将添加深度集成的 JavaScript 对象、 数组、 对象结构和函数调用。 DocumentDB 提供了开发人员的不带任何显式模式或索引提示其查询模型。

用户定义函数 (Udf) 可以注册的 DocumentDB，并引用作为 SQL 查询，从而扩展以支持自定义应用程序逻辑的语法。 这些 Udf 以 JavaScript 程序编写并在数据库中执行。 

对于.NET 开发人员，DocumentDB 还提供 LINQ 查询提供程序作为[.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)的一部分。 

### <a name="transactions-and-javascript-execution"></a>交易记录和 JavaScript 执行
DocumentDB 允许您编写应用程序逻辑以命名完全用 JavaScript 编写的程序。 这些程序集注册并且可以发布数据库上给定集合中的文档的操作。 JavaScript 可以注册为触发器、 存储的过程或用户定义的函数的执行。 触发器和存储的过程可以创建、 读取、 更新和删除文档，而用户定义的函数作为查询执行逻辑而无需写入访问权限集合的一部分来执行。

DocumentDB 中的 JavaScript 执行仿效关系数据库系统，使用 JavaScript 作为现代替代事务处理 SQL 支持的概念。 与快照隔离环境 ACID 事务内执行所有 JavaScript 逻辑。 其执行期间，如果 JavaScript 将引发一个异常，然后整个事务被中止。

## <a name="next-steps"></a>下一步行动
已经有一个 Azure 帐户？ 然后您可以开始使用[Azure 门户网站](https://portal.azure.com/#gallery/Microsoft.DocumentDB)中的 DocumentDB 通过[创建一个 DocumentDB 数据库帐户](documentdb-create-account.md)。

没有 Azure 帐户？ 您可以：

- 注册[Azure 的免费试用版](https://azure.microsoft.com/free/)，这为您提供 30 天和 200 美元可以尝试所有的 Azure 服务。 
- 如果您订阅了 MSDN，您可以获得[每月免费 Azure 片尾在 150 美元](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)用于任何 Azure 服务。 

然后，您准备就绪以了解详细信息，请访问我们[学习路径](https://azure.microsoft.com/documentation/learning-paths/documentdb/)浏览所有可用的学习资源。 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
