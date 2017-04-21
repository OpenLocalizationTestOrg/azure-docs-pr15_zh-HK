<properties
   pageTitle="使用 MapReduce 和卷曲与 Hadoop 在 HDInsight |Microsoft Azure"
   description="了解如何远程运行 MapReduce 作业使用 Hadoop 使用 Curl HDInsight。"
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

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>运行 MapReduce 作业使用 Hadoop 使用 Curl HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

在本文中，您将学习如何使用卷曲在 Hadoop HDInsight 群集上运行 MapReduce 作业。

卷曲用于说明如何可以与 HDInsight 交互使用原始 HTTP 请求运行 MapReduce 作业。 这是通过使用 HDInsight 群集提供的 WebHCat REST API （以前称为 Templeton）。

> [AZURE.NOTE] 如果您已熟悉如何使用 Hadoop 的基于 Linux 的服务器，但您还不熟悉 HDInsight，请参阅[您需要了解的有关在 HDInsight 上的基于 Linux 的 Hadoop](hdinsight-hadoop-linux-information.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* Hadoop HDInsight 群集上的 (Linux 或基于 Windows 的)

* [卷曲](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>运行 MapReduce 作业使用卷曲

> [AZURE.NOTE] 当您使用包含 WebHCat 卷曲或任何其他其余部分通信时，您必须通过提供 HDInsight 群集管理员的用户名和密码验证请求。 用于向服务器发送请求的 URI 的一部分，还必须使用群集名称。
>
> 本节中的命令，将**用户名**替换用户进行身份验证群集，和**密码**的用户帐户的密码。 **群集名称**替换您的群集的名称。
>
> REST API，使用[基本的访问身份验证](http://en.wikipedia.org/wiki/Basic_access_authentication)保护。 始终应通过使用 HTTPS 来确保您的凭据被安全地发送到服务器的请求。

1. 从命令行中使用下面的命令验证可以连接到 HDInsight 群集︰

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    您应该收到类似下面的响应︰

        {"status":"ok","version":"v1"}

    在此命令中使用的参数如下所示︰

    * **-u**︰ 指示的用户名称和密码用于对请求进行身份验证
    * **-G**︰ 指示这是 GET 请求

    将 uri， **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**，开头是相同的所有请求。

2. 若要提交 MapReduce 作业，请使用下面的命令︰

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    (/ Mapreduce/jar) 的 URI 的末尾告诉 WebHCat，此请求将开始从 jar 文件中类的 MapReduce 作业。 在此命令中使用的参数如下所示︰

    * **-d**:`-G`未使用的因此请求默认为 POST 方法。 `-d`指定随请求发送的数据值。

        * **user.name**︰ 运行该命令的用户
        * **jar**︰ 包含类的 jar 文件的位置运行
        * **类**︰ 包含 MapReduce 逻辑的类
        * **参数**︰ 参数传递给 MapReduce 作业;在这种情况下，输入的文本文件和用于输出目录

    此命令应返回可用于检查作业的状态的作业 ID:

        {"id":"job_1415651640909_0026"}

3. 若要检查作业的状态，请使用下面的命令。 返回上一步中的值替换**作业 ID** 。 例如，如果返回值为`{"id":"job_1415651640909_0026"}`，然后将作业 ID `job_1415651640909_0026`。

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    如果该作业已完成，状态会是"成功"。

    > [AZURE.NOTE] 此 Curl 请求返回的 JSON 文档信息作业;jq 用于检索的状态值。

4. 当作业的状态更改为**成功**时，则可以从 Azure Blob 存储检索作业的结果。 `statusdir`查询与传递的参数包含位置的输出文件中。在这种情况下， **wasbs: / 示例/卷曲**。 此地址存储在 HDInsight 群集使用的默认存储容器的**示例/卷曲**目录中的作业的输出。

您可以列出并使用[Azure CLI](../xplat-cli-install.md)下载这些文件。 例如，列表中的文件**示例/卷曲**，使用下面的命令︰

    azure storage blob list <container-name> example/curl

要下载一个文件，请使用以下方法︰

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] 您必须指定使用包含 blob 存储帐户名称`-a`和`-k`参数或一组**AZURE\_存储\_帐户**和**AZURE\_存储\_访问\_键**环境变量。 [如何将上载的数据复制到 HDInsight](hdinsight-upload-data.md)的详细信息，请参阅。

##<a id="summary"></a>摘要

本文档中所示，您可以使用原始 HTTP 请求运行、 监视和查看您 HDInsight 群集配置单元作业的结果。

有关本文中使用的 REST 接口的详细信息，请参阅[WebHCat 参考](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 中的 MapReduce 作业有关的一般信息︰

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)
