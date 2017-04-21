<properties
   pageTitle="监视和管理 HDInsight 群集使用 Apache Ambari REST API |Microsoft Azure"
   description="了解如何使用 Ambari 来监控和管理基于 Linux 的 HDInsight 群集。 在本文中，您将学习如何使用 HDInsight 群集中包含 Ambari REST API。"
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

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>通过使用 Ambari REST API 来管理 HDInsight 群集

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari 简化的管理和监视 Hadoop 群集通过提供一种易于使用 web 用户界面和 REST API。 Ambari 包含在基于 Linux 的 HDInsight 群集上，并用于监视群集并进行配置更改。 在本文中，您学习了通过执行常见任务使用卷曲 Ambari REST API，使用的基础知识。

##<a name="prerequisites"></a>系统必备组件

* [卷曲](http://curl.haxx.se/)︰ cURL 是一种跨平台实用工具，可用于从命令行使用 REST Api。 在本文中，它用于与 Ambari REST API 进行通信。
* [jq](https://stedolan.github.io/jq/): jq 是跨平台命令行实用程序，用于处理 JSON 文档。 在本文中，它用来分析从 Ambari REST API 返回的 JSON 文档。
* [Azure CLI](../xplat-cli-install.md): 跨平台命令行实用程序，用于使用 Azure 服务。

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Ambari 是什么？

[Apache Ambari](http://ambari.apache.org)通过提供易于使用的 web 用户界面，用于配置、 管理和监视 Hadoop 群集使 Hadoop 管理更简单。 开发人员可以使用[Ambari REST Api](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)将这些功能集成到其应用程序。

默认情况下，使用基于 Linux 的 HDInsight 群集提供了 Ambari。

##<a name="rest-api"></a>REST API，

在 HDInsight Ambari REST API 的基 URI 为 https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME，其中__群集名称__是您群集的名称。

> [AZURE.IMPORTANT] 虽然 URI (CLUSTERNAME.azurehdinsight.net) 的完全限定的域名称 (FQDN) 部分中的群集名称是不区分大小写，在 URI 中的其他匹配项是区分大小写。 例如，如果您的群集名为 MyCluster，下面是有效的 Uri:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> 以下 Uri 返回一个错误，因为名称的第二个匹配项是不正确的大小写。
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

在 HDInsight 的 Ambari 连接需要 HTTPS。 在对连接进行身份验证，必须使用 （默认为__管理员__） 的管理员帐户名和密码创建群集时提供。

下面的示例使用卷曲若要进行 GET 请求针对 REST API。 __密码__替换群集的管理员密码。 __群集名称__替换该群集的名称︰

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
响应是 JSON 文档开头与以下类似的信息︰

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

由于这是 JSON，可以很方便地使用 JSON 解析器来使用数据。 例如，下面的示例使用 jq 只显示`health_report`元素。

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>示例︰ 获取的 FQDN 的群集节点

在使用 HDInsight，您可能需要知道群集节点的完全限定的域名 (FQDN)。 您可以轻松地检索使用以下群集中的各个节点的 FQDN:

* __头节点__︰`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __辅助节点__︰`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper 节点__︰`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

注意上述每个示例遵循相同的模式︰

1. 查询组件，我们知道在这些节点上运行。
2. 检索`host_name`元素，其中包含这些节点的 FQDN。

`host_components`返回文档中的元素包含多个项。 使用`.host_components[]`，然后指定元素中的路径将循环访问每个项并拉出特定的路径的值。 如果只需要一个值，例如第一的 FQDN 项，可以返回为一个集合项，然后选择特定的条目︰

    jq '[.host_components[].HostRoles.host_name][0]'

这从集合中返回的第一个的 FQDN。

##<a name="example-get-the-default-storage-account-and-container"></a>示例︰ 获取默认存储帐户和容器

创建一个 HDInsight 的群集时，您必须使用 Azure 存储帐户和 blob 容器作为默认存储群集。 可以使用 Ambari 来创建群集后检索此信息。 例如，如果您希望以编程方式对容器直接写入数据。

以下将检索群集默认存储区的 WASB URI:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] 此方法返回第一个应用于服务器的配置 (`service_config_version=1`，) 它包含此信息。 如果检索群集创建后已被修改的值，可能需要列出配置版本和检索最新的一个。

返回一个值类似于以下示例中，在__容器__是默认容器和__帐户名__是 Azure 存储帐户名︰

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

然后可以使用[Azure CLI](../xplat-cli-install.md)使用此信息来上载或下载数据的容器。

1. 获取存储帐户的资源组。 从 Ambari 检索存储帐户名称替换__帐户名__︰

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    这将返回该帐户的资源组名称。
    
    > [AZURE.NOTE] 如果此命令返回执行任何操作，则可能需要更改到 Azure 资源管理器模式的 Azure CLI 并再次运行该命令。 若要切换到 Azure 资源管理器模式，请使用下面的命令︰
    >
    > `azure config mode arm`
    
2. 获取项的存储帐户。 __组名称__替换上一步的资源组。 替换为存储帐户名的__帐户名__︰

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    本示例返回主键的帐户。
    
3. 使用上载命令将文件存储在容器︰

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    替换为存储帐户名的__帐户名__。 替换以前检索的键__ACCOUNTKEY__ 。 __文件路径__是上载，而__BLOBPATH__是容器中的路径所需的文件的路径。

    例如，如果想要在 wasbs://example/data/filename.txt 出现在 HDInsight 的文件，则__BLOBPATH__将`example/data/filename.txt`。

##<a name="example-update-ambari-configuration"></a>更新 Ambari 配置示例︰

1. 获取当前的配置，Ambari 将存储为"需配置":

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    本示例返回 JSON 文档包含 （由_标记_值） 的当前配置为在群集上安装的组件。 下面的示例摘自从触发群集类型返回的数据。
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    从该列表中，您需要复制该组件的名称 (例如，__触发\_储蓄\_sparkconf__和__标记__值。
    
2. 通过使用以下命令检索的组件和标记的配置。 替换为__触发的储蓄-sparkconf__和__初始__的组件和标记您想要检索的配置。

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    卷曲检索 JSON 文档中，然后 jq 用于对数据进行修改以创建模板。 然后，可使用模板来添加/修改配置值。 特别是它执行以下任务︰
    
    * 创建一个包含字符串"版本"和日期存储在__newtag__中的唯一值。
    * 创建新的所需配置的根文档。
    * 获取内容的`.items[]`数组并将其添加在__desired_config__元素之下。
    * 删除__href__、__版本__和__配置__元素，这些元素不需要提交一个新的配置。
    * 添加新的__标签__元素并将其值设置为__版本 # # #__。 数字部分基于当前日期。 每个配置必须有一个唯一的标记。
    
    最后，将数据保存到__newconfig.json__文档。 文档结构应类似于下面的示例︰
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. __属性__对象中打开__newconfig.json__文件并修改/添加值。 下面的示例更改的__"spark.yarn.am.memory"__从__"1 g"__为__"3g"__和，并添加一个新元素的值为__"256 m"__ __"spark.kryoserializer.buffer.max"__ 。

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    完成修改后，请保存该文件。

4. 使用下面的命令提交到 Ambari 更新的配置。

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    此命令管道对 curl 请求，将其提交给新的所需配置为群集的__newconfig.json__文件的内容。 CURL 请求返回的 JSON 文档。 此文档中的__versionTag__元素应与您提交的版本和__配置__对象将包含请求的配置更改。

###<a name="example-restart-a-service-component"></a>示例︰ 重新启动一个服务组件

在这种情况下，如果查看 Ambari web 用户界面时，触发服务则表示，它需要新的配置生效之前必须重新启动。 使用下列步骤重新启动该服务。

1. 使用以下方法来启用触发服务维护模式︰

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    此命令向服务器发送一个 JSON 文档 (包含在`echo`语句) 维护模式开启。
    您可以验证该服务目前在维护模式下，使用下列请求︰
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    这将返回的值为`"ON"`。

3. 接下来，使用以下方法来关闭该服务︰

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    此命令返回类似于以下的响应。
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    `href`返回此 URI 的值使用群集节点的内部 IP 地址。 若要使用此选项从群集之外，替换群集的 FQDN 的"10.0.0.18:8080"部分。 例如，以下命令检索请求的状态。
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    如果此方法返回的值为`"COMPLETED"`，然后在请求完成。

4. 前面的请求完成后，使用以下方法来启动该服务。

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    一旦该服务重新启动时，它是新的配置设置。

5. 最后，使用以下方法来关闭维护模式。

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>下一步行动

REST API 的完整参考，请参阅[Ambari API 参考 V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)。

> [AZURE.NOTE] Ambari 中的某些功能被禁用，因为它由 HDInsight 云服务;例如，添加或从群集中删除主机或添加新的服务。
