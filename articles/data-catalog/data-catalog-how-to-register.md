<properties
   pageTitle="如何注册数据源 |Microsoft Azure"
   description="如何文章突出显示如何注册 Azure 数据目录，其中包括在注册过程中提取的元数据字段的数据源。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-register-data-sources"></a>如何注册数据源

## <a name="introduction"></a>介绍
**Microsoft Azure 数据目录**是查询的一种完全托管的云服务，作为注册的系统和企业数据源的系统。 换句话说， **Azure 数据目录**是帮助人了解、 理解和使用数据源，并帮助企业从他们现有的数据中获取更大的价值。 并使数据源可通过**Azure 数据目录**发现的第一步是注册该数据源。
## <a name="registering-data-sources"></a>注册数据源
注册是从数据源中提取元数据并将这些数据复制到**Azure 数据目录**服务的过程。 数据保持在当前驻留，和保持在管理员的控制和当前系统的策略。

若要注册的数据源，只需启动从**Azure 数据目录**入口的**Azure 数据目录**数据源注册工具。 登录您的工作或学校帐户 （相同 Azure Active Directory 的凭据用于登录到门户中），然后选择您想要注册的数据源。
[Azure 数据目录入门](data-catalog-get-started.md)教程有关详细的分步信息，请参阅。

已注册的数据源，一旦目录跟踪它的位置和索引它的元数据，以便用户可以搜索、 浏览和查找数据源，然后使用它的位置连接到该应用程序或他们选择的工具。

## <a name="sources-supported"></a>支持的来源
请参阅[数据目录 DSR](data-catalog-dsr.md)的当前支持的数据源的列表。
<br/>


## <a name="structural-metadata"></a>结构的元数据
要注册的数据源，当注册工具将提取所选的对象的结构信息--这是到为结构化引用的元数据。

对于所有的对象，此结构的元数据将包括对象的位置，以便发现数据的用户可以使用这些信息连接到在客户端工具他们选择的对象。 其他结构的元数据包含对象名称和类型，和属性中的列名称和数据类型。

## <a name="descriptive-metadata"></a>描述性元数据
除了核心结构元数据从数据源提取数据源注册工具还抽取描述性元数据也。 对于 SQL Server Analysis Services 和 SQL Server 报告服务，这是来自公开这些服务的描述属性。 对于 SQL Server 使用 ms_description 扩展属性提供的值将被提取。 对于 Oracle 数据库，数据源注册工具将从 ALL_TAB_COMMENTS 视图中提取的注释列。

除了从数据源中提取描述元数据的情况下，用户还可以输入描述性元数据中使用的数据源注册工具。 用户可以添加标记，并可以确定专家正在注册的对象。 所有这些描述性元数据复制到**Azure 数据目录**服务以及结构的元数据。

## <a name="including-previews"></a>其中包括预览

默认情况下，仅将元数据从数据源提取并复制到**Azure 数据目录**服务，但它了解数据源通常由更容易通过查看数据的示例包含。

**Azure 数据目录**数据源注册工具允许用户中每个表和视图，注册了包括数据的快照预览。 如果用户选择中注册过程包括预览，注册工具将包括从每个表和视图的达 20 个记录。 此快照复制到目录结构和描述性元数据。


> [AZURE.NOTE]  具有大量列的宽表可能有纳入其预览的记录少于 20 个。


## <a name="including-data-profiles"></a>包括数据配置文件

就像包括预览可以为搜索**Azure 数据目录**中的数据源的用户提供有价值的上下文，包括数据配置文件可以也更加容易理解中发现的数据源。

**Azure 数据目录**数据源注册工具允许用户添加为每个表和视图的注册数据配置文件。 如果用户选择在注册过程包括数据配置文件，注册工具将有关数据汇总统计报告包括在每个表和视图，包括︰

* 行数和对象中的数据的大小
* 最新的数据和对象架构更新日期
* 空的记录和非重复值的列的数量
* 列的最小值、 最大、 平均值和标准偏差值

这些统计数据将复制到结构和描述性元数据目录。

> [AZURE.NOTE]  文本和日期列将不包括平均值或标准偏差的统计信息，其数据配置文件中。

## <a name="updating-registrations"></a>更新登记

注册数据源将使在**Azure 数据目录**使用的元数据和可选预览在注册过程中提取被发现。 如果数据源需要更新目录中 （例如，如果对象的架构已更改，或最初排除的表应包含，或者用户要更新预览中包括的数据） 可以重新运行数据源注册工具。

重新注册一个已注册的数据源执行合并"upsert"的操作︰ 将更新现有的对象，同时将创建的新对象。 通过**Azure 数据目录**门户的用户提供的任何元数据将保持不变。

## <a name="summary"></a>摘要
**Azure 数据目录**中注册的数据源使该数据源更易于发现和理解，通过将结构和描述性元数据从数据源复制到的目录服务。 一旦已注册的数据源，它可以再进行批注，托管，并发现使用**Azure 数据目录**入口。

## <a name="see-also"></a>请参见
- 有关如何注册数据源的逐步骤详细信息[Azure 数据目录入门](data-catalog-get-started.md)教程。
