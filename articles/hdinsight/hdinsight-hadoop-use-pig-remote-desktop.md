<properties
   pageTitle="在 HDInsight 中的远程桌面使用 Hadoop 猪 |Microsoft Azure"
   description="了解如何使用猪的命令来运行远程桌面连接中猪的拉丁语语句，HDInsight 中的基于 Windows 的 Hadoop 群集。"
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>运行远程桌面连接中的猪的作业

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

本文档提供使用猪的命令到基于 Windows 的 HDInsight 群集从远程桌面连接运行猪的拉丁语语句的演练。 猪的拉丁语允许您创建 MapReduce 应用程序描述的数据转换，而不是映射并减少函数。

在本文中，了解如何

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要。

* 基于 Windows 的 HDInsight (在 HDInsight 上的 Hadoop) 群集

* 运行 Windows 10、 Windows 8 或 Windows 7 的客户端计算机

##<a id="connect"></a>使用远程桌面连接

启用远程桌面 HDInsight 群集，然后按照下面的说明在[连接到 HDInsight 群集使用 RDP](hdinsight-administer-use-management-portal.md#rdp)连接到它。

##<a id="pig"></a>使用猪的命令

2. 远程桌面连接后，请使用桌面上的图标开始**Hadoop 命令行**。

2. 使用以下方法来启动猪的命令︰

        %pig_home%\bin\pig

    您将看到`grunt>`提示。

3. 输入以下语句︰

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    此命令将 sample.log 文件的内容加载到该日志文件。 可以通过使用以下命令来查看该文件的内容︰

        DUMP LOGS;

4. 通过应用正则表达式提取每个记录的日志记录级别转换数据︰

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    您可以使用**转储**查看转换后的数据。 在这种情况下， `DUMP LEVELS;`。

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
    <td>频率 = foreach GROUPEDLEVELS 生成组为日志级别，计数 (FILTEREDLEVELS。日志级别） 随着计数;</td><td>创建一组新的数据，其中包含每个唯一的日志级别值以及它多少次发生。 这存储到频率</td>
    </tr>
    <tr>
    <td>结果 = 顺序计数 desc; 通过频率</td><td>按计数 （降序） 进行排序的日志级别，并存储到结果</td>
    </tr>
    </table>

6. 您也可以通过使用保存转换的结果`STORE`语句。 例如，下面的命令保存`RESULT`到**/example/data/pigout**目录中您的群集的默认存储容器︰

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] 数据存储在名为**一部分 nnnnn**文件中指定的目录中。 如果该目录已存在，您将收到一条错误消息。

7. 若要退出 grunt 提示符处，输入以下语句。

        QUIT;

###<a name="pig-latin-batch-files"></a>猪的拉丁语批处理文件

小猪命令也可用于运行文件中包含的猪的拉丁语。

3. 在退出 grunt 提示之后, 打开**记事本**并创建一个名为**pigbatch.pig**的**%pig_home%**目录中的新文件。

4. 键入或粘贴到**pigbatch.pig**文件中，以下几行，然后将其保存︰

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. 使用以下方法来运行**pigbatch.pig**文件使用的小猪命令。

        pig %PIG_HOME%\pigbatch.pig

    在批处理作业完成后，您应看到下面的输出，这应该是当使用相同`DUMP RESULT;`在前面的步骤︰

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>摘要

正如您所看到的小猪命令允许您以交互方式运行 MapReduce 操作或运行的批处理文件中存储的猪的拉丁文作业。

##<a id="nextsteps"></a>下一步行动

在 HDInsight 中的小猪有关的一般信息︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)
