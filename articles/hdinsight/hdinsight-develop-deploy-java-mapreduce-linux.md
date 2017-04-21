<properties
    pageTitle="为基于 Linux 的 HDInsight 开发 Java MapReduce 程序 |Microsoft Azure"
    description="了解如何开发 Java MapReduce 程序并将它们部署到基于 Linux 的 HDInsight。"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>为 HDInsight Linux 上的 Hadoop 开发 Java MapReduce 程序

本文档将指导您完成使用 Apache Maven 创建 MapReduce 应用程序，然后部署在 HDInsight 群集上基于 Linux 的 Hadoop 运行。

##<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 或更高 （或等同，如 OpenJDK）

- [Apache Maven](http://maven.apache.org/)

- **Azure 的订阅**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>配置环境变量

Java 和 JDK 安装时，可以设置以下环境变量。 但是，您应该检查它们存在，它们包含您系统的正确值。

* **JAVA_HOME** -应指向 Java 运行时环境 (JRE) 的安装目录。 例如，在 OS X、 Unix 或 Linux 系统中，它应具有的值类似于`/usr/lib/jvm/java-7-oracle`。 在 Windows 中，它将具有类似于一个值`c:\Program Files (x86)\Java\jre1.7`

* **路径**-应包含以下路径︰

    * **JAVA_HOME**（或等效的路径）

    * **JAVA_HOME\bin**（或等效的路径）

    * Maven 时的安装目录

##<a name="create-a-new-maven-project"></a>创建一个新的 Maven 项目

1. 在终端会话中或在开发环境中的命令行中，将目录更改到您想要存储该项目的位置。

3. 安装使用 Maven， __mvn__命令用于生成项目的基架。

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    这将在当前目录中，由__artifactID__参数 (**wordcountjava**在本例。) 指定的名称创建一个新目录该目录将包含以下各项︰

    * __pom.xml__ -[项目对象模型 (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)包含用于生成项目的信息和配置详细信息。

    * __src__ -包含__主/java/组织/apache/hadoop/示例__目录中，并将在此创作应用程序的目录。

3. 删除__src/test/java/org/apache/hadoop/examples/apptest.java__文件，因为它不会在此示例中使用。

##<a name="add-dependencies"></a>添加依赖项

1. 编辑__pom.xml__文件并将以下内容添加在`<dependencies>`部分中︰

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    这会告诉 Maven 项目需要库 (内列出&lt;artifactId\>) 与特定版本 (内列出&lt;版本\>)。 在编译时，这会将下载默认 Maven 存储库中。 可以使用[Maven 存储库中搜索](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar)以查看详细信息。

    `<scope>provided</scope>`告诉 Maven 这些依赖项不应打包应用程序时，它们将由 HDInsight 群集在运行时提供。

2. __Pom.xml__文件中添加以下。 这必须是在`<project>...</project>`标记的文件。例如，之间`</dependencies>`， `</project>`。

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    第一个插件配置[Maven 底纹插件](http://maven.apache.org/plugins/maven-shade-plugin/)，用于生成 uberjar （有时称为 fatjar），其中包含所需的应用程序的依赖项。 它还可以防止重复的许可证中的 jar 包，这可能导致在某些系统上的问题。

    第二个插件配置 Maven 编译器，编译器用于设置 Java 使用 HDInsight 群集上的版本与此应用程序所需的版本。

3. 保存__pom.xml__文件。

##<a name="create-the-mapreduce-application"></a>创建 MapReduce 应用程序

1. 转到__wordcountjava/src/主/java/组织/apache/hadoop/示例__目录并将__App.java__文件重命名为__WordCount.java__。

2. 在文本编辑器中打开__WordCount.java__文件并替换为以下内容︰

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

    请注意，程序包名称是**org.apache.hadoop.examples** ，类名是**字数统计**。 当您提交 MapReduce 作业时，您将使用这些名称。

3. 保存该文件。

##<a name="build-the-application"></a>生成应用程序

1. 更改到__wordcountjava__目录中，如果您还不存在。

2. 使用以下命令生成包含该应用程序的 JAR 文件︰

        mvn clean package

    这将清除任何以前的生成项目，下载不已安装，然后生成和打包的应用程序的任何依赖项。

3. 一旦在该命令完成， __wordcountjava/目标__目录将包含一个名为__wordcountjava-1.0-SNAPSHOT.jar__文件。

    > [AZURE.NOTE] __Wordcountjava-1.0-SNAPSHOT.jar__文件是 uberjar，它包含字数统计作业，不仅作业需要在运行时的依赖项。


##<a id="upload"></a>将上载罐

使用下面的命令将 jar 文件上传到 HDInsight headnode:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

这从本地系统中复制文件到 head 节点。

> [AZURE.NOTE] 如果您使用密码来保护您的 SSH 帐户，系统将提示您输入密码。 如果您使用 SSH 密钥，您可能需要使用`-i`参数和专用密钥的路径。 例如， `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`。

##<a name="run"></a>运行 MapReduce 作业

1. 连接到 HDInsight 使用 SSH 在下列文章中所述︰

    - [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 从 SSH 会话，请使用下面的命令运行 MapReduce 应用程序︰

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    这将使用字数统计 MapReduce 应用统计字数在 davinci.txt 文件中，并存储到结果__wasbs: / 示例/数据/wordcountout/__。 输入的文件和输出存储到群集的默认存储区中。

3. 作业完成后，使用以下方法来查看结果︰

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    您应该收到文字和计数的值类似于以下的列表︰

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>下一步行动

在本文中，您学习了如何开发一个 Java MapReduce 作业。 请参阅以下文档以其他方式使用 HDInsight。

- [使用 HDInsight 配置单元][hdinsight-use-hive]
- [使用 HDInsight 的小猪][hdinsight-use-pig]
- [HDInsight 使用 MapReduce](hdinsight-use-mapreduce.md)

有关详细信息，请参见[Java 开发人员中心](https://azure.microsoft.com/develop/java/)。

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

