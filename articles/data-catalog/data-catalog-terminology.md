<properties
   pageTitle="Azure 数据目录术语 |Microsoft Azure"
   description="本文介绍了概念和 Azure 数据目录文档中使用的术语。"
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
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure 数据目录术语

## <a name="catalog"></a>目录

Azure 数据目录是基于云的元数据存储库可以注册哪些数据源和数据资产。 目录将作为中央存储位置从数据源中提取结构化元数据和由用户添加描述性元数据。

## <a name="data-source"></a>数据源

数据源是系统或管理数据资产的容器。 例如，SQL Server 数据库，Oracle 数据库，SQL Server Analysis Services 数据库 （表格或多维） 和 SQL Server 报表服务服务器。

## <a name="data-asset"></a>数据资产

数据资产是可以与目录中注册的数据源中包含的对象。 示例包括 SQL Server 表和视图、 Oracle 表和视图、 SQL Server Analysis Services 度量值、 维度和 Kpi，以及 SQL Server 报表服务报告。

## <a name="data-asset-location"></a>数据资产的位置

目录存储数据源或数据资源，可以用于连接到使用客户端应用程序的源的位置。 根据数据源类型变化的格式和位置的详细信息。 例如，SQL Server 表可以通过其四部分名称 – 服务器名称、 数据库名称、 架构名称、 对象名称 – 标识而 SQL Server 报告服务报告可以通过其 URL 标识。

## <a name="structural-metadata"></a>结构的元数据

结构的元数据是描述数据资产的结构的数据源中提取元数据。 这包括资产位置、 对象名称和类型，及其特定类型的其他特性。 例如，表和视图的结构元数据包含的名称和对象的列的数据类型。

## <a name="descriptive-metadata"></a>描述性元数据

描述性元数据是元数据中描述的目的或意图的数据资产。 目录用户使用 Azure 数据目录入口，通常添加描述性元数据，但它可以还提取数据源的注册过程。 例如，Azure 数据目录注册工具将提取说明从 SQL Server Analysis Services 和 SQL Server 报告服务，在说明属性和[ms_description 扩展属性](https://technet.microsoft.com/library/ms190243.aspx)在 SQL Server 数据库中，如果用值填充这些属性了。

## <a name="request-access"></a>请求的访问权限

数据资产的描述性元数据可以包含有关如何请求访问数据源的数据资产。 此信息显示资产所在的数据，并可以包括一个或多个下列选项︰

- 电子邮件地址的用户或团队负责授予访问权限的数据源。
- 有案可稽过程必须遵循用户访问数据源的 URL。
- 身份和访问管理工具 （例如 Microsoft 标识管理器），可用于访问数据源的 URL。
- 自由文本项描述用户能够对数据源的访问。

## <a name="preview"></a>预览

在 Azure 数据目录中的预览，可以在注册期间，从数据源中提取和存储在与数据资产元数据目录中的最多 20 个记录的快照。 预览可以帮助用户发现数据资产更好地了解其功能和用途。 换句话说，看到示例数据可以比看到刚才的列名和数据类型更重要。
预览只支持对于表和视图，并且必须显式选择用户在注册过程。

## <a name="data-profile"></a>数据配置文件

在 Azure 数据目录中的数据配置文件是可以在注册期间，从数据源中提取和存储在与数据资产元数据目录中注册的数据资产有关的表级和列级元数据的快照。 数据配置文件可帮助用户发现数据资产更好地了解其功能和用途。 类似于预览，数据配置文件必须显式选择用户在注册过程。

> [AZURE.NOTE] 提取数据配置文件是一个代价高昂的操作对于大表和视图，并可能会大大增加注册数据源所需的时间。

## <a name="user-perspective"></a>用户的视角

在 Azure 数据目录中，任何用户都可以为已注册的数据资产提供描述性元数据。 每个用户有不同的角度来看，数据和使用它。 例如，负责服务器的管理员可能会提供其服务级别协议 (SLA) 或备份窗口; 备份的详细信息数据管家可以提供指向文档的业务流程的数据支持;然后分析人员可能会提供最相关的其他分析师，并且可以对这些用户发现并理解数据所需要的最有价值的条款中的描述。

每个这些视角是本身就有价值，并使用 Azure 数据目录的每个用户可以提供所有用户可以都使用这些信息来了解数据和它的用途时，有意义的信息。

## <a name="expert"></a>专家

一位专家是已发现具有明智"专业"的角度来看数据资产的用户。 任何用户可以添加自己或其他用户作为资产方面的专家。 被列为专家不转让任何其他特权在 Azure 数据目录;它允许用户方便地找到那些最有可能在查看某项资产的描述性元数据时很有用的视角。

## <a name="owner"></a>所有者

所有者是具有管理 Azure 数据目录中的数据资产的其他权限的用户身份。 用户可以获得注册的数据资产的所有权，并作为共同所有者，所有者可以添加其他用户。 有关详细信息，请参阅[如何管理数据资产](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] 仅在标准版的 Azure 数据目录中有所有权和管理。

## <a name="registration"></a>注册

注册是从数据源中提取数据资产元数据并将其复制到 Azure 数据目录服务的操作。 已注册的数据资产可以进行批注和发现。

## <a name="see-also"></a>请参见

- [什么是 Azure 数据目录？](data-catalog-what-is-data-catalog.md) -这篇文章概述了 Azure 数据目录服务，它提供了的值，以及它支持的方案。

- [开始使用 Azure 数据目录](data-catalog-get-started.md)-这篇文章提供了端到端教程，说明如何使用数据源发现 Azure 数据目录。  
