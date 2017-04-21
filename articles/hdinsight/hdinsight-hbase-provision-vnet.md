<properties
    pageTitle="在虚拟网络中创建 HBase 群集 |Microsoft Azure"
    description="开始在 Azure HDInsight 使用 HBase。 了解如何创建 HDInsight HBase Azure 虚拟网络上的群集。"
    keywords=""
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
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>在 Azure 虚拟网络中创建 HBase 群集 

了解如何创建在[Azure 虚拟网络]的 Azure HDInsight HBase 群集[1]。

通过虚拟网络集成，HBase 群集可以部署到与应用程序相同的虚拟网络，以便应用程序可以与 HBase 直接通信。 好处包括︰

- 直接连接的 HBase 群集，从而使通过 HBase Java 远程过程的通信节点的 web 应用程序调用 (RPC) 的 Api。
- 改进了性能，通过不让您的通信通过多个网关和负载平衡器转。
- 能够在更安全的方式处理敏感信息，而无需公开一个公共的终结点。

###<a name="prerequisites"></a>系统必备组件
在开始本教程之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- **带有 Azure PowerShell 的工作站**。 请参阅[安装和使用 Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)。 

## <a name="create-hbase-cluster-into-virtual-network"></a>创建虚拟网络到 HBase 群集

在本节中，将与 Azure 虚拟网络使用[Azure 资源管理器模板](../resource-group-template-deploy.md)中的从属 Azure 存储帐户创建基于 Linux 的 HBase 群集。 其他群集创建方法和了解这些设置，请参阅[创建 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md)。 关于使用模板来创建在 HDInsight 的 Hadoop 群集的详细信息，请参阅[HDInsight 使用 Azure 资源管理器模板在创建 Hadoop 群集](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] 某些属性已硬编码到该模板。 例如︰
>
> * __位置__︰ 东亚美国 2
> * __Cluster 版本︰ 3.4
> * __群集节点计数工作人员__︰ 4
> * __默认存储帐户__︰&lt;群集名称 > 存储
> * __虚拟网络名称__︰&lt;群集名称 >-vnet
> * __网络的虚拟地址空间__︰ 10.0.0.0/16
> * __子网名称__︰ 默认
> * __子网的地址范围__︰ 10.0.0.0/24
>
> &lt;群集名称 > 将替换时使用该模板提供的群集名称。

1. 请单击下面的图像可以在 Azure 的门户打开该模板。 该模板位于公钥 blob 容器中。 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. 从**自定义部署**刀片式服务器，输入以下命令︰

    - **订阅**︰ 请选择用于创建 HDInsight 群集、 从属存储帐户和 Azure 的虚拟网络 Azure 订阅。
    - **资源组**︰ 选择**新建**，然后指定新资源组名称。
    - **位置**︰ 为资源组选择一个位置。
    - **群集名称**︰ 输入，您将创建的 Hadoop 群集的名称。
    - **群集登录名和密码**︰ 默认登录名是**管理员**。
    - **SSH 的用户名和密码**︰ 默认的用户名是**sshuser**。  您可以重命名它。 
    - **我同意这些条款和上述情况**: （选择）
    

3. 单击**购买**。 大约需要大约 20 分钟的时间来创建一个群集。 创建群集后，您可以单击以打开它的门户群集刀片。

完成本教程后，您可能想要删除该群集。 使用 HDInsight，您的数据以便不使用时，可以安全地删除群集 Azure 存储中存储。 您还收取 HDInsight 群集，即使不在使用中。 由于群集的费用超过存储的费用很多时候，它删除群集时未使用经济意义。 删除群集的说明，请参阅[管理 Hadoop 群集中使用 Azure 的门户网站 HDInsight](hdinsight-administer-use-management-portal.md#delete-clusters)。

开始使用新的 HBase 群集，您可以使用的过程中[开始使用 HBase 使用 Hadoop HDInsight 中](hdinsight-hbase-tutorial-get-started.md)找到。

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>连接到使用 HBase Java RPC Api HBase 群集

1.  作为服务 (IaaS) 的虚拟机创建一个基础结构，到 Azure 的相同的虚拟网络中，并位于同一子网。 有关创建新的 IaaS 虚拟机的说明，请参阅[创建虚拟机运行 Windows 服务器](../virtual-machines/virtual-machines-windows-hero-tutorial.md)。 当采用本文中的步骤，您必须使用以下网络配置︰

    - __虚拟网络__︰&lt;群集名称 >-vnet
    - __子网__︰ 默认

    > [AZURE.IMPORTANT] 更换&lt;群集名称 > 与您在前面步骤中创建的 HDInsight 群集时使用的名称。

    使用这些值，则将配置虚拟机与 HDInsight 群集使用相同的虚拟网络和子网。 这将使他们能够直接相互通信。

2.  当使用远程连接到 HBase Java 应用程序，必须使用完全限定的域名称 (FQDN)。 为此，您必须获得 HBase 群集的特定于连接的 DNS 后缀。 要做到这一点，可以使用以下方法之一︰

    * 使用 Web 浏览器以使 Ambari 调用︰
    
        浏览到 https://&lt;群集名称 >.azurehdinsight.net/api/v1/clusters/&lt;群集名称 > / hosts?minimal_response = true。 它将 JSON 文件与 DNS 后缀。

    * 使用 Ambari 网站︰

        1. 浏览到 https://&lt;群集名称 >。 azurehdinsight.net。
        2. 从顶部的菜单中单击**主机**。

    * 使用卷曲进行 REST 调用︰

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        在返回的 JavaScript 对象符号 (JSON) 数据，找到"host_name"条目。 这将包含在群集中的节点的 FQDN。 例如︰

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        与群集名称的域名称开头的部分是的 DNS 后缀。 例如，mycluster.b1.cloudapp.net。

    * 使用 Azure PowerShell
    
        使用下面的 Azure PowerShell 脚本注册**获取 ClusterDetail**函数，可用于返回的 DNS 后缀︰

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        运行 Azure PowerShell 脚本后，使用下面的命令使用**Get ClusterDetail**函数返回的 DNS 后缀。 使用此命令时，请指定您的 HDInsight HBase 群集名称、 管理员名和管理员密码。

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        这将返回的 DNS 后缀。 例如， **yourclustername.b4.internal.cloudapp.net**。

    * 使用 RDP
    
        您还可以使用远程桌面连接到 HBase 群集 （您将连接到的头节点） 和运行**ipconfig**命令提示符下以获取 DNS 后缀。 启用远程桌面协议 (RDP) 使用 RDP，连接到群集上的说明，请参阅[管理 Hadoop 群集中使用 Azure 的门户网站 HDInsight][hdinsight-admin-portal]。
        
        ![hdinsight.hbase.dns.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

要验证的虚拟机可以与 HBase 群集通信，请使用命令`ping headnode0.<dns suffix>`从虚拟机。 例如，ping headnode0.mycluster.b1.cloudapp.net。

在 Java 应用程序中使用此信息，您可以按照[使用 Maven 构建 Java 应用程序使用 HBase HDInsight (Hadoop) 与](hdinsight-hbase-build-java-maven.md)要创建的应用程序中的步骤操作。 若要连接到的远程 HBase 服务器应用程序，修改**hbase site.xml**文件，在此示例中为 Zookeeper 使用 FQDN。 例如︰

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] 了解更多有关在 Azure 中的名称解析虚拟网络，包括如何使用您自己的 DNS 服务器，请参阅[域名解析 (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)。

##<a name="next-steps"></a>下一步行动

在本教程中，您学习了如何创建一个 HBase 群集。 若要了解详细信息，请参阅︰

- [开始使用 HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [在 HDInsight 中配置 HBase 复制](hdinsight-hbase-geo-replication.md)
- [在 HDInsight 中创建 Hadoop 群集](hdinsight-provision-clusters.md)
- [开始使用 Hadoop 在 HDInsight 中使用 HBase](hdinsight-hbase-tutorial-get-started.md)
- [分析与 HBase 中 HDInsight 的 Twitter 观点](hdinsight-hbase-analyze-twitter-sentiment.md)
- [虚拟网络概述][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "新 HBase 群集配置详细信息"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "将操作脚本保存用于自定义一个 HBase 群集"

[azure-preview-portal]: https://portal.azure.com
