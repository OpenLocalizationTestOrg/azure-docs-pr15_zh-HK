<properties
   pageTitle="使用 Hadoop MapReduce 和 PowerShell |Microsoft Azure"
   description="了解如何使用 PowerShell 远程运行 MapReduce 作业使用 Hadoop HDInsight。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>HDInsight 使用 PowerShell 运行 Hadoop 使用 MapReduce 作业

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

本文档提供使用 Azure PowerShell Hadoop HDInsight 群集上运行 MapReduce 作业的示例。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

- **Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 （基于 Windows 或基于 Linux 的）**

- **带有 Azure PowerShell 的工作站**。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>运行 MapReduce 作业使用 Azure PowerShell

Azure PowerShell 提供*cmdlet* ，您可以在 HDInsight 上远程运行 MapReduce 作业。 在内部，这是通过使用 HDInsight 群集上运行对[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (以前称为 Templeton) 的其余部分调用来实现的。

在远程 HDInsight 群集中运行 MapReduce 作业时，将使用以下 cmdlet。

* **登录 AzureRmAccount**: Azure 订阅验证 Azure PowerShell

* **新 AzureRmHDInsightMapReduceJobDefinition**︰ 使用指定的 MapReduce 信息创建新的*作业定义*

* **开始-AzureRmHDInsightJob**︰ 将作业定义发送到 HDInsight，启动该作业，并返回一个*作业*对象，可用于检查作业的状态

* **等待-AzureRmHDInsightJob**︰ 使用作业对象来检查作业的状态。 它一直等到作业完成或超时等待。

* **获得 AzureRmHDInsightJobOutput**︰ 用于检索作业的输出

以下步骤演示如何使用这些 cmdlet HDInsight 群集中运行作业。

1. 使用编辑器，将以下代码保存为**mapreducejob.ps1**。 您必须使用 HDInsight 群集的名称替换**群集名称**。

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. 打开一个新的**Azure PowerShell**命令提示符。 将目录更改到的位置的**mapreducejob.ps1**文件，然后使用下面的命令来运行此脚本︰

        .\mapreducejob.ps1
    
    运行该脚本时，可能提示您进行身份验证到 Azure 订购。 您还将需要 HDInsight 群集提供的 HTTPS/管理员帐户名和密码。

3. 当作业完成时，您会收到与下面类似的输出︰

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    该输出指示作业已成功完成。

    > [AZURE.NOTE] 如果**退出代码**为 0 以外的值，请参阅[疑难解答](#troubleshooting)。

    本示例还将运行的脚本的目录中存储到**output.txt**文件下载的文件。

###<a name="view-output"></a>查看输出

在文本编辑器中查看单词和作业所产生的计数打开**output.txt**文件。

> [AZURE.NOTE] MapReduce 作业的输出文件是不可变的。 所以如果您重新运行此示例，您需要更改输出文件的名称。

##<a id="troubleshooting"></a>故障排除

在作业完成时，则不返回任何信息，如果在处理过程中可能时出错。 若要查看此作业的错误信息，请将以下命令添加到**mapreducejob.ps1**文件的末尾，保存它，然后再次运行。

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

此方法返回的信息写入到 STDERR 在服务器上运行该作业时，它可能会帮助您确定该作业失败的原因。

##<a id="summary"></a>摘要

如您所见，Azure PowerShell 提供了一种简便方法在 HDInsight 群集上运行 MapReduce 作业，监视作业状态，并检索输出。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 中的 MapReduce 作业有关的一般信息︰

* [在 HDInsight Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)
