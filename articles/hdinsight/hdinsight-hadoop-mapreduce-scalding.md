<properties
 pageTitle="开发使用 Maven Scalding MapReduce 作业 |Microsoft Azure"
 description="了解如何使用 Maven 创建 Scalding MapReduce 作业，然后部署并在 Hadoop HDInsight 群集上运行作业。"
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>开发在 HDInsight 上的 Apache Hadoop Scalding MapReduce 作业

Scalding 是一个 Scala 库，可以轻松地创建 Hadoop MapReduce 作业。 它提供了简洁的语法，以及使用 Scala 的紧密集成。

在本文中，学习如何使用 Maven 可以创建基本的单词计数 MapReduce 作业用 Scalding 编写的。 然后，您将学习如何部署和 HDInsight 群集上运行此作业。

## <a name="prerequisites"></a>系统必备组件

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
* **Windows 或 Linux 基于 Hadoop HDInsight 群集上**。 有关更多信息，请参见[HDInsight 上的提供 Linux 基于 Hadoop](hdinsight-hadoop-provision-linux-clusters.md)或[上 HDInsight 基于提供 Windows 的 Hadoop](hdinsight-provision-clusters.md) 。

* **[Maven](http://maven.apache.org/)**

* **[Java 平台 JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 或更高版本**

## <a name="create-and-build-the-project"></a>创建和生成项目

1. 使用下面的命令创建一个新的 Maven 项目︰

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    此命令将创建名为**scaldingwordcount**，一个新的目录并创建 Scala 应用程序的基架。

2. 在**scaldingwordcount**目录中，打开**pom.xml**文件并替换为以下内容︰

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    该文件介绍了项目、 相关性和插件。 这里是重要的项︰

    * **maven.compiler.source**和**maven.compiler.target**︰ 设置此项目的 Java 版本

    * **存储库**︰ 包含此项目使用的依赖项文件存储库

    * **scalding core_2.11**和**hadoop 核心**︰ 此项目取决于 Scalding 和 Hadoop 核心包

    * **scala 的 maven 插件**︰ 插件来编译 scala 应用程序

    * **底纹的 maven 插件**︰ 插件来创建阴影 (fat) 的 jar。 此插件应用筛选器和转换;specificially:

        * **滤镜**︰ 应用的筛选器修改附带的 jar 文件中的元信息。 若要防止签名在运行时的异常，这排除各种可能会包含依赖项的签名文件。

        * **执行**︰ 包阶段执行配置指定**com.twitter.scalding.Tool**类为包的主类。 没有它，您将需要指定 com.twitter.scalding.Tool，以及包含应用程序逻辑，使用 hadoop 命令运行该作业时的类。

3. 删除的**src/测试**目录，因为您将无法创建测试此示例。

4. 打开**src/main/scala/com/microsoft/example/App.scala**文件并替换为以下内容︰

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    这将实现基本的单词计数作业。

5. 保存并关闭该文件。

6. 使用**scaldingwordcount**目录中的以下命令来构建和打包的应用程序︰

        mvn package

    此作业完成后，可以在**target/scaldingwordcount-1.0-SNAPSHOT.jar**找到包含的字数统计应用程序的包。

## <a name="run-the-job-on-a-linux-based-cluster"></a>在基于 Linux 的群集上运行作业

> [AZURE.NOTE] 下列步骤将使用 SSH 和 Hadoop 命令。 运行 MapReduce 作业的其他方法，请参阅[使用 MapReduce Hadoop 在 HDInsight 中](hdinsight-use-mapreduce.md)。

1. 使用下面的命令以将包上传到 HDInsight 群集︰

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    这从本地系统中复制文件到 head 节点。

    > [AZURE.NOTE] 如果您使用密码来保护您的 SSH 帐户，系统将提示您输入密码。 如果您使用 SSH 密钥，您可能需要使用`-i`参数和专用密钥的路径。 例如，`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. 使用下面的命令以连接到群集的头节点︰

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] 如果您使用密码来保护您的 SSH 帐户，系统将提示您输入密码。 如果您使用 SSH 密钥，您可能需要使用`-i`参数和专用密钥的路径。 例如，`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. 一旦连接到 head 节点，请使用下面的命令运行 word count 作业

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    这将运行 WordCount 类更早实现。 `--hdfs`指示要使用 HDFS 的作业。 `--input`指定的输入的文本文件，而`--output`指定的输出位置。

4. 作业完成之后，使用以下查看输出。

        hdfs dfs -text wasbs:///example/wordcountout/*

    这将显示类似于以下的信息︰

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>在基于 Windows 的群集上运行作业

下列步骤将使用 Windows PowerShell。 运行 MapReduce 作业的其他方法，请参阅[使用 MapReduce Hadoop 在 HDInsight 中](hdinsight-use-mapreduce.md)。

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. 到 Azure 帐户启动 Azure PowerShell 和日志中。 在让您的凭据后，该命令将返回有关您的帐户信息。

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. 如果您有多个订阅，提供您想要用于部署的订阅 id。

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] 您可以使用`Get-AzureRMSubscription`以获得与您的帐户，其中包括为每个订阅 Id 关联的所有订阅的列表。

4. 使用下面的脚本可以上载和运行 WordCount 作业。 更换`CLUSTERNAME`的名称您的 HDInsight 群集，并确保`$fileToUpload`是__scaldingwordcount-1.0-SNAPSHOT.jar__文件的正确路径。

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     运行该脚本时，系统会提示您输入 HDInsight 群集的管理员用户名和密码。 作业正在运行时出现的任何错误将被记录到控制台。
     
6. 完成作业后，输出将下载到文件__output.txt__当前目录中。 使用下面的命令来显示结果。

        cat output.txt

    该文件应包含类似下面的值︰

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>下一步行动

现在，您学习了如何使用 Scalding 来创建 HDInsight MapReduce 作业，使用下面的链接继续探讨其他方法以使用 Azure HDInsight。

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)

* [使用 HDInsight 的小猪](hdinsight-use-pig.md)

* [HDInsight 使用 MapReduce 作业](hdinsight-use-mapreduce.md)
