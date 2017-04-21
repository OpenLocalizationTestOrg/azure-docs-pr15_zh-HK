<properties 
    pageTitle="使用 Order By DocumentDB 数据排序 |Microsoft Azure" 
    description="了解如何使用 ORDER BY DocumentDB 查询中 LINQ 和 SQL，以及如何指定 ORDER BY 的查询的索引策略。" 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>使用 Order By 的 DocumentDB 数据进行排序
Microsoft Azure DocumentDB 支持 JSON 文档通过使用 SQL 查询的文档。 可以在 SQL 查询语句中使用 ORDER BY 子句对查询结果进行排序。

阅读这篇文章之后, 您将能够回答以下问题︰ 

- 如何使用 Order By 查询？
- 如何为 Order By 配置索引创建策略？
- 什么接下来？

此外提供了[示例](#samples)和[常见问题解答](#faq)。

在 SQL 查询的完整参考资料，请参阅[DocumentDB 查询方法辅导教程](documentdb-sql-query.md)。

## <a name="how-to-query-with-order-by"></a>如何按顺序与查询
像在 ANSI SQL 的现在可以在查询 DocumentDB 时包括可选 Order By 子句在 SQL 语句中。 子句还可以包括一个可选的升序/降序参数，以指定必须在其中检索结果的顺序。 

### <a name="ordering-using-sql"></a>使用 SQL 排序
例如，这里有一个查询以检索按降序头衔的十大书籍。 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>排序与筛选中使用 SQL
可以使用 Books.ShippingDetails.Weight，如文档内任何嵌套的属性进行排序，您可以在 WHERE 子句中使用 Order By 的组合如在此示例中指定其他筛选器︰

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>针对.NET 使用 LINQ 提供程序的顺序
使用.NET SDK 版本 1.2.0 和更高，您还可以在此示例中使用类似的 LINQ 查询中使用 OrderBy() 或 OrderByDescending() 子句︰

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB 支持与其他查询类型即将订购单个数值、 字符串或布尔值属性，每个查询。 请参见[什么接下来](#Whats_coming_next)更多详细信息。

## <a name="configure-an-indexing-policy-for-order-by"></a>为 Order By 配置索引创建策略

回想起 DocumentDB 支持两种类型的索引 （哈希和范围），可设置特定路径/属性，数据类型 （字符串/数字） 和不同的精度值 （最大精度或固定的精度值）。 由于 DocumentDB 使用哈希索引的默认值，必须要使用 Order By 范围与数字、 字符串或两者上的自定义索引策略与创建新的收藏集。 

>[AZURE.NOTE] 在 2015 使用 REST API，2015年-06-03 版 7 月 7 日上引入了字符串范围索引。 为了为 Order By 创建策略，针对字符串，必须使用 SDK 版本 1.2.0.NET SDK 或版本 1.1.0 Python，Node.js 或 Java SDK。
>
>在 REST API 版本 2015年-06-03 之前, 默认集合索引策略的字符串和数字是哈希。 这已更改为哈希字符串，和范围的数字。 

有关更多详细信息请参阅[DocumentDB 索引创建策略](documentdb-indexing-policies.md)。

### <a name="indexing-for-order-by-against-all-properties"></a>对所有属性的顺序由索引
下面是如何创建集合使用"所有区域"都索引为 Order By 针对任何/所有数值或字符串内的 JSON 文档中显示的属性。 这里我们重写默认的索引类型字符串值范围和最大精度 (-1)。
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] 请注意，Order By 只会返回结果与 RangeIndex 索引的数据类型 （字符串和数字）。 例如，如果您有索引的数字只有 RangeIndex 策略的默认，Order By 根据路径的字符串值将返回任何文档。

### <a name="indexing-for-order-by-for-a-single-property"></a>索引为 Order By 的单个属性
下面是针对只需标题属性，该属性是一个字符串索引为 Order By，您就可以创建一个集合。 有两种途径，一个用于标题属性 （"/ 标题 /？"） 与区域索引，另一个用于每个其他属性使用的默认索引方案，即哈希字符串和范围的数字。                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>示例
看一看演示如何使用 Order By，包括创建的索引策略和分页使用 Order By 此[Github 示例项目](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)。 这些示例是开放源代码，我们鼓励您提交请求请求与其他 DocumentDB 开发中获益的贡献。 请参阅有关如何参与指导的[贡献的准则](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md)。  

## <a name="faq"></a>常见问题

**Order By 的查询的预期请求单元 (RU) 消耗是什么？**

由于 Order By 利用了 DocumentDB 索引查找，使用 Order By 的查询请求单位数将类似于没有 Order By 的等效查询。 DocumentDB 在任何其他操作，如请求单位数取决于文档大小/形状以及查询的复杂性。 


**什么是预期索引开销为 Order By？**

索引的存储开销将属性的数目成比例。 在最坏的情况下，索引开销将 100%的数据。 是没有区别的吞吐量 （申请单位） 开销范围/顺序的索引和默认哈希索引。

**如何查询中使用 Order By DocumentDB 我现有数据？**

为了使用 Order By 的查询结果进行排序，您必须修改使用范围索引类型与用于排序的属性的集合的索引策略。 请参阅[修改索引策略](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection)。 

**Order By 的当前限制有哪些？**

Order By 可以指定仅对一个属性时，任何一个数字或字符串时范围索引的最大精度 (-1)。

不能执行下列各项︰
 
- Order By 与内部字符串属性，如 id、 _rid 和 _self （即将提供）。
- Order By 属性从文档内联接 （即将提供） 的结果。
- 通过 （即将提供） 的多个属性进行排序。
- Order By 的查询数据库、 集合、 用户、 权限或附件 （即将提供）。
- 通过计算属性与如结果排序表达式或 UDF/建立的函数。

Order By 是当前不支持跨分区查询时在 Azure 门户中使用查询浏览器。

## <a name="troubleshooting"></a>故障排除

如果您收到错误，Order By 不支持，请进行检查以确保您正在使用 Order By 支持的[SDK](documentdb-sdk-dotnet.md)版本。 

## <a name="next-steps"></a>下一步行动

分叉[Github 示例项目](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)并开始订购您的数据 ！ 

## <a name="references"></a>引用
* [DocumentDB 查询参考](documentdb-sql-query.md)
* [DocumentDB 索引策略引用](documentdb-indexing-policies.md)
* [DocumentDB SQL 参考](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [通过样本的 DocumentDB 订单](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

