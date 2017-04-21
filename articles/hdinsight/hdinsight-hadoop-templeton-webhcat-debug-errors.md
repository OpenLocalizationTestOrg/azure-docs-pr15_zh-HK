<properties
 pageTitle="了解和解决上 HDInsight 的 WebHCat 错误"
 description="了解如何关于常见错误返回到由 WebHCat 在 HDInsight 上，以及如何解决这些问题。"
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
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>理解和解决在 HDInsight WebHCat (Templeton，) 从接收到的错误

当使用 WebHCat （以前称为 Templeton，） 使用 HDInsight，您可能会收到错误。 本文对常见的错误--为什么会发生和能做什么来解决这些问题提供指导。

##<a name="what-is-webhcat"></a>WebHCat 是什么？

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat)是[HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog)、 表和存储管理层对 Hadoop 的 REST API。 WebHCat HDInsight 群集默认处于启用状态并使用的各种工具提交作业、 无需登录到群集中获取作业状态，等等。

##<a name="modifying-configuration"></a>修改配置

> [AZURE.IMPORTANT] 本文档中列出的错误的几个已超出配置的最大原因。 解析步骤中提到，您可以更改一个值，必须使用下列方法之一执行更改︰

* 对于**Windows**群集︰ 使用脚本的操作来配置在群集创建过程中的值。 有关详细信息，请参见[开发脚本操作](hdinsight-hadoop-script-actions.md)。

* 对于**Linux**群集︰ 使用 Ambari （web 或 REST API） 来修改该值。 有关详细信息，请参阅[管理 HDInsight 使用 Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>默认配置

以下是默认的配置值可以影响 WebHCat 的性能，或者如果超出这些值，就会导致错误︰

| 设置 | 它的作用 | 默认值 |
| ------- | ------------ | ------------- |
| [yarn.scheduler.capacity.maximum 应用程序][maximum-applications] | 最多可以同时处于活动状态的作业数 （挂起或运行） | 10000 |
| [templeton.exec.max-过程][max-procs] | 最多可以同时处理的请求数 | 20 |
| [ms-mapreduce.jobhistory.max-年龄][max-age-ms] | 作业历史记录将被保留的天数 | 7 天 |

##<a name="too-many-requests"></a>请求太多

**HTTP 状态代码**︰ 429

| 原因 | 解析 |
| ----- | ---------- |
| 您已超出了每分钟 （默认值为 20） 由 WebHCat 提供的最大并发请求 | 超过的最大并发请求数的工作负荷，以确保没有提交减少或增加并发请求限制修改`templeton.exec.max-procs`。 有关详细信息，请参阅[修改配置](#modifying-configuration) |

##<a name="server-unavailable"></a>服务器不可用

**HTTP 状态代码**︰ 503

| 原因 | 解析 |
| ---------------- | ------------------- |
| 这通常发生在故障转移群集主要和次要 HeadNode 之间 | 等待两分钟，然后重试此操作 |

##<a name="bad-request-content-could-not-find-job"></a>错误的请求内容︰ 找不到工作

**HTTP 状态代码**︰ 400

| 原因 | 解析 |
| ---------------- | ------------------- |
| 作业详细信息已清除作业历史记录的清除程序 | 作业历史记录的默认保留期为 7 天。 这可通过修改更改`mapreduce.jobhistory.max-age-ms`。 有关详细信息，请参阅[修改配置](#modifying-configuration) |
| 作业已中止故障转移 | 重试作业提交最多两分钟的 |
| 使用了无效的作业 id | 检查是否正确的作业 id |

##<a name="bad-gateway"></a>网关错误

**HTTP 状态代码**︰ 502

| 原因 | 解析 |
| ---------------- | ------------------- |
| 在 WebHCat 过程中发生内部垃圾回收 | 等待垃圾回收完成，或重新启动 WebHCat 服务 |
| 等待来自 ResourceManager 服务的响应时超时。 当活动应用程序的配置最大值 （默认值为 10000） 可以发生这种情况 | 等待当前运行的作业完成或并行作业限制增加通过修改`yarn.scheduler.capacity.maximum-applications`。 有关详细信息，请参阅[修改配置](#modifying-configuration)  |
| 在尝试检索所有作业时[获得 /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs)调用通过`Fields`设置为`*` | 无法检索*作业的详细信息*，而使用`jobid`以检索作业仅大于某些作业 id 的详细信息。 或者，请不要使用`Fields` |
| WebHCat 服务在 HeadNode 故障转移期间处于关闭状态 | 等待两分钟，然后重试该操作 |
| 有 500 多个未完成的作业提交到 WebHCat | 等待，直到当前挂起的作业才能提交详细的作业已完成 |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
