<properties
   pageTitle="Azure 数据湖存储概述 |Microsoft Azure"
   description="了解什么是 Azure 数据湖商店和其他数据存储在其提供的价值"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Azure 湖存储数据的概览

Azure 数据湖商店是大数据分析工作负荷企业级超大规模存储库。 Azure 数据湖使您能够捕获数据的操作和探索性分析在一个单一位置中的任何大小、 类型和接收速度。

> [AZURE.TIP] 使用[数据湖商店学习路径](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/)探索 Azure 数据湖商店服务。

Azure 数据湖商店可以通过 （使用 HDInsight 群集可用） Hadoop 使用 WebHDFS 兼容的 REST Api。 它专门设计用于启用对存储数据的分析，并进行的数据分析方案的性能优化。 开箱即用，它包括所有的企业级功能 — — 安全性、 可管理性、 可扩展性、 可靠性和可用性 — — 真实的企业用例非常重要。


![Azure 数据湖](./media/data-lake-store-overview/data-lake-store-concept.png)

一些 Azure 数据湖的主要功能包括以下内容。

### <a name="built-for-hadoop"></a>Hadoop 为构建

Azure 数据湖商店是 Apache Hadoop 文件系统与 Hadoop 分布式文件系统 (HDFS) 和使用 Hadoop 生态系统兼容。  现有的 HDInsight 应用程序或服务使用 WebHDFS API，可以轻松地集成与数据湖存储区。 数据湖商店还公开应用程序的其余部分 WebHDFS 兼容接口

数据湖存储区中存储的数据可以使用 Hadoop 分析框架，例如，MapReduce 或配置单元进行轻松地分析。 可以设置 Microsoft Azure HDInsight 群集，并将其配置为直接访问数据湖存储中的数据。

### <a name="unlimited-storage-petabyte-files"></a>无限制的存储，petabyte 文件

Azure 数据湖商店提供无限的存储并且适合用于存储各种数据的分析。 它不会施加任何限制帐户大小、 文件的大小或将存储在数据湖泊的数据量。 单个文件的范围从千到 petabytes 的大小，使其成为不错的选择，来存储任何类型的数据。 通过创建多个副本来持久存储数据，其数据可以存储在数据湖边的时间长短没有限制。

### <a name="performance-tuned-for-big-data-analytics"></a>针对大数据分析性能优化

Azure 数据湖商店生成运行较大的比例，需要巨大的吞吐量来查询和分析大量的数据分析系统。 数据湖经过数个存储服务器传播文件的各个部分。 读取文件并行执行数据分析时，这将会提高的读的吞吐量。


### <a name="enterprise-ready-highly-available-and-secure"></a>企业就绪︰ 高可用性和安全

Azure 数据湖存储提供了工业标准的可用性和可靠性。 冗余的拷贝，以防止任何意外的失败，从而持久存储您的数据资产。 企业可以使用 Azure 数据湖在其解决方案中作为其现有的数据平台的重要组成部分。

数据湖商店还提供了企业级存储的数据的安全性。 有关详细信息，请参阅[保护 Azure 数据湖存储区中的数据](#DataLakeStoreSecurity)。


### <a name="all-data"></a>所有数据

Azure 数据湖存储库可以存储任何数据以其本机格式时，无需任何事先的转换。 数据湖商店不需要一个架构来定义之前已经加载了数据，并由单独的分析框架来解释数据并定义时的分析架构。 能够存储任意大小和格式的文件，便于数据湖存储来处理结构化、 半结构化和非结构化数据。

Azure 数据湖存储数据的容器是实质上的文件夹和文件。 您对使用 Sdk，Azure 门户和 Azure Powershell 所存储的数据进行操作。 只要您将您的数据放入使用这些接口的存储和使用适当的容器，您可以存储任何类型的数据。 数据湖商店不执行根据它所存储的数据类型的任何的数据特殊处理。


## <a name="DataLakeStoreSecurity"></a>保护 Azure 数据湖存储区中的数据

Azure 数据湖商店使用 Azure Active Directory 进行身份验证和访问控制列表 (Acl) 来管理对数据的访问。

| 功能                                 | 说明                              |
|-----------------------------------------|------------------------------------------|
| 身份验证 | Azure 数据湖商店集成使用 Azure 活动目录 (AAD) 的身份和访问管理 Azure 数据湖存储中存储的所有数据。 由于集成，Azure 数据湖受益所有 AAD 功能包括多因素身份验证、 条件访问、 基于角色的访问控制、 应用程序使用监控、 安全监视和警报，等等。Azure 数据湖存储进行验证所使用的 REST 接口中支持 OAuth 2.0 协议。 |
| 访问控制                          | Azure 数据湖商店通过支持 POSIX 样式所公开的 WebHDFS 协议的权限提供的访问控制。 在当前版本中，可以在根文件夹、 子文件夹，以及个别文件启用 Acl。 应用于根文件夹的 Acl 将还适用于所有子文件夹/文件。|

想要了解更多关于保护数据湖存储区中的数据。 请按照下面的链接。

* 如何保护数据湖存储区中的数据的说明，请参阅[保护 Azure 数据湖存储区中的数据](data-lake-store-secure-data.md)。
* 喜欢的视频？ [观看这段视频](https://mix.office.com/watch/1q2mgzh9nn5lx)如何保护数据湖存储中的数据。

## <a name="applications-compatible-with-azure-data-lake-store"></a>Azure 数据湖商店与兼容的应用程序

Azure 数据湖商店是兼容与 Hadoop 生态系统中的多数开放源组件。 它还可以集成可以很好地与其他 Azure 服务。 这使得数据湖存储您的数据存储需要一个完美的选择。 请按照下面的链接以了解更多有关如何可以同时使用开源组件，以及其他 Azure 服务使用数据湖商店。

* 开放源代码应用程序数据湖商店与互操作的列表，请参见[应用程序和服务符合 Azure 数据湖存储区](data-lake-store-compatible-oss-other-applications.md)。
* 请参阅以了解如何数据湖商店可以使用与其他 Azure 服务，以便进行更广泛的方案[与其他 Azure 服务的集成](data-lake-store-integrate-with-other-services.md)。
* 请参阅[使用数据湖商店的方案](data-lake-store-data-scenarios.md)若要了解如何在接收数据、 处理数据、 下载数据，和可视化数据等的方案中使用数据湖存储区。

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>什么是 Azure 数据湖存储文件系统 (adl: / /)？

数据湖商店可以通过新的文件系统，AzureDataLakeFilesystem (adl: / /)，Hadoop 环境 （使用 HDInsight 群集可用） 中。 应用程序和服务使用 adl: / / 能够利用更多不是 WebHDFS 中当前可用的性能优化。 因此，数据湖商店，灵活地可以获得最佳的性能与使用 adl 的推荐选项: / / 或继续使用 WebHDFS API 直接维护现有的代码。 Azure HDInsight 充分利用了 AzureDataLakeFilesystem 数据湖存储区上提供最佳的性能。

您可以访问您的数据在数据湖商店中使用`adl://<data_lake_store_name>.azuredatalakestore.net`。 有关如何访问数据湖存储区中的数据的详细信息，请参阅[查看所存储的数据的属性](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>如何开始使用 Azure 数据湖商店？

请参阅[数据湖商店开始使用 Azure 门户](data-lake-store-get-started-portal.md)，如何调配数据湖商店使用 Azure 门户。 一旦您已设置 Azure 数据湖，可以了解如何使用数据湖存储大数据产品 Azure 数据湖分析或 Azure HDInsight 等。 您还可以创建.NET 应用程序来创建一个 Azure 数据湖存储帐户并执行操作，例如上载数据，下载数据，等等。

- [开始使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
- [学习如何使用 Azure 数据湖存储区使用.NET SDK](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>数据湖存储视频

如果您愿意看录像来了解，数据湖商店提供一系列功能视频。

* [创建一个 Azure 数据湖存储帐户](https://mix.office.com/watch/1k1cycy4l4gen)
* [使用数据资源管理器来管理 Azure 数据湖存储区中的数据](https://mix.office.com/watch/icletrxrh6pc)
* [连接到 Azure 数据湖库的 Azure 数据湖分析](https://mix.office.com/watch/qwji0dc9rx9k)
* [通过数据湖分析访问 Azure 数据湖商店](https://mix.office.com/watch/1n0s45up381a8)
* [连接到 Azure 数据湖库的 Azure HDInsight](https://mix.office.com/watch/l93xri2yhtp2)
* [通过配置单元和小猪的访问 Azure 数据湖存储区](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [使用 DistCp （Hadoop 分布式副本） Azure 数据湖存储之间复制数据](https://mix.office.com/watch/1liuojvdx6sie)
* [Apache Sqoop 用于关系数据源和 Azure 数据湖存储之间移动数据](https://mix.office.com/watch/1butcdjxmu114)
* [数据业务流程为 Azure 数据湖商店使用 Azure 数据工厂](https://mix.office.com/watch/1oa7le7t2u4ka)
* [保护 Azure 数据湖存储区中的数据](https://mix.office.com/watch/1q2mgzh9nn5lx)



