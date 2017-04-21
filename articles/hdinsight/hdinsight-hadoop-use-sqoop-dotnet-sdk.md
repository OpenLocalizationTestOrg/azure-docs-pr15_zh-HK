<properties
    pageTitle="在 HDInsight 中使用 Hadoop Sqoop |Microsoft Azure"
    description="了解如何使用 HDInsight.NET SDK 运行 Sqoop 的导入和导出 Hadoop 群集 SQL Azure 数据库之间。"
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
   ms.date="09/14/2016"
    ms.author="jgao"/>

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>运行 Hadoop 在 HDInsight 中使用.NET SDK Sqoop 作业

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

了解如何使用 HDInsight.NET SDK 以运行导入和导出 HDInsight 群集和 SQL Azure 数据库或 SQL Server 数据库之间的 HDInsight Sqoop 作业。

> [AZURE.NOTE] 这篇文章中的步骤可与任何基于 Windows 或基于 Linux 的 HDInsight 群集;但是，这些步骤仅适用来自 Windows 客户端。 使用本文顶部的选项卡上选择器来选择其他方法。

###<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- **在 HDInsight 的 Hadoop 群集**。 请参阅[创建群集和 SQL 数据库](hdinsight-use-sqoop.md#create-cluster-and-sql-database)。

## <a name="run-sqoop-using-net-sdk"></a>运行使用.NET SDK Sqoop

HDInsight.NET SDK 提供了.NET 客户端库，这使它易于使用 HDInsight 群集从.NET。 在本部分中，您将创建一个 C# 控制台应用程序导出到您之前在此教程中创建的 SQL 数据库表的 hivesampletable。

**若要提交 Sqoop 作业**

1. 在 Visual Studio 中创建一个 C# 控制台应用程序。
2. 在 Visual Studio 程序包管理器控制台上运行以下 Nuget 命令导入包。

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. 在 Program.cs 文件中使用以下代码︰

        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
        
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. 按**f5 键**以运行该程序。 

##<a name="limitations"></a>限制

* 批量导出-基于与 Linux 的 HDInsight、 Sqoop 连接器用于将数据导出到 Microsoft SQL Server 或 SQL Azure 数据库当前不支持大容量插入。

* 属性的基于 Linux 的 HDInsight，当使用`-batch`开关执行插入时，Sqoop 将执行多个而不是批处理插入操作的插入。

##<a name="next-steps"></a>下一步行动

现在您已了解如何使用 Sqoop。 若要了解详细信息，请参阅︰

- [与 HDInsight 使用 Oozie](hdinsight-use-oozie.md): Oozie 工作流中的使用 Sqoop 操作。
- [使用 HDInsight 分析航班延迟数据](hdinsight-analyze-flight-delay-data.md)︰ 使用配置单元来分析飞行延迟数据，然后使用 Sqoop 将数据导出到 SQL Azure 数据库。
- [上载到 HDInsight 的数据](hdinsight-upload-data.md)︰ 找到其他方法，用于将数据上载到 HDInsight/Azure Blob 存储。


