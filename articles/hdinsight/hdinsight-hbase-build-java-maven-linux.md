<properties
    pageTitle="构建一个 HBase 应用使用 Maven 和 Java，然后将部署到基于 Linux 的 HDInsight |Microsoft Azure"
    description="了解如何使用 Apache Maven 构建一个基于 Java 的 Apache HBase 的应用程序，然后将其部署到云中的 Azure 的基于 Linux 的 HDInsight。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>使用 Maven 构建 Java 应用程序使用 HBase 基于 Linux 的 HDInsight (Hadoop)

了解如何创建和使用 Apache Maven 构建[Apache HBase](http://hbase.apache.org/)中 Java 应用程序。 然后基于 Linux 的 HDInsight 群集使用该应用程序。

[Maven](http://maven.apache.org/)是软件项目管理和理解的工具，使您可以生成软件、 文档和报表的 Java 项目。 在本文中，您将学习如何使用它来创建一个基本的 Java 应用程序的创建，查询，并在基于 Linux 的 HDInsight 群集 HBase 表中删除。

> [AZURE.NOTE] 本文档中的步骤假定您正在使用基于 Linux 的 HDInsight 群集。 使用基于 Windows 的 HDInsight 群集的信息，请参阅[使用 Maven 构建 Java 应用程序使用 HBase 与基于 Windows 的 HDInsight](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>要求

* [Java 平台 JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 或更高版本

* [Maven](http://maven.apache.org/)

* [使用 HBase 基于 Linux 的 Azure HDInsight 群集](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] 使用 HDInsight 群集版本 3.2 和 3.3，3.4 测试过此文档中的步骤。 在示例中提供的默认值是 HDInsight 3.4 群集。

* **SSH 和 SCP 的熟悉**。 在 HDInsight 中使用 SSH 和 SCP 的详细信息，请参阅以下资源︰

    * **Linux、 Unix 或 OS X 客户机**︰ 请[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 或 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows 客户端**︰ 请[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>创建项目

1. 从命令行开发环境中，将目录更改为您想要创建的项目，例如，位置`cd code/hdinsight`。

2. 安装使用 Maven， __mvn__命令用于生成项目的基架。

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    这创建在当前目录中，由__artifactID__参数 (**hbaseapp**在本例。) 指定的名称命名新的目录该目录将包含以下各项︰

    * __pom.xml__: 项目对象模型 ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) 包含用于生成项目的信息和配置详细信息。

    * __src__︰ 包含__main/java/com/microsoft/示例__目录中，并将在此创作应用程序的目录。

3. 因为它不会在此示例中使用的__src/test/java/com/microsoft/examples/apptest.java__文件，请将其删除。

##<a name="update-the-project-object-model"></a>更新的项目对象模型

1. 编辑__pom.xml__文件并添加下面的代码在`<dependencies>`部分中︰

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    这会告诉 Maven 项目需要__hbase 客户端__版本__1.1.2__。 在编译时，这会将下载默认 Maven 存储库中。 可以使用[Maven 中央存储库中搜索](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar)要了解有关此依赖项的详细信息。

    > [AZURE.IMPORTANT] 版本号必须与 HBase HDInsight 群集中提供的版本相匹配。 使用下表来查找正确的版本号。

  	| HDInsight 群集版本 | 使用 HBase 版本 |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 和 3.4 | 1.1.2 |

    HDInsight 版本和组件的详细信息，请参阅[什么是不同的 Hadoop 组件可用于 HDInsight](hdinsight-component-versioning.md)。

2. 如果您使用的 HDInsight 3.3 或 3.4 群集，您还必须添加到以下`<dependencies>`部分中︰

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    这将加载菲尼克斯核心组件，它们需要使用 Hbase 版本 1.1.x。

2. 将以下代码添加到__pom.xml__文件中。 这必须是在`<project>...</project>`文件中的标记之间，如`</dependencies>`和`</project>`。

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                <source>1.7</source>
                <target>1.7</target>
              </configuration>
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

    这将配置资源 (__conf/hbase-site.xml__)，包含 HBase 的配置信息。

    > [AZURE.NOTE] 您还可以设置通过代码的配置值。 请参阅下面有关如何执行此操作的__CreateTable__示例中的注释。

    这还将配置的[Maven 编译器插件](http://maven.apache.org/plugins/maven-compiler-plugin/)和[阴影的 Maven 插件](http://maven.apache.org/plugins/maven-shade-plugin/)。 编译器插件用于编译拓扑结构。 插件底纹用于防止许可证重复生成的 Maven 的 JAR 包中。 使用此选项的原因是重复的许可证文件，导致 HDInsight 群集上运行时错误。 使用 maven 底纹-插件与`ApacheLicenseResourceTransformer`实现防止此错误。

    Maven 底纹插件也会产生 uber jar （或 fat jar，），其中包含由应用程序所需的所有依赖项。

3. 保存__pom.xml__文件。

4. 创建一个名为__conf__的__hbaseapp__目录中的新目录。 这将用来容纳 HBase 连接的配置信息。

5. 使用下面的命令将从 HDInsight 服务器 HBase 配置复制到__conf__目录。 更换**用户名**的 SSH 登录名称。 **群集名称**替换您的 HDInsight 群集名称︰

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] 如果您使用 SSH 帐户的密码，将提示您输入密码。 如果您使用的帐户使用 SSH 密钥，您可能需要使用`-i`参数指定的密钥文件的路径。 下面的示例将加载私钥从`~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>创建应用程序

1. 转到__hbaseapp/src/主/java/com/microsoft/示例__目录并将 app.java 文件重命名为__CreateTable.java__。

2. 打开__CreateTable.java__文件，然后用以下内容替换现有的内容︰

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    这是__CreateTable__类，这将创建一个名为__人__表并填充某些预定义的用户。

3. 将__CreateTable.java__文件保存。

4. 在__hbaseapp/src/主/java/com/microsoft/示例__目录中，创建一个名为__SearchByEmail.java__的新文件。 使用以下内容作为本文件的内容︰

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    可以使用__SearchByEmail__类查询的行按电子邮件地址。 因为它使用正则表达式筛选器，您可以使用类时提供的字符串或正则表达式。

5. 将__SearchByEmail.java__文件保存。

6. 在__hbaseapp/src/主/hava/com/microsoft/示例__目录中，创建一个名为__DeleteTable.java__的新文件。 使用以下内容作为本文件的内容︰

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    此类是用于清理此示例通过创建__CreateTable__类的表禁用拖放。

7. 将__DeleteTable.java__文件保存。

##<a name="build-and-package-the-application"></a>构建和打包的应用程序

2. 从__hbaseapp__目录中，使用以下命令生成 JAR 文件包含应用程序︰

        mvn clean package

    这清除任何以前的生成项目、 下载尚未安装的任何依赖项，然后将生成并打包应用程序。

3. 在此命令完成后， __hbaseapp/目标__目录将包含一个名为__hbaseapp-1.0-SNAPSHOT.jar__文件。

    > [AZURE.NOTE] __Hbaseapp-1.0-SNAPSHOT.jar__文件是 uber （有时称为 fat 的 jar，） 它包含运行应用程序所需的所有依赖关系的 jar。

##<a name="upload-the-jar-file-and-run-jobs"></a>将 JAR 文件上载和运行作业

1. 使用以下方法来上传到 HDInsight 群集罐。 更换**用户名**的 SSH 登录名称。 **群集名称**替换您的 HDInsight 群集名称︰

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    这将文件上载到您的 SSH 用户帐户的主目录。

    > [AZURE.NOTE] 如果您使用 SSH 帐户的密码，将提示您输入密码。 如果您使用的帐户使用 SSH 密钥，您可能需要使用`-i`参数指定的密钥文件的路径。 下面的示例将加载私钥从`~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. 使用 SSH 连接到 HDInsight 群集。 更换**用户名**的 SSH 登录名称。 **群集名称**替换您的 HDInsight 群集名称︰

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] 如果您使用 SSH 帐户的密码，将提示您输入密码。 如果您使用的帐户使用 SSH 密钥，您可能需要使用`-i`参数指定的密钥文件的路径。 下面的示例将加载私钥从`~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. 建立连接后，使用以下方法来创建新的 Java 应用程序使用 HBase 表︰

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    这将创建一个新的 HBase 表相似的__人__，和用数据填充它。

4. 接下来，使用下面的电子邮件地址存储在表中搜索︰

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    您应该收到以下结果︰

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>删除表

完成示例是，使用 Azure PowerShell 会话中的以下命令来删除在此示例中使用的__人__表︰

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

