<properties
pageTitle="端口使用的 HDInsight |Azure"
description="在 HDInsight 上运行 Hadoop 服务使用的端口的列表。"
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>端口和 Uri 由 HDInsight

本文档提供在基于 Linux 的 HDInsight 群集上运行 Hadoop 服务使用的端口的列表。 它也用来连接到群集使用 SSH 端口上提供信息。

## <a name="public-ports-vs-non-public-ports"></a>公共和非公共端口的端口

基于 linux * 的 HDInsight 群集只公开三个端口公开上互联网;22、 23 和 443。 这些用来安全地访问群集使用 SSH 和安全支持 HTTPS 协议公开服务。

HDInsight 实现由几个 Azure 虚拟机 （群集中的节点） 的内部，在 Azure 虚拟网络上运行。 通过在虚拟的网络中，可以访问端口未通过互联网公开。 例如，如果您连接到使用 SSH 的头节点之一，从 head 节点然后直接即可运行在群集节点上的服务。

> [AZURE.IMPORTANT] 当您创建 HDInsight 群集中，如果没有作为一个配置选项指定 Azure 虚拟网络时，则创建一个。但是，不能对此自动创建虚拟网络联接其他计算机 （例如其他 Azure 的虚拟机或您的客户端开发计算机）。 

加入更多机到虚拟网络，必须先创建虚拟网络，并创建 HDInsight 群集时指定它。 有关详细信息，请参阅[使用 Azure 虚拟网络的扩展 HDInsight 功能](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>公共端口

在 HDInsight 群集中所有节点位于 Azure 的虚拟网络，并不能直接从 internet 访问。 公共网关提供对以下端口，共有 HDInsight 群集的所有类型的互联网访问。

| 服务 | 端口 | 协议 | 说明 |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | 将客户端连接到主 headnode 上的 sshd。 请参阅[使用 SSH 使用基于 Linux 的 HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | 将客户端连接到边缘节点 （仅适用于 HDInsight 特优） 上的 sshd。 请参阅[开始使用 HDInsight R 服务器](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | 将客户端连接到辅助 headnode 上的 sshd。 请参阅[使用 SSH 使用基于 Linux 的 HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari web 用户界面。 请参阅[管理 HDInsight 使用 Ambari Web 用户界面](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari REST API。 请参阅[管理 HDInsight 使用 Ambari REST API，](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog REST API。 请参阅[使用 Curl 蜂巢](hdinsight-hadoop-use-pig-curl.md)，[使用猪带曲线](hdinsight-hadoop-use-pig-curl.md)，[使用 MapReduce 随以卷](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | 连接到使用 ODBC 配置单元。 请参阅[将 Excel 连接到 Microsoft ODBC 驱动程序的 HDInsight](hdinsight-connect-excel-hive-odbc-driver.md)。 |
| HiveServer2 | 443 | JDBC | 连接到使用 JDBC 的配置单元。 请参阅[连接到使用该配置单元的 JDBC 驱动程序的 HDInsight 配置单元](hdinsight-connect-hive-jdbc-driver.md) |

以下是可用于特定的群集类型︰

| 服务 | 端口 | 协议 |群集类型 | 说明 |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase REST API。 请参阅[开始使用 HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| 晚 | 443 | HTTPS |  触发 | 触发 REST API。 请参阅[使用远程晚提交触发作业](hdinsight-apache-spark-livy-rest-interface.md) |
| 风暴 | 443 | HTTPS | 风暴 | 冲击 web 用户界面。 请参阅[部署和管理风暴拓扑上 HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>身份验证

在 internet 上公开的所有服务必须经过身份都验证︰

| 端口 | 凭据 |
| ---- | ----------- |
| 22 或 23 | 在群集创建过程中指定 SSH 用户凭据 |
| 443 | 登录名 (默认︰ admin) 和在群集创建过程中设置的密码 |

## <a name="non-public-ports"></a>非公共端口

> [AZURE.NOTE] 某些服务才可对特定的群集类型。 例如，HBase 才对 HBase 群集类型可用。

### <a name="hdfs-ports"></a>HDFS 端口

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode web 用户界面 | 头节点 | 30070 | HTTPS | Web 用户界面以查看当前状态 |
| NameNode 元数据服务 | 头节点 | 8020 | IPC | 文件系统元数据 
| DataNode | 工作人员的所有节点 | 30075 | HTTPS | Web 用户界面查看状态、 日志等。 |
| DataNode | 工作人员的所有节点 | 30010 | &nbsp; | 数据传输 |
| DataNode | 工作人员的所有节点 | 30020 | IPC | 元数据操作 |
| 辅助 NameNode | 头节点 | 50090 | HTTP | 检查点的 NameNode 元数据 |

### <a name="yarn-ports"></a>YARN 端口

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| 资源管理器 web 用户界面 | 头节点 | 8088 | HTTP | Web 用户界面为资源管理器 |
| 资源管理器 web 用户界面 | 头节点 | 8090 | HTTPS | Web 用户界面为资源管理器 |
| 资源管理器管理接口 | 头节点 | 8141 | IPC | 应用程序的提交 (配置单元时，配置单元服务器，小猪，等等。) |
| 资源管理器调度程序 | 头节点 | 8030 | HTTP | 管理接口 |
| 资源管理器应用程序接口 | 头节点 | 8050 | HTTP |应用程序管理器接口地址 |
| NodeManager | 工作人员的所有节点 | 30050 | &nbsp; | 容器管理员地址 |
| NodeManager web 用户界面 | 工作人员的所有节点 | 30060 | HTTP | 资源管理器界面 |
| 时间线地址 | 头节点 | 10200 | RPC | 时间轴服务 RPC 服务。 |
| 时间线 web 用户界面 | 头节点 | 8181 | HTTP | 时间轴服务 web 用户界面 |

### <a name="hive-ports"></a>配置单元端口

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | 头节点 | 10001 | 储蓄 | 用于以编程方式连接到配置单元 (储蓄/JDBC) 服务 |
| HiveServer | 头节点 | 10000 | 储蓄 | 用于以编程方式连接到配置单元 (储蓄/JDBC) 服务 |
| 配置单元 Metastore | 头节点 | 9083 | 储蓄 | 以编程方式连接到元数据配置单元 (储蓄/JDBC) 服务 |

### <a name="webhcat-ports"></a>WebHCat 端口

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat 服务器 | 头节点 | 30111 | HTTP | 在 HCatalog 和其他 Hadoop 服务 web API |

### <a name="mapreduce-ports"></a>MapReduce 的端口

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | 头节点 | 19888 | HTTP | MapReduce JobHistory web 用户界面 |
| JobHistory | 头节点 | 10020 | &nbsp; | MapReduce JobHistory 服务器 |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | 与申请变径输出转移中间映射 |

### <a name="oozie"></a>Oozie

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie 服务器 | 头节点 | 11000 | HTTP | Oozie 服务 URL |
| Oozie 服务器 | 头节点 | 11001 | HTTP | Oozie 管理端口 |

### <a name="ambari-metrics"></a>Ambari 标准

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| 时间线 （应用程序历史记录） | 头节点 | 6188 | HTTP | 时间轴服务 web 用户界面 |
| 时间线 （应用程序历史记录） | 头节点 | 30200 | RPC | 时间轴服务 web 用户界面 |

### <a name="hbase-ports"></a>HBase 端口

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | 头节点 | 16000 | &nbsp; | &nbsp; |
| HMaster 信息 Web 用户界面 | 头节点 | 16010 | HTTP | HBase 主 web 用户界面的端口 |
| 区域服务器 | 工作人员的所有节点 | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | 客户端用来连接到 ZooKeeper 端口 |

### <a name="kafka-ports"></a>Kafka 端口

| 服务 | 节点 | 端口 | 协议 | 说明 |
| ------- | ------- | ---- | -------- | ----------- |
| 代理程序  | 辅助节点 | 9092 | [Kafka 线路协议](http://kafka.apache.org/protocol.html) | 用于客户端通信 |
| &nbsp; | Zookeeper 节点 | 2181 | &nbsp; | 客户端用来连接到 Zookeeper 端口 |
