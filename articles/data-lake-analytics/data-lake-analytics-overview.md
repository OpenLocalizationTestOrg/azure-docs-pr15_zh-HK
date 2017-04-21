<properties 
   pageTitle="Microsoft Azure 数据湖分析概述 |Azure" 
   description="数据湖分析是一种允许您使用数据来促进您的业务使用从您的数据在云中，而不考虑其所在，不管其规模如何获得见解 Azure 大数据计算服务。 数据湖分析使这最简单，最具可扩展性，和最经济的方式。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Microsoft Azure 数据湖分析的概述

## <a name="what-is-azure-data-lake-analytics"></a>什么是 Azure 数据湖分析？

Azure 数据湖分析是一种新的服务，为大数据分析很容易构建。 您可以集中精力编写、 运行和管理的作业，而不是运行此服务允许分布式基础结构。 而不是部署、 配置和调整的硬件，您可以编写查询来转换数据并提取有价值的见解。 分析服务可以通过仅仅设置为您需要多少能源拨立即处理作业的任何比例。 当它运行时进行经济仅支付作业系统。 分析服务支持 Azure Active Directory 让您只需管理访问权限和角色，您的内部标识系统与集成在一起。 它还包括 U SQL，统一了 SQL 的好处与用户代码的表现力的语言。 U-SQL 可扩展分布式运行时使您能够有效地分析数据存储区中并在 Azure，SQL Azure 数据库和 Azure SQL 数据仓库中的 SQL 服务器。


## <a name="key-capabilities"></a>关键功能

- **动态缩放** 

    数据湖分析完全针对云规模和性能是体系结构。  动态调配资源，并允许您执行万亿字节或甚至千兆兆字节的数据上的分析。 在作业完成后，它即将结束时的资源自动，和只支付使用的处理能力。 当您增加或减少的数据存储的大小或使用的计算量，您不必重新编写代码。 这样您专注于您的业务逻辑只而不是在您处理和存储大数据集。 

- **更快地开发、 调试和优化更聪明地使用熟悉的工具**

    数据湖分析的深度集成与 Visual Studio，以便可以使用熟悉的工具来运行、 调试和优化您的代码。 U SQL 作业的可视化效果使您可以查看您的代码如何运行在比例，以便您可以轻松地识别性能瓶颈和优化成本。 

- **U-SQL︰ 简单和熟悉的功能强大、 可扩展**

    数据湖分析包括 U SQL，扩展了熟悉的、 简单的声明性性质的 C# 表现力与 SQL 的查询语言。 在相同的分布式运行时，背后的大数据系统在 Microsoft 内部建立了 U SQL 语言。 数以百万计的 SQL 和.NET 开发人员现在可以处理和分析所有其数据与他们已有的技能。

- **无缝集成与您的 IT 投资**

    数据湖分析可用于标识、 管理、 安全性和数据仓库的现有的 IT 投资。 这简化了数据管理，并可以轻松地扩展您当前的数据应用程序。 数据湖分析与 Active Directory 集成的用户管理和权限和附带了内置监视和审核。

- **价格合理且经济高效**

    数据湖分析是一种经济高效的解决方案，用于运行工作负载大数据。 在处理数据时，支付在每个工作的基础上。 没有硬件、 许可证或特定于服务的支持协议是必需的。 系统会自动随着向上或向下作业开始和完成，意味着永远不会支付超过您的需要。 

- **适用于所有的 Azure 数据**

    数据湖分析可以使用多个 Azure 数据源︰ Azure Blob 存储、 SQL Azure 数据库当然数据湖分析精心优化和使用 Azure 数据湖商店-大数据的工作负载提供最高级别的性能、 吞吐量和为您的并行化。

## <a name="see-also"></a>请参见

- 入门
    - [有关数据使用 Azure 门户的湖泊分析入门](data-lake-analytics-get-started-portal.md)
    - [有关数据使用 Azure PowerShell 的湖泊分析入门](data-lake-analytics-get-started-powershell.md)
    - [有关数据使用 Azure.NET SDK 的湖泊分析入门](data-lake-analytics-get-started-net-sdk.md)
    - [开发数据湖工具用于 Visual Studio U SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)
    - [Azure 数据湖分析 U SQL 语言入门](data-lake-analytics-u-sql-get-started.md)
    
- U SQL 与开发
    - [Azure 数据湖分析 U SQL 语言入门](data-lake-analytics-u-sql-get-started.md)
    - [Azure 数据湖分析作业中使用 U SQL 窗口函数](data-lake-analytics-use-window-functions.md)
    - [开发数据湖分析作业的 U SQL 用户定义运算符](data-lake-analytics-u-sql-develop-user-defined-operators.md)
    
- 管理
    - [管理 Azure 数据湖分析使用 Azure 门户](data-lake-analytics-manage-use-portal.md)
    - [管理使用 Azure PowerShell Azure 数据湖分析](data-lake-analytics-manage-use-powershell.md)
    - [监视和故障排除使用 Azure 门户的 Azure 数据湖分析作业](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
    - [对于 Azure 数据湖分析访问诊断日志](data-lake-analytics-diagnostic-logs.md)

- 端到端教程
    - [使用 Azure 数据湖分析交互式教程](data-lake-analytics-use-interactive-tutorials.md)
    - [分析网站日志使用 Azure 数据湖分析](data-lake-analytics-analyze-weblogs.md)

- 让我们知道您的想法
    - [我们的文档待办事项上的注释](data-lake-analytics-documentation-backlog.md)
    - [提交请求功能](http://aka.ms/adlafeedback)
    - [在论坛中获得帮助](http://aka.ms/adlaforums)


