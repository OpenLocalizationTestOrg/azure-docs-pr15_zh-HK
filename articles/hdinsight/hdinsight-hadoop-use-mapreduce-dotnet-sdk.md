<properties
    pageTitle="提交使用 HDInsight.NET SDK 的 MapReduce 作业 |Microsoft Azure"
    description="了解如何提交到 Azure HDInsight Hadoop 使用 HDInsight.NET SDK 的 MapReduce 作业。"
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
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>运行 MapReduce 作业使用 HDInsight.NET SDK

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

了解如何使用 HDInsight.NET SDK 的 MapReduce 作业提交。 HDInsight 群集附带一些 MapReduce 示例 jar 文件。 Jar 文件是*/example/jars/hadoop-mapreduce-examples.jar*。  一个示例就是*字数统计*。 开发 C# 控制台应用程序提交的字数统计作业。  该作业读取*/example/data/gutenberg/davinci.txt*文件，并将结果输出到*/example/data/davinciwordcount*。  如果您想要重新运行应用程序，您必须清除输出文件夹。

> [AZURE.NOTE] 来自 Windows 客户端必须执行本文中的步骤操作。 使用 Linux、 OS X 或 Unix 客户端要使用的配置单元的信息，请使用制表符选择器显示文章的顶部。

##<a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **在 HDInsight 的 Hadoop 群集**。 请参阅[开始使用 HDInsight 中的基于 Linux 的 Hadoop](hdinsight-use-sqoop.md#create-cluster-and-sql-database)。
- **Visual Studio 2012/2013年/2015年**。

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>提交使用 HDInsight.NET SDK 的 MapReduce 作业

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

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
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

- 提交配置单元作业创建群集，请参阅[开始使用 Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)。
- 有关如何创建 HDInsight 群集，请参阅[HDInsight 中的基于创建 Linux 的 Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)。
- 有关管理 HDInsight 群集，请参阅[HDInsight 中的管理 Hadoop 群集](hdinsight-administer-use-management-portal.md)。
- 学习 HDInsight.NET SDK，请参阅[HDInsight.NET SDK 参考](https://msdn.microsoft.com/library/mt271028.aspx)。
- 为非交互式验证到 Azure，请参阅[创建非交互式身份验证.NET HDInsight 应用程序](hdinsight-create-non-interactive-authentication-dotnet-applications.md)。




