<properties
   pageTitle="在 HDInsight 使用 Azure CLI 创建基于 Windows 的 Hadoop 群集"
    description="了解如何使用 Azure CLI 为 Azure HDInsight 创建群集。"
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>在 HDInsight 使用 Azure CLI 创建基于 Windows 的 Hadoop 群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

了解如何创建使用 Azure CLI 的 HDInsight 群集。 其他群集创建工具和功能单击此页面顶部选项卡中的选择或者查看[群集创建方法](hdinsight-provision-clusters.md#cluster-creation-methods)。

##<a name="prerequisites"></a>系统必备组件︰

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


在开始这篇文章中的说明操作之前，必须具有下列︰

- **Azure 的订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **Azure CLI**。

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>连接到 Azure

使用下面的命令以连接到 Azure:

    azure login

使用工作或学校的帐户进行验证的详细信息，请参阅[连接到 Azure CLI 从 Azure 订阅](../xplat-cli-connect.md)。

使用以下命令切换到 ARM 模式︰

    azure config mode arm

要获取帮助，请使用**-h**开关。  例如︰

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>创建群集

您可以创建一个 HDInsight 群集之前，必须具有 Azure 资源管理 (ARM) 和 Azure Blob 存储帐户。 若要创建一个 HDInsight 群集，您必须指定以下各项︰

- **Azure 资源组**︰ 必须在 Azure 资源组中创建数据湖分析帐户。 Azure 的资源管理器使您可以使用作为一个组应用程序中的资源。 您可以部署、 更新或删除单个、 协调操作中的应用程序的所有资源。

    若要列出您的订阅中的资源组︰

        azure group list

    若要创建新的资源组︰

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **HDInsight 群集名称**

- **位置**︰ 支持 HDInsight 群集的 Azure 数据中心之一。 有关受支持的位置的列表，请参阅[HDInsight 定价](https://azure.microsoft.com/pricing/details/hdinsight/)。

- **默认存储帐户**︰ HDInsight 作为默认文件系统使用 Azure Blob 存储容器。 您可以创建一个 HDInsight 群集之前，Azure 存储帐户是必需的。

    若要创建一个新的 Azure 存储帐户︰

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]必须在数据中心中的 HDInsight 与搭配使用存储帐户。
    > 由于 ZRS 不支持表无法 ZRS，存储帐户类型。

    使用 Azure 门户创建 Azure 存储帐户的信息，请参阅 [创建、 管理或删除存储帐户] [azure 的创建-storageaccount]。

    如果您已经拥有存储帐户，但不是知道用户名和帐户密码，您可以使用以下命令检索的信息︰

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    通过使用 Azure 门户中获取的信息的详细信息，请参阅[关于 Azure 存储帐户](../storage/storage-create-storage-account#manage-your-storage-account)的"管理您的存储帐户"一节。

- **（可选） 默认 Blob 容器**︰ **azure hdinsight 群集创建**命令创建的容器，如果它不存在。 如果您选择预先创建的容器，您可以使用下面的命令︰

    azure 存储容器创建--帐户名称"<Storage Account Name>"— 帐户密钥<Storage Account Key>[容器名称]

一旦准备好存储帐户，您就可以创建群集︰


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>创建群集使用配置文件
通常情况下，创建一个 HDInsight 群集、 运行作业，并删除群集来削减成本。 命令行界面可以保存到一个文件中，配置的选项，以便您可以在每次创建群集重用它。  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

示例︰ 创建配置文件包含在创建群集时运行的脚本操作。

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>通过脚本操作创建群集

创建一个配置文件包含在创建群集时运行的脚本操作。

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

用脚本操作创建群集

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


常规的脚本操作信息，请参阅[自定义 HDInsight 群集使用脚本操作 (Linux)](hdinsight-hadoop-customize-cluster.md)。


## <a name="create-clusters-using-arm-templates"></a>创建群集使用 ARM 模板

您可以使用 CLI 通过调用 ARM 模板创建群集。 请参阅[使用 Azure CLI 部署](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli)。

## <a name="see-also"></a>请参见

- [开始使用 Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) -了解如何开始使用 HDInsight 群集
- [以编程方式提交 Hadoop 的作业](hdinsight-submit-hadoop-jobs-programmatically.md)-了解如何以编程方式将作业提交到 HDInsight
- [HDInsight 使用 Azure CLI 中的 Hadoop 群集管理](hdinsight-administer-use-command-line.md)
- [使用 Azure CLI 的 Mac、 Linux、 Windows Azure 服务管理](../virtual-machines-command-line-tools.md)
