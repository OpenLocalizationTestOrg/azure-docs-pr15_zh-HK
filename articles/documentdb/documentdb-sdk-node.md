<properties
    pageTitle="DocumentDB Node.js API 和 SDK |Microsoft Azure"
    description="了解所有关于 Node.js API 和 SDK 包括发行日期、 退休日期和每个 DocumentDB Node.js SDK 版本之间所做的更改。"
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB Api 和 Sdk

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [其余部分](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js API 和 SDK

<table>
<tr><td>**下载 SDK**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API 文档**</td><td>[Node.js API 参考文档](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**SDK 安装说明**</td><td>[安装说明](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**参加 SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**示例**</td><td>[Node.js 代码示例](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**获取启动的教程**</td><td>[开始使用 Node.js SDK](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web 应用程序教程**</td><td>[构建 Node.js web 应用程序使用 DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**当前受支持的平台**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>发行说明

###<a name="1.10.0"/>1.10.0</a>

- 增加了支持跨分区并行查询。
- 添加了对分区集合顶部/ORDER BY 的查询的支持。

###<a name="1.9.0"/>1.9.0</a>

- 添加重试策略支持的合并请求。 （合并的请求接收请求速率过大异常，错误代码为 429）。默认情况下，DocumentDB 重试九次为每个请求时遇到了错误代码 429，考虑响应标头中的 retryAfter 时间。 固定的重试间隔时间可以立即设置 RetryOptions 属性的一部分 ConnectionPolicy 对象上如果要忽略返回服务器在重试之间的 retryAfter 时间。 DocumentDB 现在最多为每 30 秒的等待时间的请求被阻止 （不考虑重试次数），并返回响应，错误代码为 429。 这一次还可以在中对 ConnectionPolicy 对象的 RetryOptions 属性重写。

- DocumentDB 现在返回 x-ms-调节-重试的次数和 x-ms-throttle-retry-wait-time-ms 中每个请求来表示油门的响应标头重试计数和 cummulative 时间等待重试之间的请求。

- 已添加的 RetryOptions 类，公开可用于重写某些默认重试选项的 ConnectionPolicy 类的 RetryOptions 属性。

###<a name="1.8.0"/>1.8.0</a>

 - 添加对多区域数据库帐户的支持。

###<a name="1.7.0"/>1.7.0</a>

- 添加对文档的时间为 Live(TTL) 功能的支持。

###<a name="1.6.0"/>1.6.0</a>

- 实现的[分区的集](documentdb-partition-data.md)和[用户定义的性能级别](documentdb-performance-levels.md)。

###<a name="1.5.6"/>1.5.6</a>

- 修正了 RangePartitionResolver.resolveForRead 错误，它不返回由于 concat 坏结果的链接。

###<a name="1.5.5"/>1.5.5</a>

- 固定的 hashParitionResolver resolveForRead()︰ 当提供任何分区键时引发异常，而不是返回所有已注册的链接的列表。

###<a name="1.5.4"/>1.5.4</a>

- 修复问题[#100](https://github.com/Azure/azure-documentdb-node/issues/100) -专用 HTTPS 代理︰ 避免修改 DocumentDB 用于全局代理。 对所有的 lib 的请求使用专用的代理。

###<a name="1.5.3"/>1.5.3</a>

- 修复问题[#81](https://github.com/Azure/azure-documentdb-node/issues/81) -正确介质 id 中控点划线。

###<a name="1.5.2"/>1.5.2</a>

- 修复问题[#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter 侦听器泄漏警告。

###<a name="1.5.1"/>1.5.1</a>

- 修复问题[#92](https://github.com/Azure/azure-documentdb-node/issues/90) -重命名文件夹希到系统区分大小写的哈希值。

### <a name="1.5.0"/>1.5.0</a>

- 通过添加哈希和范围分区解析程序实施分片支持。

### <a name="1.4.0"/>1.4.0 版</a>

- 实现 Upsert。 在 documentClient 上的新的 upsertXXX 方法。

### <a name="1.3.0"/>1.3.0</a>

- 跳过将版本号与其他部门的协调。

### <a name="1.2.2"/>1.2.2</a>

- 拆分 Q 承诺到新存储库的包装。
- 更新包文件 npm 注册表。

### <a name="1.2.1"/>1.2.1</a>

- 实现 ID 基于路由。
- 修复问题[#49](https://github.com/Azure/azure-documentdb-node/issues/49) -当前属性与方法 current() 的冲突。

### <a name="1.2.0"/>1.2.0</a>

- 添加了对地理空间索引的支持。
- 验证所有资源的 id 属性。 资源的 id 不能包含？，/，#，#47; & #47;，字符或结尾使用空格。
- ResourceResponse 中添加新标题"索引转换进度"。

### <a name="1.1.0"/>1.1.0</a>

- 实现 V2 的索引策略。

### <a name="1.0.3"/>1.0.3</a>

- 核心和承诺 SDK 中的实现问题 [#40] (https://github.com/Azure/azure-documentdb-node/issues/40) 的 eslint 和 grunt 配置。

### <a name="1.0.2"/>1.0.2</a>

- 问题[#45](https://github.com/Azure/azure-documentdb-node/issues/45) -承诺包装不包括数据头错误。

### <a name="1.0.1"/>1.0.1</a>

- 冲突加上 readConflicts、 readConflictAsync 和 queryConflicts 查询的实现的能力。
- 更新的 API 文档。
- 问题[#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync 错误。

### <a name="1.0.0"/>1.0.0</a>

- GA SDK。

## <a name="release--retirement-dates"></a>发行和退休日期
Microsoft 将提供通知至少**12 个月**之前撤出来光滑过渡到/支持较新的版本的 SDK。

新的特性和功能和优化仅添加到当前的 SDK，这种建议，您一定要升级到最新 SDK 版本早尽可能。

DocumentDB 使用废弃的 SDK 对任何请求将被拒绝服务。

> [AZURE.WARNING]
将在**2 月 29，2016年**终止的 Node.js Azure DocumentDB SDK 版本**1.0.0 版**之前的所有版本。

<br/>

| 版本 | 发行日期 | 退休日期
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 2016 年 10 月 3日， |---
| [1.9.0](#1.9.0) | 2016 年 7 月 7日， |---
| [1.8.0](#1.8.0) | 2016 年 6 月 14日， |---
| [1.7.0](#1.7.0) | 2016 年 4 月 26日， |---
| [1.6.0](#1.6.0) | 2016 年 3 月 29日， |---
| [1.5.6](#1.5.6) | 2016 年 3 月 8日， |---
| [1.5.5](#1.5.5) | 2016 年 2 月 2日， |---
| [1.5.4](#1.5.4) | 2016 年 2 月 1日， |---
| [1.5.2](#1.5.2) | 2016 年 1 月 26日， |---
| [1.5.2](#1.5.2) | 2016 年 1 月 22日， |---
| [1.5.1](#1.5.1) | 2016 年 1 月 4日， |---
| [1.5.0](#1.5.0) | 2015 年 12 月 31日日 |---
| [1.4.0 版](#1.4.0) | 2015 年 10 月 6 日， |---
| [1.3.0](#1.3.0) | 2015 年 10 月 6 日， |---
| [1.2.2](#1.2.2) | 于 2015 2015年 9 月 10日日 |---
| [1.2.1](#1.2.1) | 于 2015 2015年 8 月 15日日 |---
| [1.2.0](#1.2.0) | 于 2015 2015年 8 月 5日日 |---
| [1.1.0](#1.1.0) | 于 2015 2015年 7 月 9日日 |---
| [1.0.3](#1.0.3) | 于 2015 2015年 6 月 4日日 |---
| [1.0.2](#1.0.2) | 于 2015 2015年 5 月 23日日 |---
| [1.0.1](#1.0.1) | 于 2015 2015年 5 月 15日日 |---
| [1.0.0](#1.0.0) | 于 2015 2015年 4 月 8日日 |---
| 0.9.4-prerelease | 2015 年 4 月 6 日， | 2016 年 2 月 29日，
| 0.9.3-prerelease | 2015 年 1 月 14日日 | 2016 年 2 月 29日，
| 0.9.2-prerelease | 到 2014 2014年 12 月 18日， | 2016 年 2 月 29日，
| 0.9.1-prerelease | 到 2014 2014年 8 月 22日， | 2016 年 2 月 29日，
| 0.9.0-prerelease | 到 2014 2014年 8 月 21日， | 2016 年 2 月 29日，


## <a name="faq"></a>常见问题
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>请参见

若要了解有关 DocumentDB 的详细信息，请参阅[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)服务页面。
