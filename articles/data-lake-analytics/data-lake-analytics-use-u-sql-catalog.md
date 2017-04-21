<properties
   pageTitle="引入了 Azure 数据湖分析 U SQL 目录 |Azure"
   description="引入了 Azure 数据湖分析 U SQL 目录"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>使用 U SQL 目录

U SQL 目录用于组织数据和代码，以便它们可以共享由 U SQL 脚本。 目录提供了 Azure 数据湖中的数据可能的最高性能。

Azure 数据湖分析的每个帐户都有一个与之关联的 U SQL 目录。 不能删除 U SQL 目录。 当前 U SQL 目录不能共享数据湖存储帐户之间。

每个 U SQL 目录包含数据库称为**主机**。 不能删除主服务器数据库。  每个 U SQL 目录可以包含多个其他数据库。

U SQL 数据库中包含︰

- 程序集--共享 U SQL 脚本间的.NET 代码。
- 表值函数 – 共享 U SQL 脚本间 U SQL 代码。
- 表 – U SQL 脚本间共享数据。
- 架构的共享在 U SQL 脚本之间的表架构。

## <a name="manage-catalogs"></a>管理目录
Azure 数据湖分析的每个帐户都有与之关联的默认 Azure 数据湖存储帐户。 此数据湖存储帐户被称为默认数据湖存储帐户。 U SQL 目录存储在 /catalog 文件夹下的默认数据湖存储帐户。 不要删除 /catalog 文件夹中的任何文件。

### <a name="use-azure-portal"></a>使用 Azure 的门户网站

请参阅[管理数据湖分析使用门户](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>使用 Visual Studio 数据湖工具。

数据湖工具 Visual Studio 为可用于管理目录。  有关这些工具的详细信息，请参见[使用数据湖 Visual Studio 工具](data-lake-analytics-data-lake-tools-get-started.md)。

**管理目录**

1. 打开 Visual Studio，并连接到 azure。 有关说明，请参阅[连接到 Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure)。
1. 通过按下**CTRL + ALT + S**打开**服务器资源管理器**。
2. 从**服务器资源管理器中**，展开**Azure**、 展开**数据湖分析**、 展开数据湖分析帐户，展开**数据库**，然后展开**主**。



    - 若要添加新的数据库，**数据库**，用鼠标右键单击，然后单击**创建数据库**。
    - 若要添加新的程序集，**程序集**，用鼠标右键单击，然后单击**注册程序集**。
    - 若要添加新架构，**架构**中，用鼠标右键单击，然后单击"创建架构 * *。
    - 若要添加新的表，右击**表**，然后单击""创建表 * *。
    - 若要添加新的表值函数，请参见[开发 U-SQL 用户定义的运算符的数据湖分析作业](data-lake-analytics-u-sql-develop-user-defined-operators.md)。


![浏览 U SQL Visual Studio 目录](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


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

- 端到端教程
    - [使用 Azure 数据湖分析交互式教程](data-lake-analytics-use-interactive-tutorials.md)
    - [分析网站日志使用 Azure 数据湖分析](data-lake-analytics-analyze-weblogs.md)
