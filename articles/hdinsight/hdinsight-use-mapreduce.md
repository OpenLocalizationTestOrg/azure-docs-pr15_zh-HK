<properties
   pageTitle="MapReduce 与 Hadoop 在 HDInsight |Microsoft Azure"
   description="了解如何在 HDInsight 群集在 Hadoop 运行 MapReduce 作业。 您将运行实现为一个 Java MapReduce 作业基本单词计数操作。"
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

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>在 HDInsight 上的 Hadoop 使用 MapReduce

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

在本文中，您将学习如何在 HDInsight 群集在 Hadoop 运行 MapReduce 作业。 我们运行基本的单词计数操作实现为一个 Java MapReduce 作业。

##<a id="whatis"></a>MapReduce 是什么？

Hadoop MapReduce 是一个软件框架，编写处理大量数据的作业。 输入的数据拆分成独立的块，然后并行处理跨您的群集中的节点。 MapReduce 作业包括两个功能︰

* **映射器**︰ 使用输入的数据、 进行分析 （通常使用筛选器和排序操作），并发出元 （键-值对）
* **变径**︰ 使用元组映射器发出和执行映射数据创建一个较小，组合结果摘要操作

下图说明了一个基本的单词计数 MapReduce 作业示例︰

![HDI。WordCountDiagram][image-hdi-wordcountdiagram]

此作业的输出是每个单词出现在文本所分析的次数的计数。

* 在映射程序接受输入的文本作为输入的每一行并将其分成词。 它发出一个键/值对一个单词出现该单词的每个时间跟 1。 将输出发送到变径先进行排序。

* 变径总和两项单独的每个单词计数并发出包含跟其具体值的总和的单词的单个键/值对。

MapReduce 可以实现多种语言。 Java 是最常见的实现，并用于在此文档中的演示。

### <a name="hadoop-streaming"></a>Hadoop 流

语言或框架基于 Java 和 Java 虚拟机 （例如，Scalding 或级联，） 可以直接作为 MapReduce 作业，类似于 Java 应用程序已运行。 其他人，如 C# 或 Python 或独立可执行文件，必须使用 Hadoop 流。

Hadoop 流通过标准输入和标准输出的通信映射器和变径的映射器和变径从 STDIN，一次读取一行数据并将输出写到标准输出。 读取或发出的映射器和变径的每一行必须由选项卡 charaacter 分隔的键/值对的格式︰

    [key]/t[value]

有关详细信息，请参阅[Hadoop 流](http://hadoop.apache.org/docs/r1.2.1/streaming.html)。

使用 Hadoop 流与 HDInsight 的示例，请参见以下资源︰

* [开发 Python MapReduce 作业](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>有关示例数据

在此示例中，对于示例数据，您将使用它作为文本文档 HDInsight 群集中提供了列奥纳多达芬奇，笔记本。

示例数据存储在 Azure Blob 存储，HDInsight 对 Hadoop 群集使用与默认文件系统中。 HDInsight 可以访问存储 Blob 存储中使用的**wasb**前缀的文件。 例如，若要访问的 sample.log 文件，将使用以下语法︰

    wasbs:///example/data/gutenberg/davinci.txt

由于 Azure Blob 存储是 HDInsight 的默认存储区，还可以使用**/example/data/gutenberg/davinci.txt**来访问该文件。

> [AZURE.NOTE] 在前面的语法中， **wasbs: / /**用来访问 HDInsight 群集的默认存储容器中存储的文件。 如果您指定附加的存储帐户设置您的群集，并且想要访问这些帐户中存储的文件时，您可以通过指定容器名称和存储帐户的地址来访问数据。 例如， **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**。

##<a id="job"></a>有关示例 MapReduce

在此示例中使用 MapReduce 作业位于**wasbs://example/jars/hadoop-mapreduce-examples.jar**，并为它提供 HDInsight 群集。 此文件中包含的单词计数示例，您将运行**davinci.txt**。

> [AZURE.NOTE] 在 HDInsight 2.1 群集文件位置是**wasbs:///example/jars/hadoop-examples.jar**。

为了便于参考，下面是单词计数 MapReduce 作业的 Java 代码︰

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

编写您自己的 MapReduce 作业的说明，请参阅[HDInsight 的开发 Java MapReduce 程序](hdinsight-develop-deploy-java-mapreduce-linux.md)。

##<a id="run"></a>运行 MapReduce

HDInsight 可以通过多种方法运行 HiveQL 作业。 使用下表来确定哪种方法最适合您，再按照演练的链接。

| **使用此选项**...                                                    | **.以执行此操作**                                       | ..厎此**群集操作系统** | ...from 此**客户端操作系统** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | 使用 Hadoop 命令通过**SSH**                  | Linux                                     | Linux 和 Unix、 Mac OS X 和 Windows        |
| [卷曲](hdinsight-hadoop-use-mapreduce-curl.md)                     | 通过使用**其他**远程提交作业               | Linux 或窗口                          | Linux 和 Unix、 Mac OS X 和 Windows        |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | 通过使用**Windows PowerShell**远程提交作业 | Linux 或窗口                          | 窗口                                  |
| [远程桌面](hdinsight-hadoop-use-mapreduce-remote-desktop)    | 使用 Hadoop 命令通过**远程桌面**       | 窗口                                   | 窗口                                  |

##<a id="nextsteps"></a>下一步行动

虽然 MapReduce 提供了非常强大的诊断功能，它可以是有点挑战性为主。 有几个基于 Java 的框架，这些框架更容易地定义 MapReduce 应用程序，以及技术，如猪和配置单元时，提供了在 HDInsight 中使用数据的简单方法。 若要了解详细信息，请参阅下列文章︰

* [HDInsight 为开发 Java MapReduce 程序](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Python 的 HDInsight 流 MapReduce 程序开发](hdinsight-hadoop-streaming-python.md)

* [开发在 HDInsight 上的 Apache Hadoop Scalding MapReduce 作业](hdinsight-hadoop-mapreduce-scalding.md)

* [使用 HDInsight 配置单元][hdinsight-use-hive]

* [使用 HDInsight 的小猪][hdinsight-use-pig]

* [运行 HDInsight 示例][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
