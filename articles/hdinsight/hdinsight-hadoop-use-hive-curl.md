<properties
   pageTitle="使用 HDInsight 中的卷曲的 Hadoop 配置单元 |Microsoft Azure"
   description="了解如何远程豚将作业提交到 HDInsight 使用卷曲。"
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>带曲线的 HDInsight 在 Hadoop 使用运行配置单元查询

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

在本文中，您将学习如何使用卷曲在 Hadoop Azure HDInsight 群集上运行配置单元查询。

卷曲用于说明如何可以与 HDInsight 交互使用原始 HTTP 请求运行、 监视和检索配置单元查询的结果。 这是通过使用 HDInsight 群集提供的 WebHCat REST API （以前称为 Templeton）。

> [AZURE.NOTE] 如果您已熟悉使用 Hadoop 的基于 Linux 的服务器，但 HDInsight 的新，请参阅[您需要了解 Hadoop 在基于 Linux 的 HDInsight](hdinsight-hadoop-linux-information.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* Hadoop HDInsight 群集上的 (Linux 或基于 Windows 的)

* [卷曲](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>通过使用卷的运行配置单元查询

> [AZURE.NOTE] 当卷曲或任何其他通讯的其余部分使用 WebHCat，您必须通过 HDInsight 群集管理员提供的用户名和密码验证请求。 此外必须使用群集名称作为一部分用于向服务器发送请求统一资源标识符 (URI)。
>
> 本节中的命令，**用户名**替换用户验证到群集中，并替换**密码**与用户帐户的密码。 **群集名称**替换您的群集的名称。
>
> REST API，通过[基本身份验证](http://en.wikipedia.org/wiki/Basic_access_authentication)被安全。 始终应通过使用安全 HTTP (HTTPS)，以帮助确保您的凭据被安全地发送到服务器的请求。

1. 从命令行，使用下面的命令验证可以连接到 HDInsight 群集︰

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    您应该收到类似下面的响应︰

        {"status":"ok","version":"v1"}

    在此命令中使用的参数如下所示︰

    * **-u** -的用户名称和密码用于对请求进行身份验证。
    * **-G** -表示，这是一个 GET 请求。

    开头的 URL， **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**，将为所有请求相同。 路径， **/status**，指示该请求返回 WebHCat (也称为 Templeton) 状态的服务器。 您可以通过使用下面的命令来请求配置单元的版本︰

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    这应返回类似于以下响应︰

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. 使用以下方法来创建一个名为**log4jLogs**的新表︰

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    在此命令中使用的参数如下所示︰

    * **-d** -因为`-G`不使用，则该请求默认为 POST 方法。 `-d`指定随请求发送的数据值。

        * **user.name** -用户运行命令。

        * **执行**-执行 HiveQL 语句。

        * **statusdir** -此作业的状态将被写入该目录。

    这些语句执行以下操作︰

    * **删除表**中的删除表和数据文件，如果表已存在。

    * **创建外部表**-在配置单元中创建一个新的外部表。 外部表在配置单元中存储的表定义。 将数据留在原来的位置。

        > [AZURE.NOTE] 在预期基础数据更新的外部源，例如自动化的数据上载过程中，或者通过 MapReduce 的另一个操作，但却总是希望配置单元查询要使用的最新数据时，应使用外部表。
        >
        > 删除外部表 does**不**删除数据，仅表定义。

    * **行格式**的说明配置单元设置数据的格式。 在这种情况下，由空格分隔每个日志中的字段。

    * **存储作为文本文件位置**-告诉配置单元数据的存储 （示例中的数据目录），并且它将存储为文本。

    * **选择**选择所有列**t4**其中包含值**[错误]**的行数。 随着三个包含此值的行，这应该返回值为**3** 。

    > [AZURE.NOTE] 请注意，HiveQL 语句之间的空格替换为`+`字符时使用卷曲。 用引号括起来的值包含一个空格，如分隔符，不应该用`+`。

    * **像"%25.log"INPUT__FILE__NAME** -此限制为仅搜索使用以结尾的文件。 日志。 如果不存在，则配置单元将尝试搜索该目录及其子目录，包括与为此表定义的列架构不匹配的文件中的所有文件。

    > [AZURE.NOTE] 请注意该 %25 是 URL 编码形式的 %，因此实际状况是`like '%.log'`。 %必须是 URL 编码的因为它被视为在 Url 中的特殊字符。

    此命令应返回可用于检查作业的状态的作业 ID。

        {"id":"job_1415651640909_0026"}

3. 若要检查作业的状态，请使用下面的命令。 返回上一步中的值替换**作业 ID** 。 例如，如果返回值为`{"id":"job_1415651640909_0026"}`，然后将**作业 ID** `job_1415651640909_0026`。

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    如果作业完成后，状态将会**成功**。

    > [AZURE.NOTE] 此 Curl 请求返回的 JavaScript 对象符号 (JSON) 文档信息作业; 与jq 用于检索的状态值。

4. 后该作业的状态都更改为**成功**，则可从 Azure Blob 存储检索该作业的结果。 `statusdir`查询与传递的参数包含位置的输出文件中。在这种情况下， **wasbs: / 示例/卷曲**。 此地址将作业的输出存储在 HDInsight 群集使用的默认存储容器的**示例/卷曲**目录中。

    您可以列出并使用[Azure CLI](../xplat-cli-install.md)下载这些文件。 例如，列表中的文件**示例/卷曲**，使用下面的命令︰

        azure storage blob list <container-name> example/curl

    要下载一个文件，请使用以下方法︰

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] 您必须指定使用包含 blob 存储帐户名称`-a`和`-k`参数或一组**AZURE\_存储\_帐户**和**AZURE\_存储\_访问\_键**环境变量。 请参阅 < href ="data.md-hdinsight-上载"目标 ="_blank"的详细信息。

6. 使用下面的语句创建名为**errorLogs**的新内部表︰

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    这些语句执行以下操作︰

    * 如果不存在，则**创建表如果不存在**-创建一个表。 由于不使用**外部**关键字，则这种内部表，它存储在配置单元数据仓库和完全由配置单元。

        > [AZURE.NOTE] 与外部表，除去内部的表将删除基础数据。

    * **存储 AS ORC**的优化行纵栏式 (ORC) 格式存储的数据。 这是用于存储配置单元数据的高度优化和有效格式。
    * **插入覆盖...选择**-从**log4jLogs**表中包含**[错误]**，选择行，然后**errorLogs**表中插入数据。
    * **选择**新的**errorLogs**表中选择所有行。

7. 使用作业 ID 返回检查作业的状态。 它已成功，一旦使用 Azure CLI Mac、 Linux 和所述的 Windows 以前下载并查看结果。 输出应包含三行，其中包含**[错误]**。


##<a id="summary"></a>摘要

本文档中所示，可以使用原始的 HTTP 请求来运行，监视，请查看 HDInsight 群集配置单元作业的结果。

本文中所使用的 REST 接口的详细信息，请参阅<a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat 参考</a>。

##<a id="nextsteps"></a>下一步行动

有关使用 HDInsight 配置单元的一般信息︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

如果您使用 Tez 配置单元，请参阅以下文档以供调试信息︰

* [在基于 Windows 的 HDInsight 上使用 Tez 用户界面](hdinsight-debug-tez-ui.md)

* [使用基于 Linux 的 HDInsight Ambari Tez 视图](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


