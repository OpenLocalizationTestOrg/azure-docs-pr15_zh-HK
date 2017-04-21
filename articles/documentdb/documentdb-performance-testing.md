<properties 
    pageTitle="DocumentDB 规模和性能测试 |Microsoft Azure" 
    description="学习如何执行的规模和性能测试与 Azure DocumentDB"
    keywords="性能测试"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>性能和可扩展性测试使用 Azure DocumentDB
性能和规模测试是应用程序开发中的一个关键步骤。 对于许多应用程序，数据库层具有重大影响的整体性能和可扩展性，并因此是性能测试的一个重要组成部分。 [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)是专为弹性比例和可预知的性能，并因此极适合需要高性能的数据库层的应用程序。 

这篇文章是针对开发人员实现性能测试套件，其 DocumentDB 的工作负载，或用于高性能应用程序方案评估 DocumentDB 的引用。 它主要侧重于独立的性能测试的数据库中，但还包括用于生产应用程序的最佳做法。

阅读这篇文章之后, 您将能够回答以下问题︰   

- 在哪里可以找到一个示例.NET 客户端应用程序进行性能测试的 Azure DocumentDB？ 
- 如何从客户端应用程序实现高吞吐量级别与 Azure DocumentDB？

入门的代码，请从[DocumentDB 性能测试示例](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)下载的项目。 

> [AZURE.NOTE] 此应用程序的目标是演示用于客户机的数量很少提取 DocumentDB 更好性能的最佳做法。 这不，进行了展示服务，它可以扩展 limitlessly 的高峰产能。

如果您正在寻找提高 DocumentDB 性能的客户端配置选项，请参阅[DocumentDB 性能提示](documentdb-performance-tips.md)。

## <a name="run-the-performance-testing-application"></a>运行测试应用程序的性能
入门的最快方法是编译并运行下面，.NET 示例下面的步骤中所述。 此外可以查看源代码，然后向客户端应用程序中实现类似的配置。

**第 1 步︰**下载[DocumentDB 性能测试示例](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)，从该项目或分叉 Github 资料库。

**第 2 步︰**修改为 EndpointUrl、 AuthorizationKey、 CollectionThroughput 和 DocumentTemplate （可选） 在 App.config 的设置。

> [AZURE.NOTE] 之前提供具有高吞吐量的集合，请参阅[定价页](https://azure.microsoft.com/pricing/details/documentdb/)来估计每个集合的成本。 DocumentDB 账单存储和吞吐量分别按每小时，以便您可以通过删除或降低 DocumentDB 集合的吞吐量，测试完成后保存成本。

**第 3 步︰**编译并运行控制台应用程序从命令行。 您应该看到类似于下面的输出︰

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**步骤 4 （如果需要）︰**吞吐量报告 (RU/s) 从工具应等于或大于集合中的资源调配的吞吐量。 如果不是，增加一点点 DegreeOfParallelism 可以帮助您达到该极限值。 如果从客户端应用程序的吞吐量 plateaus，则启动该应用程序在相同或不同机器上的多个实例将帮助您跨不同实例达到设置的限制。 如果您需要此步骤的帮助，请写一封电子邮件到askdocdb@microsoft.com或填充的支持票。

运行该应用程序之后，您可以尝试不同的[索引策略](documentdb-indexing-policies.md)和[一致性级别](documentdb-consistency-levels.md)，以了解它们对吞吐量和滞后时间的影响。 此外可以查看源代码，然后实现类似的配置您自己的测试套件或生产应用程序。

## <a name="next-steps"></a>下一步行动
在本文中，我们看您如何执行性能和可扩展性，使用 DocumentDB 使用.NET 控制台应用程序进行测试。 请参阅下面的链接以获取有关使用 DocumentDB。

* [DocumentDB 的性能测试示例](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [客户端配置选项来改善 DocumentDB](documentdb-performance-tips.md)
* [服务器端 DocumentDB 中的分区](documentdb-partition-data.md)
* [DocumentDB 集合和性能级别](documentdb-performance-levels.md)
* [在 MSDN 上的 DocumentDB.NET SDK 文档](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB.NET 示例](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB 博客上性能提示](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
