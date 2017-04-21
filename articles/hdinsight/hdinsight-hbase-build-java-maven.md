<properties
pageTitle="生成使用 Maven，HBase 应用程序并将其部署到基于 Windows 的 HDInsight |Microsoft Azure"
description="了解如何使用 Apache Maven 构建一个基于 Java 的 Apache HBase 的应用程序，然后将其部署到基于 Windows Azure HDInsight 群集。"
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"
tags="azure-portal"/>

<tags
ms.service="hdinsight"
ms.workload="big-data"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>使用 Maven 构建的 Java 应用程序与基于 Windows 的 HDInsight (Hadoop) 使用 HBase

了解如何创建和使用 Apache Maven 构建[Apache HBase](http://hbase.apache.org/)中 Java 应用程序。 然后使用 Azure HDInsight (Hadoop) 应用程序。

[Maven](http://maven.apache.org/)是软件项目管理和理解的工具，使您可以生成软件、 文档和报表的 Java 项目。 在本文中，您将学习如何使用它来创建一个基本的 Java 应用程序中，创建查询和删除在 Azure HDInsight 群集 HBase 表。

> [AZURE.NOTE] 本文档中的步骤假定您使用的基于 Windows HDInsight 群集。 使用基于 Linux 的 HDInsight 群集的信息，请参阅[使用 Maven 构建 Java 应用程序使用基于 Linux 的 HDInsight 与 HBase](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>要求

* [Java 平台 JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 或更高版本

* [Maven](http://maven.apache.org/)


* [使用 HBase 的基于 Windows 的 HDInsight 群集](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] 使用 HDInsight 群集版本 3.2 和 3.3 测试过此文档中的步骤。 在示例中提供的默认值是 HDInsight 3.3 群集。

##<a name="create-the-project"></a>创建项目

1. 从命令行开发环境中，将目录更改为您想要创建的项目，例如，位置`cd code\hdinsight`。

2. 安装使用 Maven， __mvn__命令用于生成项目的基架。

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    此命令由__artifactID__参数 (**hbaseapp**在本例。) 指定的名称在当前的位置，创建一个目录此目录包含以下各项︰

    * __pom.xml__: 项目对象模型 ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) 包含用于生成项目的信息和配置详细信息。

    * __src__︰ 包含__main\java\com\microsoft\examples__目录中，并将在此创作应用程序的目录。

3. 删除__src\test\java\com\microsoft\examples\apptest.java__文件，因为不在本示例中使用它。

##<a name="update-the-project-object-model"></a>更新的项目对象模型

1. 编辑__pom.xml__文件并添加下面的代码在`<dependencies>`部分中︰

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    本部分将告诉 Maven 项目需要__hbase 客户端__版本__1.1.2__。 在编译时，该依赖项将从默认 Maven 存储库下载。 可以使用[Maven 中央存储库中搜索](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar)要了解有关此依赖项的详细信息。

    > [AZURE.IMPORTANT] 版本号必须与 HBase HDInsight 群集中提供的版本相匹配。 使用下表来查找正确的版本号。

  	| HDInsight 群集版本 | 使用 HBase 版本 |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    HDInsight 版本和组件的详细信息，请参阅[什么是不同的 Hadoop 组件可用于 HDInsight](hdinsight-component-versioning.md)。

2. 如果您使用的 HDInsight 3.3 群集，您还必须添加到以下`<dependencies>`部分中︰

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    该依赖项将加载菲尼克斯核心组件，它由 Hbase 版本 1.1.x。

2. 将以下代码添加到__pom.xml__文件中。 此分区必须位于`<project>...</project>`文件中的标记之间，如`</dependencies>`和`</project>`。

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

    `<resources>`节配置资源 (__conf\hbase site.xml__) 包含 HBase 的配置信息。

    > [AZURE.NOTE] 您还可以设置通过代码的配置值。 请参阅下面有关如何执行此操作的__CreateTable__示例中的注释。

    这`<plugins>`节配置的[Maven 编译器插件](http://maven.apache.org/plugins/maven-compiler-plugin/)和[阴影的 Maven 插件](http://maven.apache.org/plugins/maven-shade-plugin/)。 编译器插件用于编译拓扑结构。 插件底纹用于防止许可证重复生成的 Maven 的 JAR 包中。 使用此选项的原因是重复的许可证文件，导致 HDInsight 群集上运行时错误。 使用 maven 底纹-插件与`ApacheLicenseResourceTransformer`实现防止此错误。

    Maven 底纹插件也会产生 uber jar （或 fat jar），其中包含由应用程序所需的所有依赖项。

3. 保存__pom.xml__文件。

4. 创建一个名为__conf__的__hbaseapp__目录中的新目录。 在__conf__目录中，创建一个名为__hbase site.xml__文件。 使用以下内容作为该文件的内容︰

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    此文件将用于加载 HDInsight 群集的 HBase 配置。

    > [AZURE.NOTE] 这是一个最小的 hbase site.xml 文件，并包含 HDInsight 群集的裸机最低设置。

3. 保存__hbase site.xml__文件。

##<a name="create-the-application"></a>创建应用程序

1. 转到__hbaseapp\src\main\java\com\microsoft\examples__目录，并将 app.java 文件重命名为__CreateTable.java__。

2. 打开__CreateTable.java__文件和现有的内容替换为以下代码︰

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. 在__hbaseapp\src\main\java\com\microsoft\examples__目录中，创建一个名为__SearchByEmail.java__的新文件。 作为该文件的内容中使用以下代码︰

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

6. 在__hbaseapp\src\main\hava\com\microsoft\examples__目录中，创建一个名为__DeleteTable.java__的新文件。 作为该文件的内容中使用以下代码︰

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

1. 打开一个命令提示符并将目录更改到__hbaseapp__目录。

2. 使用以下命令生成 JAR 文件包含应用程序︰

        mvn clean package

    这清除任何以前的生成项目、 下载尚未安装的任何依赖项，然后将生成并打包应用程序。

3. 在此命令完成后， __hbaseapp\target__目录中包含一个名为__hbaseapp-1.0-SNAPSHOT.jar__文件。

    > [AZURE.NOTE] __Hbaseapp-1.0-SNAPSHOT.jar__文件是 uber （有时称为 fat 的 jar，） 它包含运行应用程序所需的所有依赖关系的 jar。

##<a name="upload-the-jar-file-and-start-a-job"></a>将 JAR 文件上载和启动作业

有许多方法可以将文件上传到 HDInsight 群集，[上载数据的 Hadoop 作业中 HDInsight](hdinsight-upload-data.md)中所述。 下列步骤将使用 Azure PowerShell。

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. 在安装和配置 Azure PowerShell 之后, 创建一个名为__hbase runner.psm1__的新文件。 使用以下内容作为本文件的内容︰

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    此文件包含两个模块︰

    * __添加 HDInsightFile__ -用于将文件上传到 HDInsight

    * __开始-HBaseExample__ -用于运行前面创建的类

2. 保存__hbase runner.psm1__文件。

3. 打开一个新窗口，Azure PowerShell，将目录更改到__hbaseapp__目录中，然后运行下面的命令。

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    将路径更改为更早版本创建的__hbase runner.psm1__文件的位置。 这将注册模块，为此 Azure PowerShell 会话。

2. 使用下面的命令将__hbaseapp-1.0-SNAPSHOT.jar__上传到 HDInsight 群集。

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    __Hdinsightclustername__替换 HDInsight 群集的名称。 该命令将__hbaseapp-1.0-SNAPSHOT.jar__上载到主 HDInsight 群集存储中的__示例__jar/位置。

3. 文件上载后，使用下面的代码使用__hbaseapp__创建表︰

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    __Hdinsightclustername__替换 HDInsight 群集的名称。

    此命令创建一个名为 HDInsight 群集中的__人们__的新表。 此命令在控制台窗口中不显示任何输出。

2. 若要搜索的表中的条目，请使用下面的命令︰

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    __Hdinsightclustername__替换 HDInsight 群集的名称。

    此命令使用**SearchByEmail**类搜索其中的__contactinformation__列族和__电子邮件__栏中，包含字符串__contoso.com__任何行。 您应该收到以下结果︰

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    使用__fabrikam.com__的`-emailRegex`值，则返回用户在电子邮件字段中包含__fabrikam.com__ 。 通过使用常规的基于表达式的筛选实现此搜索，因为您还可以输入正则表达式，如__^ r__，电子邮件开头字母 r 的返回项目。

##<a name="delete-the-table"></a>删除表

完成示例是，使用 Azure PowerShell 会话中的以下命令来删除在此示例中使用的__人__表︰

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

__Hdinsightclustername__替换 HDInsight 群集的名称。

##<a name="troubleshooting"></a>故障排除

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>没有搜索结果或使用启动 HBaseExample 时出现意外的结果

使用`-showErr`参数来查看运行作业时产生的标准误差 (STDERR)。
