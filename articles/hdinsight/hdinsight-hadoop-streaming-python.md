<properties
   pageTitle="开发 Python MapReduce 作业的 HDInsight |Microsoft Azure"
   description="了解如何创建和运行基于 Linux 的 HDInsight 群集上的 Python MapReduce 作业。"
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

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Python 的 HDInsight 流程序开发

Hadoop 提供使您能够编写地图和减少非 Java 语言的函数的 MapReduce 的流式 API。 在本文中，您将学习如何使用 Python 执行 MapReduce 操作。

> [AZURE.NOTE] 虽然本文档中的 Python 代码可以使用基于 Windows 的 HDInsight 群集，此文档中的步骤适用于基于 Linux 的群集。

本文基于信息和示例由迈克尔 • Noll 在[编写 Python 中的 Hadoop MapReduce 程序](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)发布。

##<a name="prerequisites"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* 在 HDInsight 群集上基于 Linux 的 Hadoop

* 文本编辑器

    > [AZURE.IMPORTANT] 文本编辑器必须使用换行符作为行尾。 如果它使用 CRLF，这会导致错误，对基于 Linux 的 HDInsight 群集运行 MapReduce 作业时。 如果您不能确定，使用在[运行 MapReduce](#run-mapreduce)部分可选步骤将任何 CRLF 转换为换行符。

* 对于 Windows 客户端，PuTTY 和 PSCP。 这些实用程序并<a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">PuTTY 下载页</a>中提供。

##<a name="word-count"></a>字数统计

对于此示例，您将使用映射器和变径实现基本的单词计数。 映射器换成各个单词，句子，变径聚合词并计算生成的输出。

下面的流程图展示了什么映射期间发生的情况，并减少阶段。

![图图的减少](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>为什么 Python 吗？

Python 是代码的一通用、 高级的编程语言允许您明确概念比许多其他语言更少行。 它具有最近成为流行与数据科学家，作为一种原型设计语言由于其解释的性质、 动态类型化，和优雅的语法进行适合于快速应用程序开发。

Python 被安装在 HDInsight 的所有群集。

##<a name="streaming-mapreduce"></a>MapReduce 流

Hadoop 可以指定包含映射的文件，并减少作业所使用的逻辑。 映射的具体要求，并减少逻辑是︰

* **输入**︰ 地图和减少组件必须从 STDIN 中读取输入的数据。

* **输出**︰ 地图和减少组件必须将输出数据写入到标准输出。

* **数据格式**︰ 消耗和产生的数据必须用制表符分隔的键/值对。

通过使用**sys**模块从 STDIN 中读取并使用**打印**来打印到标准输出，Python 可以轻松地处理这些要求。 剩下的任务只格式的数据，并用一个选项卡 (`\t`) 字符之间的键和值。

##<a name="create-the-mapper-and-reducer"></a>创建映射器和变径

映射器和变径是文本文件，此案例**mapper.py**和**reducer.py**中 （以使它清除它在做什么）。 您还可以通过使用您选择的编辑器。

###<a name="mapperpy"></a>Mapper.py

创建一个名为**mapper.py**的新文件，并使用下面的代码作为内容︰

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

花点时间通读代码，以便您可以了解它的作用。

###<a name="reducerpy"></a>Reducer.py

创建一个名为**reducer.py**的新文件，并使用下面的代码作为内容︰

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>上载的文件

**Mapper.py**和**reducer.py**必须在群集的头节点上才能运行它们。 将它们上载的最简单方法是使用**scp** (为**pscp**如果您使用的 Windows 客户端)。

从客户端，在**mapper.py**和**reducer.py**，相同的目录中使用下面的命令。 **用户名**替换一个 SSH 用户和**群集名称**的群集名称。

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

这从本地系统中复制文件到 head 节点。

> [AZURE.NOTE] 如果您使用密码来保护您的 SSH 帐户，系统将提示您输入密码。 如果您使用 SSH 密钥，您可能需要使用`-i`参数和私有密钥，例如，通往`scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`。

##<a name="run-mapreduce"></a>运行 MapReduce

1. 通过使用 SSH 连接到群集︰

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] 如果您使用密码来保护您的 SSH 帐户，系统将提示您输入密码。 如果您使用 SSH 密钥，您可能需要使用`-i`参数和私有密钥，例如，通往`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`。

2. （可选）如果您使用的文本编辑器来 CRLF 用作行尾时创建的 mapper.py 和 reducer.py 的文件，或不知道什么行尾编辑器使用，使用下面的，命令将出现的 CRLF mapper.py 和 reducer.py 中转换为换行符。

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. 使用下面的命令启动 MapReduce 作业。

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    此命令具有以下几个部分︰

    * **hadoop streaming.jar**︰ 执行流式 MapReduce 操作时使用。 它可以与您提供的外部 MapReduce 代码接口 Hadoop。

    * **-文件**︰ 告诉 Hadoop 指定的文件所需的此 MapReduce 作业，并应将它们复制到工作人员的所有节点。

    * **-映射器**︰ 告诉 Hadoop 用作映射程序的文件。

    * **-变径**︰ 告诉 Hadoop 哪些文件以用作变径。

    * **-输入**︰ 从词，我们应该计算的输入的文件。

    * **的输出**︰ 输出将被写入该目录。

        > [AZURE.NOTE] 由该作业将创建此目录。

应该看到在作业开始时，大量的**信息**语句并最后看到的**地图**和**减少**操作显示为百分比。

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

当它完成时，您将收到有关该作业的状态信息。

##<a name="view-the-output"></a>查看输出

在作业完成时，请使用下面的命令以查看输出︰

    hdfs dfs -text /example/wordcountout/part-00000

此时应显示列表中的单词和单词的次数出现。 下面是输出数据的示例︰

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>下一步行动

现在，您学习了如何使用 HDInsight 流 MapRedcue 作业，使用下面的链接继续探讨其他方法以使用 Azure HDInsight。

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [使用 HDInsight 的小猪](hdinsight-use-pig.md)
* [HDInsight 使用 MapReduce 作业](hdinsight-use-mapreduce.md)
