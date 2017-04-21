<properties
   pageTitle="在基于 Linux 的 HDInsight 上使用 Hadoop 的提示 |Microsoft Azure"
   description="在熟悉的 Linux 环境在 Azure 的云环境中运行使用基于 Linux 的 HDInsight (Hadoop) 群集获得实现提示。"
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>在 Linux 中使用 HDInsight 信息

基于 Linux 的 Azure HDInsight 群集提供 Hadoop 熟悉的 Linux 环境，在 Azure 的云环境中运行。 对于大多数情况，它应能 Hadoop 在 Linux 安装任何其他的样子。 本文档调用特定的差异，您应该知道。

##<a name="prerequisites"></a>系统必备组件

许多在此文档中的步骤使用以下实用程序，可能需要在您的系统上安装。

* [卷曲](https://curl.haxx.se/)的用于与基于 web 的服务进行通信
* [jq](https://stedolan.github.io/jq/) -用于解析 JSON 文档
* [Azure CLI](../xplat-cli-install.md) -用于远程管理 Azure 服务

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>域名称

从互联网连接到群集时要使用的完全合格的域名称 (FQDN) 是**&lt;群集名称 >。 azurehdinsight.net**或 （对于仅 SSH)**&lt;群集名称的 ssh >。 azurehdinsight.net**。

在内部，群集中的每个节点都有在群集配置过程中分配的名称。 要查找群集名称，可以上 Ambari Web 用户界面，请访问__主机__页或使用以下方法来从 Ambari REST API 返回的主机的列表︰

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

群集的名称密码的管理员帐户和__群集名称__替换__密码__。 这将返回的 JSON 文档，包含在群集中的主机的列表，然后 jq 拉`host_name`为每个主机群集中的元素值。

如果需要为特定服务中找到的节点的名称，您可以查询该组件 Ambari。 例如，要查找 HDFS 名称节点主机，使用以下方法。

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

这将返回一个 JSON 文档描述该服务，然后 jq 拉仅`host_name`主机的值。

## <a name="remote-access-to-services"></a>远程访问服务

* **Ambari (web)** -https://&lt;群集名称 >。 azurehdinsight.net

    通过使用群集管理员用户和密码，身份验证，然后登录到 Ambari 中。 它还使用群集管理员用户名和密码。

    身份验证是明文-始终使用 HTTPS 来帮助确保连接是安全的。

    > [AZURE.IMPORTANT] 虽然 Ambari 群集是直接通过 Internet 访问，一些功能依赖于访问内部域名称用于群集节点。 因为这是一个内部的域的名称，并不是公共的您将收到"服务器未找到"错误，当试图通过互联网访问某些功能。
    >
    > 要使用 Ambari web 用户界面的全部功能，请使用 SSH 隧道代理 web 通信到群集的头节点。 请参阅[使用 SSH 隧道 Ambari web 用户界面、 ResourceManager、 JobHistory、 NameNode，Oozie 和其他的 web 用户界面的访问](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;群集名称 >.azurehdinsight.net/ambari

    > [AZURE.NOTE] 通过使用群集管理员用户名和密码进行身份验证。
    >
    > 身份验证是明文-始终使用 HTTPS 来帮助确保连接是安全的。

* **WebHCat (Templeton)** -https://&lt;群集名称 >.azurehdinsight.net/templeton

    > [AZURE.NOTE] 通过使用群集管理员用户名和密码进行身份验证。
    >
    > 身份验证是明文-始终使用 HTTPS 来帮助确保连接是安全的。

* **SSH** - &lt;群集名称 >-ssh.azurehdinsight.net 端口 22 或 23 上的。 使用端口 22 23 用于连接到辅助过程中连接到主 headnode。 头节点的详细信息，请参阅[HDInsight 中的 Hadoop 群集的可用性和可靠性](hdinsight-high-availability-linux.md)。

    > [AZURE.NOTE] 您只能访问群集的头节点通过 SSH 客户机。 一旦建立连接，然后可以从 headnode 通过 SSH 访问辅助节点。

## <a name="file-locations"></a>文件位置

可以在群集节点上找到 Hadoop 相关文件`/usr/hdp`。 此目录包含以下子目录︰

* __2.2.4.9-1__︰ 此目录命名为 HDInsight，因此群集上的数目可能不同于此处列出所使用的 Hortonworks 数据平台的版本。
* __当前__︰ 此目录中包含链接到__2.2.4.9-1__目录下的目录和存在，使您不必键入的版本号 （可能会更改，） 每次想要访问一个文件。

示例数据和 JAR 文件可以位于 Hadoop 分布式文件系统 (HDFS) 或 Azure Blob 存储在 / 示例 '或' wasbs: / / 示例。

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS、 Azure Blob 存储和存储最佳做法

在大多数 Hadoop 版本，HDFS 受到本地存储在群集中的计算机上。 虽然这是有效的它可以是一个基于云的解决方案的成本高昂进行计费每小时或按分钟计算资源。

HDInsight 使用 Azure Blob 存储为默认存储，它提供了以下好处︰

* 廉价的长期存储

* 从外部的服务，如网站、 文件上传/下载实用程序、 各种语言的 Sdk 和 web 浏览器的可访问性

由于 HDInsight 的默认存储区，您通常不需要执行任何操作即可使用它。 例如，以下命令将列出存储 Azure Blob 存储在**/example/data**文件夹中的文件︰

    hdfs dfs -ls /example/data

某些命令可能要求您指定要使用 Blob 存储。 对于这些数据，作为前缀的命令**wasb: / /**，或**wasbs: / /**。

HDInsight 还允许您将多个 Blob 存储帐户与群集相关联。 若要访问非默认 Blob 存储帐户，您可以使用格式**wasbs: / /&lt;container-name>@&lt;帐户名称 >.blob.core.windows.net/**。 例如，下面将列出指定的容器和 Blob 存储帐户的**/example/data**目录中的内容︰

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>群集使用 Blob 存储？

在群集创建过程中您选择使用现有的 Azure 存储帐户和容器，或新建一个。 然后，您可能忘记了它。 可以使用 Ambari REST API 来查找默认存储帐户和容器。

1. 使用下面的命令以检索使用卷曲，HDFS 配置信息和其使用[jq](https://stedolan.github.io/jq/)筛选︰

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] 这将返回第一个应用于服务器的配置 (`service_config_version=1`，) 将包含此信息。 如果要检索群集创建后已被修改的值，可能需要列出配置版本和检索最新的一个。

    这将返回一个值类似于以下内容，其中__容器__是默认容器，__帐户名__是 Azure 存储帐户名称︰

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. 获取存储客户的资源组，请使用[Azure CLI](../xplat-cli-install.md)。 在以下命令中，替换从 Ambari 的存储帐户名称__帐户名__︰

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    这将返回该帐户的资源组名称。
    
    > [AZURE.NOTE] 如果此命令返回执行任何操作，则可能需要更改到 Azure 资源管理器模式的 Azure CLI 并再次运行该命令。 若要切换到 Azure 资源管理器模式，请使用下面的命令。
    >
    > `azure config mode arm`
    
2. 获取项的存储帐户。 __组名称__替换上一步的资源组。 替换为存储帐户名的__帐户名__︰

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    这将返回主键的帐户。

此外，您还可以使用 Azure 门户的存储信息︰

1. 在[Azure 门户](https://portal.azure.com/)中，选择 HDInsight 群集。

2. 从__基础__部分中，选择__的所有设置__。

3. 从__设置__，选择__Azure 存储密钥__。

4. 从__Azure 存储键__，选择一个列出的存储帐户。 这将显示有关存储帐户信息。

5. 选择的钥匙图标。 这将显示该存储帐户的快捷键。

### <a name="how-do-i-access-blob-storage"></a>如何访问 Blob 存储？

而不通过 Hadoop 命令从群集中，有多种方式来访问 blob:

* [用于 Mac、 Linux 和 Windows azure CLI](../xplat-cli-install.md)︰ 使用 Azure 的命令的命令行界面。 安装后，使用`azure storage`命令使用存储、 帮助或`azure blob`blob 特定的命令。

* [blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python 脚本使用 Azure 存储中的 blob。

* 大量的 Sdk:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [拼音](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [REST API 存储](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>扩展您的群集

缩放功能的群集允许您更改数据在 Azure HDInsight 中运行而无需删除和重新创建群集的群集使用的节点数。

您可以执行其他作业时缩放操作或进程正在运行在群集上。

受不同的群集类型扩展，如下所示︰

* __Hadoop__︰ 缩小在群集中的节点数，当群集中的服务会重新启动。 这可能导致作业正在运行或挂起的缩放操作完成时失败。 操作完成后，您可以重新提交作业。

* __HBase__︰ 缩放操作完成后的几分钟内自动平衡区域服务器。 若要手动平衡区域服务器，使用以下步骤︰

    1. 连接到使用 SSH HDInsight 群集。 在 HDInsight 中使用 SSH 的详细信息，请参阅以下文档︰

        * [SSH 使用从 Linux 和 Unix 中，Mac OS X HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [SSH 使用从 Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. 使用以下方法来启动 HBase 外壳程序︰

            hbase shell

    2. HBase 外壳已加载后，使用以下方法来手动平衡区域服务器︰

            balancer

* __冲击__︰ 缩放操作执行完之后，应该重新平衡任何正在运行的风暴拓扑。 这样的拓扑来重新调整并行度设置基于新的群集中的节点数。 若要重新平衡运行的拓扑结构，使用下列选项之一︰

    * __SSH__︰ 连接到服务器并使用下面的命令来重新平衡拓扑︰

            storm rebalance TOPOLOGYNAME

        您还可以指定参数替代最初由拓扑结构的并行性提示。 例如，`storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10`将重新配置 5 个辅助进程、 蓝色管口组件，3 执行器和黄色螺栓组件 10 个执行器的拓扑。

    * __风暴的用户界面__︰ 使用下列步骤来重新平衡使用风暴 UI 的拓扑结构。

        1. 在 web 浏览器，其中群集名称是风暴群集的名称打开__https://CLUSTERNAME.azurehdinsight.net/stormui__ 。 出现提示时，输入 HDInsight 群集管理员 （管理员） 的名称和密码创建群集时指定。

        3. 选择您想要重新平衡，拓扑结构，然后选择__重新平衡__按钮。 执行重新平衡操作之前，请输入延迟。

扩展您的 HDInsight 群集的详细信息，请参阅︰

* [通过使用 Azure 门户管理中 HDInsight 的 Hadoop 群集](hdinsight-administer-use-portal-linux.md#scaling)

* [通过使用 Azure PowerShell 管理中 HDinsight 的 Hadoop 群集](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>如何安装色调 （或其他 Hadoop 组件）？

HDInsight 是一个托管的服务，这意味着群集中的节点可能会被破坏和以重新设置自动 Azure 如果检测到问题。 因此，不建议直接在群集节点上手动安装的事情。 当您需要安装以下，相反，使用[HDInsight 脚本操作](hdinsight-hadoop-customize-cluster.md)︰

* 服务或如触发或色调的网站。
* 要求在群集中的多个节点上的配置更改的组件。 例如，必需的环境变量，创建日志记录目录或创建一个配置文件。

脚本操作 Bash 脚本运行期间群集资源调配，并可以用于安装和配置在群集上的其他组件。 示例脚本提供有关安装以下组件︰

* [色相](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

开发您自己的脚本操作的信息，请参阅[使用 HDInsight 脚本操作开发](hdinsight-hadoop-script-actions-linux.md)。

###<a name="jar-files"></a>Jar 文件

一些 Hadoop 技术可在自包含的 jar 文件都包含函数，用作部件的 MapReduce 作业，或从猪或配置单元中。 尽管这些都可以使用脚本操作安装，他们通常不需要任何设置可以只设置后上载到群集和直接使用。 如果要确保群集中的映像的组件仍然有效的 makle，您可以存储在 WASB 的 jar 文件。

例如，如果您想要使用最新版本的[DataFu](http://datafu.incubator.apache.org/)，可以下载包含项目 jar，并将其上传到 HDInsight 群集。 有关如何使用它从猪或配置单元，然后按照 DataFu 文档。

> [AZURE.IMPORTANT] 一些独立的 jar 文件的组件提供 HDInsight，但不是在路径中。 如果您正在寻找一个特定的组件，可以使用以下搜索它在群集上︰
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> 这将返回所有匹配的 jar 文件的路径。

如果群集已经提供了作为独立的 jar 文件，组件的版本，但您想要使用一个不同的版本，可以将该组件的新版本上载到群集并尝试使用它在您的作业。

> [AZURE.WARNING] 完全支持使用 HDInsight 群集提供组件和 Microsoft 支持将有助于确定和解决与这些组件相关的问题。
>
> 自定义组件接收商业上合理的支持，以帮助您进一步排查该问题。 这可能导致解决问题或要求您能够进行深入的专业技能，为该技术在其中找到的开放源代码技术可用的频道。 例如，有许多社区站点可以使用，如︰ [HDInsight 的 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight)、 [http://stackoverflow.com](http://stackoverflow.com)。 Apache 项目还有项目网站上[http://apache.org](http://apache.org)，例如︰ [Hadoop](http://hadoop.apache.org/)，[触发](http://spark.apache.org/)。

## <a name="next-steps"></a>下一步行动

* [从基于 Windows 的 HDInsight 迁移到基于 Linux 的](hdinsight-migrate-from-windows-to-linux.md)
* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [使用 HDInsight 的小猪](hdinsight-use-pig.md)
* [HDInsight 使用 MapReduce 作业](hdinsight-use-mapreduce.md)
