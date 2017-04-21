<properties
   pageTitle="在 HDInsight 使用 Azure PowerShell 创建基于 Windows 的 Hadoop 群集 |Microsoft Azure"
    description="了解如何使用 Azure PowerShell Azure HDInsight 为创建群集。"
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>在 HDInsight 使用 Azure PowerShell 创建基于 Windows 的 Hadoop 群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

了解如何创建使用 Azure PowerShell HDInsight 群集。 Azure PowerShell 是提供管理与 Windows PowerShell Azure 的 cmdlet 的模块。 其他群集创建工具和功能单击此页面顶部选项卡中的选择或者查看[群集创建方法](hdinsight-provision-clusters.md#cluster-creation-methods)。


##<a name="prerequisites"></a>系统必备组件︰

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

在开始这篇文章中的说明操作之前，必须具有下列︰

- Azure 的订阅。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- Azure PowerShell。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>创建群集
Azure PowerShell 是功能强大的脚本编写环境，可用于控制和自动化的部署和管理您的工作负载在 Azure 中。 本节说明如何通过使用 Azure PowerShell 创建 HDInsight 群集。 在配置工作站运行 HDInsight Windows PowerShell cmdlet 的信息，请参阅[安装和配置 Azure PowerShell](../powershell-install-configure.md)。 在 HDInsight 中使用 Azure PowerShell 的详细信息，请参阅[管理 HDInsight 使用 PowerShell](hdinsight-administer-use-powershell.md)。 有关 HDInsight Windows PowerShell cmdlet 的列表，请参阅[HDInsight cmdlet 参考](https://msdn.microsoft.com/library/azure/dn858087.aspx)。


下面的过程需要使用 Azure PowerShell 创建 HDInsight 群集︰

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>创建群集使用 ARM 模板

可以使用 Azure PowerShell ARM 模板，这将创建一个 HDInsight 的群集部署。  请参见[调用模板中使用 Azure PowerShell](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell)。

##<a name="customize-clusters"></a>自定义群集

- 请参阅[使用引导数据库的自定义 HDInsight 群集](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell)。
- 请参阅[基于自定义 Windows HDInsight 群集使用脚本的操作](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell)。


##<a name="next-steps"></a>下一步行动
在本文中，您学习了几种方法来创建一个 HDInsight 的群集。 若要了解详细信息，请参阅下列文章︰

* [开始使用 Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) -了解如何开始使用 HDInsight 群集
* [以编程方式提交 Hadoop 的作业](hdinsight-submit-hadoop-jobs-programmatically.md)-了解如何以编程方式将作业提交到 HDInsight
* [管理 Hadoop 群集中使用 PowerShell 的 HDInsight](hdinsight-administer-use-powershell.md) -了解如何通过使用 Azure PowerShell 处理 HDInsight
* [Azure HDInsight SDK 文档] [ hdinsight-sdk-documentation] -发现 HDInsight SDK




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
