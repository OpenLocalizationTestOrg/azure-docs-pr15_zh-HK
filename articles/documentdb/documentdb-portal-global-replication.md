<properties
    pageTitle="DocumentDB 全局数据库复制 |Microsoft Azure"
    description="了解如何管理全球复制您的 DocumentDB 帐户通过 Azure 的门户。"
    services="documentdb"
    keywords="全球数据库复制"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>如何执行使用 Azure 门户 DocumentDB 全局数据库复制

了解如何使用 Azure 门户复制全局数据可用性的 Azure DocumentDB 中的多个区域中的数据。

复制工作在 DocumentDB 中如何全局数据库有关的信息，请参阅[DocumentDB 全局分布数据](documentdb-distribute-data-globally.md)。 有关以编程方式执行全局数据库复制的信息，请参阅[开发与多区域 DocumentDB 帐户](documentdb-developing-with-multiple-regions.md)。

> [AZURE.NOTE] DocumentDB 数据库的全局分布是普遍适用和自动启用任何新创建的 DocumentDB 帐户。 我们正在努力使全球分布上现有的所有帐户，但在此期间，如果您希望在您的帐户上启用全局通讯组请[与支持人员联系](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)，我们将它为您立即启用。

## <a id="addregion"></a>添加全局数据库区域

DocumentDB 是在大多数[Azure 地区] [azureregions]。 在选择数据库帐户的默认一致性级别后，可以将一个或多个区域 （具体取决于您选择的默认一致性级别和全局通讯组需求） 相关联。

1. 在[Azure 的门户](https://portal.azure.com/)，在 Jumpbar 中，请单击**DocumentDB 帐户**。
2. 在**DocumentDB 帐户**刀片式服务器，选择要修改的数据库帐户。
3. 在帐户刀片式服务器，单击菜单中的**全局范围内复制的数据**。
4. 在**全局范围内复制数据**刀片式服务器，选择的地区来添加或删除，然后单击**保存**。 向区域中添加成本，[定价页](https://azure.microsoft.com/pricing/details/documentdb/)或[分布数据全局与 DocumentDB](documentdb-distribute-data-globally.md)文章的详细信息，请参阅。

    ![单击要添加或删除它们的映射中的区域][1]

### <a name="selecting-global-database-regions"></a>选择全局数据库区域

在配置两个或多个区域时，建议根据中所述的地区对选择地区[的业务连续性和灾难恢复 (BCDR): Azure 配对地区] [bcdr]文章。

具体而言，在配置多个区域时，请确保从每个配对的地区列选择相同数量的 （+ /-1 为奇/偶) 的区域。 例如，如果您想要将部署到四个美国地区，选择两个美国区域从左边的列和中右的两个。 因此，下面是一组适当︰ 美国西部、 东部美国、 美国中北部和美国南部中部。

本指南非常重要，只有两个地区针对灾难恢复方案配置时应遵循。 对于两个以上的区域，按照本指南是一个好习惯，但不是严重，只要所选地区的一些符合这种配对。

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>下一步行动

了解如何通过读取[DocumentDB 中的一致性级别](documentdb-consistency-levels.md)管理全局范围内复制帐户的一致性。

复制工作在 DocumentDB 中如何全局数据库有关的信息，请参阅[DocumentDB 全局分布数据](documentdb-distribute-data-globally.md)。 有关以编程方式复制多个区域中的数据的信息，请参见[开发与多区域 DocumentDB 帐户](documentdb-developing-with-multiple-regions.md)。

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
