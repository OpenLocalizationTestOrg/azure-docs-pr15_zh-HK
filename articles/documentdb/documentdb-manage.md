<properties
    pageTitle="DocumentDB 存储和性能 |Microsoft Azure" 
    description="了解有关数据存储和文档存储在 DocumentDB 和如何可以扩展以满足您的应用程序的容量需求的 DocumentDB。" 
    keywords="文档存储"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>了解存储以及在 DocumentDB 中提供可预知的性能
Azure DocumentDB 是完全托管的、 可扩展的面向文档的 NoSQL 数据库服务的 JSON 文档。 DocumentDB，不需要出租的虚拟机、 部署软件，或监视数据库。 DocumentDB 是拥有并由 Microsoft 工程师提供世界类可用性、 性能和数据保护连续监控。  

您可以开始创建[数据库帐户](documentdb-create-account.md)DocumentDB 和[DocumentDB 数据库](documentdb-create-database.md)通过[Azure 的门户](https://portal.azure.com/)。 DocumentDB 数据库提供单位的固态驱动器 (SSD) 备份存储和吞吐量。 通过在您的数据库帐户，保留吞吐量可缩放向上或向下在任何时候以满足您的应用程序的每个集合中[创建数据库集合](documentdb-create-collection.md)来调配这些存储单元。 

如果您的应用程序超过一个或多个集合保留的吞吐量，请求在每个集合的基础上受到限制。 这意味着，虽然其他人可能会阻止某些应用程序请求可能会成功。

本文概述了资源和指标可用于管理容量和规划数据存储。 

## <a name="database-account"></a>数据库帐户
为 Azure 的订阅服务器，可以设置一个或多个 DocumentDB 数据库帐户来管理您的数据库资源。 每个预订都与一个 Azure 订阅关联。 

通过[Azure 的门户网站](documentdb-create-account.md)，或通过使用[Azure CLI 的 ARM 模板](documentdb-automation-resource-manager-cli.md)，可以创建 DocumentDB 帐户。

## <a name="databases"></a>数据库
一个 DocumentDB 数据库可以包含几乎无限组成集合的文档存储量。 集合提供性能隔离-每个集合可以提供与不在同一个数据库或帐户共享与其他集合的吞吐量。 DocumentDB 数据库是弹性的大小，范围从 Gb 到 Tb 的 SSD 备份文档存储和资源调配的吞吐量。 与传统的 RDBMS 数据库，不同的是 DocumentDB 中的数据库范围不到一台计算机，并可以跨多个计算机或群集。  

使用 DocumentDB，您可以根据需要扩展您的应用程序，您可以创建多个集合或数据库。 通过[Azure 的门户网站](documentdb-create-database.md)或通过任何一个[DocumentDB Sdk](documentdb-dotnet-samples.md)，则可以创建数据库。   

## <a name="database-collections"></a>数据库集合
DocumentDB 中的每个数据库都可以包含一个或多个集合。 集合作为文档存储和处理的高可用的数据分区。 具有异构架构的文档可以存储每个集合。 DocumentDB 的自动索引和查询功能，可以轻松地过滤和检索文档。 集合提供文档存储和查询执行的范围。 集合也是它所包含的所有文档的事务领域。 集合分配基于值设置 Azure 的门户网站或通过 Sdk 的吞吐量。 

集合是自动划分为一个或多个物理服务器的 DocumentDB。 在创建集合时，您可以指定的资源调配的吞吐量，以每秒和分区键属性的请求单元。 此属性的值是 DocumentDB 用于分发间分区和类似查询的路由请求的文档。 分区键值还用作存储的过程和触发器的事务边界。 每个集合都有特定于该集合，不共享相同的帐户中的其他集合与吞吐量的保留的金额。 因此，您可以向外扩展应用程序存储和吞吐量方面。 

通过[Azure 的门户网站](documentdb-create-collection.md)或通过任何一个[DocumentDB Sdk](documentdb-sdk-dotnet.md)创建集合。   
 
## <a name="request-units-and-database-operations"></a>申请单位以及数据库操作

在创建集合时，您将保留吞吐量，以每秒[请求单位 (RU)](documentdb-request-units.md) 。 相反的考虑和管理硬件资源，您可以**请求单元**看作一个度量值的资源要求来执行各种数据库操作和应用程序的请求提供服务。 1 KB 文档读取使用同一个 1 RU 无论有多少个项存储在集合或同时运行的并发请求数。 根据 DocumentDB，包括复杂的操作，如 SQL 查询有一个可预测的 RU 值，可以在开发时确定的所有请求。 如果您知道您的文档的大小和频率的每个操作 （读取、 写入和查询） 来支持您的应用程序，可以提供恰到好处的吞吐量以满足应用程序的需求，并随着您的性能需求的变化上下数据库。 

每个集合可以与吞吐量的 100 个请求单位每秒钟，从高达数以百万计的每秒请求单位数百块预留。 调配的吞吐量可以调整整个生命周期中的集合来适应不断变化的处理需求和访问您的应用程序的模式。 有关详细信息，请参阅[DocumentDB 的性能级别](documentdb-performance-levels.md)。 

>[AZURE.IMPORTANT] 集合是收费的实体。 成本取决于集合以请求每秒单位数以及以千兆字节计总消耗的存储资源调配吞吐量。 

如插入、 删除、 查询或存储的过程的执行特定的操作会消耗多少请求单位？ 申请单位是标准化的请求处理成本的措施。 1 KB 文档的读取是 1 RU，但请求插入、 替换或删除同一文档将消耗更多处理服务，从而更多的申请单位。 每个服务的响应包含自定义标头 (`x-ms-request-charge`) 报告使用请求的请求单元。 此标头也是可以通过[Sdk](documentdb-sdk-dotnet.md)访问的。 在.NET SDK 中， [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge)是[ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx)对象的一个属性。 如果要评估您的吞吐量需求之后再进行一次调用，可用于[容量规划师](documentdb-request-units.md#estimating-throughput-needs)帮助进行此估计。 

>[AZURE.NOTE] 1 个请求单位为 1 KB 文档的基线对应于文档的简单获取[会话](documentdb-consistency-levels.md)一致性。 

有几个因素影响消耗对 DocumentDB 数据库帐户的操作的请求单元。 这些因素包括︰

- 文档的大小。 随着文档大小的增加单位消耗来读取或写入的数据也会增加。
- 属性计数。 假定默认索引的所有属性，使用编写文档的单位将作为属性计数的增加而增加。
- 数据的一致性。 当使用强或包围的失效数据一致性级别，需要消耗额外单位读取文档。
- 索引的属性。 在每个集合的索引策略确定哪些属性进行索引，默认情况。 您可以减少您请求单位消耗量限制索引的属性的数目。 
- 文档索引。 默认情况下，每个文档将自动设置索引，将消耗少的申请单位，如果您选择不进行索引的文档部分。

有关详细信息，请参阅[DocumentDB 请求单位](documentdb-request-units.md)。 

例如，下面是显示多少请求单位，为规定在三种不同的文档大小 （1 KB、 4 KB 和 64 KB） 和两个不同的性能级别表 （500 读取/秒 + 100 写入/秒和 500 读取/秒 + 500 写入/秒）。 在会话中，配置数据的一致性和索引的策略被设置为无。

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>文档大小</strong></p></td>
            <td valign="top"><p><strong>读取/秒</strong></p></td>
            <td valign="top"><p><strong>写入/秒</strong></p></td>
            <td valign="top"><p><strong>申请单位</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1000 的 RU s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 每个 s 1,350 RU</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 每个 s 4,150 RU</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 高 9800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 每个 s 29,000 RU</p></td>
        </tr>
    </tbody>
</table>

查询、 存储的过程和触发器使用请求单位根据正在执行的操作的复杂性。 在开发应用程序时，检查请求的费用标头，以更好地了解每个操作消耗请求单位容量的方式。  


## <a name="choice-of-consistency-level-and-throughput"></a>选择一致性级别和吞吐量
默认一致性级别的选择有一定影响的吞吐量和滞后时间。 以编程方式并通过 Azure 的门户，可以设置默认的一致性级别。 您还可以重写基于每个请求的一致性级别。 默认情况下，一致性级别是设置为**会话**，它提供单调性的读/写操作和读写保证。 会话的一致性非常适合以用户为中心的应用程序，并提供一致性和性能权衡的完美结合。    

更改您的一致性级别在 Azure 的门户网站上的说明，请参阅[如何管理 DocumentDB 帐户](documentdb-manage-account.md#consistency)。 或者，一致性级别的详细信息，请参阅[使用一致性级别](documentdb-consistency-levels.md)。

## <a name="provisioned-document-storage-and-index-overhead"></a>设置的文档存储和索引开销
DocumentDB 支持单分区和分区集的创建。 在 DocumentDB 中的每个分区支持 10 GB 的备份的 SSD 存储。 文档存储 10GB 包括文档及相关索引的存储。 默认情况下，DocumentDB 集合配置为自动索引的所有文档而无需显式要求任何辅助索引或架构。 根据应用程序使用 DocumentDB，2-20%之间为典型的索引开销。 所使用的 DocumentDB 的索引技术可以确保，无论属性的值，索引开销不超过 80%以上的具有默认设置的文档的大小。 

默认情况下所有文档会自动建立都索引的 DocumentDB。 但是，如果您想要微调索引开销，您可以选择删除某些文档正在被编制索引时插入或替换文档中的所述[DocumentDB 索引创建策略](documentdb-indexing-policies.md)。 您可以配置一个 DocumentDB 集合来排除在索引集合中的所有文档。 您还可以配置要有选择地索引某些属性或使用通配符的 JSON 文档，路径[配置集合的索引策略](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection)中所述的 DocumentDB 集合。 排除属性或文档还可以提高写入吞吐量 – 这意味着您将占用更少的申请单位。   

## <a name="next-steps"></a>下一步行动

要继续学习有关 DocumentDB 的工作原理，请参阅[分区和 Azure DocumentDB 中的缩放](documentdb-partition-data.md)。

有关监视性能级别在 Azure 的门户网站上的说明，请参阅[显示器 DocumentDB 帐户](documentdb-monitor-accounts.md)。 选择集合的性能级别的详细信息，请参阅[DocumentDB 中的性能级别](documentdb-performance-levels.md)。
 
