<properties
   pageTitle="开发 U SQL 脚本使用 Visual Studio 数据湖工具 |Azure"
   description="了解如何安装 Visual Studio，如何开发和测试 U SQL 脚本的数据湖工具。 "
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

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>有关 Azure 数据湖分析 U SQL 语言入门教程︰

U SQL 是代码的一种语言，统一了 SQL 具有表现力的您自己来处理所有数据在任何规模较大的优势。 U-SQL 可伸缩的分布式的查询功能使您能够有效地分析数据存储区中和跨关系存储 SQL Azure 数据库等。  它使您能够通过将架构应用于非结构化的数据读取、 插入自定义逻辑和 UDF，和包括的可扩展性，允许对如何扩展在执行进行精细的控制的过程。 若要了解有关 U SQL 的设计理念，请参考此[Visual Studio 的博客文章](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/)。

有一些差异从 ANSI SQL 或 T SQL。 例如，如选择其关键字必须是所有字母都大写。

它的内部 select 子句，谓语等在 C# 中的位置的类型系统和表达式语言。
这意味着数据类型是 C# 类型和数据类型使用 C# 空语义，以及内部谓词的比较操作遵循 C# 语法 （如 = ="foo"）。  这也意味着的值是完整的.NET 对象，使您可以轻松地使用任何方法来操作对象 (如"f o o o"。Split(' '))。

有关详细信息，请参阅[U SQL 参考](http://go.microsoft.com/fwlink/p/?LinkId=691348)。

###<a name="prerequisites"></a>系统必备组件

您必须先完成[教程︰ 开发 U SQL 脚本使用 Visual Studio 数据湖工具](data-lake-analytics-data-lake-tools-get-started.md)。

在本教程中，使用下面 U SQL 脚本运行数据湖分析作业︰

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

此脚本没有任何转换步骤。 它读取源文件名为**SearchLog.tsv**、 schematizes，并输出到一个名为**SearchLog 第一个 u sql.csv**文件的行集。

请注意数据类型的工期域旁边的问号。 这意味着工期域可为 null。

一些概念和脚本中找到的关键字︰

- **行集合变量**︰ 生成行集的每个查询表达式可以分配给一个变量。 U SQL 遵循 T SQL 变量命名模式，例如，**@searchlog**脚本中。
    请注意分配不会强制执行。 它只需命名表达式，使您能够累积到更复杂的表达式。
- **提取**为您提供了读取定义架构的能力。 由列名称和 C# 类型名称对每个列指定了架构。 它使用所谓**提取器**，例如， **Extractors.Tsv()**提取 tsv 文件。 您可以开发自定义提取程序。
- **输出**将行集，并将其序列化。 Outputters.Csv() 输出到指定的位置以逗号分隔的文件。 您还可以开发自定义的 Outputters。
- 请注意这两条路径是相对路径。 您也可以使用绝对路径。  例如

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    您必须使用绝对路径访问中链接的存储帐户的文件。  在链接的 Azure 存储帐户中存储的文件的语法如下︰

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] 目前不支持 azure Blob 容器公钥 blob 或公共容器的访问权限。

## <a name="use-scalar-variables"></a>使用标量变量

可用于标量变量以及使您的脚本维护更容易。 以前的 U SQL 脚本也可以编写如下︰

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>转换的行集

使用**选择**来转换的行集︰

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

WHERE 子句使用[C# 布尔表达式](https://msdn.microsoft.com/library/6a71f45d.aspx)。 C# 表达式语言可用于执行您自己的表达式和函数。 您甚至可以执行更复杂的筛选功能通过将它们结合起来用逻辑连词 (And) 和 disjunctions (ORs)。

下面的脚本使用 DateTime.Parse() 方法，并结合。

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

请注意，第二个查询操作的第一个行集的结果，因此，结果是两个筛选器的组合。 您可以重复使用的变量的名称和名称的范围是从词法上。

## <a name="aggregate-rowsets"></a>聚合的行集

U SQL 提供了熟悉**ORDER BY**， **GROUP BY**和聚合。

下面的查询查找每个地区，总的持续时间，然后输出顺序 5 工期顶部。

U SQL 行集不保留它们的顺序的下一个查询。 因此，排序输出，您需要添加 ORDER BY OUTPUT 语句如下所示︰

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U SQL ORDER BY 子句必须与 SELECT 表达式中提取子句组合。

让 U SQL 子句可用于将输出限制为满足 HAVING 条件的组︰

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>联接数据

U SQL 提供通用联接运算符，如内部联接、 左/右/全外部联接、 半联接联接表不仅任何行集 （甚至包括那些从文件生成）。

下面的脚本加入广告印象日志 searchlog，并为我们提供了查询字符串的通告在给定日期。

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U SQL 只支持 ANSI 兼容连接语法︰ Rowset1 Rowset2 ON 联接谓词。 不支持从 Rowset1，Rowset2 位置谓词的旧语法。
在联接谓词必须相等联接和任何表达式。 如果您想要使用的表达式，则将其添加到前一个行集的 select 子句。 如果您想要做不同的比较，可以将其移动到 WHERE 子句中。


## <a name="create-databases-table-valued-functions-views-and-tables"></a>创建数据库、 表值函数、 视图和表

可以使用 U SQL 数据库和架构的上下文中使用的数据。 因此您不需要始终读取或写入的文件。

每个 U SQL 脚本运行 （主） 的默认数据库和默认架构 (DBO) 作为其默认上下文。 您可以创建您自己的数据库和/或架构。 要变更上下文，使用**使用**语句来更改上下文。


### <a name="create-a-table-valued-function-tvf"></a>创建表值函数 (TVF)

在前面的 U SQL 脚本，您重复使用读取同一源文件中提取。 U SQL 表值函数使您能够封装的数据供将来重复使用。   

下面的脚本将创建默认数据库和架构中调用*Searchlog()* TVF:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

下面的脚本说明了如何使用 TVF 中前面的脚本定义︰

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>创建视图

如果您只有一个查询表达式，您想要抽象并不想将其参数化，则可以创建视图而不是表值函数。

以下脚本创建了一个名为*SearchlogView*中的默认数据库和架构视图︰

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

下面的脚本说明如何使用定义的视图︰

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>创建表

类似于关系数据库的表，U SQL 允许您使用预定义架构创建表或创建表格并推断架构填充 （也称为创建表作为选择或 CTAS） 的表的查询。

下面的脚本创建一个数据库和两个表︰

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>查询表

通过数据文件，可以为您查询相同的方式查询 （在前面的脚本中创建） 的表。 而不是创建一个使用提取的行集，您现在可以只引用表名称。

您以前使用该转换脚本进行修改，以从表中读取︰

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

注意，您当前不能运行选择在同一脚本中为脚本的表，您可在其中创建表。


##<a name="conclusion"></a>结论

在本教程中包含的内容是仅 U SQL 的一小部分。 由于本教程的范围，它不能面面俱到，如︰

- 使用跨应用才能成行解压缩字符串、 数组和映射的部分。
- 操作 （文件集和分区的表） 的数据的分区的集。
- 制定用户定义的运算符，例如提取程序、 outputters、 处理器、 C# 中的用户定义聚合函数。
- 使用 U SQL 窗口函数。
- 管理视图、 表值函数和存储的过程与 U SQL 代码。
- 处理节点上运行任意自定义代码。
- 通过这些和 U SQL 和 Azure 数据湖数据连接到 Azure SQL 数据库和 federate 查询。

## <a name="see-also"></a>请参见

- [Microsoft Azure 数据湖分析的概述](data-lake-analytics-overview.md)
- [开发数据湖工具用于 Visual Studio U SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)
- [使用窗口函数 U SQL Azure 数据湖分析作业](data-lake-analytics-use-window-functions.md)
- [监视和故障排除使用 Azure 门户的 Azure 数据湖分析作业](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>让我们知道您的想法

- [提交请求功能](http://aka.ms/adlafeedback)
- [在论坛中获得帮助](http://aka.ms/adlaforums)
- [在 U SQL 提供反馈](http://aka.ms/usqldiscuss)
