
<properties
    pageTitle="DocumentDB Java API 和 SDK |Microsoft Azure"
    description="了解所有关于 Java API 和 SDK 包括发行日期、 退休日期和每个 DocumentDB Java SDK 版本之间所做的更改。"
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB Api 和 Sdk

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [其余部分](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java API 和 SDK

<table>
<tr><td>**SDK 下载**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API 文档**</td><td>[Java API 参考文档](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**参加 SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**入门**</td><td>[开始使用 Java SDK](documentdb-java-application.md)</td></tr>
<tr><td>**当前支持的运行时**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>发行说明

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - 添加了对 BoundedStaleness 的一致性级别的支持。
  - 添加了对分区集合的 CRUD 操作的直接连接的支持。
  - 解决查询的 SQL 数据库中的错误。
  - 其中可能未正确设置会话令牌的会话缓存中修复的错误。

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - 增加了支持跨分区并行查询。
  - 添加了对分区集合顶部/ORDER BY 的查询的支持。
  - 添加了对强一致性的支持。
  - 添加的名称基于请求时使用直接连接的支持。
  - 修复，可以使在所有请求重试之间保持一致的活动 Id。
  - 修复时重新创建具有相同名称的集合与会话缓存相关的错误。
  - 添加多边形和 LineString 数据类型时指定集合索引地区防御空间查询的策略。
  - 与 Java 文档中的 Java 1.8 的固定的问题。

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - 修复了错误 PartitionKeyDefinitionMap 缓存单个分区集合并进行额外的读取分区键的请求。
  - 修复 bug，以便不提供不正确的分区键值后再重试。

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - 添加对多区域数据库帐户的支持。
  - 增加了自动重试选项自定义的最大重试次数和重试的最大等待时间的合并请求支持。  请参阅 RetryOptions 和 ConnectionPolicy.getRetryOptions()。
  - 不建议使用的 IPartitionResolver 基于分区的自定义代码。 请实现更高的存储和吞吐量，使用分区的集合。

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- 添加重试限制策略支持。  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- 对于文档实时 (TTL) 支持的添加的时间。

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- 实现的[分区的集](documentdb-partition-data.md)和[用户定义的性能级别](documentdb-performance-levels.md)。

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- 修复了错误 HashPartitionResolver 以生成哈希值小端字节序与其他 Sdk 保持一致。

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- 添加哈希和范围分区解析程序来协助与分片应用程序跨多个分区。

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0 版](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- 实现 Upsert。 为支持 Upsert 功能添加了新的 upsertXXX 方法。
- 实现基于 ID 的路由。 没有公共的 API 更改，内部的所有更改。

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- 已跳过版本将版本号与其他部门的协调

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- 支持地理空间索引
- 验证所有资源的 id 属性。 资源的 id 不能包含 ?，/，#\,字符或结尾使用空格。
- ResourceResponse 中添加新标题"索引转换进度"。

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- 实现 V2 索引策略

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>发行和退休日期
Microsoft 将提供通知至少**12 个月**之前撤出来光滑过渡到/支持较新的版本的 SDK。

新的特性和功能和优化仅添加到当前的 SDK，这种建议，您一定要升级到最新 SDK 版本早尽可能。

DocumentDB 使用废弃的 SDK 对任何请求将被拒绝服务。

> [AZURE.WARNING]
所有之前的版本中为 Java Azure DocumentDB SDK 版本**1.0.0 版**将在**2 月 29，2016年**终止。

<br/>

| 版本 | 发行日期 | 退休日期
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 2016 年 10 月 28日， |---
| [1.9.0](#1.9.0) | 2016 年 10 月 3日， |---
| [1.8.1](#1.8.1) | 2016 年 6 月 30日， |---
| [1.8.0](#1.8.0) | 2016 年 6 月 14日， |---
| [1.7.1](#1.7.1) | 2016 年 4 月 30日， |---
| [1.7.0](#1.7.0) | 2016 年 4 月 27日， |---
| [1.6.0](#1.6.0) | 2016 年 3 月 29日， |---
| [1.5.1](#1.5.1) | 2015 年 12 月 31日日 |---
| [1.5.0](#1.5.0) | 2015 年 12 月 4日日 |---
| [1.4.0 版](#1.4.0) | 于 2015 2015年 10 月 5日日 |---
| [1.3.0](#1.3.0) | 于 2015 2015年 10 月 5日日 |---
| [1.2.0](#1.2.0) | 于 2015 2015年 8 月 5日日 |---
| [1.1.0](#1.1.0) | 于 2015 2015年 7 月 9日日 |---
| [1.0.1](#1.0.1) | 于 2015 2015年 5 月 12日日 |---
| [1.0.0](#1.0.0) | 于 2015 2015年 4 月 7日日 |---
| 0.9.5-prelease | 于 2015 2015年 03 月 9日日 | 2016 年 2 月 29日，
| 0.9.4-prelease | 于 2015 2015年 2 月 17日日 | 2016 年 2 月 29日，
| 0.9.3-prelease | 2015 年 1 月 13日日 | 2016 年 2 月 29日，
| 0.9.2-prelease | 到 2014 2014年 12 月 19日， | 2016 年 2 月 29日，
| 0.9.1-prelease | 到 2014 2014年 12 月 19日， | 2016 年 2 月 29日，
| 0.9.0-prelease | 到 2014 2014年 12 月 10日， | 2016 年 2 月 29日，

## <a name="faq"></a>常见问题
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>请参见

若要了解有关 DocumentDB 的详细信息，请参阅[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)服务页面。
