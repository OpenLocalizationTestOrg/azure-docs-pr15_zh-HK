<properties 
    pageTitle="DocumentDB Python API 和 SDK |Microsoft Azure" 
    description="了解所有关于 Python API 和 SDK 包括发行日期、 退休日期和每个 DocumentDB Python SDK 版本之间所做的更改。" 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB Api 和 Sdk

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [其余部分](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python API 和 SDK

<table>
<tr><td>**下载 SDK**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API 文档**</td><td>[Python API 参考文档](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**SDK 安装说明**</td><td>[Python SDK 安装说明](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**参加 SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**入门**</td><td>[开始使用 Python SDK](documentdb-python-application.md)</td></tr>
<tr><td>**当前受支持的平台**</td><td>[Python 2.7](https://www.python.org/downloads/)和[Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>发行说明

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- 添加了对 Python 3.5 英寸的支持。
- 添加的连接池使用请求模块的支持。
- 增加了会话一致性支持。
- 添加了对分区集合顶部/排序查询的支持。


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- 添加重试策略支持的合并请求。 （合并的请求接收请求速率过大异常，错误代码为 429）。默认情况下，DocumentDB 重试九次为每个请求时遇到了错误代码 429，考虑响应标头中的 retryAfter 时间。 固定的重试间隔时间可以立即设置 RetryOptions 属性的一部分 ConnectionPolicy 对象上如果要忽略返回服务器在重试之间的 retryAfter 时间。 DocumentDB 现在最多为每 30 秒的等待时间的请求被阻止 （不考虑重试次数），并返回响应，错误代码为 429。 这一次还可以在中对 ConnectionPolicy 对象的 RetryOptions 属性重写。

- DocumentDB 现在返回 x-ms-调节-重试的次数和 x-ms-throttle-retry-wait-time-ms 中每个请求来表示油门的响应标头重试计数和 cummulative 时间等待重试之间的请求。

- 删除 RetryPolicy 类和相应的属性 (retry_policy) 的 document_client 类中公开，而是引入 RetryOptions 类公开可用于重写某些默认重试选项的 ConnectionPolicy 类的 RetryOptions 属性。

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - 添加对多区域数据库帐户的支持。

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- 添加对文档的时间为 Live(TTL) 功能的支持。

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- 与服务器端分区，以允许 partitionkey 路径中使用特殊字符相关的 bug 修复。

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- 实现的[分区的集](documentdb-partition-data.md)和[用户定义的性能级别](documentdb-performance-levels.md)。 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- 添加哈希和范围分区解析程序来协助与分片应用程序跨多个分区。

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- 实现 Upsert。 为支持 Upsert 功能添加了新的 UpsertXXX 方法。
- 实现基于 ID 的路由。 没有公共的 API 更改，内部的所有更改。

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- 支持地理空间索引。
- 验证所有资源的 id 属性。 资源的 id 不能包含 ?，/，#\,字符或结尾使用空格。
- ResourceResponse 中添加新标题"索引转换进度"。

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- 实现 V2 的索引策略。

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- 支持代理连接。

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK。

## <a name="release--retirement-dates"></a>发行和退休日期
Microsoft 将提供通知至少**12 个月**之前撤出来光滑过渡到/支持较新的版本的 SDK。

新的特性和功能和优化仅添加到当前的 SDK，这种建议，您一定要升级到最新 SDK 版本早尽可能。 

DocumentDB 使用废弃的 SDK 对任何请求将被拒绝服务。

> [AZURE.WARNING]
将在**2 月 29，2016年**终止的 Python Azure DocumentDB SDK 版本**1.0.0 版**之前的所有版本。 

<br/>

| 版本 | 发行日期 | 退休日期 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 2016 年 9 月 29日， |---
| [1.9.0](#1.9.0) | 2016 年 7 月 7日， |---
| [1.8.0](#1.8.0) | 2016 年 6 月 14日， |---
| [1.7.0](#1.7.0) | 2016 年 4 月 26日， |---
| [1.6.1](#1.6.1) | 2016 年 4 月 8日， |---
| [1.6.0](#1.6.0) | 2016 年 3 月 29日， |---
| [1.5.0](#1.5.0) | 2016 年 1 月 3日， |---
| [1.4.2](#1.4.2) | 2015 年 10 月 6 日， |---
| [1.4.1](#1.4.1) | 2015 年 10 月 6 日， |---
| [1.2.0](#1.2.0) | 2015 年 8 月 6 日， |---
| [1.1.0](#1.1.0) | 于 2015 2015年 7 月 9日日 |---
| [1.0.1](#1.0.1) | 于 2015 2015年 5 月 25日日 |---
| [1.0.0](#1.0.0) | 于 2015 2015年 4 月 7日日 |---
| 0.9.4-prelease | 2015 年 1 月 14日日 | 2016 年 2 月 29日，
| 0.9.3-prelease | 到 2014 2014年 12 月 9日， | 2016 年 2 月 29日，
| 0.9.2-prelease | 到 2014 2014年 11 月 25日日， | 2016 年 2 月 29日，
| 0.9.1-prelease | 到 2014 2014年 9 月 23日， | 2016 年 2 月 29日，
| 0.9.0-prelease | 到 2014 2014年 8 月 21日， | 2016 年 2 月 29日，

## <a name="faq"></a>常见问题
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>请参见

若要了解有关 DocumentDB 的详细信息，请参阅[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)服务页面。 
