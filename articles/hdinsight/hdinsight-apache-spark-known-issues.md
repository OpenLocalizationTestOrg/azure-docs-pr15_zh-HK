<properties 
    pageTitle="已知问题的 Apache 触发在 HDInsight |Microsoft Azure" 
    description="Apache 触发 HDInsight 中的已知的问题。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>在 HDInsight Linux 上的 Apache 触发群集的已知的问题

本文档跟踪的 HDInsight 触发公共预览的所有已知的问题。  

##<a name="livy-leaks-interactive-session"></a>晚泄漏交互式会话
 
晚与交互式会话 （从 Ambari 或 headnode 0 的虚拟机重新启动由于） 仍处于活动状态重新启动后，将泄漏的交互式作业会话。 正因为如此，新的作业可以停滞在接受状态，且无法启动。

**缓解措施︰**

使用以下步骤变通解决该问题︰

1. Ssh 到 headnode。 
2. 运行以下命令来查找晚通过启动交互式作业的应用程序 Id。 

        yarn application –list

    名称会晚如果作业已启动与晚交互式会话没有显式名称与指定，对于晚会话启动 Jupyter 笔记本的默认作业，该作业名称将着手 remotesparkmagics_ *。 

3. 运行以下命令来取消这些作业。 

        yarn application –kill <Application ID>

新作业将开始运行。 

##<a name="spark-history-server-not-started"></a>触发历史记录服务器未启动 

创建群集后触发历史记录服务器不会自动启动。  

**缓解措施︰** 

从 Ambari 手动启动服务器历史记录。

## <a name="permission-issue-in-spark-log-directory"></a>在触发日志目录的权限问题 

当 hdiuser 提交的作业 spark-submit 时，没有错误 java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log （权限拒绝） 和驱动程序日志处于不一致的。 

**缓解措施︰**
 
1. 将 hdiuser 添加到 Hadoop 组。 
2. 提供群集创建后 /var/log/spark 777 权限。 
3. 更新使用 Ambari 为目录权限为 777 的触发日志位置。  
4. 运行触发-将作为提交 sudo。  

## <a name="issues-related-to-jupyter-notebooks"></a>问题与 Jupyter 笔记本

以下是一些已知的问题，与 Jupyter 笔记本。


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>文件名中包含非 ASCII 字符的笔记本

可以触发 HDInsight 群集中使用的 Jupyter 笔记本不应在文件名中包含非 ASCII 字符。 如果您试图上载的文件通过 Jupyter 用户界面具有非 ASCII 文件名，则将静默失败 （即 Jupyter 不会让您上载的文件，但它也不会引发可见的错误）。 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>加载的更大尺寸的笔记本时的错误

您可能会看到错误**`Error loading notebook`**加载笔记本的尺寸较大时。  

**缓解措施︰**

如果您收到此错误，并不意味着您的数据已损坏或丢失。  笔记本中的磁盘上仍然是`/var/lib/jupyter`，并且可以 SSH 到群集来访问它们。 您可以复制您的笔记本从群集到本地计算机上 （使用 SCP 或 WinSCP） 作为备份以防止笔记本中的任何重要数据的丢失。 您可以到您在端口 8001 不用通过网关访问 Jupyter headnode 的然后 SSH 隧道。  从那里，您可以清除笔记本的输出并重新保存该笔记本的大小最小化。

若要防止此错误再次发生，必须遵循一些最佳做法︰

* 请务必保持较小的笔记本大小。 发送回 Jupyter 的任何输出触发作业从笔记本中保持不变。  最佳做法是使用 Jupyter 一般以避免运行`.collect()`上大 RDD 的或 dataframes;相反，如果您想要查看 RDD 的内容，考虑运行`.take()`或`.sample()`，以便您的输出不会太大。
* 此外，当您保存一个笔记本，清除所有输出单元以减小大小。

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>笔记本初始启动时间比预期长 

Jupyter 笔记本使用触发魔术中的第一条代码语句可能需要超过一分钟。  

**解释︰**
 
这是因为在运行的第一个代码单元。 这将在后台启动会话配置并触发，SQL，并设置配置单元的上下文。 这些上下文设置、 运行的第一个语句，这给人的印象，之后该语句用了很长时间才能完成。

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>以创建会话超时时间 Jupyter 笔记本

当触发群集资源不足时，Jupyter 笔记本中的添姿加 Pyspark 内核将会尝试创建该会话的超时。 

**缓解措施︰** 

1. 释放一些触发由群集中的资源︰

    - 通过转到关闭和暂停菜单或单击关闭笔记本资源管理器中停止其他触发笔记本。
    - 停止从 YARN 其他触发应用程序。

2. 请重新启动您试图启动笔记本。 足够的资源，应该可供您立即创建会话。

##<a name="see-also"></a>请参见

* [概述︰ 在 Azure HDInsight 上的 Apache 触发](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>方案

* [触发与 BI︰ 执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)

* [机器学习与触发︰ 用于分析使用 HVAC 数据的生成温度 HDInsight 中使用触发](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [机器学习与触发︰ 使用 HDInsight 来预测食品检查结果中的触发](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [HDInsight 用于构建实时流的应用程序中触发流︰ 使用触发](hdinsight-apache-spark-eventhub-streaming.md)

* [在 HDInsight 中使用触发网站日志分析](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>创建和运行应用程序

* [创建独立的应用程序使用 Scala](hdinsight-apache-spark-create-standalone-application.md)

* [在群集上使用晚触发远程运行作业](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>工具和扩展

* [使用 HDInsight 工具插件为 IntelliJ 创意来创建和提交触发 Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)

* [使用 HDInsight 工具插件为 IntelliJ 创意来触发应用程序进行远程调试](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [在 HDInsight 上触发群集使用 Zeppelin 笔记本](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [内核可用于触发 HDInsight 群集中的 Jupyter 笔记本](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [外部包使用 Jupyter 笔记本](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [在您的计算机上安装 Jupyter 并连接到一个 HDInsight 触发的群集](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>管理资源

* [管理在 Azure HDInsight Apache 触发群集的资源](hdinsight-apache-spark-resource-manager.md)

* [跟踪和调试 HDInsight 在 Apache 触发群集上运行的作业](hdinsight-apache-spark-job-debugging.md)
