<properties
   pageTitle="在 HDInsight 中使用 Hadoop 猪 |Microsoft Azure"
   description="了解如何使用 Hadoop HDInsight 上的小猪。"
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>使用 Hadoop HDInsight 上的小猪

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache 猪](http://pig.apache.org/)是程序的 Hadoop 使用创建的过程称为*猪的拉丁*语言的一个平台。 猪是 Java 的替代方法，用于创建*MapReduce*解决方案，并且包括 Azure HDInsight。

在本文中，您将学习如何使用 HDInsight 中的小猪。

##<a id="why"></a>为什么使用猪？

在 Hadoop 使用 MapReduce 处理数据的挑战之一通过使用仅映射和归约函数实现处理逻辑。 对于复杂的处理，通常需要处理分成多个链接的 MapReduce 操作组合在一起以实现所期望的结果。

而不是强迫您使用只映射和减少函数，猪可以定义为一系列的数据流动来产生所需的输出的转换处理。

猪的拉丁语言可以描述数据从原始的输入，通过一个或多个变换，以产生所需的输出流。 猪的拉丁语程序遵循这个普通模式︰

- **负载**︰ 读取数据从文件系统进行操作
- **转换**︰ 操作数据
- **转储或存储区**︰ 数据输出到屏幕或将其存储为处理

猪的拉丁语还支持用户定义函数 (UDF)，它允许您调用外部组件的实现逻辑很难对猪的拉丁语中的模型。

猪的拉丁语有关的详细信息，请参阅[猪的拉丁语参考手册，第 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html)和[猪的拉丁语参考手册 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html)。

与小猪一起使用 Udf 的示例，请参见以下文档︰

* [与猪的 HDInsight 中使用 DataFu](hdinsight-hadoop-use-pig-datafu-udf.md)的 DataFu 是一套有用的 Udf Apache 通过维护

* [使用 Python 猪和 HDInsight 中的配置单元](hdinsight-python.md)

* [使用 C# 配置单元和 HDInsight 中的小猪](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>有关示例数据

此示例使用*log4j*示例文件，它们存储在**/example/data/sample.log**到 blob 存储容器中。 包含行字段的每个日志文件中的包含`[LOG LEVEL]`字段，以显示的类型和严重程度，例如︰

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

在前面的示例中，日志级别是错误。

> [AZURE.NOTE] 可以使用[Apache Log4j](http://en.wikipedia.org/wiki/Log4j)日志记录工具还生成 log4j 文件，然后将该文件上载到您的 blob。 说明，请参阅[将数据上载到 HDInsight](hdinsight-upload-data.md) 。 有关在 Azure 存储中的 blob 与 HDInsight 的使用方式的详细信息，请参阅[使用 Azure Blob 存储与 HDInsight](hdinsight-hadoop-use-blob-storage.md)。

示例数据存储在 Azure Blob 存储，HDInsight 对 Hadoop 群集使用与默认文件系统中。 HDInsight 可以访问存储 blob 中使用的**wasb**前缀的文件。 例如，若要访问的 sample.log 文件，将使用以下语法︰

    wasbs:///example/data/sample.log

因为 WASB 是 HDInsight 的默认存储区，还可以通过使用**/example/data/sample.log**从猪的拉丁语来访问文件。

> [AZURE.NOTE] 该语法， **wasbs: / /**，用来访问 HDInsight 群集的默认存储容器中存储的文件。 如果您指定附加的存储帐户设置您的群集，并且想要访问这些帐户中存储的文件时，您可以访问数据，通过指定容器名称和存储帐户地址，例如︰ **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**。


##<a id="job"></a>有关示例作业

下面的猪的拉丁文作业从 HDInsight 群集的默认存储区中加载**sample.log**文件。 然后执行一系列的转换，导致输入数据中的每个日志级别发生的次数的计数。 结果转储到标准输出。

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

下图显示每个转换的数据的用途的分类。

![图形化表示形式转换][image-hdi-pig-data-transformation]

##<a id="run"></a>猪的拉丁文作业运行

HDInsight 可以通过多种方法运行猪的拉丁文作业。 使用下表来确定哪种方法最适合您，再按照演练的链接。

| 如果需要，请**使用此选项**...                                   | ...an**交互式**外壳程序 | ...**批处理**操作 | ..厎此**群集操作系统** | ...from 此**客户端操作系统** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux 和 Unix、 Mac OS X 和 Windows        |
| [卷曲](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux 或窗口                          | Linux 和 Unix、 Mac OS X 和 Windows        |
| [Hadoop 的.NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux 或窗口                          | （现在） 窗口                        |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux 或窗口                          | 窗口                                  |
| [远程桌面](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | 窗口                                   | 窗口                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>使用内部部署 SQL Server Integration Services Azure HDInsight 上运行猪的作业

您还可以使用 SQL Server Integration Services (SSI) 运行猪的作业。 SSIS Azure 功能包提供了以下组件，在 HDInsight 上使用猪的作业。


- [Azure HDInsight 猪的任务][pigtask]
- [Azure 订阅连接管理器][connectionmanager]


了解更多有关 Azure 功能包为 SSIS[这里][ssispack]。


##<a id="nextsteps"></a>下一步行动

现在，您学习了如何使用 HDInsight 的小猪，使用下面的链接继续探讨其他方法以使用 Azure HDInsight。

* [将数据上传到 HDInsight][hdinsight-upload-data]
* [使用 HDInsight 配置单元][hdinsight-use-hive]
* [使用 HDInsight Sqoop](hdinsight-use-sqoop.md)
* [HDInsight 使用 Oozie](hdinsight-use-oozie.md)
* [HDInsight 使用 MapReduce 作业][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
