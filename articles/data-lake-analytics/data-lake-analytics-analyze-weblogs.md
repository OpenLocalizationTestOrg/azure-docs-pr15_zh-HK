<properties
   pageTitle="分析网站日志使用 Azure 数据湖分析 |Azure"
   description="了解如何分析使用数据湖分析网站日志。 "
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

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>教程︰ 分析网站日志使用 Azure 数据湖分析

了解如何分析网站日志使用数据的湖泊分析，尤其是在找出哪些引用它们尝试访问网站时遇到错误。

>[AZURE.NOTE] 如果只想看到使用的应用程序，从而节省了时间，若要通过[使用 Azure 数据湖分析交互式教程](data-lake-analytics-use-interactive-tutorials.md)。 本教程基于相同的背景情况和相同的代码。 本教程的目的是为开发人员提供了创建和运行端到端的数据湖分析应用程序的体验。

## <a name="prerequisites"></a>系统必备组件︰

- **Visual Studio 2015年，Visual Studio 2013年更新 4 或用 Visual C++ 安装的 Visual Studio 2012**。
- **Microsoft 的.NET 版本 2.5 或更高的 Azure SDK**。  安装使用[Web 平台安装程序](http://www.microsoft.com/web/downloads/platform.aspx)。
- **[Visual Studio 工具数据湖](http://aka.ms/adltoolsvs)**。

    一旦安装了 Visual Studio 的数据湖工具，您将看到 Visual Studio 中的**数据湖**菜单︰

    ![U SQL Visual Studio 菜单](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **数据湖分析和 Visual Studio 数据湖工具的基本知识**。 若要开始，请参阅︰

    - [开始使用 Azure 数据湖分析使用 Azure 门户](data-lake-analytics-get-started-portal.md)。
    - [使用 Visual Studio 工具数据湖开发 U-SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)。

- **一种数据湖分析帐户。**  请参阅[创建 Azure 数据湖分析帐户](data-lake-analytics-get-started-portal.md#create_adl_analytics_account)。

    数据湖工具不支持创建数据湖分析帐户。  因此，您需要创建使用 Azure 门户，Azure PowerShell，.NET SDK 或 Azure CLI。
- **将示例数据上载到数据湖分析帐户。** 请参阅[上载到默认数据湖存储帐户的 SearchLog.tsv](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account)。

    若要运行数据湖分析作业，将需要一些数据。 即使数据湖工具支持上载数据，您将使用门户上载为了便于按照本教程中的示例数据。

## <a name="connect-to-azure"></a>连接到 Azure

您可以生成并测试任何 U SQL 脚本之前，您必须首先连接到 Azure。

**若要连接到数据湖分析**

1. 打开 Visual Studio。
2. 从**数据湖**菜单中，单击**选项和设置**。
4. 如果某人已经登录，单击**登录**，或者**更改用户**，请按照说明进行操作。
5. 单击**确定**以关闭选项和设置对话框。

**若要浏览数据湖分析帐户**

1. 从 Visual Studio，通过按**CTRL + ALT + S**打开**服务器资源管理器**。
2. 从**服务器资源管理器中**，展开**Azure**，然后再展开**数据湖分析**。 如果有的话，您应看到数据湖分析帐户的列表。 不能创建从工作室的数据湖分析帐户。 若要创建一个帐户，请参阅[开始使用 Azure 数据湖分析使用 Azure 门户](data-lake-analytics-get-started-portal.md)或[入门使用 Azure PowerShell Azure 数据湖分析](data-lake-analytics-get-started-powershell.md)。

## <a name="develop-u-sql-application"></a>开发 U SQL 应用程序

U SQL 应用程序是主要的 U SQL 脚本。 若要了解有关 U SQL 的详细信息，请参阅[开始使用 U SQL](data-lake-analytics-u-sql-get-started.md)。

您可以向应用程序添加添加用户定义的运算符。  有关详细信息，请参见[开发 U-SQL 用户定义的运算符的数据湖分析作业](data-lake-analytics-u-sql-develop-user-defined-operators.md)。

**若要创建和提交数据湖分析作业**

1. 从**文件**菜单上，单击**新建**，然后单击**项目**。
2. 选择 U SQL 项目类型。

    ![新 U SQL Visual Studio 项目](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. 单击**确定**。 与 Script.usql 文件，visual studio 将创建一个解决方案。
4. 在 Script.usql 文件中输入下面的脚本︰

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    要了解 U SQL，请参阅[入门数据湖分析 U SQL 语言](data-lake-analytics-u-sql-get-started.md)。    

5. 向项目中添加新的 U SQL 脚本并输入以下命令︰

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. 切换回第一个 U SQL 脚本和旁边的**提交**按钮，指定分析帐户。
7. 从**解决方案资源管理器中**，右键单击**Script.usql**，然后单击**生成脚本**。 验证在输出窗格中的结果。
8. 从**解决方案资源管理器中**，右键单击**Script.usql**，，然后单击**提交脚本**。
9. 验证**分析帐户**是一个要用来运行该作业，然后单击**提交**。 完成提交后，提交结果和作业链接是在 Visual Studio 的结果窗口的数据湖工具可用。
10. 等待，直到作业成功完成。  如果作业失败，它很可能缺少源文件。  请参阅本教程中的系统必备部分。 有关故障排除的其他信息，请参阅[显示器并解决 Azure 数据湖分析作业](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)。

    完成作业后，您应看到下面的屏幕︰

    ![数据湖分析分析网络日志网站日志](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. 现在**Script1.usql**重复步骤 7-10。

>[AZURE.NOTE]您不能读取或写入到 U SQL 表已创建或修改同一个脚本中。  这就是为什么使用此示例中使用两个脚本。

**若要查看作业输出**

1. 从**服务器资源管理器中**，展开**Azure**、 展开**数据湖分析**、 展开数据湖分析帐户，展开**存储帐户**，用鼠标右键单击默认的数据湖存储帐户，然后单击**资源管理器**。
2.  **示例**打开的文件夹中，双击，然后双击**输出**。
3.  双击**UnsuccessfulResponsees.log**。
4.  您还可以直接定位到输出双击作业的关系图视图中的输出文件。

## <a name="see-also"></a>请参见

要开始使用数据使用不同的工具的湖泊分析，请参阅︰

- [有关数据使用 Azure 门户的湖泊分析入门](data-lake-analytics-get-started-portal.md)
- [有关数据使用 Azure PowerShell 的湖泊分析入门](data-lake-analytics-get-started-powershell.md)
- [数据使用.NET SDK 的湖泊分析入门](data-lake-analytics-get-started-net-sdk.md)

若要查看更多开发主题︰

- [开发数据湖工具用于 Visual Studio U SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure 数据湖分析 U SQL 语言入门](data-lake-analytics-u-sql-get-started.md)
- [开发数据湖分析作业的 U SQL 用户定义运算符](data-lake-analytics-u-sql-develop-user-defined-operators.md)
