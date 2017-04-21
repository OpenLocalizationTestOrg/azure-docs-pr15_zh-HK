<properties
    pageTitle="Hadoop 的可用性群集在 HDInsight |Microsoft Azure"
    description="HDInsight 将部署有其他的头节点群集中具有高可用性和高可靠性。"
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>在 HDInsight 中的基于 Windows 的 Hadoop 群集的可用性和可靠性


>[AZURE.NOTE] 使用本文档中的步骤适用于基于 Windows HDInsight 群集。 如果您正在使用基于 Linux 的群集，请参阅[HDInsight 中的基于 Linux 的 Hadoop 群集的可用性和可靠性](hdinsight-high-availability-linux.md)Linux 特有的信息。

HDInsight 使客户能够部署各种群集类型，为不同的数据分析工作负荷。 目前提供的群集类型是用于查询和分析工作负荷、 NoSQL 的工作负载，HBase 群集和实时事件处理工作负载的风暴簇的 Hadoop 群集。 在一个给定的群集类型，有不同角色的不同节点。 例如︰



- HDInsight 的 Hadoop 群集部署具有两个角色︰
    - 头节点 （节点 2）
    - 数据节点 （至少一个节点）

- HDInsight 的 HBase 群集部署有三个角色︰
    - 头部的服务器 （节点 2）
    - 区域服务器 （至少一个节点）
    - 主/Zookeeper 节点 （3 节点）

- HDInsight 的风暴群集部署有三个角色︰
    - Nimbus 节点 （节点 2）
    - 监控器服务器 （至少一个节点）
    - Zookeeper 节点 （3 节点）

Hadoop 群集的标准实现通常有单一的头节点。 HDInsight 删除辅助头节点 /head 添加了此单点故障服务器/Nimbus 节点以提高可用性和可靠性管理工作负载所需的服务。 这些头的节点头服务器/Nimbus 节点用来管理辅助节点故障平稳，但主在头节点上运行的服务的任何中断会导致群集停止工作。


[ZooKeeper](http://zookeeper.apache.org/ )节点 (ZKs) 已添加，用于领导选举的头节点，以确保该工作节点和网关 (GWs) 知道何时在活动的头节点 (Head Node0) 变为非活动状态时故障转移到辅助头节点 (节点 1 头)。

![在 HDInsight Hadoop 实现高度可靠的头节点的关系图。](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>检查活动头节点服务状态
若要确定哪些头节点处于活动状态并查看该头节点上运行的服务的状态，必须通过使用远程桌面协议 (RDP) 连接到 Hadoop 群集。 RDP 的说明，请参阅[管理 Hadoop 群集中使用 Azure 的门户网站 HDInsight](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)。 一旦您已连接到群集，双击**Hadoop 服务可用**图标在桌面上 Namenode 获得有关的头节点的状态、 Jobtracker、 Templeton、 Oozieservice、 Metastore 和 Hiveserver2 服务正在运行，或者 HDI 3.0、 Namenode，资源管理器、 服务器历史记录、 Templeton、 Oozieservice、 Metastore 和 Hiveserver2 的服务。

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

屏幕抓图，在活动的头节点是*headnode0*。

## <a name="access-log-files-on-the-secondary-head-node"></a>访问辅助头节点上的日志文件

若要访问作业辅助头节点上的日志中，它已成为活动的头节点，浏览 JobTracker 用户界面仍然起作用与其主要的主动节点。 若要访问 JobTracker，必须通过在上一节中所述使用 RDP 连接到 Hadoop 群集。 向群集远程之后， **Hadoop 名称节点状态**图标在桌面上双击，然后单击**NameNode 日志**以获取到辅助头节点上的日志目录上的。

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>配置头节点大小
头节点作为大型虚拟机 (Vm)，默认情况下分配。 这种大小足以满足大多数运行在群集上的 Hadoop 作业的管理。 但有可能需要超大 Vm 的头节点的情况。 例如，当群集管理大量的小型 Oozie 作业。

可以通过使用 Azure PowerShell cmdlet 或 HDInsight SDK 配置超大的虚拟机。

创建和使用 Azure PowerShell 的群集资源调配[管理 HDInsight 使用 PowerShell](hdinsight-administer-use-powershell.md)中进行了说明。 超大的头节点的配置需要添加`-HeadNodeVMSize ExtraLarge`参数与`New-AzureRmHDInsightcluster`在此代码中使用的 cmdlet。

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

有关 SDK，故事是相似的。 [使用 HDInsight.NET SDK](hdinsight-provision-clusters.md#sdk)中介绍了创建和使用 SDK 的群集资源调配。 超大的头节点的配置需要添加`HeadNodeSize = NodeVMSize.ExtraLarge`参数与`ClusterCreateParameters()`在此代码中使用方法。

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>下一步行动

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [连接到使用 RDP 的 HDInsight 群集](hdinsight-administer-use-management-portal.md#rdp)
- [使用 HDInsight.NET SDK](hdinsight-provision-clusters.md#sdk)
