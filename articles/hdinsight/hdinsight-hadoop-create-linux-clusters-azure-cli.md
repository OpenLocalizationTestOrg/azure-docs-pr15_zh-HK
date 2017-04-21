<properties
    pageTitle="在 Linux 中使用跨平台 Azure CLI 的 HDInsight 上创建 Hadoop，HBase 或风暴的群集 |Microsoft Azure"
    description="了解如何创建基于 Linux 的 HDInsight 群集使用跨平台 Azure CLI、 Azure 资源管理器模板和 Azure REST API。 可以指定群集类型 （Hadoop，HBase 或占领），也可以使用脚本安装自定义组件."
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
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>在 HDInsight 使用 Azure CLI 创建基于 Linux 的群集

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure CLI 可以跨平台命令行实用程序，您可以管理 Azure 服务。 它可以用于，Azure 资源管理模板，并创建一个 HDInsight 群集，以及相关的存储帐户和其他服务。

Azure 的资源管理模板是 JSON 文档描述一个__资源组__，并在其所有的资源 （如 HDInsight。)这种基于模板的方法允许您定义所需的 HDInsight 在一个模板中的所有资源。 它还允许您管理组的更改作为一个整体通过__部署__，它将更改应用于整个组。

此文档中的步骤完成创建新的 HDInsight 群集使用 Azure CLI 和模板的过程。

> [AZURE.IMPORTANT] 此文档中的步骤使用 HDInsight 群集默认的辅助节点 (4) 数。 如果您计划多 32 个工作节点上 （在群集创建过程中或通过扩展群集），您必须选择至少 8 核和 14 GB ram 的头节点大小。
>
> 节点的大小和相关的成本的详细信息，请参阅[HDInsight 定价](https://azure.microsoft.com/pricing/details/hdinsight/)。

##<a name="prerequisites"></a>系统必备组件

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- __Azure CLI__。 此文档中的步骤是最后一次经 Azure CLI 版本 0.10.1。

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>登录到 Azure 订阅

按照[连接到 Azure 订阅从 Azure 命令行界面 (Azure CLI)](../xplat-cli-connect.md)中记录的步骤，并连接到您的预订使用的__登录__方法。

##<a name="create-a-cluster"></a>创建群集

在安装和配置 Azure CLI 后，应从命令提示符、 外壳或终端会话执行下列步骤。

1. 使用下面的命令来验证到 Azure 订购︰

        azure login

    提示您提供用户名和密码。 如果您有多个 Azure 的订阅，请使用`azure account set <subscriptionname>`设置 Azure CLI 命令使用的订阅。

3. 切换到 Azure 资源管理器模式下使用以下命令︰

        azure config mode arm

4. 创建资源组。 此资源组将包含 HDInsight 群集和关联的存储帐户。

        azure group create groupname location
        
    * 替换为组的唯一名称的__组名__。 
    * 替换为要创建的组中的地理区域__位置__。 
    
        对于有效的位置的列表，请使用`azure location list`命令，然后再使用一个__名称__列中的位置。

5. 创建存储帐户。 此存储帐户将用作 HDInsight 群集的默认存储。

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * 在上一步中创建的组的名称替换__组名__。
     * 在上一步中使用的相同位置替换__位置__。 
     * __Storagename__替换存储帐户的唯一名称。
     
     > [AZURE.NOTE] 在此命令中使用的参数的详细信息，使用`azure storage account create -h`若要查看此命令的帮助。

5. 检索用来访问存储帐户。

        azure storage account keys list -g groupname storagename
        
    * 替换资源组名的__组名__。
    * 替换存储帐户的名称为__storagename__ 。
    
    在返回的数据，保存__key1____参数__值。

6. 创建一个 HDInsight 的群集。

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * 替换资源组名的__组名__。

    * __Hadoop__替换为您要创建的群集类型。 例如， `Hadoop`， `HBase`，`Storm`或`Spark`。

        > [AZURE.IMPORTANT] HDInsight 群集分为多种类型，分别对应于工作负荷或群集进行的优化的技术。 没有受支持的方法，以创建一个组合多个类型，如风暴和 HBase 在一个群集上的群集。 

    * 在前面的步骤中使用的相同位置替换__位置__。

    * 替换存储的帐户名称为__storagename__ 。

    * 在上一步中获得的密钥替换__storagekey__ 。 

    * 对于`--defaultStorageContainer`参数、 群集使用相同的名称您的使用。

    * 替换为__管理员__和__httppassword__的名称和您想要通过 HTTPS 访问群集时使用的密码。

    * 替换为__sshuser__和__sshuserpassword__的用户名和密码，您想要使用在访问群集时使用 SSH 时

    它可能需要几分钟时间来完成在群集创建过程。 通常大约 15。

##<a name="next-steps"></a>下一步行动

现在，您已成功创建使用 Azure CLI HDInsight 群集，使用以下方法来学习如何使用您的群集︰

###<a name="hadoop-clusters"></a>Hadoop 群集

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [使用 HDInsight 的小猪](hdinsight-use-pig.md)
* [HDInsight 使用 MapReduce](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase 群集

* [在 HDInsight 上的 HBase 入门](hdinsight-hbase-tutorial-get-started-linux.md)
* [开发 HBase HDInsight 上的 Java 应用程序](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>风暴的群集

* [在 HDInsight 上的风暴为开发 Java 拓扑](hdinsight-storm-develop-java-topology.md)
* [在 HDInsight 上的风暴中使用 Python 组件](hdinsight-storm-develop-python-topology.md)
* [部署和监视在 HDInsight 上的风暴与拓扑](hdinsight-storm-deploy-monitor-topology-linux.md)
