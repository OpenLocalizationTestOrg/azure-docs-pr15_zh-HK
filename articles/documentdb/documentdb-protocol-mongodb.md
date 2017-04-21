<properties 
    pageTitle="MongoDB 的 DocumentDB 协议支持 |Microsoft Azure" 
    description="了解关于 MongoDB，目前在公共预览中可用的 DocumentDB 协议支持。" 
    keywords="mongodb"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-protocol-support-for-mongodb"></a>MongoDB 的 DocumentDB 协议支持

## <a name="what-is-azure-documentdb"></a>什么是 Azure DocumentDB？
Azure DocumentDB 是为快速、 可预知的性能、 高可用性、 自动按比例缩放，和易用性开发构建完全托管的 NoSQL 数据库服务。 其灵活的数据模型、 一致的低延迟和丰富的查询功能使其非常适合于 web、 移动，游戏，IoT 和许多其他应用程序需要无缝扩展。 在[DocumentDB 介绍](documentdb-introduction.md)阅读的详细信息。

## <a name="what-is-documentdb-protocol-support-for-mongodb"></a>MongoDB 的 DocumentDB 协议支持是什么？
DocumentDB 数据库现在可以用作 MongoDB 为编写的应用程序的数据存储区。 MongoDB 使用现有[驱动程序](https://docs.mongodb.org/ecosystem/drivers/)，应用程序可以很容易地和透明地与通信 DocumentDB，在许多情况下，只需更改连接字符串。  使用此预览功能，客户可以轻松地构建和 Azure 云-利用 DocumentDB 的完全管理而且可扩展 NoSQL 数据库的同时继续使用 MongoDB 熟悉的技能和工具中运行的应用程序。

DocumentDB 协议 MongoDB 实现的核心 MongoDB API 函数的创建、 读取、 更新和删除 (CRUD) 数据的支持，以及在数据库中查询。 当前实现的功能已确定优先级基于公共平台、 框架、 工具和评估其云平台 Azure 的大规模 MongoDB 客户的需求。
  

## <a name="next-steps"></a>下一步行动


- 了解如何[创建](documentdb-create-mongodb-account.md)具有协议的 DocumentDB 帐户支持 MongoDB 的。
- 学习如何与协议的 DocumentDB 帐户[连接](documentdb-connect-mongodb-account.md)到支持的 MongoDB。
- 了解如何[使用 MongoChef](documentdb-mongodb-mongochef.md)协议与 DocumentDB 帐户到支持 MongoDB 的。
- DocumentDB 使用 MongoDB[样本](documentdb-mongodb-samples.md)的协议支持。

 
