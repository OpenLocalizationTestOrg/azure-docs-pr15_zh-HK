<properties
    pageTitle="监视 Hadoop 群集中使用 Ambari API 的 HDInsight |Microsoft Azure"
    description="Apache Ambari Api 用于创建、 管理和监视 Hadoop 群集。 直观的运算符工具和 Api 隐藏 Hadoop 的复杂性。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>HDInsight 使用 Ambari API 中的监视 Hadoop 群集

了解如何使用 Ambari Api 监视 HDInsight 群集。

> [AZURE.NOTE] 这篇文章中的信息是主要用于基于 Windows 的 HDInsight 群集，提供 Ambari REST API 的只读版本。 基于 Linux 的群集，请参阅[管理 Hadoop 群集使用 Ambari](hdinsight-hadoop-manage-ambari.md)。

## <a name="what-is-ambari"></a>Ambari 是什么？

[Apache Ambari] [ambari-home]用于资源调配、 管理和监控 Apache Hadoop 群集。 它包括一个直观的运算符工具集合和强大的一组 Api，这些隐藏的 Hadoop，简化的群集操作的复杂性。 有关 Api 的详细信息，请参阅[Ambari API 参考][ambari-api-reference]。 

HDInsight 目前支持只 Ambari 的监视功能。 HDInsight 版本 3.0 和 2.1 群集支持 Ambari API 1.0。 本文介绍了 HDInsight 版本 3.1 和 2.1 群集访问 Ambari Api。 两者之间的关键区别是某些组件已更改引入了新功能 （如作业历史记录服务器上）。 

**系统必备组件**

在开始本教程之前，您必须具有以下︰

- **带有 Azure PowerShell 的工作站**。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- （可选）[卷曲][curl]. 要安装此更新，请参阅[卷曲发布和下载][curl-download]。

    >[AZURE.NOTE] 在 Windows 中，使用双引号而不是单引号都开始选项值时使用 cURL 命令。

- **Azure HDInsight 群集**。 有关群集配置的说明，请参阅[开始使用 HDInsight] [hdinsight-get-started]或[群集调配 HDInsight][hdinsight-provision]。 您将需要以下数据以转到教程︰

    群集属性|Azure PowerShell 变量名|值|说明
    ---|---|---|---
    HDInsight 群集名称|$clusterName||HDInsight 群集的名称。
    群集用户名|$clusterUsername||创建群集时，指定了群集用户名称。
    群集的密码|$clusterPassword||群集的用户密码。

    >[AZURE.NOTE] 填充表中的值。 这将有助于通过本教程。

## <a name="jump-start"></a>快速入门

有多种方法使用 Ambari 来监视 HDInsight 群集。

**使用 Azure PowerShell**

下面是 Azure PowerShell 脚本以使 MapReduce 作业跟踪程序信息*HDInsight 3.1 群集中。*  关键的区别是我们拉从 YARN 服务 （而非 MapReduce） 这些详细信息。

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

以下是用于 MapReduce 作业跟踪程序信息*HDInsight 2.1 群集中*获取 Azure PowerShell 脚本︰

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

输出为︰

![Jobtracker 输出][img-jobtracker-output]

**使用卷曲**

以下是使用 cURL 获取群集信息的示例︰

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

输出为︰

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10/8/2014年的释放**︰

当使用 Ambari 端点，"https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}"， *host_name*字段返回完全限定的域名称 (FQDN) 的节点，而不是主机名。 10/8/2014年版本中，本示例返回之前只是"**headnode0**"。 10/8/2014年发布后，您将获得 FQDN"**headnode0。 {ClusterDNS}.azurehdinsight.net**"，如前面的示例中所示。 此更改才能促进的方案 （如 HBase 和 Hadoop） 的多个群集类型可以部署一个虚拟网络 (VNET) 中的位置。 发生这种情况，例如，当使用 Hadoop 作为后端平台 HBase。

## <a name="ambari-monitoring-apis"></a>Ambari 监控 Api

下表列出了一些最常见的 Ambari 监控 API 调用。 有关 API 的详细信息，请参阅[Ambari API 参考][ambari-api-reference]。

监视器 API 调用|URI|说明
---|---|---
获取群集|`/api/v1/clusters`|
获取群集信息。|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|群集服务主机
获取服务|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|服务包括︰ hdfs，mapreduce
获取服务的信息。|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
获得服务组件|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode datanode<br/>MapReduce: jobtracker;tasktracker
获取组件信息。|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo，主机组件、 衡量标准
获取主机|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0 workernode0
获取主机的信息。|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
获取主机组件|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode resourcemanager
获取主机组件的信息。|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles，组件、 主机、 度量标准
获取配置|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|配置类型︰ 核心站点、 hdfs 站点，mapred 网站、 网站用户配置单元
获取配置信息。|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|配置类型︰ 核心站点、 hdfs 站点，mapred 网站、 网站用户配置单元


##<a name="next-steps"></a>下一步行动

现在您已了解如何使用 Ambari 监控 API 调用。 若要了解详细信息，请参阅︰

- [管理使用 Azure 门户 HDInsight 群集][hdinsight-admin-portal]
- [管理使用 Azure PowerShell HDInsight 群集][hdinsight-admin-powershell]
- [管理 HDInsight 群集使用命令行界面][hdinsight-admin-cli]
- [HDInsight 文档][hdinsight-documentation]
- [开始使用 HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
