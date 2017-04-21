<properties 
    pageTitle="分区和缩放在 Azure DocumentDB |Microsoft Azure"      
    description="了解如何分区工作在 Azure DocumentDB、 如何配置分区和分区键和如何选择您的应用程序的正确的分区键。"         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>分区和 Azure DocumentDB 中的缩放
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)旨在帮助您实现快速、 可预知的性能和可扩展性以及您的应用程序无缝地随着它的发展。 本文将概述如何分区的工作原理，在 DocumentDB，并介绍了如何配置 DocumentDB 集合，能够有效地扩展您的应用程序。

阅读这篇文章之后, 您将能够回答以下问题︰   

- 在 Azure DocumentDB 分区的工作方式是怎样的？
- 如何配置 DocumentDB 中的分区
- 分区键是什么和如何做我选择正确的分区键为我的应用程序？

若要开始使用的代码，请从[DocumentDB 性能测试驱动程序示例](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark)下载的项目。 

## <a name="partitioning-in-documentdb"></a>DocumentDB 中的分区

在 DocumentDB，您可以存储和查询架构灵活的 JSON 文档以任何规模的订单的毫秒的响应时间。 DocumentDB 提供用于存储数据称为**集合**的容器。 集合的逻辑资源，可以跨一个或多个物理分区的服务器。 DocumentDB 基于存储大小和集合资源调配的吞吐量由确定分区数。 在 DocumentDB 中的每个分区有一个固定的 SSD 备份存储与之关联，并复制以实现高可用性。 分区管理完全由 Azure DocumentDB，您无需编写复杂的代码或管理您的分区。 DocumentDB 集合是**无限**存储和吞吐量。 

分区是对应用程序完全透明。 DocumentDB 支持快速读取和写入操作、 SQL 和 LINQ 查询、 JavaScript 基于事务性逻辑、 一致性级别和通过为单个集合资源的 REST API 调用的细粒度访问控制。 此服务跨分区和查询请求进行路由到适当的分区处理分发数据。 

这是如何工作的？ 在 DocumentDB 中创建集合时，您会注意到，没有一个**分区键属性**配置值，您可以指定。 这是 JSON 属性 （或路径），DocumentDB 可用于分发在多个服务器或分区之间数据文档中。 DocumentDB 将哈希分区密钥值，并使用哈希的结果确定 JSON 文档将存储在该分区。 同一个分区键的所有文档将都存储在相同的分区。 

例如，请考虑将有关的员工和他们的部门的数据存储在 DocumentDB 的应用程序。 让我们选择`"department"`作为分区键属性，以便按部门横向扩展数据。 在 DocumentDB 中的每个文档必须包含强制参数`"id"`属性中必须是唯一的每个文档使用相同的分区键值，例如`"Marketing`"。 存储在集合中的每个文档必须具有唯一 id 和分区键的组合如`{ "Department": "Marketing", "id": "0001" }`， `{ "Department": "Marketing", "id": "0002" }`，和`{ "Department": "Sales", "id": "0001" }`。 换句话说，该复合属性的 (分区键，id) 是您收藏的主键。

### <a name="partition-keys"></a>分区键
选择分区键是您必须在设计时进行一项重要决定。 您必须选择一个 JSON 属性名称具有广泛的值并可能具有均匀分布的访问模式。 分区键指定为 JSON 路径，如`/department`表示属性部门。 

下表显示了分区键定义和对应于每个 JSON 值的示例。

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>分区的注册表项路径</strong></p></td>
            <td valign="top"><p><strong>说明</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/department</p></td>
            <td valign="top"><p>值对应的 JSON 的 doc.department 文档所在的文档。</p></td>
        </tr>
        <tr>
            <td valign="top"><p>名称/属性 /</p></td>
            <td valign="top"><p>JSON doc.properties.name 文档所在的文档 （嵌套属性） 的值相对应。</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/id</p></td>
            <td valign="top"><p>对应于 doc.id 的 JSON 值 （id 和分区键是相同的属性）。</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/"部门名称"</p></td>
            <td valign="top"><p>JSON 文档所在的文档的文档 ["部门名称"] 的值相对应。</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] 分区的注册表项路径的语法类似于索引策略与路径相对应的属性而不是值的关键区别的路径的路径规范，即没有任何通配符结尾。 例如，您可以指定/部门 /？ 编制部门下的值，但指定为分区键定义的 /department。 分区的注册表项路径将隐式索引，而不能被索引使用索引策略覆盖。

让我们看一看分区键的选择如何影响应用程序的性能。

### <a name="partitioning-and-provisioned-throughput"></a>分区和调配吞吐量
DocumentDB 专为可预知的性能。 在创建集合时，您将保留吞吐量，以**[请求单位](documentdb-request-units.md)(RU) 每秒**。 每个请求都将分配系统资源，如 CPU 和 IO 操作所消耗的量成正比的请求单位费用。 与会话一致性 1 kB 文档的读取消耗 1 申请单位。 读了 1 RU 而不考虑存储的项数或同时运行的并发请求数。 较大的文档需要更高的请求单位大小。 如果您知道您的实体和需要支持的应用程序的读取次数的大小，您可以设置恰到好处的应用程序的需要读取所需的吞吐量。 

当 DocumentDB 存储文档时，其均匀地分布它们基于分区键值的分区之间。 吞吐量也均匀地分布在可用分区，即每个分区的吞吐量 = （每个集合的总吞吐量） / （分区的数目）。 

>[AZURE.NOTE] 要获得完整的集合的吞吐量，您必须选择分区键，可均匀分布不同的分区键的值的数量之间的请求。

## <a name="single-partition-and-partitioned-collections"></a>一个分区和分区的集
DocumentDB 支持单分区和分区集的创建。 

- **分区集**可以跨越多个分区，支持大量存储和吞吐量。 您必须指定分区键的集合。
- **单分区集合**具有较低的价格选项和查询并在集合中的所有数据之间进行交易的能力。 他们有一个分区的可扩展性和存储限制。 没有指定这些集合的一个分区键。 

![DocumentDB 中的分区的集][2] 

不需要大量的存储或吞吐量的情况下，单个分区集合是非常适合。 请注意单分区集合具有可扩展性和存储限制的多个分区，即 最多 10 GB 的存储空间和高达每秒 10000 个请求单位。 

分区的集合可以支持大量存储和吞吐量。 默认提供程序但是被配置为存储多达 250 gb 的存储空间和扩展达 250000 个请求每秒单位数。 如果您需要更高的存储或每个集合的吞吐量，请联系[Azure 支持](documentdb-increase-limits.md)让这些增加为您的帐户。

下表列出了使用单分区和分区的集的差异︰

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>单个分区集合</strong></p></td>
            <td valign="top"><p><strong>分区的集</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>分区键</p></td>
            <td valign="top"><p>无</p></td>
            <td valign="top"><p>必填</p></td>
        </tr>
        <tr>
            <td valign="top"><p>主关键字的文档</p></td>
            <td valign="top"><p>"id"</p></td>
            <td valign="top"><p>复合键&lt;分区键&gt;和"id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>最小存储</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>最大存储空间</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>无限制 (默认为 250 GB)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>最小的吞吐量</p></td>
            <td valign="top"><p>每秒 400 请求单位</p></td>
            <td valign="top"><p>每秒 10000 个请求单位</p></td>
        </tr>
        <tr>
            <td valign="top"><p>最大吞吐量</p></td>
            <td valign="top"><p>每秒 10000 个请求单位</p></td>
            <td valign="top"><p>无限制 （250000 请求每秒单位数，默认情况下）</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API 版本</p></td>
            <td valign="top"><p>所有</p></td>
            <td valign="top"><p>API 2015-12-16 和更高版本</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>使用 Sdk

Azure DocumentDB 添加了自动分区使用[REST API 版本 2015年-12-16](https://msdn.microsoft.com/library/azure/dn781481.aspx)的支持。 若要创建分区的集合，您必须下载 SDK 版本 1.6.0 或更高版本中支持的 SDK 平台 （.NET Node.js，Java，Python） 之一。 

### <a name="creating-partitioned-collections"></a>创建分区的集合

下面的示例演示.NET 代码段创建集合来存储设备的 20000 申请单位，每秒吞吐的遥测数据。 SDK 设置的 OfferThroughput 值 (这又设置`x-ms-offer-throughput`在 REST API 的请求标头)。 在此我们设置`/deviceId`分区键。 选择分区键的集合元数据，如名称和索引策略的其余部分一起保存。

对于本示例，我们选用`deviceId`因为我们所知的 （a） 由于没有大量的设备，写操作可以均匀地分布在分区和使到我们扩展数据库以摄取大量的数据和 （b） 许多像提取设备最新的读取请求的作用于单个 deviceId 并可以从单个分区。

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] 若要创建分区的集合，必须指定每秒单位数 > 10000 请求吞吐量值。 由于吞吐量 100 的倍数，这必须是 10,100 或更高版本。

此方法使 REST API，叫 DocumentDB，并且该服务将提供基于请求的吞吐量的分区数。 随着您的性能需求的发展，您可以更改集合中的吞吐量。 有关详细信息，请参阅[性能级别](documentdb-performance-levels.md)。

### <a name="reading-and-writing-documents"></a>读取和写入文档

现在，让我们将数据插入到 DocumentDB。 下面是示例类含设备读取，并对 CreateDocumentAsync 的调用插入新设备读取到一个集合。

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


让我们读的分区键和 id，文档更新，并作为最后一步，然后将其删除的分区键和 id。 请注意，读取包含 PartitionKey 值 (对应于`x-ms-documentdb-partitionkey`在 REST API 的请求标头)。

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>查询分区的集

当查询分区集合中的数据时，DocumentDB 自动将查询传递到对应于分区键值 （如果有的话） 的筛选器中指定的分区。 例如，此查询将送交只包含分区键"XMS 0001"的分区。

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

下面的查询没有分区键 (DeviceId) 上的筛选器，则按扇形展开为执行针对该分区的索引所在的所有分区。 注意，您需要指定 EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition`在 REST API) 具有 SDK 可跨分区执行的查询。

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>并行查询执行

DocumentDB Sdk 1.9.0 支持并行查询执行选项，允许您执行低延迟查询针对分区集合，即使它们需要接触大量的分区之上。 例如，下面的查询配置为并行运行跨分区。

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

您可以通过调整以下参数管理并行查询执行︰

- 通过设置`MaxDegreeOfParallelism`，您可以控制即并发网络连接到该集合的分区最大数量的并行度。 如果将此参数设置为-1，并行度由 SDK。 如果`MaxDegreeOfParallelism`并不指定或设置为 0，这是默认值，将有一个网络连接到该集合的分区。
- 通过设置`MaxBufferedItemCount`，您可以换取查询延迟和客户端内存利用率。 如果省略此参数，或将此值设置为-1，由 SDK 管理缓冲的并行查询执行期间的项目数。

给出相同的集合的状态，并行查询将如下所示串行执行相同的顺序返回结果。 当执行跨分区包含的查询的排序 （ORDER BY 和/或顶部），DocumentDB SDK 跨分区问题并行查询，并合并客户端生成全局排序的结果中的部分排序的结果。

### <a name="executing-stored-procedures"></a>执行存储的过程

您还可以执行原子事务文档具有相同的设备 ID，例如如果您要维护聚合或单个文档中的某个设备的最新状态。 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

在下一节我们看如何您可以移动到分区集单分区集合中。

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>从单分区迁移到分区集
当使用单分区集合的应用程序需要更高的吞吐量 (> 10000 的 RU s) 或较大数据存储 (> 10 GB)，您可以使用[DocumentDB 数据迁移工具](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d)将单分区集合中的数据迁移到分区的集合。 

以单分区集合中迁移到分区集

1. 单分区集合中的数据导出到 JSON。 有关其他详细信息，请参阅[导出到 JSON 文件](documentdb-import-data.md#export-to-json-file)。
2. 下面的示例中所示，请将数据导入使用分区键定义和超过 10000 请求第二个吞吐量，单位创建分区集合。 有关其他详细信息，请参阅[导入到 DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) 。

![将数据迁移到 DocumentDB 中的分区集][3]  

>[AZURE.TIP] 为加快导入速度，可考虑增加数的并行请求到 100 或更高版本以利用更高的吞吐量可用于分区的集合。 

现在，我们已经完成了基础知识，让我们看一下几个重要的设计考虑事项时使用 DocumentDB 中的分区键。

## <a name="designing-for-partitioning"></a>分区设计
选择分区键是您必须在设计时进行一项重要决定。 本节介绍一些优缺点选择分区键的集合。

### <a name="partition-key-as-the-transaction-boundary"></a>分区键作为事务边界
您选择的分区键应该平衡能够根据需要跨多个分区键以确保可扩展性的解决方案分发您的实体交易记录使用的需要。 在一个极端情况下，您可以设置相同的分区键的所有文档，但这可能会限制您的解决方案的可伸缩性。 另一种极端情况是，您可以指定每个文档，它会具有高可扩展性，但是会阻止您使用跨文档交易都通过存储的过程和触发器的唯一的分区键。 理想的分区键是，使您可以使用高效的查询并具有足够的基数，以确保您的解决方案是可扩展性。 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>避免存储和性能瓶颈 
还有一点需要选取属性允许写入分布不同值的数量。 对相同的分区键的请求不能超过的吞吐量的单个分区，并且将被控制。 因此务必选择一个分区键，它不会导致应用程序中的**"热点"** 。 具有相同的分区键的文档的总存储大小也不能超过 10 GB 存储中。 

### <a name="examples-of-good-partition-keys"></a>正确的分区键的示例
以下是有关如何选择您的应用程序的分区键的几个示例︰

* 如果您在实施用户配置文件后端，则用户 ID 是分区键的理想选择。
* 如果您正在存储 IoT 数据如设备状态，设备 ID 是用于分区键的不错选择。
* 如果您使用 DocumentDB 时间序列数据进行日志记录，则主机名称或进程 ID 是分区键的理想选择。
* 如果您有一个多租户的体系结构，租户 ID 是一个不错的选择分区键。

请注意，某些使用情况下 （如上文所述 IoT 和用户配置文件） 中，分区键可能与您的 id （文档键） 相同。 在其他类似的时间序列数据，您可能有不同 id 的分区键。

### <a name="partitioning-and-loggingtime-series-data"></a>分区和日志记录/时间系列数据
DocumentDB 的最常见的使用案例之一就是记录和遥测。 务必要挑选正确的分区键，因为您可能需要读取/写入大量数据。 选择将取决于您读和写速率以及您希望运行的查询的类型。 以下是一些提示有关如何选择正确的分区键。

- 如果您使用的案例涉及的小速率写入 acculumating 长的一段时间，并需要通过时间戳的范围查询和其他筛选器，然后使用的时间戳的汇总如日期分区键是一种好方法。 这允许您查询对所有数据的日期从单个分区。 
- 如果您的工作负载是写粗，通常更常见的是，应使用不基于时间戳，以便 DocumentDB 可以将均匀分布在多个分区的分区键。 下面是主机名、 进程 ID、 活动 ID 或高基数与另一个属性是一个不错的选择。 
- 第三种方法是的混合体一个其中有多个集合，另一个用于每一天/月，分区键是像主机名的细粒度属性。 这样做的好处，您可以设置基于时间窗口的不同的性能级别，例如当前月的集合提供更高的吞吐量因为它服务于读取和写入操作，而与前一个月降低吞吐量，因为它们仅用于读取。

### <a name="partitioning-and-multi-tenancy"></a>分区和多租户
如果要实现使用 DocumentDB 的多租户应用程序，有两个主要模式实现组织与 DocumentDB – 租户，每一个分区键和租户每个集合。 以下是优点和缺点每个︰

* 一个分区键，每个租户︰ 在此模型中，承租人搭配在单个集合中。 但可以执行单个分区的查询和插入单组织内的文档。 您还可以在组织内的所有文档实现事务性逻辑。 由于多个租户共享集合，您可以节省存储和吞吐量成本池以承租人在单个集合中的资源，而不是为每个租户调配额外预留空间。 缺点是没有性能隔离每个租户。 性能/吞吐量增加适用于整个集合与目标的增加的承租人。
* 一个集合，每个租户︰ 每个租户有它自己的集合。 在此模型中，您可以保留每个租户的性能。 使用 DocumentDB 的新基于消耗定价模型，此模型是多租户应用程序数目较少的租户提供更有成本效益的。

您还可以使用组合/分层配置小租户，将大租户迁移到各自集合的方法。

## <a name="next-steps"></a>下一步行动
在本文中，我们已经介绍如何分区在 Azure DocumentDB 的工作原理、 如何创建分区的集合，并为您的应用程序，您就可以选择正确的分区键。 

-   规模和性能测试与 DocumentDB 执行。 一个示例，请参阅[性能和规模测试使用 Azure DocumentDB](documentdb-performance-testing.md) 。
-   开始使用[Sdk](documentdb-sdk-dotnet.md)或[REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)编码
-   了解[DocumentDB 中的调配的吞吐量](documentdb-performance-levels.md)
-   如果您想要自定义应用程序如何执行分区，您可以插入自己的客户端分区实现。 请参阅[客户端分区支持](documentdb-sharding.md)。

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
