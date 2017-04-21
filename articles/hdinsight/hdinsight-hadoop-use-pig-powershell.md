<properties
   pageTitle="猪的 Hadoop 使用 PowerShell 中 HDInsight |Microsoft Azure"
   description="了解如何猪将作业提交到 Hadoop 群集上使用 Azure PowerShell 的 HDInsight。"
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>运行使用 PowerShell 的猪的作业

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

本文档提供使用 Azure PowerShell 豚将作业提交到 HDInsight 群集上 Hadoop 的示例。 小猪，可使用一种语言 （猪的拉丁语） 建模的数据转换，编写 MapReduce 作业，而不是映射并减少函数。

> [AZURE.NOTE] 本文档不提供示例中使用的猪的拉丁语语句执行的操作的详细的说明。 有关此示例中使用猪的拉丁语的信息，请参阅[使用 Hadoop 在 HDInsight 上使用的小猪](hdinsight-use-pig.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要。

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **带有 Azure PowerShell 的工作站**。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>运行使用 PowerShell 的猪的作业

Azure PowerShell 提供*cmdlet* ，您可以在 HDInsight 上远程运行猪的作业。 在内部，这是通过使用 HDInsight 群集上运行对[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (以前称为 Templeton) 的其余部分调用来实现的。

在运行远程 HDInsight 群集上的小猪作业时使用以下 cmdlet:

* **登录 AzureRmAccount**︰ 对 Azure PowerShell Azure 订阅进行身份验证

* **新 AzureRmHDInsightPigJobDefinition**︰ 使用指定的猪的拉丁语语句创建新的*作业定义*

* **开始-AzureRmHDInsightJob**︰ 将作业定义发送到 HDInsight，启动该作业，并返回一个*作业*对象，可用于检查作业的状态

* **等待-AzureRmHDInsightJob**︰ 使用作业对象来检查作业的状态。 它将一直等待直到作业已经完成，或者超过了等待时间。

* **获得 AzureRmHDInsightJobOutput**︰ 用于检索作业的输出

以下步骤演示如何使用这些 cmdlet HDInsight 群集上运行作业。

1. 使用编辑器，将以下代码保存为**pigjob.ps1**。 您必须使用 HDInsight 群集的名称替换**群集名称**。

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. 打开一个新的 Windows PowerShell 命令提示符。 将目录更改到的位置的**pigjob.ps1**文件，然后使用下面的命令来运行此脚本︰

        .\pigjob.ps1
        
    您第一次将提示登录到 Azure 订购。 然后，您将 HTTPs/管理员帐户名称和密码为 HDInsight 群集要求。

7. 当作业完成时，它应该返回信息与以下类似︰

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>故障排除

在作业完成时，则不返回任何信息，如果在处理过程中可能时出错。 若要查看此作业的错误信息，请将以下命令添加到**pigjob.ps1**文件的末尾，保存它，然后再次运行。

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

这将返回的信息写入到 STDERR 在服务器上运行该作业时，它可能会帮助您确定该作业失败的原因。

##<a id="summary"></a>摘要

如您所见，Azure PowerShell 轻松地在 HDInsight 群集上运行豚作业，监视作业状态，并检索输出。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 中的小猪有关的一般信息︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)
