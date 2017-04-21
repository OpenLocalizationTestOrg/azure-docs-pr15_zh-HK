<properties 
    pageTitle="DocumentDB 数据库问题-常见问题 |Microsoft Azure" 
    description="NoSQL 的 JSON 文档数据库服务获得 Azure DocumentDB 有关的常见问题的解答。 回答有关容量、 性能级别和扩展的数据库问题。" 
    keywords="常问问题、 documentdb、 azure、 Microsoft azure 的数据库问题"
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
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>DocumentDB 有关的常见问题

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>有关 Microsoft Azure DocumentDB 基础数据库问题

### <a name="what-is-microsoft-azure-documentdb"></a>什么是 Microsoft Azure DocumentDB？ 
Microsoft Azure DocumentDB 是超快的速度，行星比例 NoSQL 文档数据库作为-服务提供丰富查询架构的数据，有助于提供可配置的和可靠的性能、 和实现快速发展，通过托管平台强大的后盾和 Microsoft Azure 的到达。 DocumentDB 是合适的解决方案为 web、 移动，游戏和 IoT 应用程序在可预测的吞吐量、 高可用性、 低延迟、 和架构的数据模型时的关键要求。 DocumentDB 提供了架构的灵活性和丰富索引通过本机的 JSON 数据模型，并且支持多文档事务与集成的 JavaScript。  
  
多个数据库问题、 回答和部署和使用此服务的说明，请参阅[DocumentDB 文档页中找到](https://azure.microsoft.com/documentation/services/documentdb/)的。

### <a name="what-kind-of-database-is-documentdb"></a>哪种类型是数据库的 DocumentDB？
DocumentDB 是 NoSQL 文档面向的数据库，以 JSON 格式存储数据。  DocumentDB 支持嵌套、 自助 contained 数据结构可通过丰富的 DocumentDB [SQL 查询语法](documentdb-sql-query.md)查询。 DocumentDB 提供了高性能事务处理服务器端 JavaScript 通过[存储过程、 触发器和用户定义的函数](documentdb-programming.md)。 该数据库还支持具有相关联[的性能级别](documentdb-performance-levels.md)的开发人员可调式的一致性级别。
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>DocumentDB 数据库有像关系数据库 (RDBMS) 的表吗？
否，DocumentDB 的 JSON 文档集合中存储的数据。  DocumentDB 资源信息，请参阅[DocumentDB 资源模型和概念](documentdb-resources.md)。 如 DocumentDB NoSQL 解决方案有何不同从关系解决方案的详细信息，请参阅[NoSQL vs SQL](documentdb-nosql-vs-sql.md)。

### <a name="do-documentdb-databases-support-schema-free-data"></a>DocumentDB 数据库支持架构的数据吗？
是的 DocumentDB 允许应用程序存储任意 JSON 文档架构定义或提示。 数据可立即用于查询通过 DocumentDB SQL 查询界面。   

### <a name="does-documentdb-support-acid-transactions"></a>DocumentDB 是否支持 ACID 事务？
是的 DocumentDB 支持跨文档表示为 JavaScript 存储过程和触发器的交易记录。 交易记录应用范围限定为每个集合中的单个分区和执行与 ACID 语义所有或任何其他并发执行的代码和用户请求与隔离。  如果通过服务器端执行的 JavaScript 应用程序代码，则将引发异常，则回滚整个事务。 有关交易记录的详细信息，请参阅[数据库程序事务](documentdb-programming.md#database-program-transactions)。

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>对于 DocumentDB 的典型用例是什么？  
DocumentDB 是一个不错的选择，对于新的 web、 移动，游戏和 IoT 自动缩放，可预知的性能，其中无架构的数据快速的毫秒响应时间、 顺序和查询功能的应用程序非常重要。 DocumentDB 适合于快速开发和支持应用程序数据模型的连续迭代。 应用程序管理用户生成的内容和数据是[DocumentDB 的常见使用情形](documentdb-use-cases.md)。  

### <a name="how-does-documentdb-offer-predictable-performance"></a>DocumentDB 提供可预知的性能有什么服务。
[申请单位](documentdb-request-units.md)是吞吐量的 DocumentDB 中的度量值。 1 RU 对应 1 KB 文档获取的吞吐量。 在 DocumentDB 中的每个操作，包括读取、 写入、 SQL 查询和存储的过程的执行具有确定性 RU 值根据完成该操作所需的吞吐量。 而不是考虑 CPU、 I/O 和内存和它们如何影响您的应用程序吞吐量，可想到一个 RU 度量值。

每个 DocumentDB 集合可以预留与调配的吞吐量，以每秒吞吐量的 RUs。 对于任何规模的应用程序，可以基准来衡量其 RU 值和设置集合来跨所有请求处理的请求单位总计的单个请求。 此外可以放大或缩小集合的吞吐量为您的应用程序发展变化的需要。 有关请求单位并帮助确定您的收藏需要请读取[性能管理和容量](documentdb-manage.md)再试[吞吐量计算器](https://www.documentdb.com/capacityplanner)。 

### <a name="is-documentdb-hipaa-compliant"></a>是 DocumentDB HIPAA 兼容？
是的 DocumentDB 是 HIPAA 兼容。 HIPAA 建立使用，披露要求和保护健康的个人识别信息。 有关详细信息，请参阅[Microsoft 信任中心](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA)。

### <a name="what-are-the-storage-limits-of-documentdb"></a>DocumentDB 的存储极限是什么？ 
没有理论的集合可以在 DocumentDB 中存储的数据总量限制。 如果您想要存储超过 250 GB 的单个集合中的数据，请[联系支持部门](documentdb-increase-limits.md)移动到具有增加您帐户的配额。 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>DocumentDB 的吞吐量限制有哪些？ 
您的工作负载可以分布大致均匀地在大量足够大的分区键时没有理论的集合可以在 DocumentDB，支持的吞吐量总量限制。 如果您想要超过 250000 单位每秒每个集合或帐户的请求，请[联系支持部门](documentdb-increase-limits.md)移动到具有您帐户的配额增加了。 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Microsoft Azure DocumentDB 成本是多少？
请[DocumentDB 定价详细信息](https://azure.microsoft.com/pricing/details/documentdb/)页面的详细信息，参阅。 DocumentDB 的使用费由集合在使用中，集合处于联机状态，数数和消耗的存储容量和资源调配的吞吐量为每个集合。 

### <a name="is-there-a-free-account-available"></a>有免费的帐户可用吗？
如果您是新手 Azure，您可以注册[免费的 Azure 帐户](https://azure.microsoft.com/free/)，它为您提供 30 天和 200 美元可以尝试所有的 Azure 服务。 或者，如果您有一个 Visual Studio 订阅，您有资格[中免费的 Azure 信用，每月 150 美元](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)用于任何 Azure 服务。  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>如何获取 DocumentDB 的其他帮助？
如果您需要任何帮助，请给我们动身的[堆栈溢出](http://stackoverflow.com/questions/tagged/azure-documentdb)， [Azure DocumentDB MSDN 开发人员论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)上或安排[与 DocumentDB 的工程团队的 1:1 聊天](http://www.askdocdb.com/)。 保持最新的 DocumentDB 的最新新闻和功能，并遵照我们[使用 Twitter](https://twitter.com/DocumentDB)。

## <a name="set-up-microsoft-azure-documentdb"></a>设置 Microsoft Azure DocumentDB

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>如何注册为 Microsoft Azure DocumentDB？
Microsoft Azure DocumentDB 位于[Azure 门户网站][azure-portal]。  首先您必须注册为 Microsoft Azure 订阅。  注册 Microsoft Azure 订阅后，您可以将 DocumentDB 帐户添加到 Azure 订购。 有关添加 DocumentDB 帐户的说明，请参阅[创建 DocumentDB 数据库帐户](documentdb-create-account.md)。   

### <a name="what-is-a-master-key"></a>主关键是什么？
主密钥是安全令牌访问帐户的所有资源。 与键的个人具有读取和写入数据库帐户中的所有资源的访问。 在分发主密钥时要格外小心。 主键的主服务器和辅助主密钥**密钥**刀片式服务器的[Azure 门户]中有[azure-portal]。 有关密钥的详细信息，请参阅[查看、 复制和再生的访问键](documentdb-manage-account.md#keys)。

### <a name="how-do-i-create-a-database"></a>如何创建数据库？
您可以创建数据库使用[Azure 门户]()[创建 DocumentDB 的数据库](documentdb-create-database.md)，一个[DocumentDB 的 Sdk](documentdb-sdk-dotnet.md)，或通过[REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)中所述。  

### <a name="what-is-a-collection"></a>集合是什么？
集合是 JSON 文档和关联的 JavaScript 应用程序逻辑的容器。 集合是收费的实体，其中[成本](documentdb-performance-levels.md)由吞吐量和 storaged 使用。 集合可以跨一个或多个分区 / 服务器，并且可以扩展以处理无限的容量的存储和传送率。

集合也是 DocumentDB 的计费实体。 每个集合被记帐小时基础调配的吞吐量和使用的存储空间。 有关详细信息，请参阅[DocumentDB 定价](https://azure.microsoft.com/pricing/details/documentdb/)。  

### <a name="how-do-i-set-up-users-and-permissions"></a>如何设置用户和权限？
您可以创建用户和权限是使用一个[DocumentDB Sdk](documentdb-sdk-dotnet.md)或通过[REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)。   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>关于针对 Microsoft Azure DocumentDB 开发的数据库问题

### <a name="how-to-do-i-start-developing-against-documentdb"></a>如何为我启动针对 DocumentDB 开发？
为.NET、 Python，Node.js，JavaScript 和 Java 提供了[Sdk](documentdb-sdk-dotnet.md) 。  开发人员还可以利用[rest 风格的 HTTP Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)与从不同的平台和语言的 DocumentDB 资源进行交互。 

DocumentDB [.NET](documentdb-dotnet-samples.md)、 [Java](https://github.com/Azure/azure-documentdb-java)、 [Node.js](documentdb-nodejs-samples.md)，和[Python](documentdb-python-samples.md) Sdk 示例是在 GitHub 上可用。

### <a name="does-documentdb-support-sql"></a>DocumentDB 不支持 SQL 吗？
DocumentDB SQL 查询语言是增强支持 SQL 的查询功能子集。 DocumentDB SQL 的查询语言提供了丰富的层次结构和关系运算符和扩展性通过 JavaScript 基于用户定义的函数 (Udf)。 JSON 语法允许建模的 JSON 文档具有标签树作为为树中的节点，由 DocumentDB 自动索引技术以及对 SQL 查询语言 DocumentDB。  有关如何使用 SQL 语法的详细信息，请参阅[查询 DocumentDB] [query]文章。

### <a name="what-are-the-data-types-supported-by-documentdb"></a>由 DocumentDB 支持的数据类型有哪些？
在 DocumentDB 中所支持的基元数据类型都相同的 json 格式。 JSON 具有简单类型系统，其中包含字符串、 数字 （IEEE754 双精度） 和布尔值的真，假和空。  日期时间、 Guid、 Int64 和几何等更复杂的数据类型可以通过创建的嵌套对象使用 {} 的运算符和阵列使用 [] 运算符在 JSON 和 DocumentDB 两个表示。 

### <a name="how-does-documentdb-provide-concurrency"></a>DocumentDB 是如何提供并发的？
DocumentDB 支持通过 HTTP 实体标记或 etag 的乐观并发控制 (OCC)。 DocumentDB 中的每个资源都有 etag，和在服务器上设置 etag 当每次更新文档。 所有响应消息中包含 etag 标头和当前值。 Etag 可以与如果匹配标题，用于使服务器能够决定资源应进行更新。 如果匹配值是要对照检查的 etag 值。 如果 etag 值匹配服务器 etag 值，资源将被更新。 如果 etag 不再是当前的服务器将拒绝与操作"HTTP 412 前置条件失败"响应代码。 客户端然后需要重新读取要获取该资源的当前 etag 值的资源。 此外，etag 可与如果无的标头来确定是否需要重新读取资源。 

在.NET 中使用开放式并发，使用[AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx)类。 有关.NET 示例，请参阅[Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs)在 github 上的文档管理示例。

### <a name="how-do-i-perform-transactions-in-documentdb"></a>如何在 DocumentDB 中执行的交易记录？
DocumentDB 支持使用语言集成交易都通过 JavaScript 存储过程和触发器。 在脚本内部的所有数据库操作都执行下作用于该集合是一个单分区集合中，或在集合中，相同的分区键值与文档如果对集合进行分区的快照隔离。 文档版本 (Etag) 的快照是在事务开始时获取并提交仅当该脚本成功完成。 如果 JavaScript 将引发错误，则回滚事务。 有关详细信息，请参阅[DocumentDB 服务器端编程](documentdb-programming.md)。

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>如何插入 DocumentDB 批量插入文档？ 
有三种方法可以到 DocumentDB 批量插入的文档︰

- 数据迁移工具，[将数据导入 DocumentDB](documentdb-import-data.md)中所述。
- 文档资源管理器在 Azure 门户中，[批量添加文档资源管理器的文档](documentdb-view-json-document-explorer.md#BulkAdd)中所述。
- [DocumentDB 服务器端编程](documentdb-programming.md)中所述的存储过程。

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB 支持资源的链接缓存？
是的因为 DocumentDB 是 rest 风格的服务，资源链接是不可变的并可以缓存。 DocumentDB 客户端可以读取任何资源如文档或收集和更新其本地复制仅当服务器版本已更改时才对指定的"如果-无-匹配"头。 

### <a name="is-a-local-instance-of-documentdb-available"></a>提供了 DocumentDB 的本地实例。
这一次 DocumentDB 的本地实例不可用。 您可以跟踪本地仿真程序和[反馈论坛](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance)上的对其进行投票的状态。


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
