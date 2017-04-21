<properties
    pageTitle="运行使用 HDInsight.NET SDK 配置单元查询 |Microsoft Azure"
    description="了解如何 Hadoop 将作业提交到 Azure HDInsight Hadoop 使用 HDInsight.NET SDK。"
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

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>运行使用 HDInsight.NET SDK 配置单元查询

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


了解如何使用 HDInsight.NET SDK 配置单元查询提交。

> [AZURE.NOTE] 来自 Windows 客户端必须执行本文中的步骤操作。 使用 Linux、 OS X 或 Unix 客户端要使用的配置单元的信息，请使用制表符选择器显示文章的顶部。

##<a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **在 HDInsight 的 Hadoop 群集**。 请参阅[开始使用 HDInsight 中的基于 Linux 的 Hadoop](hdinsight-use-sqoop.md#create-cluster-and-sql-database)。
- **Visual Studio 2012/2013年/2015年**。

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>提交使用 HDInsight.NET SDK 配置单元查询

HDInsight.NET SDK 提供了.NET 客户端库，这使它易于使用 HDInsight 群集从.NET。 

**提交作业**

1. 在 Visual Studio 中创建一个 C# 控制台应用程序。
2. 从 Nuget 程序包管理器控制台上运行以下命令。

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. 使用以下代码︰

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. 按**f5 键**以运行该应用程序。


## <a name="next-steps"></a>下一步行动

在本文中，您学习了几种方法来创建一个 HDInsight 的群集。 若要了解详细信息，请参阅下列文章︰

* [开始使用 Azure HDInsight][hdinsight-get-started]
* [在 HDInsight 中创建 Hadoop 群集][hdinsight-provision]
* [通过使用 Azure 门户管理中 HDInsight 的 Hadoop 群集](hdinsight-administer-use-management-portal.md)
* [HDInsight.NET SDK 参考](https://msdn.microsoft.com/library/mt271028.aspx)
* [使用 HDInsight 的小猪](hdinsight-use-pig.md)
* [使用 HDInsight Sqoop](hdinsight-use-sqoop-mac-linux.md)
* [创建.NET HDInsight 应用程序的非交互式身份验证](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


