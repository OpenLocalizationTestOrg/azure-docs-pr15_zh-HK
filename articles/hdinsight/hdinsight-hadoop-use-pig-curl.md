<properties
   pageTitle="猪的 Hadoop 使用卷曲在 HDInsight |Microsoft Azure"
   description="了解如何使用卷曲在 Azure HDInsight 在 Hadoop 群集上运行猪的拉丁文作业。"
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>猪的作业与 Hadoop HDInsight 上使用来运行卷曲

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

在本文中，您将学习如何使用卷曲在 Azure HDInsight 群集上运行猪的拉丁文作业。 猪的拉丁语编程语言可以描述应用于输入数据，以产生所需的输出的转换。

卷曲用于说明如何可以与 HDInsight 交互使用原始 HTTP 请求运行、 监视和检索结果的猪的作业。 这是通过使用 WebHCat REST API （以前称为 Templeton） 所提供的 HDInsight 群集。

> [AZURE.NOTE] 如果您已熟悉使用 Hadoop 的基于 Linux 的服务器，但 HDInsight 的新，请参阅[基于 Linux 的 HDInsight 提示](hdinsight-hadoop-linux-information.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 （基于 Linux 的或基于 Windows 的）

* [卷曲](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>通过使用 Curl 运行猪的作业

> [AZURE.NOTE] 当卷曲或任何其他通讯的其余部分使用 WebHCat，您必须通过 HDInsight 群集中提供的管理员用户名和密码验证请求。 此外必须使用群集名称作为一部分的用来向服务器发送请求统一资源标识符 (URI)。
>
> 本节中的命令，**用户名**替换用户验证到群集中，并替换**密码**与用户帐户的密码。 **群集名称**替换您的群集的名称。
>
> REST API，通过[基本的访问身份验证](http://en.wikipedia.org/wiki/Basic_access_authentication)被安全。 始终应通过使用安全 HTTP (HTTPS)，以帮助确保您的凭据被安全地发送到服务器的请求。

1. 从命令行，使用下面的命令验证可以连接到 HDInsight 群集︰

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    您应该收到类似下面的响应︰

        {"status":"ok","version":"v1"}

    在此命令中使用的参数如下所示︰

    * **-u**︰ 用户名和密码用来对请求进行身份验证
    * **-G**︰ 指示这是 GET 请求

    开头的 URL， **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**，将为所有请求相同。 路径， **/status**，指示该请求返回 WebHCat (也称为 Templeton) 状态的服务器。

2. 使用下面的代码提交到群集的猪的拉丁文作业︰

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    在此命令中使用的参数如下所示︰

    * **-d**︰ 因为`-G`不使用，则该请求将默认为 POST 方法。 `-d`指定随请求发送的数据值。

        * **user.name**︰ 运行该命令的用户
        * **执行**︰ 猪的拉丁语要执行的语句
        * **statusdir**︰ 此作业的状态将被写入到的目录

    > [AZURE.NOTE] 请注意，猪的拉丁语语句中的空格替换为`+`字符时使用卷曲。

    此命令应返回可用于例如检查作业的状态的作业 ID:

        {"id":"job_1415651640909_0026"}

3. 若要检查作业的状态，请使用下面的命令。 返回上一步中的值替换**作业 ID** 。 例如，如果返回值为`{"id":"job_1415651640909_0026"}`，然后将**作业 ID** `job_1415651640909_0026`。

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    如果作业完成后，状态将会**成功**。

    > [AZURE.NOTE] 此 Curl 请求返回的 JavaScript 对象符号 (JSON) 文档与工作有关的信息和 jq 用于检索的状态值。

##<a id="results"></a>查看结果

当作业的状态更改为**成功**时，则可以从 Azure Blob 存储检索作业的结果。 `statusdir`查询与传递的参数包含位置的输出文件中。在这种情况下， **wasbs: / 示例/pigcurl/**。 此地址存储在 HDInsight 群集使用的默认存储容器的**示例/pigcurl**目录中的作业的输出。

您可以列出并使用[Azure CLI](../xplat-cli-install.md)下载这些文件。 例如，列表中的文件**示例/pigcurl**，使用下面的命令︰

    azure storage blob list <container-name> example/pigcurl

要下载一个文件，请使用以下方法︰

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] 您必须指定使用包含 blob 存储帐户名称`-a`和`-k`参数或一组**AZURE\_存储\_帐户**和**AZURE\_存储\_访问\_键**环境变量。

##<a id="summary"></a>摘要

本文档中所示，可以使用原始的 HTTP 请求来运行，监视，请查看 HDInsight 群集上的小猪作业的结果。

有关本文中所使用的 REST 接口的详细信息，请参阅[WebHCat 参考](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 小猪的一般信息︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)
