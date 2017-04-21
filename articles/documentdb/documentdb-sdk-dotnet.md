<properties 
    pageTitle="DocumentDB.NET API 和 SDK |Microsoft Azure" 
    description="了解所有关于.NET API 和 SDK 包括发行日期、 退休日期和每个 DocumentDB.NET SDK 版本之间所做的更改。" 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB Api 和 Sdk 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [其余部分](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB.NET API 和 SDK

<table>
<tr><td>**SDK 下载**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API 文档**</td><td>[.NET API 参考文档](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**示例**</td><td>[.NET 代码示例](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**入门**</td><td>[开始使用 DocumentDB.NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Web 应用程序教程**</td><td>[与 DocumentDB 的 web 应用程序开发](documentdb-dotnet-application.md)</td></tr>
<tr><td>**当前受支持的框架**</td><td>[Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>发行说明

> [AZURE.IMPORTANT] 从版本 1.9.2 发行版开始，您可能会收到查询分区的集合时 System.NotSupportedException。 若要避免此错误，请确保主机进程是 64 位。 对于可执行文件的项目，这可以通过取消选中在项目属性窗口中的生成选项卡上的"更愿意 32 位"选项完成。

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - 添加直接连接支持分区的集合。
  - 改进了的性能限制失效的一致性级别。
  - 添加多边形和 LineString 数据类型时指定集合索引地区防御空间查询的策略。
  - 添加在平移谓词时对 StringEnumConverter、 IsoDateTimeConverter 和 UnixDateTimeConverter 的 LINQ 支持。
  - 各种的 SDK 错误修复程序。

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - 修复了导致下面的 NotFoundException︰ 读取的会话不可用于输入的会话令牌。 此异常出现在某些情况下，查询地理分布的帐户的读取区域时。
  - 公开的 ResourceResponse 类，这使到基础流的直接访问，从响应中的 ResponseStream 属性。

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - 修改的 ResourceResponse、 FeedResponse、 StoredProcedureResponse 和 MediaResponse 的类来实现相应的公共接口，以便他们可以模拟测试驱动部署 (TDD)。
  - 修复导致格式不正确的分区键头，使用自定义的 JsonSerializerSettings 对象的序列化数据时的问题。

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - 修复了导致长时间运行的查询失败，出现错误︰ 当前时间位于无效授权令牌。
  - 解决删除从原始 SqlParameterCollection 跨分区顶部/按顺序查询问题。

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - 添加了对分区集的并行查询的支持。
  - 增加了支持跨分区 ORDER BY 和顶部分区集合的查询。
  - 修复丢失的引用到 DocumentDB.Spatial.Sql.dll 和 Microsoft.Azure.Documents.ServiceInterop.dll 对 DocumentDB Nuget 程序包的引用 DocumentDB 项目时所需。
  - 固定在 LINQ 中使用用户定义的函数时使用不同类型的参数的能力。 
  - 修复了错误的全局范围内复制帐户其中定向 Upsert 调用读取而不是写入位置的位置。
  - 添加的方法到 IDocumentClient 接口缺少︰ 
      - UpsertAttachmentAsync 方法，其采用 mediaStream 和选项作为参数
      - 接受选项作为参数的 CreateAttachmentAsync 方法
      - CreateOfferQuery 采用 querySpec 作为参数的方法。
  - 非密封中的 IDocumentClient 接口公开的公共类。

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - 添加对多区域数据库帐户的支持。
  - 增加了重试合并请求支持。  用户可以自定义配置的 ConnectionPolicy.RetryOptions 属性的重试次数和最大等待时间数。
  - 添加新的 IDocumentClient 接口定义的所有 DocumenClient 属性和方法的签名。  作为此次更改的一部分，也将更改创建 IQueryable 并 IOrderedQueryable 到 DocumentClient 类本身的方法的扩展方法。
  - 添加配置选项设置为给定的 DocumentDB 端点的 Uri ServicePoint.ConnectionLimit。  使用 ConnectionPolicy.MaxConnectionLimit 来更改默认值设置为 50。
  - 不建议使用的 IPartitionResolver，它的实现。  对 IPartitionResolver 的支持现已过时。 建议用于更高的存储和吞吐量的分区的集合。


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - 添加到的 Uri 重载基于 ExecuteStoredProcedureAsync 方法，采用 RequestOptions 作为参数。
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - 对于文档实时 (TTL) 支持的添加的时间。

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - 解决 bug Nuget 包装的.NET SDK 中将演示文稿打包作为 Azure 云服务解决方案的一部分。
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - 实现的[分区的集](documentdb-partition-data.md)和[用户定义的性能级别](documentdb-performance-levels.md)。 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[固定]**查询 DocumentDB 终结点将引发: System.Net.Http.HttpRequestException︰ 将内容复制到流时出错。

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - 扩展的 LINQ 支持包括新的页面，条件表达式运算符以及范围比较。
    - 采用运算符，若要启用 LINQ 中的选择顶部行为
    - CompareTo 运算符，若要启用字符串范围比较
    - 条件 （？） 和联合运算符 （？）
  - **[固定]**组合与模型投影时的 ArgumentOutOfRangeException 在 linq 查询中的位置。  [# 81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[固定]**如果选择不是最后一个表达式 LINQ 提供程序假定没有投影和产生选择 * 不正确。  [# 58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - 实现的 Upsert，添加 UpsertXXXAsync 方法
 - 所有请求的性能改进
 - LINQ 提供程序的条件，支持合并和 CompareTo 方法为字符串
 - **[固定]**LINQ 提供程序--> 实现包含列表生成 IEnumerable 和阵列上为相同的 SQL 方法
 - **[固定]**BackoffRetryUtility 使用相同的 HttpRequestMessage 再次而不是创建一个新的重试
 - **[已过时]**--> UriFactory.CreateCollection 现在应该使用 UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[固定]**当使用非 en 区域性信息如 nl NL 等等的本地化问题。 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0 版](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - 基于 ID 的路由
    - 新 UriFactory 的帮助器，有助于构造具有基于资源的链接
    - 在 DocumentClient 采取在 URI 中的新重载
  - 添加了 IsValid() 和 IsValidDetailed() LINQ 的地理空间中
  - LINQ 提供程序支持增强
    - **数学**的 Abs，Acos，Asin Atan，天花板，Cos Exp、 地板、 日志、 Log10，Pow、 往返、 符号、 正弦、 Sqrt，Tan，截断
    - **字符串**的连接，包含 EndsWith IndexOf、 计数、 ToLower、 TrimStart、 替换、 反转，TrimEnd、 StartsWith、 子字符串、 ToUpper
    - **阵列**的连接，包含计数
    - **IN**运算符

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - 添加了用于修改索引创建策略的支持
    - 在 DocumentClient 中的新 ReplaceDocumentCollectionAsync 方法
    - 新的 IndexTransformationProgress 属性，在 ResourceResponse<T>用于跟踪的索引策略更改百分比进度
    - DocumentCollection.IndexingPolicy 现在是可变
  - 添加空间索引和查询的支持
    - 新的 Microsoft.Azure.Documents.Spatial 命名空间进行序列化/反序列化空间类型，如点和多边形
    - 新的空间索引类索引存储在 DocumentDB 中的 GeoJSON 数据
  - **[自动]** : 不正确的 SQL 查询生成的 linq 表达式[#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- 在 Newtonsoft.Json v5.0.7 上的依赖项 
- 若要支持 Order By 的更改
  - LINQ 提供程序支持 OrderBy() 或 OrderByDescending()
  - 若要支持 Order By IndexingPolicy 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- 数据分区使用的新的 HashPartitionResolver 和 RangePartitionResolver 类和 IPartitionResolver 的支持
- DataContract 的序列化
- 在 LINQ 提供程序的 Guid 支持
- LINQ 中的 UDF 支持

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
没有更改的预览和上市之间的 NuGet 程序包名称 我们已从**Microsoft.Azure.Documents.Client**到**Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- [已过时] 预览 Sdk

## <a name="release--retirement-dates"></a>发行和退休日期
Microsoft 将提供通知至少**12 个月**之前撤出来光滑过渡到/支持较新的版本的 SDK。

新的特性和功能和优化仅添加到当前的 SDK，但这种建议总是升级到最新 SDK 版本早尽可能。 

DocumentDB 使用废弃的 SDK 对任何请求将被拒绝服务。

> [AZURE.WARNING]
将在**2 月 29，2016年**终止的.NET Azure DocumentDB SDK 版本**1.0.0 版**之前的所有版本。 
 
<br/>
 
| 版本 | 发行日期 | 退休日期 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 2016 年 9 月 27日， |---
| [1.9.5](#1.9.5) | 2016 年 9 月 1日， |---
| [1.9.4](#1.9.4) | 2016 年 8 月 24日， |---
| [1.9.3](#1.9.3) | 2016 年 8 月 15日， |---
| [1.9.2](#1.9.2) | 2016 年 7 月 23日， |---
| 1.9.1 | 不建议使用 |---
| 1.9.0 | 不建议使用 |---
| [1.8.0](#1.8.0) | 2016 年 6 月 14日， |---
| [1.7.1](#1.7.1) | 2016 年 5 月 6 日， |---
| [1.7.0](#1.7.0) | 2016 年 4 月 26日， |---
| [1.6.3](#1.6.3) | 2016 年 4 月 8日， |---
| [1.6.2](#1.6.2) | 2016 年 3 月 29日， |---
| [1.5.3](#1.5.3) | 2016 年 2 月 19日， |---
| [1.5.2](#1.5.2) | 2015 年 12 月 14日日 |---
| [1.5.1](#1.5.1) | 于 2015 2015年 11 月 23日日 |---
| [1.5.0](#1.5.0) | 于 2015 2015年 10 月 5日日 |---
| [1.4.1](#1.4.1) | 2015 年 8 月 25日日， |---
| [1.4.0 版](#1.4.0) | 于 2015 2015年 8 月 13日日 |---
| [1.3.0](#1.3.0) | 于 2015 2015年 8 月 5日日 |---
| [1.2.0](#1.2.0) | 2015 年 7 月 6 日， |---
| [1.1.0](#1.1.0) | 于 2015 2015年 4 月 30日日 |---
| [1.0.0](#1.0.0) | 于 2015 2015年 4 月 8日日 |---
| [0.9.3-prelease](#0.9.x-preview) | 于 2015 2015年 3 月 12日日 | 2016 年 2 月 29日，
| [0.9.2-prelease](#0.9.x-preview) | 2015 年 1 月， | 2016 年 2 月 29日，
| [.9.1-prelease](#0.9.x-preview) | 到 2014 2014年 10 月 13日， | 2016 年 2 月 29日，
| [0.9.0-prelease](#0.9.x-preview) | 到 2014 2014年 8 月 21日， | 2016 年 2 月 29日，

## <a name="faq"></a>常见问题
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>请参见

若要了解有关 DocumentDB 的详细信息，请参阅[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)服务页面。 
