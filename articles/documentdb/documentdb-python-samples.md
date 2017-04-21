<properties 
    pageTitle="NoSQL Python 示例 DocumentDB |Microsoft Azure" 
    description="查找 NoSQL Python 示例在 github 上 DocumentDB，NoSQL 数据库中包括的 JSON 文档的 CRUD 操作中的常见任务。" 
    keywords="python 的示例"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>DocumentDB Python 示例

> [AZURE.SELECTOR]
- [.NET 示例](documentdb-dotnet-samples.md)
- [Node.js 示例](documentdb-nodejs-samples.md)
- [Python 的示例](documentdb-python-samples.md)
- [Azure 代码示例库](https://azure.microsoft.com/documentation/samples/?service=documentdb)

执行 CRUD 操作和 Azure DocumentDB 资源上其他常见操作的示例解决方案包含在[azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub 存储库中。 这篇文章提供了︰

- 在每个 Python 的示例项目文件的任务链接。 
- 链接到相关的 API 参考内容。

**系统必备组件**

1. 要使用这些 Python 示例的 Azure 帐户，您需要︰
    - 您可以[免费开设 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)︰ 获取积分可用于试验 Azure 服务付费，并且即使他们习惯之后最多可以保留该帐户并使用释放 Azure 服务，例如网站。 永远不会将收取您的信用卡，除非您明确更改您的设置并问计。
   - 您可以[激活 Visual Studio 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)︰ 您的 Visual Studio 预订为您提供了积分可用于付费的 Azure 服务每月。
2. 您还需要[Python SDK](documentdb-sdk-python.md)。 

    > [AZURE.NOTE] 每个示例都是独立的它本身设置并清理后本身。 在这种情况下，示例发出多个调用[document_client。CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)。 这是您的订购每次将货款的使用所创建的集合的性能层每 1 小时。 

## <a name="database-examples"></a>数据库示例

[Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py)文件[和数据库管理](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement)项目的演示如何执行以下任务。

任务 | API 参考
--- | ---
[创建数据库](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client。CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[查询数据库的帐户](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client。QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[读取数据库 Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client。ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[列出数据库帐户](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client。ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[删除数据库](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client。DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>集合示例 

[CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement)项目的[Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py)文件演示如何执行以下任务。

任务 | API 参考
--- | ---
[创建集合](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client。CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[读取数据库中的所有集合的列表](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client。ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[获取一个标记集合 id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client。ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[获取集合的性能层](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[更改集合的性能层](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client。ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[删除集](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client。DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
