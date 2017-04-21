<properties
    pageTitle="Hadoop 群集使用 Azure CLI 管理 |Microsoft Azure"
    description="如何使用 Azure CLI 管理中 HDIsight 的 Hadoop 群集"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>HDInsight 使用 Azure CLI 中的 Hadoop 群集管理

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

了解如何使用[Azure 命令行界面](../xplat-cli-install.md)来管理 Azure HDInsight 中的 Hadoop 群集。 在 Node.js 实现 Azure CLI。 可以在任何支持 Node.js，包括 Windows、 Mac 和 Linux 的平台上使用它。

本文介绍了仅使用 HDInsight Azure CLI。 有关如何使用 Azure CLI 的一般指南，请参阅[安装和配置 Azure CLI][azure-command-line-tools]。

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **Azure CLI** -请参阅[安装和配置 Azure CLI](../xplat-cli-install.md)的安装和配置信息。
- **连接到 Azure**，使用下面的命令︰

        azure login

    使用工作或学校的帐户进行验证的详细信息，请参阅[连接到 Azure CLI 从 Azure 订阅](xplat-cli-connect.md)。
    
- **切换到 Azure 资源管理器模式**，使用下面的命令︰

        azure config mode arm

要获取帮助，请使用**-h**开关。  例如︰

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>创建群集

请参阅[创建 Linux 基于 HDInsight 使用 Azure CLI 中的群集](hdinsight-hadoop-create-linux-clusters-azure-cli.md)。

##<a name="list-and-show-cluster-details"></a>列出并显示群集的详细信息
使用以下命令列出并显示群集的详细信息︰

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI。CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>删除群集

使用下面的命令删除群集︰

    azure hdinsight cluster delete <Cluster Name>

您可以通过删除包含群集资源组中删除群集。 请注意，此操作将删除包括默认的存储帐户组中的所有资源。

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>扩展群集

若要更改 Hadoop 群集大小︰

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>启用/禁用 HTTP 访问群集

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>启用/禁用群集的 RDP 访问

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>下一步行动
在本文中，您学习了如何来执行不同的 HDInsight 群集管理任务。 若要了解详细信息，请参阅下列文章︰

* [通过使用 Azure 门户管理 HDInsight] [hdinsight-admin-portal]
* [通过使用 Azure PowerShell 管理 HDInsight] [hdinsight-admin-powershell]
* [开始使用 Azure HDInsight] [hdinsight-get-started]
* [如何使用 Azure CLI] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "列出并显示群集"
