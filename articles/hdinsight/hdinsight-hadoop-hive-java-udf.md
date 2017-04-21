<properties
pageTitle="使用 HDInsight 中的配置单元的 Java 用户定义函数 (UDF) |Microsoft Azure"
description="了解如何创建和使用 Java 用户定义函数 (UDF)，从 HDInsight 中的配置单元。"
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>使用 Java UDF 与 HDInsight 中的配置单元

配置单元是非常适合于处理数据在 HDInsight，但有时您需要较一般用途语言。 配置单元使您可以创建用户定义的函数 (UDF) 使用不同的编程语言。 在本文中，您将学习如何配置单元中使用 Java UDF。

## <a name="requirements"></a>要求

* Azure 的订阅

* HDInsight 群集 (Windows 或基于 Linux 的)

    > [AZURE.NOTE] 本文档中的大多数步骤适用于这两种群集类型;但是，要将已编译的 UDF 上载到群集并运行所使用的步骤适用于基于 Linux 的群集。 可以使用基于 Windows 群集的信息提供了链接。

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 或更高 （或等同，如 OpenJDK）

* [Apache Maven](http://maven.apache.org/)

* 文本编辑器或 IDE Java

    > [AZURE.IMPORTANT] 如果您正在使用基于 Linux 的 HDInsight 服务器，但创建 Windows 客户端上的 Python 文件，您必须使用编辑器 LF 将用作行尾。 如果不能确定是否您的编辑器使用换行符或 CRLF，请参阅[疑难解答](#troubleshooting)部分中的步骤，删除 HDInsight 群集上使用的实用程序的 CR 字符。

## <a name="create-an-example-udf"></a>创建 UDF 的示例

1. 从命令行，使用以下方法来创建一个新的 Maven 项目︰

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] 如果您正在使用 PowerShell，必须放在引号。 例如， `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`。

    这将创建一个名为__exampleudf__，其中包含 Maven 项目的新目录。

2. 创建项目后，删除已创建的项目; 一部分的__exampleudf/src/测试__目录它不会用于此示例。

3. 打开__exampleudf/pom.xml__中，并替换现有`<dependencies>`具有以下项︰

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    这些条目指定的 Hadoop 并附带 HDInsight 3.3 和 3.4 群集配置单元的版本。 可以找到有关 Hadoop 和 HDInsight [HDInsight 的组件版本控制](hdinsight-component-versioning.md)文档中提供的配置单元的版本信息。

    添加`<build>`节之前`</project>`结尾的文件的行。 这一节应包含下列信息︰

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
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
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    这些条目定义如何生成项目。 具体而言，项目使用的 Java 和如何建立到群集部署的 uberjar 版本。

    一旦进行了更改，则保存该文件。

4. 将__exampleudf/src/main/java/com/microsoft/examples/App.java__重命名为__ExampleUDF.java__，然后在编辑器中打开该文件。

5. __ExampleUDF.java__文件的内容替换为以下内容，然后保存该文件。

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    这将实现 UDF 可接受一个字符串值，并返回字符串的小写版本。

## <a name="build-and-install-the-udf"></a>生成并安装好该 UDF

1. 使用下面的命令来编译和打包 UDF:

        mvn compile package

    这将生成，然后打包到__exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__的 UDF。

2. 使用`scp`命令将该文件复制到 HDInsight 群集。

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    将__myuser__替换为群集的 SSH 用户帐户。 群集名称替换__mycluster__ 。 如果您使用密码来保护 SSH 帐户，系统会提示您输入密码。 如果您使用证书，您可能需要使用`-i`参数指定的私钥文件。

3. 连接到群集使用 SSH。 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    在 HDInsight 中使用 SSH 的详细信息，请参阅以下文档。

    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)

4. 从 SSH 会话，将 jar 文件复制到 HDInsight 的存储。

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>从配置单元使用 UDF

1. 使用以下方法来从 SSH 会话启动 Beeline 客户端。

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    此命令假定为群集的登录帐户使用__管理员__的默认值。

2. 一旦您到达`jdbc:hive2://localhost:10001/>`提示下，输入下列表达式，将 UDF 添加到配置单元并将其公开为一个函数。

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. 使用 UDF 转换为小写字符串表中检索值。

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    这将从表中选择的设备平台 （Android、 Windows、 iOS 等）、 将字符串转换为小写的格式，并显示它们。 输出将显示类似于以下。

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>下一步行动

其他方式处理配置单元，请参阅[使用配置单元与 HDInsight](hdinsight-use-hive.md)。

Hive User-Defined 函数，请参阅[配置单元运算符和用户定义的函数](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF)部分配置单元 wiki 在 apache.org 的详细信息。
