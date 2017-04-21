<properties
    pageTitle="管理 Hadoop 群集中使用 PowerShell HDInsight |Microsoft Azure"
    description="了解如何使用 Azure PowerShell 的 HDInsight 中的 Hadoop 群集执行管理任务。"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    tags="azure-portal"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>通过使用 Azure PowerShell 管理中 HDInsight 的 Hadoop 群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell 是功能强大的脚本编写环境，可用于控制和自动化的部署和管理您的工作负载在 Azure 中。 在本文中，您将学习如何通过使用 Windows PowerShell Azure PowerShell 本地控制台管理 Azure HDInsight 中的 Hadoop 群集。 有关 HDInsight PowerShell cmdlet 的列表，请参阅[HDInsight cmdlet 参考][hdinsight-powershell-reference]。



**系统必备组件**

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

##<a name="install-azure-powershell"></a>安装 Azure PowerShell

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

如果您已安装 Azure PowerShell 版本 0.9 x，则必须先卸载该安装较新版本。

要检查已安装 PowerShell 的版本︰

    Get-Module *azure*
    
若要卸载旧版本，请运行程序和功能控制面板中。 


##<a name="create-clusters"></a>创建群集

[在 HDInsight 使用 Azure PowerShell 创建 Linux 基于群集](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)，请参阅

##<a name="list-clusters"></a>列表中的群集
使用以下命令列出当前的订阅中的所有群集︰

    Get-AzureRmHDInsightCluster

##<a name="show-cluster"></a>显示群集

使用下面的命令显示当前订阅中指定的群集的详细信息︰

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

##<a name="delete-clusters"></a>删除群集

使用下面的命令删除群集︰

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

您可以通过删除包含群集资源组中删除群集。 请注意，此操作将删除包括默认的存储帐户组中的所有资源。

    Remove-AzureRmResourceGroup -Name <Resource Group Name>
            
##<a name="scale-clusters"></a>扩展群集
缩放功能的群集允许您更改在 Azure HDInsight 中运行而无需重新创建群集的群集使用的辅助节点数。

>[AZURE.NOTE] 只有群集与 HDInsight 3.1.3 版或更高版本支持。 如果您不确定您的群集的版本，您可以检查属性页。  请参阅[列表和显示群集](hdinsight-administer-use-portal-linux.md#list-and-show-clusters)。

更改群集由 HDInsight 支持的每种类型的数据节点数的影响︰

- Hadoop

    可以无缝地增加工作人员而不会影响任何未决或正在运行的作业运行 Hadoop 群集中的节点数。 在进行该操作时，也可以提交新的作业。 缩放操作的失败是正常处理，以便群集始终处于正常工作状态。

    Hadoop 群集按比例缩小通过减少数据节点数，当群集中的服务会重新启动。 这将导致所有正在运行和挂起的作业完成时缩放操作失败。 操作完成后，您便可以但是，重新提交作业。

- HBase

    可以无缝地添加或 HBase 群集中删除节点正在运行。 在完成将缩放操作的几分钟内自动平衡区域服务器。 但是，您可以通过登录到群集的 headnode，并从命令提示符窗口中运行以下命令手动平衡区域服务器︰

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- 风暴

    可以无缝地添加或风暴群集中删除数据节点正在运行。 但后缩放操作成功完成，您将需要重新平衡拓扑。

    重新平衡可以通过两种方式来实现︰

    * 风暴的 web 用户界面
    * 命令行界面 (CLI) 工具

    请[Apache 风暴文档](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)的更多详细信息，参阅。

    风暴的 web 用户界面是 HDInsight 群集上可用︰

    ![HDInsight 冲击规模重新平衡](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    下面是如何使用的 CLI 命令来重新平衡风暴拓扑示例︰

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

若要更改使用 Azure PowerShell Hadoop 群集大小，请从客户端计算机运行下面的命令︰

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    

##<a name="grantrevoke-access"></a>授予/撤销访问权限

HDInsight 群集有以下 HTTP web 服务 （这些服务的所有具有 rest 风格的终结点）︰

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


默认情况下，这些服务要授予的访问权限。 您可以撤消/授予访问权。 若要吊销︰

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

若要授予︰

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"
    
    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

>[AZURE.NOTE] 授予/撤销访问权限，将重置群集用户名称和密码。

这也可以通过门户完成。 请参阅[使用 Azure 门户管理 HDInsight][hdinsight-admin-portal]。

##<a name="update-http-user-credentials"></a>更新 HTTP 用户凭据

它是作为[授予/撤销 HTTP 访问](#grant/revoke-access)相同的过程。如果群集已被授予的 HTTP 访问，您必须首先将其撤消。  然后再授予新 HTTP 用户凭据的访问权限。


##<a name="find-the-default-storage-account"></a>查找默认的存储帐户

下面的 Powershell 脚本演示如何获取默认存储帐户名称和群集的默认存储帐户密钥。

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 

##<a name="find-the-resource-group"></a>找到的资源组

在资源管理器模式中，每个 HDInsight 群集所属 Azure 的资源组。  若要查找的资源组︰

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


##<a name="submit-jobs"></a>提交作业

**若要提交 MapReduce 作业**

请参阅[运行 Hadoop MapReduce 示例中的基于 Windows 的 HDInsight](hdinsight-run-samples.md)。

**提交配置单元作业** 

请参阅[使用 PowerShell 的运行配置单元查询](hdinsight-hadoop-use-hive-powershell.md)。

**若要提交猪的作业**

请参阅[使用 PowerShell 的猪的运行作业](hdinsight-hadoop-use-pig-powershell.md)。

**若要提交 Sqoop 作业**

请参阅[使用 Sqoop 与 HDInsight](hdinsight-use-sqoop.md)。

**若要提交 Oozie 作业**

请参阅[使用 Hadoop 来定义并运行在 HDInsight 工作流使用 Oozie](hdinsight-use-oozie.md)。

##<a name="upload-data-to-azure-blob-storage"></a>将数据上载到 Azure Blob 存储
请参阅[上载数据为 HDInsight][hdinsight-upload-data]。


## <a name="see-also"></a>请参见
* [HDInsight cmdlet 参考文档][hdinsight-powershell-reference]
* [通过使用 Azure 门户管理 HDInsight][hdinsight-admin-portal]
* [管理 HDInsight 使用一个命令行界面][hdinsight-admin-cli]
* [创建 HDInsight 群集][hdinsight-provision]
* [将数据上传到 HDInsight][hdinsight-upload-data]
* [以编程方式提交 Hadoop 作业][hdinsight-submit-jobs]
* [开始使用 Azure HDInsight][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: powershell-install-configure.md

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
