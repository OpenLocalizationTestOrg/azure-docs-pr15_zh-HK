<properties 
    pageTitle="DocumentDB 分层资源模型和概念 |Microsoft Azure" 
    description="了解 DocumentDB 的层次结构模型的数据库、 集合、 用户定义函数 (UDF)、 文档、 管理资源和更多的权限。"
    keywords="分层模型，documentdb，azure，Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>DocumentDB 分层资源模型和概念

DocumentDB 管理的数据库实体被称为**资源**。 逻辑的 URI 是唯一标识每个资源。 您可以使用标准的 HTTP 谓词、 请求/响应头和状态代码中的资源进行交互。 

通过阅读本文，您将能够回答以下问题︰

- DocumentDB 的资源模型是什么？
- 什么是系统定义而不是用户定义资源的资源？
- 如何解决资源？
- 如何处理集合？
- 如何处理存储的过程、 触发器和用户定义函数 (Udf)

## <a name="hierarchical-resource-model"></a>分层资源模型
如下面的关系图所示，DocumentDB 分层**资源模型**由套在数据库帐户，每个可寻址通过逻辑和稳定的 URI 的资源。 一组资源将被称为**源**在这篇文章。 

>[AZURE.NOTE] DocumentDB 提供了高效的 TCP 协议中的通信模型，可通过[.NET 客户端 SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)也有 RESTful。

![DocumentDB 资源层次结构模型][1]  
**分层资源模型**   

要开始使用资源，则必须[创建一个 DocumentDB 数据库帐户](documentdb-create-account.md)使用 Azure 订购。 数据库帐户可以包含一套**数据库**，每个都包含多个**集合**，其中每个又包含**存储过程、 触发时，Udf，文档**及相关**附件**（预览功能）。 数据库也有关联的**用户**，每个都有一组的**权限**来访问集合、 存储的过程、 触发器、 Udf、 文档或附件。 尽管数据库、 用户、 权限和集合是已知的架构与系统定义的资源，文档和附件包含任意、 用户定义的 JSON 内容。  

|资源   |说明
|-----------|-----------
|数据库帐户   |数据库帐户都使用一套数据库和一个固定的大小的附件 （预览功能） 的 blob 存储相关联。 您可以创建一个或多个数据库帐户使用 Azure 订购。 有关详细信息，请访问我们的[定价页](https://azure.microsoft.com/pricing/details/documentdb/)。
|数据库   |数据库是跨集合划分文档存储的逻辑容器。 它也是一个用户容器。
|用户   |逻辑命名空间的作用域的权限。 
|权限 |授权标记关联的用户对特定资源的访问。
|集合 |集合是 JSON 文档和关联的 JavaScript 应用程序逻辑的容器。 集合是一个收费的实体，其[成本](documentdb-performance-levels.md)由与该集合关联的性能级别。 集合可以跨一个或多个分区 / 服务器，并且可以扩展以处理无限的容量的存储和传送率。
|存储的过程   |用集合注册和事务在数据库引擎中执行的 JavaScript 编写的应用程序逻辑。
|触发器    |在执行之前或之后插入的 JavaScript 编写的应用程序逻辑替换或删除操作。
|UDF    |用 JavaScript 编写的应用程序逻辑。 Udf 可以模型的自定义查询运算符，因此可将 DocumentDB 查询语言的核心。
|文档   |用户定义的 （任意） JSON 内容。 默认情况下，任何架构需要定义也不做辅助索引需要提供用于添加到集合中的所有文档。
|（预览）附件   |附件是包含引用和关联的元数据对于外部的 blob 介质的特殊文档。 开发人员可以选择已由 DocumentDB 管理的 blob 或将其存储与 OneDrive、 收存箱等外部 blob 服务提供商。 


## <a name="system-vs-user-defined-resources"></a>与用户定义的资源的系统
资源，例如数据库帐户、 数据库、 集合、 用户、 权限、 存储的过程、 触发器和 Udf 的所有具有固定的模式，称为系统资源。 与此相反的是，资源，如文档和附件在架构上有没有限制，都属于用户定义的资源。 在 DocumentDB，系统和用户定义资源的表示和管理符合标准的 json 格式。 所有的资源，系统或用户定义的具有以下的通用属性。

> [AZURE.NOTE] 注意所有系统都生成属性的资源为其 JSON 表示中下划线 (_) 前缀。

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>属性</strong></p></td>
            <td valign="top"><p><strong>可设置用户还是由系统生成？</strong></p></td>
            <td valign="top"><p><strong>目的</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>系统生成</p></td>
            <td valign="top"><p>系统生成的唯一和分层的资源标识符</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>系统生成</p></td>
            <td valign="top"><p>开放式并发控制所需资源 etag</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>系统生成</p></td>
            <td valign="top"><p>资源的上次更新时间戳</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>系统生成</p></td>
            <td valign="top"><p>可寻址的资源 URI 唯一</p></td>
        </tr>
        <tr>
            <td valign="top"><p>标识</p></td>
            <td valign="top"><p>系统生成</p></td>
            <td valign="top"><p>用户定义的唯一名称 （具有相同的分区键值） 的资源。 如果用户未指定 id，id 将生成系统</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>线表示的资源
DocumentDB 并不要求任何专有的扩展到 JSON 的标准或特殊编码;它适用于标准兼容的 JSON 文档。  
 
### <a name="addressing-a-resource"></a>解决资源
所有资源都是可寻址的 URI。 资源的**_self**属性值表示相对 URI 的资源。 URI 的格式组成 /\<进\>/ {_rid} 路径段︰  

|_Self 的值 |说明
|-------------------|-----------
|/dbs   |在数据库帐户数据库的数据源
|/dbs/ {dbName}  |数据库 id 匹配的值 {dbName}
|/colls/ /dbs/ {dbName}   |在数据库下的集合的数据源
|/dbs/ {dbName} /colls/ {collName} |集合 id 匹配的值 {collName}
|/dbs/ {dbName} {collName} /colls/ / 文档    |数据源的文档集合的
|/dbs/ {dbName} {collName} /colls/ /docs/ {文档 Id}    |文档 id 匹配的值 {doc}
|/users/ /dbs/ {dbName}   |下一个数据库用户的订阅源
|/dbs/ {dbName} /users/ {用户 Id}   |用户 id 匹配的值 {用户}
|/dbs/ {dbName} {用户 Id} /users/ / 权限   |在用户权限的数据源
|/dbs/ {dbName} {用户 Id} /users/ /permissions/ {permissionId}    |Id 为 {0} 权限} 值匹配的权限
  
每个资源都有通过 id 属性公开一个唯一的用户定义名称。 注意︰ 对于文档，如果用户未指定一个 id，我们支持的 Sdk 将自动生成的唯一 id 的文档。 Id 是用户定义的字符串，最多 256 个字符的特定父资源的上下文中是唯一。 

每个资源还具有系统生成分层资源标识符 （也称为 RID），可通过 _rid 属性。 RID 进行编码的给定资源的整个层次结构，用于分布式方式实施参照完整性的方便内部表示形式。 RID 内是唯一的数据库帐户，它内部由 DocumentDB 为而无需跨分区查找有效的路由。 _Self 和 _rid 属性的值是资源的替代并规范表示形式。 

DocumentDB REST Api 支持资源的寻址和路由的请求的 id 和 _rid 属性。

## <a name="database-accounts"></a>数据库帐户
您可以配置一个或多个 DocumentDB 数据库帐户使用 Azure 订购。

您可以[创建和管理数据库帐户 DocumentDB](documentdb-create-account.md)通过在[http://portal.azure.com/](https://portal.azure.com/)Azure 门户。 创建和管理数据库帐户需要管理访问权限，才可以在 Azure 订购下执行。 

### <a name="database-account-properties"></a>数据库帐户属性
作为资源调配和管理数据库帐户的一部分，您可以配置和阅读以下属性︰  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>属性名称</strong></p></td>
            <td valign="top"><p><strong>说明</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>策略一致性</p></td>
            <td valign="top"><p>设置此属性以配置数据库帐户下的所有集合的默认一致性级别。 您可以重写在每次请求都使用 [x-ms-一致性-级别] 请求标头的一致性级别。 <p><p>请注意，此属性仅适用于<i>用户定义的资源</i>。 已定义的资源被配置为支持所有系统读取/查询具有强的一致性。</p></td>
        </tr>
        <tr>
            <td valign="top"><p>授权码</p></td>
            <td valign="top"><p>这些是主要和辅助主机和只读键提供对所有数据库帐户下的资源管理访问权限。</p></td>
        </tr>
    </tbody>
</table>

请注意，除了资源调配、 配置和管理从 Azure 门户数据库帐户，您可以以编程方式创建和管理使用[Azure DocumentDB REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)以及[客户端 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)的 DocumentDB 数据库帐户。  

## <a name="databases"></a>数据库
DocumentDB 数据库是一种逻辑容器的一个或多个集合和用户，如下图中所示。 您可以创建任意数量的 DocumentDB 数据库帐户受优惠限制下的数据库。  

![数据库帐户和集合的层次结构模型][2]  
**数据库是用户和集合的逻辑容器**

一个数据库可以包含几乎不受限制的文档存储分区的集合，形成交易域内包含的文档。 

### <a name="elastic-scale-of-a-documentdb-database"></a>灵活的伸缩的 DocumentDB 数据库
DocumentDB 数据库是默认情况下----从几 GB 到 SSD 备份文档存储和资源调配的吞吐量 petabytes 弹性。 

与传统的 RDBMS 中的数据库，数据库 DocumentDB 中的不限于一台计算机。 与 DocumentDB，随着应用程序的规模需求的增长，您可以创建多个集合和 / 或数据库。 事实上，各种的第一方应用程序在 Microsoft 内部一直使用 DocumentDB 在使用者比例由创建非常大的 DocumentDB 数据库集合中的每个包含数千数万亿字节的文档存储。 您可以扩大或通过添加或删除收藏集来满足应用程序的规模收缩数据库。 

您可以创建任意数量的提议根据数据库中的集合中。 每个集合都有备份的 SSD 存储和根据选定的性能层设置为您的吞吐量。

DocumentDB 数据库也是用户的容器。 用户，在转弯时提供细粒度的授权和访问集合、 文档和附件的一组权限的逻辑命名空间。  
 
使用 DocumentDB 资源模型中的其他资源，如数据库可以创建、 替换、 删除、 读取或轻松地使用[Azure DocumentDB REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)或任何[客户端 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)枚举。 DocumentDB 可以保证强一致性，以进行读取或查询数据库资源的元数据。 自动删除数据库，确保您不能访问的任何集合或它所包含的用户。   

## <a name="collections"></a>集合
DocumentDB 集合是 JSON 文档的容器。 集合也是比例的单位为交易记录和查询。 

### <a name="elastic-ssd-backed-document-storage"></a>弹性 SSD 备份文档存储
集合是本质上弹性-它会自动增长和收缩添加或删除文档。 集合的逻辑资源，可以跨一个或多个物理分区的服务器。 由基于存储大小和集合资源调配的吞吐量 DocumentDB 确定集合中的分区数。 在 DocumentDB 中的每个分区有一个固定的 SSD 备份存储与之关联，并复制以实现高可用性。 分区管理完全由 Azure DocumentDB，您无需编写复杂的代码或管理您的分区。 DocumentDB 集合是**无限**存储和吞吐量。 

### <a name="automatic-indexing-of-collections"></a>自动索引的集合
DocumentDB 是一个真正无架构的数据库系统。 它不假定或要求任何架构的 JSON 文档。 当您将文档添加到集合中，DocumentDB 自动索引和它们可用于您查询。 自动索引的文档而无需架构或辅助索引是 DocumentDB 的主要功能并启用了写优化、 无锁和日志结构索引维护技术。 DocumentDB 支持持续的同时仍提供一致的查询速度极快写入卷。 文档和索引存储用于计算由每个集合的存储。 您可以控制存储和性能权衡与索引配置集合的索引策略。 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>配置集合的索引策略
每个集合的索引策略，可使性能和存储的利弊与索引相关联。 以下选项可供您作为索引配置的一部分︰  

-   选择是否集合自动索引的所有文档或不。 默认情况下，所有文档会自动建立都索引。 您可以选择关闭自动索引和有选择地向索引中添加特定的文档。 相反，您可以有选择地选择中排除特定的文档。 您可以通过设置自动属性设置为 true 或 false 集合的 indexingPolicy 上，插入、 替换或删除文档时使用 [x-ms-indexingdirective] 请求标头来达到此目的。  
-   选择是否要包括或从索引中排除特定路径或在文档中的模式。 您可以达到此目的通过设置 includedPaths 和 excludedPaths 在 indexingPolicy 集合的分别。 您还可以配置存储和性能权衡范围和散列查询特定路径模式。 
-   （一致） 选择之间同步和异步 （惰性） 索引更新。 默认情况下，对每个插入、 替换或删除的集合的文档同步更新索引。 这使得接受文档读取操作的一致性级别相同的查询。 而 DocumentDB 是写优化并支持持续的增长的文档写入，以及同步的索引维护和提供一致的查询服务，您可以配置某些延迟更新其索引的集合。 惰性索引写入性能进一步提高，非常适合于批量接收方案主要是读大量的收藏。

可以通过在集合上执行 PUT 更改索引策略。 这可以实现通过[客户端 SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)， [Azure 门户](https://portal.azure.com)或[Azure DocumentDB REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)。

### <a name="querying-a-collection"></a>查询集合
在一个集合中的文档可以具有任意架构，您可以无需提供任何架构或提前辅助索引查询在一个集合中的文档。 您可以查询集合使用空间运算符[DocumentDB SQL 语法](https://msdn.microsoft.com/library/azure/dn782250.aspx)，它提供了丰富的层次结构或关系，并通过基于 JavaScript 的 Udf 的可扩展性。 JSON 语法可以建模为树节点的标签作为树的 JSON 文档。 这被利用同时通过 DocumentDB 的自动索引技术，以及 DocumentDB 的 SQL 方言。 DocumentDB 查询语言包括三个主要方面︰   

1.  一小套自然映射到树状结构包括分层查询和预测的查询操作。 
2.  包括组合、 筛选、 投影、 聚合和自我联接的关系操作的一个子集。 
3.  纯 JavaScript 基于使用的 Udf （1） 和 (2)。  

DocumentDB 查询模型尝试的功能、 效率和简单性之间达到平衡。 DocumentDB 数据库引擎在本机编译和执行的 SQL 查询语句。 您可以查询使用[Azure DocumentDB REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)或任何[客户端 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)的集合。 .NET SDK 附带 LINQ 提供程序。

> [AZURE.TIP] 可以尝试使用 DocumentDB，并对[查询运动场](https://www.documentdb.com/sql/demo)中我们数据集执行 SQL 查询。

### <a name="multi-document-transactions"></a>多文档交易记录
数据库事务处理数据的并发更改提供安全和可预测的编程模型。 在 RDBMS 中，编写业务逻辑的传统方法是编写**存储过程**和/或**触发器**并交给事务执行数据库服务器。 在 RDBMS 中，应用程序编程人员需要处理两个不同的编程语言︰ 

- 编程语言 （如 JavaScript，Python、 C#、 Java 等） （非事务性） 应用程序
- T-SQL，事务性的编程语言，以本机方式执行数据库

通过直接在数据库引擎中深入致力于 JavaScript 和 JSON，DocumentDB 提供一个直观的编程模型直接在存储的过程和触发器的集合上执行基于的 JavaScript 应用程序逻辑。 这样，对于以下两种︰

- 高效地实现并发控制，恢复，自动 JSON 对象图，直接在数据库引擎中的索引
- 很自然地表达控制流、 可变范围、 分配和集成的异常处理具有直接的 JavaScript 编程语言的数据库事务基元

在集合级别注册的 JavaScript 逻辑即可颁发在单据上的给定集合的数据库操作。 DocumentDB 隐式包装 JavaScript 基于跨文档集合中所有存储过程和触发器中使用快照隔离环境 ACID 事务。 其执行期间，如果 JavaScript 将引发一个异常，然后整个事务被中止。 生成的编程模型是一个非常简单但功能强大。 JavaScript 开发人员仍然使用他们熟悉的语言构造和库基元时得到"持久"的编程模型。   

直接在数据库引擎作为缓冲池中的同一地址空间中执行 JavaScript 的能力使性能和文档集合的数据库操作的事务执行。 此外，DocumentDB 数据库引擎做为 JSON 的深度承诺，JavaScript 消除任何类型的系统的应用程序和数据库之间的阻抗不匹配。   

在创建集合之后, 可以注册存储的过程、 触发器和 Udf 集合使用[Azure DocumentDB REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)或任何[客户端 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)。 在注册后，可以引用并执行它们。 请考虑以下完全用 JavaScript 编写的存储的过程，下面的代码采用两个参数 （簿名称和作者姓名） 和创建一个新文档，文档中查询并将其 – 隐式的 ACID 事务内的所有更新。 在执行过程中的任何时刻，如果 JavaScript 异常，整个事务中止。

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

客户端可以"运送"到通过 HTTP 发送的事务执行的数据库上面的 JavaScript 逻辑。 有关使用 HTTP 方法的详细信息，请参阅[DocumentDB 资源与 RESTful 交互](https://msdn.microsoft.com/library/azure/mt622086.aspx)。 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


请注意，数据库本身理解 JSON 和 JavaScript，因为没有任何类型系统不匹配、"或映射"或所需的代码生成幻。   

存储的过程和触发器与集合和集合通过一个定义完善的对象模型，它公开当前集合上下文中的文档进行交互。  

DocumentDB 中的集合可以创建、 删除、 读取或枚举轻松地使用[Azure DocumentDB REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)或任何[客户端 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)。 DocumentDB 总是提供以进行读取或查询集合中的元数据的强一致性。 自动删除集合确保您不能访问任何文档、 附件、 存储的过程、 触发器和 Udf 中。   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>存储的过程、 触发器和用户定义函数 (UDF)
如前一节中所述，您可以编写应用程序逻辑，来直接在数据库引擎在事务中运行。 应用程序逻辑可以完全用 JavaScript 编写，可以建模为一个存储的过程、 触发器或 UDF。 在存储的过程或触发器中的 JavaScript 代码可以插入、 替换、 删除、 读取或查询文档集合中。 另一方面，在 UDF 中的 JavaScript 不能插入、 替换或删除文档。 Udf 枚举查询结果集的文档，并生成另一个结果集。 对于多租户，DocumentDB 强制实施严格的保留基于资源治理。 每个存储过程、 触发器或 UDF 获取固定的量程的操作系统资源来完成其工作。 此外，存储的过程、 触发器或 Udf 无法链接针对外部 JavaScript 库，如果他们超过分配给它们的资源预算被列入黑名单。 您可以注册，请使用 REST Api 集合注销存储的过程、 触发器或 Udf。  在注册时是预编译的存储的过程、 触发器或 UDF 并将其存储为其获取稍后执行的字节码。 下一节说明了如何使用 DocumentDB JavaScript SDK 进行注册、 执行和取消注册存储的过程、 触发器和 UDF。 JavaScript SDK 是一个简单的包装对[DocumentDB REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)。 

### <a name="registering-a-stored-procedure"></a>注册存储的过程
注册存储过程通过 HTTP 发送的集合中创建新的存储的过程资源。  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>执行存储的过程
存储过程的执行是通过发出针对现有存储的过程资源 HTTP 发送通过将参数传递给请求正文中的过程。

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>注销存储的过程
注销存储的过程只是通过发出针对现有存储的过程资源删除 HTTP。   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>注册前的触发器
触发器的登记是通过通过 HTTP 发送的集合中创建新触发器资源。 您可以指定是否起因是前或后触发器和操作的类型可以是关联 （例如创建、 替换、 删除或全部）。   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>执行预触发器
触发器的执行是通过时的开机自检/传/删除请求的文档资源通过请求标头中指定现有的触发器的名称。  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>注销前触发器
注销触发器只是通过发出针对现有触发器资源删除 HTTP 完成的。  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>注册 UDF
通过 HTTP 发送的集合中创建新 UDF 资源进行注册的 UDF。  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>执行作为查询的一部分的 UDF
UDF 可以被指定为 SQL 查询的一部分，并可作为一种方法来扩展核心[SQL 查询语言 DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx)。

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>注销 UDF 
注销 UDF 只是通过发出针对现有 UDF 资源删除 HTTP。  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

尽管上面的代码段演示了 （开机自检） 注册、 注销 (PUT)、 读取/列表 (GET) 和执行 (POST) 通过[DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js)，您还可以使用[REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)或其他[客户端 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)。 

## <a name="documents"></a>文档
您可以插入、 替换、 删除、 读取、 枚举和查询集合中的任意 JSON 文档。 DocumentDB 并不要求任何架构，不需要辅助索引以支持在文档集合中的查询。   

作为一个真正的开放数据库服务，DocumentDB 不创造任何专用的数据类型 （如日期时间） 或特定编码的 JSON 文档。 请注意，DocumentDB 不需要任何特殊的 JSON 约定以 codify 各种单据; 之间的关系DocumentDB 的 SQL 语法提供了非常强大的层次结构和关系查询运算符的查询和项目文档而无需任何特殊注释或需要 codify 使用文档之间的关系区分属性。  
 
作为与所有其他资源，可以创建文档，替换、 删除、 读取、 枚举和查询使用 REST Api 或任何[客户端 Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)轻松。 立即删除文档可以释放所有嵌套附件所对应的配额。 读的一致性级别的文档数据库帐户上遵循一致性策略。 可以重写此策略取决于您的应用程序的数据一致性要求基于每个请求。 当查询的文档中，读的一致性遵循设置集合的索引模式。 为"一致"，这将遵循该帐户的一致性策略。 

## <a name="attachments-and-media"></a>附件和媒体
>[AZURE.NOTE] 附件和媒体资源的预览功能。
 
DocumentDB 允许您存储二进制 blob/媒体与 DocumentDB 或到远程媒体存储区。 它还允许您以表示特殊文档附件的媒体的元数据。 DocumentDB 中的附件是特殊的 (JSON) 文档引用媒体/blob 存储在其他位置。 附件是只捕获存储远程媒体存储器中的媒体的元数据 （例如，位置、 作者等） 的特殊的文档。 

考虑社会阅读应用程序，它使用 DocumentDB 来存储墨迹注释和元数据包括注释、 突出显示，书签、 分级、 喜欢/厌恶关联的指定用户的电子书等。   

-   书籍本身的内容存储在介质存储既可用作远程媒体存储或 DocumentDB 数据库帐户的一部分。 
-   应用程序可以为不同的文档存储每个用户的元数据--例如 book1 乔的元数据存储在由 /colls/joe/docs/book1 引用的文档。 
-   指向用户的特定书的内容页的附件存储在相应的文档如 /colls/joe/docs/book1/chapter1，/colls/joe/docs/book1/chapter2 等。 

请注意，上面列出的示例使用友好的 id 来传达的资源层次结构。 通过唯一的资源 id 通过 REST Api 访问的资源。 

由 DocumentDB 管理的媒体，附件的 _media 属性将引用的 URI 由媒体。 DocumentDB 将确保对其进行垃圾收集媒体时所有未完成的引用被删除。 DocumentDB 自动生成附件上传新媒体时，填充 _media 以指向新添加的介质。 如果您选择您 （例如 OneDrive，Azure 存储收存箱等） 管理的远程 blob 存储区中存储介质，您仍可以使用附件来引用媒体。 在这种情况下，将自己创建附件并填充它的 _media 属性。   

与所有其他资源，附件可以创建、 替换、 删除、 读取或轻松地使用 REST Api 或任何客户端 Sdk 的枚举。 与文档附件的读的一致性级别数据库帐户上遵循一致性策略。 可以重写此策略取决于您的应用程序的数据一致性要求基于每个请求。 当查询附件，读的一致性遵循设置集合的索引模式。 为"一致"，这将遵循该帐户的一致性策略。 
 
## <a name="users"></a>用户
DocumentDB 用户表示对权限进行分组的逻辑命名空间。 DocumentDB 用户可以对应用户身份管理系统或预定义的应用程序角色。 对于 DocumentDB，用户只需表示抽象权限数据库下一组。   

为实现多租户应用程序中，可以在对应的 DocumentDB 中创建用户，到实际用户或应用程序的承租人。 然后，您可以创建给定用户对应于不同的集合、 文档、 附件等的访问控制权限。   

根据您的应用程序需要用户增长而扩展，您的数据可以采用 shard 的各种方法。 您可以建模，每个用户，如下所示︰   

-   每个用户映射到数据库中。
-   每个用户将映射到集合中。 
-   文档对应于多个用户转到一个专用集合。 
-   文档对应于多个用户转到一组集合。   

无论您选择特定的分片策略，可以为 DocumentDB 数据库中的用户模型实际用户和关联精细到每个用户的权限。  

![用户集合][3]  
**分片策略和建模用户**

所有其他资源，如 DocumentDB 中的用户可以创建、 替换、 删除、 读取或轻松地使用 REST Api 或任何客户端 Sdk 枚举。 DocumentDB 总是提供以进行读取或查询元数据的用户资源的强一致性。 值得指出的，自动删除用户可确保您不能访问任何包含在它的权限。 即使 DocumentDB 回收作为背景中已删除的用户的权限配额，已删除的权限就可立即供您使用。  

## <a name="permissions"></a>权限
从访问控制角度来看，资源数据库帐户，如数据库、 用户和权限被视为*管理*资源由于这些需要管理权限。 另一方面，资源包括集合、 文档、 附件、 存储的过程、 触发器和 Udf 是下给定数据库范围内，考虑*应用程序的资源*。 对应于两种类型的资源和访问它们 （即管理员和用户） 的角色，授权模型定义的*访问键*的两种类型︰*主密钥*和*资源键*。 主密钥是数据库帐户的一部分，提供给开发人员 （或管理员） 谁资源调配的数据库帐户。 该主密钥具有管理员语义，因为它可以用于授权管理和应用程序资源的访问。 与此相反，资源键是一个细粒度的访问键，它允许对*特定*应用程序资源的访问。 因此，它捕获数据库的用户和权限的用户具有对特定资源 （如集合、 文档、 附件、 存储的过程、 触发器或 UDF） 之间的关系。   

获取一个资源键的唯一方法是通过创建下给定用户的权限资源。 请注意，若要创建或检索一个权限，必须授权标头中显示一个主密钥。 资源权限将资源、 其访问权限和用户。 创建权限资源之后, 用户只需提供相关联的资源键来访问相关的资源。 因此，逻辑和简洁的权限资源表示形式可以查看的资源键。  

与所有其他资源，如 DocumentDB 中的权限可以创建、 替换、 删除、 读取或轻松地使用 REST Api 或任何客户端 Sdk 枚举。 DocumentDB 总是提供强的一致性，以进行读取或查询元数据的权限。 

## <a name="next-steps"></a>下一步行动
了解有关在[使用 DocumentDB 资源 RESTful 交互](https://msdn.microsoft.com/library/azure/mt622086.aspx)使用 HTTP 命令使用的资源。


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

