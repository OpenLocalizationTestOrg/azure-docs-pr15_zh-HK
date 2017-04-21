<properties
   pageTitle="使用 HDInsight 中的卷曲的 Hadoop Sqoop |Microsoft Azure"
   description="了解如何远程 Sqoop 将作业提交到 HDInsight 使用卷曲。"
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>带曲线的 HDInsight 在 Hadoop 运行的 Sqoop 作业

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

了解如何使用卷曲在 Hadoop 群集在 HDInsight 上运行 Sqoop 作业。

卷曲用于说明如何可以与 HDInsight 交互使用原始 HTTP 请求运行、 监视和检索 Sqoop 作业的结果。 这是通过使用 HDInsight 群集提供的 WebHCat REST API （以前称为 Templeton）。

> [AZURE.NOTE] 如果您已熟悉使用 Hadoop 的基于 Linux 的服务器，但 HDInsight 的新，请参阅[有关使用 HDInsight 在 Linux 上的信息](hdinsight-hadoop-linux-information.md)。

##<a name="prerequisites"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* Hadoop HDInsight 群集上的 (Linux 或基于 Windows 的)

* [卷曲](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>通过使用 Curl 将提交 Sqoop 作业

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

    开头的 URL， **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**，将为所有请求相同。 路径， **/status**，指示该请求返回 WebHCat (也称为 Templeton) 状态的服务器。 

2. 使用以下方法来提交 sqoop 作业︰


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    在此命令中使用的参数如下所示︰

    * **-d** -因为`-G`不使用，则该请求默认为 POST 方法。 `-d`指定随请求发送的数据值。

        * **user.name** -用户运行命令。

        * **命令**-Sqoop 命令来执行。

        * **statusdir** -此作业的状态将被写入该目录。

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

##<a name="limitations"></a>限制

* 批量导出-基于与 Linux 的 HDInsight、 Sqoop 连接器用于将数据导出到 Microsoft SQL Server 或 SQL Azure 数据库当前不支持大容量插入。

* 属性的基于 Linux 的 HDInsight，当使用`-batch`开关执行插入时，Sqoop 将执行多个而不是批处理插入操作的插入。

##<a name="summary"></a>摘要

本文档中所示，可以使用原始的 HTTP 请求来运行，监视，并在 HDInsight 群集上查看 Sqoop 作业的结果。

本文中所使用的 REST 接口的详细信息，请参阅<a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API 指南</a>。

##<a name="next-steps"></a>下一步行动

有关使用 HDInsight 配置单元的一般信息︰

* [使用 Hadoop 在 HDInsight Sqoop](hdinsight-use-sqoop.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

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


