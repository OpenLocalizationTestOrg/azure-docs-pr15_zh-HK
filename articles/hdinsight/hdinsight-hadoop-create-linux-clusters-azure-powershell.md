<properties
    pageTitle="在 Linux 中使用 Azure PowerShell 的 HDInsight 上创建 Hadoop，HBase、 占领或触发群集 |Microsoft Azure"
    description="了解如何使用 Azure PowerShell Hadoop，HBase、 占领或触发群集在 Linux 上创建的 HDInsight。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>在 HDInsight 中，通过 Azure PowerShell 创建基于 Linux 的群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell 是功能强大的脚本编写环境可以用来控制和自动化的部署和管理您在 Microsoft Azure 中的工作负载。 本文档提供有关如何通过使用 Azure PowerShell 创建基于 Linux 的 HDInsight 群集的信息。 它还包括一个示例脚本。

> [AZURE.NOTE] Azure PowerShell 才在 Windows 客户端可用。 如果您正在使用 Linux 和 Unix 中，Mac OS X 客户机，请参阅使用 Azure CLI 创建群集的信息[创建基于 Linux 的 HDInsight 群集使用 Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) 。

## <a name="prerequisites"></a>系统必备组件
您必须在开始此过程前的进行下列︰

- Azure 的订阅。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- Azure PowerShell。
    有关使用 HDInsight Azure PowerShell 的详细信息，请参阅[管理 HDInsight 使用 PowerShell](hdinsight-administer-use-powershell.md)。 有关 HDInsight Windows PowerShell cmdlet 的列表，请参阅[HDInsight cmdlet 参考](https://msdn.microsoft.com/library/azure/dn858087.aspx)。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>创建群集

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

若要通过使用 Azure PowerShell 创建 HDInsight 群集，您必须完成以下过程︰

- 创建 Azure 的资源组
- 创建一个 Azure 存储帐户
- 创建一个 Azure Blob 容器
- 创建一个 HDInsight 群集

您必须设置创建 Linux 群集的两个最重要参数是指定的 OS 类型和 SSH 用户详细信息︰

- 请确保您指定**-OSType**参数为**Linux**。
- 要在群集上的远程会话的使用 SSH，可以指定 SSH 用户密码或 SSH 公钥。 如果您指定 SSH 用户密码和 SSH 公用密钥，该密钥将被忽略。 如果您想要用于远程会话的 SSH 密钥，必须指定空的 SSH 密码时提示用户。 SSH 使用 HDInsight 的更多信息，请参见下列文章︰

    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

下面的脚本说明了如何创建新的群集︰

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

**$ClusterCredentials**为您指定的值用于创建群集的 Hadoop 用户帐户。 使用此帐户来连接到该群集。

**$SshCredentials**为您指定的值用于创建 SSH 用户输入群集。 使用此帐户在群集中启动远程 SSH 会话并运行作业。

> [AZURE.IMPORTANT] 在此脚本中，您必须指定将群集中的辅助节点数。 如果您计划使用多 32 个工作节点 （或者在创建群集或通过创建后扩展群集），还必须至少为 8 核和 14 GB 的 RAM 来指定头节点大小。
>
> 节点的大小和相关的成本的详细信息，请参阅[HDInsight 定价](https://azure.microsoft.com/pricing/details/hdinsight/)。

可能需要 20 分钟才能创建群集。

下面的示例演示如何添加一个额外的存储帐户︰

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>自定义群集

- 请参阅[使用引导数据库的自定义 HDInsight 群集](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell)。
- 请参阅[基于自定义 Windows HDInsight 群集使用脚本的操作](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell)。

## <a name="delete-the-cluster"></a>删除群集

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>下一步行动

现在，您已成功创建一个 HDInsight 群集，使用下列资源了解如何使用您的群集。

### <a name="hadoop-clusters"></a>Hadoop 群集

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [使用 HDInsight 的小猪](hdinsight-use-pig.md)
* [HDInsight 使用 MapReduce](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase 群集

* [在 HDInsight 上的 HBase 入门](hdinsight-hbase-tutorial-get-started-linux.md)
* [开发 HBase HDInsight 上的 Java 应用程序](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>风暴的群集

* [在 HDInsight 上的风暴为开发 Java 拓扑](hdinsight-storm-develop-java-topology.md)
* [在 HDInsight 上的风暴中使用 Python 组件](hdinsight-storm-develop-python-topology.md)
* [部署和监视在 HDInsight 上的风暴与拓扑](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>触发群集

* [创建独立的应用程序使用 Scala](hdinsight-apache-spark-create-standalone-application.md)
* [在群集上使用晚触发远程运行作业](hdinsight-apache-spark-livy-rest-interface.md)
* [触发与 BI︰ 执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)
* [机器学习与触发︰ 使用 HDInsight 来预测食品检查结果中的触发](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight 用于构建实时流的应用程序中触发流︰ 使用触发](hdinsight-apache-spark-eventhub-streaming.md)
