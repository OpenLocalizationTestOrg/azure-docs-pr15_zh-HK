<properties
    pageTitle="提交使用晚远程触发作业 |Microsoft Azure"
    description="了解如何使用 HDInsight 群集晚来远程提交触发作业。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>提交到 HDInsight linux 中使用晚 Apache 触发群集远程触发作业

在 Azure HDInsight 上的 Apache 触发群集包括晚，将作业提交到触发群集远程的 REST 接口。 有关详细的文档，请参阅[晚](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)。

可以使用晚运行交互式触发贝壳或提交上触发运行的批处理作业。 这篇文章谈到使用晚提交的批处理作业。 下面的语法使用卷曲进行 REST 调用晚终结点。

**系统必备组件︰**

您必须具有以下各项︰

- Azure 的订阅。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- 一个 HDInsight Linux 上的 Apache 触发群集。 有关说明，请参阅[创建 Apache 触发群集在 Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md)。

## <a name="submit-a-batch-job-the-cluster"></a>提交的批处理作业群集

提交的批处理作业之前，您必须上传与群集相关的群集存储上的应用程序 jar。 [**AzCopy**](../storage/storage-use-azcopy.md)，一种命令行实用工具，可用于执行此操作。 有很多的其他客户端，您可以使用上载数据。 您可以找到更多的关于它们在[Hadoop 作业中 HDInsight 的数据上载](hdinsight-upload-data.md)。

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**示例**︰

* 如果 jar 文件位于群集存储 (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* 如果您想要将 jar 文件名和类名作为输入文件的一部分进行传递 (在此示例中，input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>获取有关在群集上运行的批处理

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**示例**︰

* 如果您想要检索群集上运行的所有批次︰

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* 如果您想要检索带有给定 batchId 的特定批次

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>删除批处理作业

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**示例**︰

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>晚和高可用性

晚为触发在群集上运行的作业提供了高可用性。 以下是几个例子。

* 如果晚服务出现故障，已经提交到触发群集远程作业后，作业会继续在后台运行。 备份时间晚时，它会还原作业和报告回的状态。

* HDInsight 的 Jupyter 笔记本在后端由晚供电。 如果重新启动晚服务获取触发作业运行一个笔记本，笔记本将继续运行的代码单元。 

## <a name="show-me-an-example"></a>举例说明

在本节中，我们看示例如何使用晚提交触发应用程序、 监视应用程序的进度，然后删除该作业。 在此示例中，我们使用的应用程序是一个开发的文章[创建一个独立 Scala 应用程序并 HDInsight 触发群集上运行](hdinsight-apache-spark-create-standalone-application.md)。 以下步骤假定如下︰

* 您已与群集相关的存储帐户复制到应用程序 jar。
* 您可以尝试这些步骤的计算机上安装的卷。

执行以下步骤。

1. 让我们首先验证晚运行在群集上。 我们可以这样做是让运行批次的列表。 如果您正在运行一个作业使用晚第一次，这应返回零。

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    将得到如下输出︰

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    请注意最后一行在输出中的如何说**总数︰ 0**，这表示任何正在运行的批处理。

2. 让我们现在提交的批处理作业。 下面的代码段将用作参数的输入的文件 (input.txt) 传递的 jar 名称和类名。 这是推荐的方法，如果您从 Windows 计算机运行这些步骤。

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    **Input.txt**文件中的参数定义如下︰

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    您应该看到类似于下面的输出︰

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    请注意输出的最后一行的说︰**状态︰ 启动**。 它还说， **id: 0**。 这是批次 id。

3. 现在，您可以检索此使用批次 ID 的特定批次的状态

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    您应该看到类似于下面的输出︰

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    现在，输出显示**状态︰ 成功**，这表明该作业已成功完成。

4. 如果需要，您现在可以删除批次。

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    您应该看到类似于下面的输出︰

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    输出的最后一行显示该批已成功删除。 如果您删除一个作业正在运行时，它实质上是将取消该作业。 如果您删除作业已完成，否则为无论成功或完全删除作业信息。

## <a name="seealso"></a>请参见


* [概述︰ 在 Azure HDInsight 上的 Apache 触发](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>方案

* [触发与 BI︰ 执行与 BI 工具一起使用在 HDInsight 中的触发交互式数据分析](hdinsight-apache-spark-use-bi-tools.md)

* [机器学习与触发︰ 用于分析使用 HVAC 数据的生成温度 HDInsight 中使用触发](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [机器学习与触发︰ 使用 HDInsight 来预测食品检查结果中的触发](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [HDInsight 用于构建实时流的应用程序中触发流︰ 使用触发](hdinsight-apache-spark-eventhub-streaming.md)

* [在 HDInsight 中使用触发网站日志分析](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>创建和运行应用程序

* [创建独立的应用程序使用 Scala](hdinsight-apache-spark-create-standalone-application.md)

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
