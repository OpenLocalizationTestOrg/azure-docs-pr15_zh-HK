<properties 
    pageTitle="如何在 DocumentDB 中创建一个数据库 |Microsoft Azure" 
    description="了解如何使用 Azure DocumentDB，闪电般快速的、 全球范围内 NoSQL 数据库的联机服务门户创建一个数据库。" 
    keywords="如何创建数据库" 
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
    ms.date="10/17/2016" 
    ms.author="mimig"/>

# <a name="how-to-create-a-database-for-documentdb-using-the-azure-portal"></a>如何为 DocumentDB 使用 Azure 门户创建数据库

若要使用 Microsoft Azure DocumentDB，必须具有一个[DocumentDB 帐户](documentdb-create-account.md)、 数据库、 集合和文档。 在 Azure 的门户中，DocumentDB 创建数据库时立即同时创建集合时。 

在门户中创建的 DocumentDB 数据库和集合，请参阅[如何创建 DocumentDB 集合使用 Azure 的门户](documentdb-create-collection.md)。

## <a name="other-ways-to-create-a-documentdb-database"></a>若要创建一个 DocumentDB 数据库的其他方法

数据库不具有创建使用门户，您还可以创建使用[DocumentDB Sdk](documentdb-sdk-dotnet.md)或[REST API](https://msdn.microsoft.com/library/mt489072.aspx)。 有关使用.NET SDK 处理数据库的信息，请参阅[.NET 数据库示例](documentdb-dotnet-samples.md#database-examples)。 通过 Node.js SDK 处理数据库的信息，请参阅[Node.js 数据库示例](documentdb-nodejs-samples.md#database-examples)。 

## <a name="next-steps"></a>下一步行动

一旦创建了数据库和集合，您可以[添加 JSON 文档](documentdb-view-json-document-explorer.md)使用文档资源管理器在门户中，通过使用 DocumentDB 数据迁移工具时，集合中的[导入文档](documentdb-import-data.md)，或使用[DocumentDB Sdk](documentdb-sdk-dotnet.md)之一执行 CRUD 操作。 DocumentDB 具有.NET、 Java、 Python，Node.js 和 JavaScript API Sdk。 有关.NET 代码示例演示如何创建、 删除、 更新和删除文档，请参阅[.NET 文档示例](documentdb-dotnet-samples.md#document-examples)。 通过 Node.js SDK 处理的文档的信息，请参阅[Node.js 文档示例](documentdb-nodejs-samples.md#document-examples)。 

集合中的文档后，可以使用[DocumentDB SQL](documentdb-sql-query.md) [执行查询](documentdb-sql-query.md#executing-sql-queries)到针对文档使用门户、 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)，或一个[Sdk](documentdb-sdk-dotnet.md)中[查询资源管理器](documentdb-query-collections-query-explorer.md)。 
