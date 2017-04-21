<properties
    pageTitle="如何配置文件数据的数据源"
    description="突出显示如何在 Azure 数据目录中注册数据源时包括表和列级数据配置文件以及如何使用配置文件数据来了解数据源的操作方法文章。"
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>数据配置文件数据源

## <a name="introduction"></a>介绍

**Microsoft Azure 数据目录**是查询的一种完全托管的云服务，作为注册的系统和企业数据源的系统。 换句话说， **Azure 数据目录**是帮助人了解、 理解和使用数据源，并帮助企业从他们现有的数据中获取更大的价值。 数据源使用**Azure 数据目录**中注册后，它的元数据复制和索引服务，但故事并未到此结束。

**Azure 数据目录**的**数据分析**功能检查目录中支持的数据源中的数据，并收集统计信息和有关数据的信息。 很容易地添加数据资产的配置文件。 注册数据资产时，请在数据源注册工具中选择**包含数据配置文件**。

## <a name="what-is-data-profiling"></a>什么是数据分析

数据状况分析检查要注册的数据源中的数据，并收集统计信息和有关数据的信息。 在数据源发现，这些统计信息可以帮助您确定要解决其业务问题的数据的适用性。

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

下列数据源支持数据分析︰

- （包括 Azure SQL 数据库和 Azure SQL 数据仓库） 的 SQL Server 表和视图
- Oracle 表和视图
- Teradata 表和视图
- 配置单元表

包括数据配置文件，当用户注册数据资产帮助回答有关数据源，其中包括︰

-   它可用来解决我的业务问题？
-   没有数据符合特定标准或模式？
-   一些异常的数据源是什么？
-   将此数据集成到我的应用程序可能面临的挑战有哪些？

> [AZURE.NOTE] 您还可以将文档添加到资产来描述数据如何可以集成到应用程序。 请参阅[数据源的文档](data-catalog-how-to-documentation.md)。


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>如何注册数据源时包括数据配置文件

很容易地添加数据源的配置文件。 注册时数据源，数据源注册工具的**对象注册**面板中，选择**包含数据配置文件**。

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

若要了解有关如何注册数据源的详细信息，请参阅[如何注册数据源](data-catalog-how-to-register.md)和[开始使用 Azure 数据目录](data-catalog-get-started.md)。


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>筛选的数据资产，包括数据的配置文件
发现数据资产，包括数据配置文件，您可以包括`has:tableDataProfiles`或`has:columnsDataProfiles`为您的搜索条件之一。

> [AZURE.NOTE] 在数据源注册工具中选择**包含配置文件数据**包括表和列级配置文件信息。 但是，数据目录 API 允许数据资产注册的只有一组包含的配置文件信息。

## <a name="viewing-data-profile-information"></a>查看数据的档案文件信息

一旦找到合适的数据源与一个配置文件，您可以查看数据配置文件详细信息。 若要查看数据配置文件，选中数据资产和数据目录门户窗口中选择**数据配置文件**。

![](media\data-catalog-data-profile\data-catalog-view.png)

在**Azure 数据目录**中的数据配置文件显示表和列的配置文件信息，包括︰

### <a name="object-data-profile"></a>对象数据配置文件

-   行数
-   表的大小
-   上次更新对象

### <a name="column-data-profile"></a>列数据配置文件

- 列数据类型
- 不同值的数量
- 含有空值的行数
- 最小值、 最大值、 平均值、 和列的值的标准偏差

## <a name="summary"></a>摘要
数据分析提供统计数据和有关已注册的数据资产，以帮助您确定要解决的业务问题的数据的适用性。 以及添加批注和记录的数据源，数据配置文件可以使用户数据的更深的了解。


## <a name="see-also"></a>请参见
-   [如何注册数据源](data-catalog-how-to-register.md)
-   [开始使用 Azure 数据目录](data-catalog-get-started.md)
