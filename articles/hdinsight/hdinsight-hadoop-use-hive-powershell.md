<properties
   pageTitle="Hadoop 配置单元使用 PowerShell 中 HDInsight |Microsoft Azure"
   description="使用 PowerShell Hadoop HDInsight 上运行配置单元查询。"
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>运行配置单元查询使用 PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

本文档提供在 Azure 资源组模式中使用 Azure PowerShell 以运行 Hadoop HDInsight 群集上配置单元查询的一个示例。

> [AZURE.NOTE] 本文档不提供的示例中使用的 HiveQL 语句执行的操作的详细的说明。 在此示例中使用 HiveQL 上的信息，请参阅[使用配置单元与 Hadoop HDInsight 上](hdinsight-use-hive.md)。


**系统必备组件**

若要完成本文中的步骤操作，您需要。

- **Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 （基于 Windows 或基于 Linux 的）**
- **带有 Azure PowerShell 的工作站**。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>运行配置单元查询使用 Azure PowerShell

Azure PowerShell 提供*cmdlet* ，您可以在 HDInsight 上远程运行配置单元查询。 在内部，这是通过使用 HDInsight 群集上运行对[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (以前称为 Templeton) 的其余部分调用来实现的。

运行在一个远程 HDInsight 群集配置单元查询时，将使用以下 cmdlet:

* **添加 AzureRmAccount**: Azure 订阅验证 Azure PowerShell

* **新 AzureRmHDInsightHiveJobDefinition**︰ 使用指定的 HiveQL 语句创建新的*作业定义*

* **开始-AzureRmHDInsightJob**︰ 将作业定义发送到 HDInsight，启动该作业，并返回一个*作业*对象，可用于检查作业的状态

* **等待-AzureRmHDInsightJob**︰ 使用作业对象来检查作业的状态。 它将一直等待直到作业完成或超时等待。

* **获得 AzureRmHDInsightJobOutput**︰ 用于检索作业的输出

* **调用 AzureRmHDInsightHiveJob**︰ 用于运行 HiveQL 语句。 这将阻止该查询完成，然后返回结果

* **使用 AzureRmHDInsightCluster**︰ 设置当前群集用于**调用 AzureRmHDInsightHiveJob**命令

下面的步骤演示了如何使用这些 cmdlet HDInsight 群集中运行作业︰

1. 使用编辑器，将以下代码保存为**hivejob.ps1**。 您必须使用 HDInsight 群集的名称替换**群集名称**。

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. 打开一个新的**Azure PowerShell**命令提示符。 将目录更改到的位置的**hivejob.ps1**文件，然后使用下面的命令来运行此脚本︰

        .\hivejob.ps1

    当脚本运行时，将提示您输入您的群集的 HTTPS/管理员帐户凭据。 您可能还会提示您登录到 Azure 订购。
    
7. 当作业完成时，它应该返回信息与以下类似︰

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. 如上文所述，可以使用**调用配置单元**以运行查询并等待响应。 使用以下命令，并将**群集名称**替换您群集的名称︰

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    输出将如下所示︰

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] 对于较长的 HiveQL 查询，可以使用 Azure PowerShell**这里字符串**cmdlet 的 HiveQL 脚本文件。 下面的代码段演示如何使用**调用配置单元**cmdlet 运行 HiveQL 脚本文件。 HiveQL 脚本文件必须上载到 wasbs: / /。
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > 有关**此处字符串**的详细信息，请参阅<a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">使用 Windows PowerShell 这里的字符串</a>。

##<a name="troubleshooting"></a>故障排除

在作业完成时，则不返回任何信息，如果在处理过程中可能时出错。 若要查看此作业的错误信息，将以下内容添加到**hivejob.ps1**文件的末尾，保存它，然后重新运行它。

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

此方法返回时，写入到 STDERR 在服务器上运行此作业的信息，它可能会帮助您确定该作业失败的原因。

##<a name="summary"></a>摘要

如您所见，Azure PowerShell 轻松地运行在 HDInsight 群集配置单元查询，监视作业状态，并检索输出。

##<a name="next-steps"></a>下一步行动

HDInsight 中的配置单元有关的一般信息︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)
