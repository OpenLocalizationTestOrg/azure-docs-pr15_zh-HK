<properties
    pageTitle="在 HDInsight 中使用基于时间的 Hadoop Oozie 协调器 |Microsoft Azure"
    description="在 HDInsight，大数据服务中使用基于时间的 Hadoop Oozie 协调员。 了解如何定义 Oozie 工作流和协调员，并可提交作业。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>使用基于时间的 Oozie 协调 Hadoop HDInsight 中定义的工作流并协调作业

在本文中，您将学习如何定义工作流和协调员，以及如何协调作业，基于时间触发。 最好通过[使用 Oozie HDInsight 与][hdinsight-use-oozie]在阅读本文之前。 除了 Oozie，您还可以安排使用 Azure 数据工厂的作业。 Azure 数据工厂，请参阅[使用猪的数据工厂蜂巢](../data-factory/data-factory-data-transformation-activities.md)。

> [AZURE.NOTE] 这篇文章需要基于 Windows HDInsight 群集。 有关使用 Oozie，包括基于时间的作业，在基于 Linux 的群集上，请参阅[使用 Hadoop 来定义和运行的工作流的基于 Linux 的 HDInsight 上使用 Oozie](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Oozie 是什么

Apache Oozie 是管理 Hadoop 作业的工作流中的协调系统。 它集成了 Hadoop 堆栈上，并且用于 Apache MapReduce，Apache 猪、 Apache 配置单元，和 Apache Sqoop 支持 Hadoop 作业。 也可用于安排特定系统，例如 Java 程序或 shell 脚本的作业。

下图显示了您将实现的工作流︰

![工作流关系图][img-workflow-diagram]

工作流包含两个操作︰

1. 配置单元操作运行 HiveQL 脚本，以累计 log4j 日志文件中的每个日志级别类型的次数。 每个 log4j 日志包含包含要显示的类型和严重程度，例如 [日志级别] 字段字段的行︰

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    配置单元脚本输出内容类似于︰

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    有关配置单元的详细信息，请参阅[使用配置单元与 HDInsight][hdinsight-use-hive]。

2.  Sqoop 操作将 HiveQL 操作输出导出到 SQL Azure 数据库中的表。 有关 Sqoop 的详细信息，请参阅[使用 Sqoop 与 HDInsight][hdinsight-use-sqoop]。

> [AZURE.NOTE] 有关支持的 Oozie 版本 HDInsight 群集，请参阅[由 HDInsight 提供的群集版本中的新增功能？][hdinsight-versions].


##<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- **带有 Azure PowerShell 的工作站**。

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **HDInsight 群集**。 有关创建 HDInsight 群集的信息，请参阅[群集创建 HDInsight][hdinsight-provision]，[开始使用 HDInsight]或[hdinsight-get-started]。 您将需要以下数据以转到教程︰

    <table border = "1">
    <tr><th>群集属性</th><th>Windows PowerShell 变量名</th><th>值</th><th>说明</th></tr>
    <tr><td>HDInsight 群集名称</td><td>$clusterName</td><td></td><td>HDInsight 群集将在其运行本教程。</td></tr>
    <tr><td>HDInsight 群集用户名</td><td>$clusterUsername</td><td></td><td>HDInsight 群集用户名。 </td></tr>
    <tr><td>HDInsight 群集用户密码 </td><td>$clusterPassword</td><td></td><td>HDInsight 群集的用户密码。</td></tr>
    <tr><td>Azure 存储帐户名</td><td>$storageAccountName</td><td></td><td>HDInsight 群集可用的 Azure 存储帐户。 对于本教程，使用群集配置过程中指定的默认存储帐户。</td></tr>
    <tr><td>Azure Blob 容器名称</td><td>$containerName</td><td></td><td>对于此示例，使用 Azure Blob 存储容器所使用的默认 HDInsight 群集文件系统。 默认情况下，它有 HDInsight 群集相同的名称。</td></tr>
    </table>

- **Azure SQL 数据库**。 您必须配置 SQL 数据库服务器允许从您的工作站访问的防火墙规则。 说明如何创建 Azure SQL 数据库并配置防火墙，请参阅[开始使用 SQL Azure 数据库][sqldatabase-get-started]。 这篇文章提供了 Windows PowerShell 脚本创建用于本教程所需的 SQL Azure 数据库表。

    <table border = "1">
    <tr><th>SQL 数据库属性</th><th>Windows PowerShell 变量名</th><th>值</th><th>说明</th></tr>
    <tr><td>SQL 数据库服务器名称</td><td>$sqlDatabaseServer</td><td></td><td>SQL 数据库服务器 Sqoop 将数据导出到其中。 </td></tr>
    <tr><td>SQL 数据库登录名</td><td>$sqlDatabaseLogin</td><td></td><td>SQL 数据库登录名。</td></tr>
    <tr><td>SQL 数据库登录密码</td><td>$sqlDatabaseLoginPassword</td><td></td><td>SQL 数据库登录密码。</td></tr>
    <tr><td>SQL 数据库名称</td><td>$sqlDatabaseName</td><td></td><td>Sqoop 将数据导出到其中的 SQL Azure 数据库。 </td></tr>
    </table>

    > [AZURE.NOTE] 默认情况下 SQL Azure 数据库从 Azure 服务，例如 Azure HDInsight 允许连接。 如果禁用该防火墙设置，则必须启用它从 Azure 门户。 有关创建 SQL 数据库和配置防火墙规则的说明，请参阅[创建和配置 SQL 数据库][sqldatabase-get-started]。


> [AZURE.NOTE] 填充表中的值。 它将有助于通过本教程。


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie 工作流和相关的 HiveQL 脚本定义

Oozie 工作流定义是用 hPDL （一种 XML 过程定义语言） 编写的。 默认工作流文件的名称是*workflow.xml*。  将工作流在本地保存文件，并再将其部署到 HDInsight 群集以后在本教程中使用 Azure PowerShell。

工作流中的配置单元操作调用一个 HiveQL 脚本文件。 此脚本文件包含三个 HiveQL 语句︰

1. **DROP TABLE 语句**删除 log4j 配置单元表，如果存在。
2. **CREATE TABLE 语句**创建一个 log4j 配置单元外部表，其指向位置的 log4j 日志文件;
3.  **Log4j 日志文件的位置**。 字段分隔符是"，"。 默认行分隔符是"\n"。 配置单元外部表用于避免数据文件被删除从原始位置，以防您想要运行多次的 Oozie 工作流。
3. **重写插入语句**统计在 log4j 配置单元表中，每个日志级别类型的实例，并将输出保存到 Azure Blob 存储位置。

**注意**︰ 没有一个已知的配置单元的路径问题。 提交 Oozie 作业时，您将遇到此问题。 在 TechNet Wiki 上，找不到用于修正该问题的说明︰ [HDInsight 配置单元错误︰ 无法重命名][technetwiki-hive-error]。

**若要定义要调用工作流的 HiveQL 脚本文件**

1. 使用以下内容创建的文本文件︰

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    有三个变量在脚本中使用︰

    - $ {hiveTableName}
    - $ {hiveDataFolder}
    - $ {hiveOutputFolder}

    工作流定义文件 (在本教程中的 workflow.xml) 将这些在将值传递到此 HiveQL 脚本运行的时间。

2. 使用 ANSI (ASCII) 编码将该文件另存为**C:\Tutorials\UseOozie\useooziewf.hql** 。 （使用记事本，如果您的文本编辑器不提供此选项）。在本教程后面部分将到 HDInsight 群集部署此脚本文件。



**若要定义工作流**

1. 使用以下内容创建的文本文件︰

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    有两个工作流中定义的操作。 开始为操作是*RunHiveScript*。 如果该操作运行时*确定*下, 一步操作是*RunSqoopExport*。

    RunHiveScript 具有多个变量。 从工作站中使用 Azure PowerShell 提交 Oozie 作业时，将传递值。

    <table border = "1">
    <tr><th>工作流变量</th><th>说明</th></tr>
    <tr><td>$ {jobTracker}</td><td>指定的 URL 的 Hadoop 作业跟踪器。 使用 HDInsight 群集版本 3.0 和 2.0 <strong>jobtrackerhost:9010</strong> 。</td></tr>
    <tr><td>$ {nameNode}</td><td>指定的 URL 的 Hadoop 名称节点。 使用默认文件系统 wasbs: / 地址，例如<i>wasbs: / /&lt;容器名称&gt;@&lt;storageAccountName&gt;。 blob.core.windows.net</i>。</td></tr>
    <tr><td>$ {queueName}</td><td>指定将作业提交到队列名称。 使用<strong>默认值</strong>。</td></tr>
    </table>


    <table border = "1">
    <tr><th>配置单元操作变量</th><th>说明</th></tr>
    <tr><td>$ {hiveDataFolder}</td><td>配置单元创建表命令的源目录。</td></tr>
    <tr><td>$ {hiveOutputFolder}</td><td>在插入重写语句输出文件夹。</td></tr>
    <tr><td>$ {hiveTableName}</td><td>配置单元表引用 log4j 数据文件的名称。</td></tr>
    </table>


    <table border = "1">
    <tr><th>Sqoop 操作变量</th><th>说明</th></tr>
    <tr><td>$ {sqlDatabaseConnectionString}</td><td>SQL 数据库的连接字符串。</td></tr>
    <tr><td>$ {sqlDatabaseTableName}</td><td>数据将被导出到 SQL Azure 数据库表。</td></tr>
    <tr><td>$ {hiveOutputFolder}</td><td>该配置单元将覆盖语句的输出文件夹。 这是 Sqoop 导出 （导出目录） 的同一文件夹。</td></tr>
    </table>

    有关 Oozie 工作流和使用的工作流操作的详细信息，请参阅[Apache Oozie 4.0 文档][ apache-oozie-400] （用于 3.0 版 HDInsight 群集） 或[Apache Oozie 3.3.2 文档][ apache-oozie-332] （为 2.1 版 HDInsight 群集）。

2. 使用 ANSI (ASCII) 编码将该文件另存为**C:\Tutorials\UseOozie\workflow.xml** 。 （使用记事本，如果您的文本编辑器不提供此选项）。

**若要定义协调员**

1. 使用以下内容创建的文本文件︰

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    有五个在定义文件中使用的变量︰

  	| 变量          | 说明 |
  	| ------------------|------------ |
  	| $ {coordFrequency} | 作业暂停时间。 在几分钟内始终表示频率。 |
  	| $ {coordStart}     | 作业的开始时间。 |
  	| $ {coordEnd}       | 作业的结束时间。 |
  	| $ {coordTimezone}  | Oozie 没有夏时制 （通常由使用 UTC） 与处理协调员固定时区中的作业。 此时区被称为"Oozie 处理时区"。 |
  	| $ {wfPath}         | Workflow.xml 的路径。  如果工作流文件名称不是默认的文件名 (workflow.xml)，则必须指定它。 |

2. 使用 ANSI (ASCII) 编码将该文件另存为**C:\Tutorials\UseOozie\coordinator.xml** 。 （使用记事本，如果您的文本编辑器不提供此选项）。

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>部署项目 Oozie 并准备教程

您将运行一个 Azure PowerShell 脚本来执行以下︰

- 将 HiveQL 脚本 (useoozie.hql) 复制到 Azure Blob 存储 wasbs:///tutorials/useoozie/useoozie.hql。
- 将 workflow.xml 复制到 wasbs:///tutorials/useoozie/workflow.xml。
- 将 coordinator.xml 复制到 wasbs:///tutorials/useoozie/coordinator.xml。
- 复制的数据文件 (/ example/data/sample.log) 到 wasbs:///tutorials/useoozie/data/sample.log。
- 创建 SQL Azure 数据库表来存储 Sqoop 导出数据。 表名称是*log4jLogCount*。

**了解 HDInsight 存储**

HDInsight 使用 Azure Blob 存储的数据存储。 wasbs: / / 是微软 Azure Blob 存储 Hadoop 分布式文件系统 (HDFS) 实现。 有关详细信息，请参阅[使用 Azure Blob 存储与 HDInsight][hdinsight-storage]。

当您配置 HDInsight 群集时，Azure Blob 存储帐户，并从该帐户的特定容器被指定为默认的文件系统，像在 HDFS 中。 除了此存储帐户，还可以添加更多存储帐户来自同一个 Azure 订阅或不同的 Azure 订阅设置过程中。 有关添加更多存储帐户的说明，请参阅[设置 HDInsight 群集][hdinsight-provision]。 为了简化 Azure PowerShell 脚本使用在本教程中，所有文件都存储在位于*/tutorials/useoozie*的默认文件系统容器。 默认情况下，该容器具有 HDInsight 群集名称相同的名称。
语法是︰

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] 只有*wasb: / /* HDInsight 群集 3.0 版中支持的语法。 较早*asv: / /* HDInsight 2.1 和 1.6 的群集中支持的语法，但它不支持 HDInsight 3.0 群集中。

> [AZURE.NOTE] Wasb: / / 路径的虚拟路径。 有关更多信息，请参见[使用 Azure Blob 存储与 HDInsight][hdinsight-storage]。

可以使用任何以下 Uri （我使用 workflow.xml 为例），从 HDInsight 访问默认文件系统容器中存储的文件︰

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

如果想要直接从存储帐户访问该文件，该文件的 blob 名称是︰

    tutorials/useoozie/workflow.xml

**了解配置单元内部和外部表**

有几件事情需要了解的有关配置单元内部和外部表︰

- 创建表格命令创建的内部表，表也称为托管。 数据文件必须位于默认容器中。
- 创建表格命令将数据文件移动到 /hive/仓库/<TableName>文件夹中的默认容器。
- 创建外部表中的命令创建一个外部表。 数据文件可以位于默认容器的外部。
- 创建外部表中的命令不会移动的数据文件。
- 创建外部表命令不允许在 LOCATION 子句中指定的文件夹下的所有子文件夹。 这就是为什么教程将 sample.log 文件复制的原因。

有关详细信息，请参阅[HDInsight︰ 配置单元内部和外部表简介][cindygross-hive-tables]。

**若要准备教程**

1. 打开 Windows PowerShell ISE （在 Windows 8 启动屏幕中，键入**PowerShell_ISE**，，然后单击**Windows PowerShell ISE**。 有关详细信息，请参阅[Windows 8 和 Windows 启动 Windows PowerShell][powershell-start])。
2. 在底部窗格中，运行下面的命令以连接到 Azure 订购︰

        Add-AzureAccount

    系统将提示您输入您的 Azure 帐户凭据。 这种方法添加订阅连接超时后 12 小时，, 必须再次运行该 cmdlet。

    > [AZURE.NOTE] 如果您有多个 Azure 订阅和默认订阅不的是您想要使用，使用<strong>选择 AzureSubscription</strong> cmdlet 可以选择订阅。

3. 将以下脚本复制到脚本窗格中，并将前六个变量︰

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    有关的变量的详细说明，请参见[先决条件](#prerequisites)部分在本教程中

3. 将下列附加到脚本窗格中的脚本︰

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. 单击**运行脚本**或按**f5 键**以运行该脚本。 输出将类似于︰

    ![教程准备输出][img-preparation-output]

##<a name="run-the-oozie-project"></a>运行 Oozie 项目

目前，azure PowerShell 不用于定义 Oozie 的作业提供任何 cmdlet。 您可以使用**调用 RestMethod** cmdlet 调用 Oozie web 服务。 Oozie web 服务 API 是 HTTP 其余 JSON API。 关于 Oozie web 服务 API 的详细信息，请参阅[Apache Oozie 4.0 文档][ apache-oozie-400] （用于 3.0 版 HDInsight 群集） 或[Apache Oozie 3.3.2 文档][ apache-oozie-332] （为 2.1 版 HDInsight 群集）。

**若要提交 Oozie 作业**

1. 打开 Windows PowerShell ISE （在 Windows 8 启动屏幕中，键入**PowerShell_ISE**，，然后单击**Windows PowerShell ISE**。 有关详细信息，请参阅[Windows 8 和 Windows 启动 Windows PowerShell][powershell-start])。

3. 将以下脚本复制到脚本窗格中，并将首先十四个变量 （但是，请跳过**$storageUri**）。

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    有关的变量的详细说明，请参见[先决条件](#prerequisites)部分在本教程中

    $coordstart 和 $coordend 都是工作流开始时间和结束时间。 若要查找 UTC/GMT 时间，搜索"utc 时"在 bing.com 上。 $CoordFrequency 是频率在几分钟内您想要运行工作流。

3. 将下列附加到脚本。 此部件定义 Oozie 负载︰

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] 向工作流提交有效负载文件相比的主要区别是变量**oozie.coord.application.path**。 当您提交一个工作流作业时，则使用**oozie.wf.application.path** 。

4. 将下列附加到脚本。 这一部分将检查 Oozie web 服务状态︰

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. 将下列附加到脚本。 此部件创建 Oozie 作业︰

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] 当您提交一个工作流作业时，您必须使另一个 web 服务调用以创建作业后启动该作业。 在这种情况下，按时间触发协调作业。 该作业将自动启动。

6. 将下列附加到脚本。 这一部分将检查 Oozie 作业状态︰

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. （可选）将下列附加到脚本。

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. 将下列附加到脚本︰

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    如果您想要运行的其他函数中移除 # 符号。

7. 如果版本 2.1 HDinsight 群集，则替换与"https://$clusterName.azurehdinsight.net:443/oozie/v1/""https://$clusterName.azurehdinsight.net:443/oozie/v2/"。 2.1 版 HDInsight 群集执行不支持的版本 2 的 web 服务。

7. 单击**运行脚本**或按**f5 键**以运行该脚本。 输出将类似于︰

    ![教程运行工作流输出][img-runworkflow-output]

8. 连接到 SQL 数据库，导出的数据。

**若要查看作业错误日志**

要解决工作流，Oozie 日志文件可以找到在 C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log 从群集 headnode。 RDP 的信息，请参阅[管理 HDInsight 群集使用 Azure 的门户网站][hdinsight-admin-portal]。

**要重新运行本教程**

要重新运行该工作流，您必须执行以下任务︰

- 删除配置单元脚本输出文件。
- 删除 log4jLogsCount 表中的数据。

下面是一个示例 Windows PowerShell 脚本，您可以使用︰

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>下一步行动
在本教程中，您学习了如何定义 Oozie 工作流和 Oozie 协调，以及如何通过使用 Azure PowerShell 运行 Oozie 协调作业。 若要了解详细信息，请参阅下列文章︰

- [开始使用 HDInsight][hdinsight-get-started]
- [使用 HDInsight Azure Blob 存储][hdinsight-storage]
- [通过使用 Azure PowerShell 管理 HDInsight][hdinsight-admin-powershell]
- [将数据上传到 HDInsight][hdinsight-upload-data]
- [使用 HDInsight Sqoop][hdinsight-use-sqoop]
- [使用 HDInsight 配置单元][hdinsight-use-hive]
- [使用 HDInsight 的小猪][hdinsight-use-pig]
- [HDInsight 为开发 Java MapReduce 程序][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
