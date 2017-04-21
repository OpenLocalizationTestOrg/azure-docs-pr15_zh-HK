<properties
   pageTitle="在 HDInsight 群集上使用 SSH 使用 Hadoop 猪 |Microsoft Azure"
   description="了解如何连接到基于 Linux 的 Hadoop 群集使用 SSH，，然后使用猪的命令，以猪的拉丁语语句交互方式运行，或作为批处理作业。"
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>在用猪的命令 (SSH) 基于 Linux 的群集上运行猪的作业

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

在本文中您将演练连接到基于 Linux 的 Azure HDInsight 群集使用安全外壳协议 (SSH)，然后使用猪的命令猪的拉丁语语句交互方式运行，或作为批处理作业的过程。

猪的拉丁语编程语言可以描述应用于输入数据，以产生所需的输出的转换。

> [AZURE.NOTE] 如果您已熟悉使用 Hadoop 的基于 Linux 的服务器，但 HDInsight 的新，请参阅[基于 Linux 的 HDInsight 提示](hdinsight-hadoop-linux-information.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要。

* 基于 linux * 的 HDInsight (在 HDInsight 上的 Hadoop) 群集。

* SSH 客户端。 Linux 和 Unix 中，Mac OS 应附带的 SSH 客户端。 Windows 用户必须下载客户端，如[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

##<a id="ssh"></a>用 SSH 连接

通过连接到的完全合格的域名称 (FQDN) 的 HDInsight 群集使用 SSH 命令。 FQDN 将使用的名称的群集，然后**。 azurehdinsight.net**。 例如，以下将连接到名为**myhdinsight**的群集。

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**如果您提供 SSH 身份验证的证书密钥**的创建 HDInsight 群集时，您可能需要指定客户机系统上的私有密钥的位置。

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**如果您提供的密码 SSH 身份验证**创建 HDInsight 群集时，您将需要提供密码提示时。

在 HDInsight 中使用 SSH 的详细信息，请参阅[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 和 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)。

###<a name="putty-windows-based-clients"></a>PuTTY （基于 Windows 的客户端）

Windows 不提供内置的 SSH 客户端。 我们建议使用**PuTTY**，这可以从[http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)下载。

使用 PuTTY 的详细信息，请参阅[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)。

##<a id="pig"></a>使用猪的命令

2. 建立连接后，使用下面的命令启动猪的命令行界面 (CLI)。

        pig

    稍后，您会看到`grunt>`提示。

3. 输入以下语句。

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    此命令将 sample.log 文件的内容加载到日志中。 可以通过使用以下查看该文件的内容。

        DUMP LOGS;

4. 接下来，应用正则表达式通过使用以下每个记录中提取的日志记录级别来转换数据。

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    您可以使用**转储**查看转换后的数据。 在这种情况下，使用`DUMP LEVELS;`。

5. 继续使用以下语句应用转换。 使用`DUMP`查看转换后的每个步骤的结果。

    <table>
    <tr>
    <th>语句</th><th>它的作用</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = 筛选器级别的日志级别不为空;</td><td>删除行中包含 null 值的日志级别，并将结果存储到 FILTEREDLEVELS。</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = 组 FILTEREDLEVELS 的日志级别;</td><td>日志级别的行进行分组，并将结果存储到 GROUPEDLEVELS。</td>
    </tr>
    <tr>
    <td>频率 = foreach GROUPEDLEVELS 生成组为日志级别，计数 (FILTEREDLEVELS。日志级别） 随着计数;</td><td>创建一组新的数据，其中包含每个唯一的日志级别值以及它多少次发生。 这存储到的频率。</td>
    </tr>
    <tr>
    <td>结果 = 顺序计数 desc; 通过频率</td><td>按计数 （降序） 进行排序的日志级别，并存储到结果。</td>
    </tr>
    </table>

6. 您也可以通过使用保存转换的结果`STORE`语句。 例如，下面的保存`RESULT`您的群集的默认存储容器上的**/example/data/pigout**目录。

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] 数据存储在名为**一部分 nnnnn**文件中指定的目录中。 如果该目录已存在，您将收到错误。

7. 若要退出 grunt 提示符处，输入以下语句。

        QUIT;

###<a name="pig-latin-batch-files"></a>猪的拉丁语批处理文件

小猪命令也可用于运行文件中包含的猪的拉丁语。

3. 退出后的 grunt 提示，与管道标准输入下面的命令使用到名为**pigbatch.pig**的文件中。 将在您所登录的 SSH 会话的帐户的主目录中创建此文件。

        cat > ~/pigbatch.pig

4. 键入或粘贴以下行，并将在完成时按 Ctrl + D。

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. 使用以下方法来使用猪的命令运行**pigbatch.pig**文件。

        pig ~/pigbatch.pig

    一旦批处理作业完成时，您应看到下面的输出，这应该是当您使用相同`DUMP RESULT;`在前面的步骤。

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>摘要

如您所见，小猪命令允许您以交互方式使用猪的拉丁语，运行 MapReduce 操作以及运行存储在批处理文件中的语句。

##<a id="nextsteps"></a>下一步行动

猪的 HDInsight 中的一般信息。

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上。

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)
