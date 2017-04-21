<properties
   pageTitle="猪的 Hadoop 使用.NET 中 HDInsight |Microsoft Azure"
   description="了解如何使用.NET SDK 的 Hadoop 猪将作业提交到在 HDInsight 上的 Hadoop。"
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>运行 Hadoop 在 HDInsight 中使用.NET SDK 的猪的作业

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

本文档提供使用 Hadoop 的.NET SDK 豚将作业提交到 HDInsight 群集上 Hadoop 的示例。

HDInsight.NET SDK 提供了便于更方便地使用 HDInsight 群集从.NET 的.NET 客户端库。 猪可以通过 MapReduce 操作建模的一系列数据转换。 您将学习如何使用基本的 C# 应用程序提交到 HDInsight 群集猪的作业。

## <a name="prerequisites"></a>系统必备组件

若要完成本文中的步骤操作，您需要。

* Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 (Windows 或基于 Linux 的)。
* Visual Studio 2012 或 2013年或 2015年。

## <a name="create-the-application"></a>创建应用程序

HDInsight.NET SDK 提供了.NET 客户端库，这使它易于使用 HDInsight 群集从.NET。 


1. 打开 Visual Studio 2012 或 2013
2. 从**文件**菜单中，选择**新建**，然后选择**项目**。
3. 新的项目中，键入或选择以下值。

    <table>
    <tr>
    <th>属性</th>
    <th>值</th>
    </tr>
    <tr>
    <th>类别</th>
    <th>模板/视频 C# / 窗口</th>
    </tr>
    <tr>
    <th>模板</th>
    <th>控制台应用程序</th>
    </tr>
    <tr>
    <th>名称</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. 单击**确定**以创建项目。
5. 从**工具**菜单中，选择**库程序包管理器**或**Nuget 程序包管理器**，然后选择**程序包管理器控制台**。
6. 控制台可以安装.NET SDK 包中运行以下命令。

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. 从解决方案资源管理器中，双击**Program.cs**以将其打开。 现有代码替换为以下。

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. 按**f5 键**以启动该应用程序。
8. 按**enter 键**，退出应用程序。

## <a name="summary"></a>摘要

正如您所看到的.NET SDK 的 Hadoop 将允许您创建.NET 应用程序提交到 HDInsight 的群集，猪的作业并监视作业状态。

## <a name="next-steps"></a>下一步行动

猪的 HDInsight 中的一般信息。

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上。

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [使用 Hadoop 在 HDInsight 上使用 MapReduce](hdinsight-use-mapreduce.md)[预览门户]: https://portal.azure.com/
