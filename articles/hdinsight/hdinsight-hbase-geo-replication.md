<properties 
   pageTitle="配置两个数据中心之间的 HBase 复制 |Microsoft Azure" 
   description="了解如何配置 HBase 复制两个数据中心以及有关群集复制使用情形。" 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>在 HDInsight 中配置 HBase geo 复制

> [AZURE.SELECTOR]
- [配置 VPN 连接](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [配置 DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [配置 HBase 复制](hdinsight-hbase-geo-replication.md) 
 
了解如何在两个数据中心配置 HBase 复制。 一些用例包括群集复制︰

- 备份和灾难恢复
- 数据聚合
- 地理数据分发
- 在线数据摄取结合离线数据分析

群集复制使用源推方法。 HBase 群集可以是一个源，一个目标，也可以同时满足这两个角色。 复制是异步的和复制的目标是最终一致性。 当源收到与复制启用了编辑的列族时，该编辑传播到所有目标群集。 当数据复制到另一个群集中时，被跟踪源群集和所有群集的已用完数据以防止复制循环。 有关详细信息，在此教程中，您将配置源目标复制。  对于其他群集拓扑结构中，可查看[Apache HBase 参考指南](http://hbase.apache.org/book.html#_cluster_replication)。

这是该系列的第三部分︰

- [配置两个虚拟网络间的 VPN 连接][hdinsight-hbase-replication-vnet]
- [将 DNS 配置为虚拟网络][hdinsight-hbase-replication-dns]
- 配置 HBase geo 复制 （本教程）

下图阐释两个虚拟网络和网络连接中[配置两个虚拟网络间的 VPN 连接]创建[hdinsight-hbase-geo-replication-vnet]和[虚拟网络配置 DNS][hdinsight-hbase-replication-dns]: 

![HDInsight HBase 复制虚拟网络图][img-vnet-diagram]

## <a id="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- **带有 Azure PowerShell 的工作站**。

    执行 PowerShell 脚本，您必须以管理员身份运行 Azure PowerShell 并设*RemoteSigned*执行策略。 请参阅使用组 ExecutionPolicy cmdlet。
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **两个 Azure 虚拟网络与 VPN 连接和配置 DNS**。  有关说明，请参阅[配置两个 Azure 的虚拟网络间的 VPN 连接][hdinsight-hbase-replication-vnet]，和[两个 Azure 的虚拟网络之间配置 DNS][hdinsight-hbase-replication-dns]。


    运行之前 PowerShell 脚本，请确保您已连接到您使用以下 cmdlet 的 Azure 订购︰

        Add-AzureAccount

    如果您有多个 Azure 的订阅，可以使用以下 cmdlet 设置当前订阅︰

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>在 HDInsight 中规定 HBase 群集

在[配置两个 Azure 的虚拟网络间的 VPN 连接][hdinsight-hbase-replication-vnet]，创建虚拟网络在欧洲的数据中心，并在美国数据中心的虚拟网络。 两个虚拟网络通过 VPN 连接。 在此会话中，将设置一个 HBase 群集中每个虚拟网络。 稍后在本教程中，您将要进行一个 HBase 群集复制其他 HBase 群集。

Azure 传统门户网站不支持使用自定义配置选项设置 HDInsight 群集。 例如，将*hbase.replication*设置为*true*。 如果后配置群集时，可以在配置文件中设置的值，就会丢失设置之后被正在保持群集。 有关更多信息，请参见[配置 Hadoop 群集中 HDInsight][hdinsight-provision]。 提供带自定义选项的 HDInsight 群集的选项之一使用 Azure PowerShell。


**在 Contoso-VNet-欧盟 HBase 群集配置** 

1. 从工作站，打开 Windows PowerShell ISE。
2. 设置变量在脚本的开头，然后运行该脚本。

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**在 Contoso-VNet-美国 HBase 群集配置** 

- 使用同一个脚本具有以下值︰

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    由于您已连接到您的 Azure 帐户，不需要再运行下列的 comlets:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>配置 DNS 条件转发器

在[将 DNS 配置为虚拟网络][hdinsight-hbase-replication-dns]，您已配置两个网络的 DNS 服务器。 HBase 群集具有不同的域后缀。 因此，您需要配置其他 DNS 条件转发器。

若要配置条件转发器，您需要知道两个 HBase 群集的域后缀。 

**若要查找两个 HBase 群集的域后缀**

1. **Contoso-HBase 欧盟**到 RDP。  有关说明，请参阅[管理 Hadoop 群集中使用 Azure 传统门户网站 HDInsight][hdinsight-manage-portal]。 它是群集的真正的 headnode0。
2. 打开 Windows PowerShell 控制台或命令提示符。
3. 运行**ipconfig**，然后记下**特定于连接的 DNS 后缀**。
4. 请不要关闭 RDP 会话。  您将需要稍后以测试域名解析。
5. 重复相同的步骤，查明**Contoso HBase 美国****特定连接的 DNS 后缀**。


**要配置 DNS 转发器**
 
1.  **Contoso-DNS-欧盟**到 RDP。 
2.  单击窗口左下方的项。
2.  单击**管理工具**。
3.  单击**DNS**。
4.  在左窗格中，展开**DSN**， **Contoso 的 DNS-欧盟**。
5.  **条件转发器**中，用鼠标右键单击，然后单击**新条件转发器**。 
5.  输入以下信息︰
    - **DNS 域**︰ 输入 Contoso HBase 美国的 DNS 后缀。 例如︰ Contoso-HBase-US.f5.internal.cloudapp.net。
    - **主服务器的 IP 地址**︰ 输入 10.2.0.4，这是 Contoso DNS-美国的 IP 地址。 请验证 IP。 您的 DNS 服务器可以有一个不同的 IP 地址。
6.  按**enter 键**，并单击**确定**。  现在您将能够解决从 Contoso-DNS-EU Contoso DNS-美国的 IP 地址。
7.  重复步骤将 DNS 条件转发器添加到 DNS 服务在 Contoso DNS 美国虚拟机上使用下面的值︰
    - **DNS 域**︰ 输入在 Contoso HBase 欧盟的 DNS 后缀。 
    - **主服务器的 IP 地址**︰ 输入 10.2.0.4，这是对 Contoso-DNS-欧盟的 IP 地址。

**若要测试域名解析**

1. 切换到 Contoso-HBase EU RDP 窗口。
2. 打开命令提示符。
3. 运行 ping 命令︰

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    ICM 协议开启 HBase 群集的辅助节点

4. 不要关闭该 RDP 会话。 仍需要在本教程后面部分。
5. 重复相同的步骤来 ping 从 Contoso HBase 美国在 Contoso HBase 欧盟 headnode0。

>[AZURE.IMPORTANT] DNS 必须工作，才能继续下面的步骤。

## <a name="enable-replication-between-hbase-tables"></a>启用 HBase 表间复制

现在，您可以创建示例 HBase 表、 启用复制，然后用数据对其进行测试。 您将使用的示例表已列的两个系列︰ 个人和机构。 

在本教程中，您将使欧洲 HBase 群集服务器作为源群集和美国 HBase 群集与目标群集。

创建具有相同名称和列的源和目标群集上的家族 HBase 表，以便目标群集知道它将接收的数据的存储位置。 有关使用 HBase 外壳程序的详细信息，请参阅[开始使用 Apache HBase 中 HDInsight][hdinsight-hbase-get-started]。

**在 Contoso-HBase 欧盟上创建一个 HBase 表**

1. 切换到**Contoso-HBase EU** RDP 窗口。
2. 在桌面上，请单击**Hadoop 命令行**。
2. 将文件夹更改为 HBase 主目录︰

        cd %HBASE_HOME%\bin
3. 打开 HBase 外壳︰

        hbase shell
4. 创建一个 HBase 表︰

        create 'Contacts', 'Personal', 'Office'
5. RDP 会话也 Hadoop 命令行窗口，请不要关闭。 您仍将在本教程后面需要它们。
    
**在 Contoso HBase 美国创建一个 HBase 表**

- 重复相同的步骤在 Contoso HBase 美国创建的同一个表。


**若要添加为复制等 Contoso HBase 美国**

1. 切换到**Contso HBase_EU** RDP 窗口。
2. 在 HBase shell 窗口中，添加目标群集 （Contoso HBase 美国） 为对等，例如︰

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    在示例中，域后缀是*contoso-hbase-us.d4.internal.cloudapp.net*。 您需要将其更新以匹配您的美国 HBase 群集的域后缀。 请确保主机名之间没有空格。

**若要配置每个列族将被复制源群集**

1. 从**Contso-HBase EU** RDP 会话 HBase shell 窗口中，配置要复制每个列族︰

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**大容量对 HBase 表上载数据**

示例数据文件已上载到公共的 Azure Blob 容器，通过下面的 URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

文件的内容︰

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

可以将相同的数据文件上传到 HBase 群集并从中导入数据。

1. 切换到**Contoso-HBase EU** RDP 窗口。
2. 在桌面上，请单击**Hadoop 命令行**。
3. 将文件夹更改为 HBase 主目录︰

        cd %HBASE_HOME%\bin

4. 上载数据︰

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>验证正在进行数据复制。

您可以验证该复制正在通过扫描中使用下列 HBase shell 命令两个群集的表︰

        Scan 'Contacts'


## <a name="next-steps"></a>下一步行动

在本教程中，您学习了如何在两个数据中心配置 HBase 复制。 若要了解有关 HDInsight 和 HBase 的详细信息，请参阅︰

- [HDInsight 服务页](https://azure.microsoft.com/services/hdinsight/)
- [HDInsight 文档](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Apache HDInsight 中的 HBase 入门][hdinsight-hbase-get-started]
- [HDInsight HBase 概述][hdinsight-hbase-overview]
- [Azure 虚拟网络上的资源调配 HBase 群集][hdinsight-hbase-provision-vnet]
- [分析与 HBase 实时 Twitter 观点][hdinsight-hbase-twitter-sentiment]
- [分析与风暴和 HBase 中 HDInsight (Hadoop) 传感器数据][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
