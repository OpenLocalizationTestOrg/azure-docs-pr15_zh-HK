<properties
   pageTitle="分发数据全局与 DocumentDB |Microsoft Azure"
   description="了解有关使用 Azure DocumentDB，完全托管 NoSQL 数据库服务的全局数据库的行星比例地区复制、 故障切换和数据恢复。"
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>将全局与 DocumentDB 的数据分发

> [AZURE.NOTE] DocumentDB 数据库的全局分布是普遍适用和自动启用任何新创建的 DocumentDB 帐户。 我们正在努力使全球分布上现有的所有帐户，但在此期间，如果您希望在您的帐户上启用全局通讯组请[与支持人员联系](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)，我们将它为您立即启用。

Azure 的 DocumentDB 旨在满足 IoT 包含的遍布全球的设备和互联网的比例应用程序将响应度很高的经验传递给用户，遍及世界各地数以百万计的应用程序的需要。 这些数据库系统面临着多个定义完善的数据一致性和可用性保证的地理区域从对应用程序数据实现低延迟访问的挑战。 为全局分布式的数据库系统中，DocumentDB 通过提供完全托管的、 多区域数据库提供清除权衡一致性、 可用性和性能，而相应的保证所有的帐户简化了数据的全球分布。 DocumentDB 数据库帐户提供了高可用性、 单个数字毫秒延迟、 多个[定义完善的一致性级别] [consistency]，与多主页 Api，透明区域的故障切换和吞吐量和存储弹性在全球范围内扩展的能力。 

  
## <a name="configuring-multi-region-accounts"></a>配置多区域帐户

可以通过[Azure 的门户网站](documentdb-portal-global-replication.md)不到一分钟内完成配置您的 DocumentDB 帐户，以在全球范围内扩展。 所有您需要做是选择合适的一致性级别之间几个支持完善定义的一致性级别，并将任意数量的 Azure 区域与数据库帐户相关联。 DocumentDB 的一致性级别提供明确保证特定的一致性和性能之间权衡。 

![DocumentDB 提供了多个，很好地定义 （宽松） 一致性模型可供选择的][1]

DocumentDB 提供了多个，很好地定义 （宽松） 一致性模型可供选择。

选择合适的一致性级别取决于数据一致性保证应用程序的需要。 DocumentDB 自动所有指定区域间复制数据，并可以保证所选的数据库帐户的一致性。 


## <a name="using-multi-region-failover"></a>使用多区域故障转移 

Azure DocumentDB 是透明地故障转移数据库帐户能够跨多个 Azure 区域 – 新的[Api 多主页][developingwithmultipleregions]能保证您的应用程序可以继续使用逻辑终结点，不间断地通过故障转移。 故障转移由您控制，提供灵活地重新连接您的数据库帐户在该事件中所有各种可能的故障情况发生，包括应用程序、 基础设施、 服务或区域故障 （真实或模拟）。 在 DocumentDB 地区发生故障，该服务将以透明方式进行故障转移数据库帐户和应用程序继续访问数据，而不会丢失的可用性。 虽然 DocumentDB 提供[99.99%的可用性服务级别协议][sla]，您可以通过两种，[以编程方式]模拟区域故障测试应用程序的端到端可用性属性[arm]也通过 Azure 门户。


## <a name="scaling-across-the-planet"></a>在行星之间缩放
DocumentDB 允许您单独调配吞吐量和消耗任何规模，每个 DocumentDB 集合用于存储全局跨数据库帐户相关联的所有地区。 DocumentDB 集合自动分布在世界各地并管理所有数据库帐户相关联的区域。 数据库帐户中的集合可以分布在 Azure 中的区域的任何[DocumentDB 服务是用][serviceregions]。 

购买的吞吐量和存储消耗每个 DocumentDB 集合自动配置所有区域间平等。 这允许应用程序无缝地扩展到整个全球[仅对吞吐量和存储您使用的每个小时内支付][pricing]。 例如，如果您已设置 DocumentDB 集合的 2 万个 RUs，然后每个数据库帐户相关联的区域获取 2 百万个 RUs 该集合。 下面对此进行了说明。

![跨四个地区的 DocumentDB 集合的扩展吞吐量][2]

DocumentDB 保证 < 10 毫秒-读取和 < 15 毫秒-写入延迟 P99 处。 读取的请求从未跨数据中心边界，以保证[一致性要求选择了][consistency]。 写操作都确认给客户机之前本地提交仲裁。 每个数据库帐户配置了写地区优先。 具有最高优先级指定的区域将作为当前帐户写区域。 所有 Sdk 将透明地将数据库帐户写入都路由到当前写入的区域。 

最后，因为 DocumentDB 是完全[不可知的架构][ vldb] -您永远不必担心管理更新架构或辅助索引跨多个数据中心。 [SQL 查询][sqlqueries]继续工作而您的应用程序和数据模型在不断发展。 


## <a name="enabling-global-distribution"></a>启用全局通讯组 

您可以决定将由任一关联的本地或全局范围内分发数据与 DocumentDB 的一个或多个 Azure 区域数据库帐户。 您可以添加或随时给您数据库的帐户中删除区域。 若要通过门户网站启用全局通讯组，请参阅[如何执行 DocumentDB 全局数据库复制使用 Azure 的门户](documentdb-portal-global-replication.md)。 若要以编程方式启用全局通讯组，请参阅[多区域 DocumentDB 帐户与开发](documentdb-developing-with-multiple-regions.md)。

## <a name="next-steps"></a>下一步行动

了解更多有关数据分发全局与 DocumentDB 中的以下文章︰

* [吞吐量和集合的存储资源调配] [throughputandstorage]
* [多主页通过 REST 的 Api。.NET、 Java、 Python 和节点的 Sdk] [developingwithmultipleregions]
* [在 DocumentDB 中的一致性级别] [consistency]
* [可用性服务级别协议] [sla]
* [管理数据库帐户] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

