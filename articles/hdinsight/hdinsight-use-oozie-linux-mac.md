<properties
    pageTitle="在基于 Linux 的 HDInsight 使用 Hadoop Oozie 工作流 |Microsoft Azure"
    description="在基于 Linux 的 HDInsight 使用 Hadoop Oozie。 了解如何定义 Oozie 工作流，并将提交 Oozie 作业。"
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
    ms.date="10/11/2016"
    ms.author="larryfr"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>使用 Hadoop 使用 Oozie 定义和基于 Linux 的 HDInsight 上运行的工作流

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

了解如何使用 Apache Oozie 定义配置单元和 Sqoop，使用工作流和基于 Linux 的 HDInsight 群集上运行工作流。

Apache Oozie 是管理 Hadoop 作业的工作流中的协调系统。 它集成了 Hadoop 堆栈上，并且用于 Apache MapReduce，Apache 猪、 Apache 配置单元，和 Apache Sqoop 支持 Hadoop 作业。 它还可以用来计划作业所特有的系统，如 Java 程序或 shell 脚本

> [AZURE.NOTE] 用于定义工作流与 HDInsight 的另一个选项是 Azure 数据工厂。 若要了解有关 Azure 数据工厂的详细信息，请参阅[使用猪和蜂巢数据工厂][azure-data-factory-pig-hive]。

##<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- **Azure 订阅**︰[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)，请参阅。

- **Azure CLI**︰ 请参见[安装和配置的 Azure CLI](../xplat-cli-install.md)
    
    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- **HDInsight 群集**︰ 请参阅[开始使用 Linux 上的 HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Azure SQL 数据库**︰ 将使用本文中的步骤创建此

##<a name="example-workflow"></a>示例工作流

按照本文档中的说明进行操作，您将实现的工作流包含两个操作。 定义任务，如运行 Sqoop，MapReduce 或其他进程配置单元包含以下操作︰

![工作流关系图][img-workflow-diagram]

1. 配置单元操作运行 HiveQL 脚本，以从 HDInsight 中包含**hivesampletable**中提取记录。 每个数据行描述从特定移动设备的访问。 记录格式显示与以下类似︰

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    本文档中使用该配置单元脚本计算为每个平台 （如 Android 或 iPhone） 总访问量和存储到新的配置单元表的计数。

    有关配置单元的详细信息，请参阅[使用配置单元与 HDInsight][hdinsight-use-hive]。

2.  Sqoop 操作将新的配置单元表的内容导出到 SQL Azure 数据库中的表。 有关 Sqoop 的详细信息，请参阅[使用 Hadoop Sqoop 与 HDInsight][hdinsight-use-sqoop]。

> [AZURE.NOTE] 有关支持的 Oozie 版本 HDInsight 群集，请参阅[由 HDInsight 提供的 Hadoop 群集版本中的新增功能？][hdinsight-versions].

##<a name="create-the-working-directory"></a>创建工作目录

Oozie 预计作业将存储在同一目录中的所需资源。 此示例使用**wasbs: / 教程/useoozie/**。 使用以下命令创建此目录中，并将此工作流通过创建的新配置单元表数据目录︰

    hdfs dfs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE] `-p`参数导致如果不存在要创建的路径中的所有目录。 **数据**目录用于保存使用**useooziewf.hql**脚本的数据。

运行下面的命令来确保 Oozie 可以在运行配置单元和 Sqoop 作业时模拟您的用户帐户。 **用户名**替换您的登录名︰

    sudo adduser USERNAME users

如果您收到错误用户已经是一个成员的用户，您就可以忽略它。

##<a name="add-a-database-driver"></a>添加数据库驱动程序

由于此工作流使用 Sqoop 将数据导出到 SQL 数据库，则必须提供一份用来谈论到 SQL 数据库的 JDBC 驱动程序。 使用下面的命令以将其复制到工作目录︰

    hdfs dfs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/

如果您的工作流使用其他资源，如包含 MapReduce 应用程序 jar，您需要添加这些也。

##<a name="define-the-hive-query"></a>定义配置单元查询

使用以下步骤创建定义一个查询，它将在本文档后面 Oozie 工作流中使用的 HiveQL 脚本。

1. 使用 SSH 连接到基于 Linux 的 HDInsight 群集︰

    * **Linux、 Unix 或 OS X 客户机**︰ 请[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 或 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows 客户端**︰ 请[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 使用下面的命令创建一个新文件︰

        nano useooziewf.hql

1. 一旦 nano 编辑器打开时，使用以下内容作为该文件的内容︰

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    有两个脚本中使用的变量︰

    - **${hiveTableName}**: 将包含要创建的表的名称
    - **${hiveDataFolder}**: 将包含表的数据文件的存储位置

    工作流定义文件 (在本教程中的 workflow.xml) 这些将值传递给此 HiveQL 脚本在运行时。

2. 按下 Ctrl-X 退出编辑器。 出现提示时，选择**Y**若要保存该文件，然后使用**enter 键**使用的**useooziewf.hql**文件的名称。

3. 使用以下命令将**useooziewf.hql**复制到**wasbs:///tutorials/useoozie/useooziewf.hql**:

        hdfs dfs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    这些命令将存储与此群集，将保留该文件，即使删除群集关联的 Azure 存储帐户上的**useooziewf.hql**文件。 这允许您通过删除群集时他们不是在使用中，同时保留您的作业和工作流来节省资金。

##<a name="define-the-workflow"></a>定义工作流

Oozie 工作流定义是用 hPDL （一种 XML 过程定义语言） 编写的。 使用以下步骤来定义工作流︰

1. 使用下面的语句来创建和编辑新的文件︰

        nano workflow.xml

1. 一旦 nano 编辑器将打开并输入该文件的内容如下︰

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
                <arg>${hiveDataFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>sqljdbc41.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
            </action>
            <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>
            <end name="end"/>
        </workflow-app>

    有两个工作流中定义的操作︰

    - **RunHiveScript**︰ 这是启动操作，并运行**useooziewf.hql**脚本配置单元

    - **RunSqoopExport**︰ 此导出到 SQL 数据库中使用 Sqoop 从配置单元的脚本创建的数据。 这才运行**RunHiveScript**操作都将成功。

        > [AZURE.NOTE] 关于 Oozie 工作流和工作流操作使用的详细信息，请参阅[Apache Oozie 4.0 文档][ apache-oozie-400] （对于 HDInsight 3.0 版) 或[Apache Oozie 3.3.2 文档][ apache-oozie-332] （对于 HDInsight 2.1 版)。

    请注意，工作流有多个条目，如`${jobTracker}`，您在本文档后面部分中的作业定义中使用的值将被替换的。

    此外请注意`<archive>sqljdbc4.jar</arcive>`Sqoop 部分中的条目。 这会指示 Oozie 以使该归档文件可用于 Sqoop 时，该操作运行。

2. 使用 Ctrl-X，然后**Y**和**enter 键**来保存该文件。

3. 使用下面的命令可以将**workflow.xml**文件复制到**wasbs:///tutorials/useoozie/workflow.xml**:

        hdfs dfs -copyFromLocal workflow.xml /tutorials/useoozie/workflow.xml

##<a name="create-the-database"></a>创建数据库

按照创建新的数据库[创建 SQL 数据库](../sql-database/sql-database-get-started.md)文档中的步骤。 在创建数据库时，使用__oozietest__作为数据库的名称。 此外记的名称为数据库服务器，使用根据这将需要在下一节。

###<a name="create-the-table"></a>创建表

> [AZURE.NOTE] 有许多方法来连接到 SQL 数据库创建表。 下列步骤将从 HDInsight 群集使用[FreeTDS](http://www.freetds.org/) 。

3. 使用以下命令在 HDInsight 群集上安装 FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. FreeTDS 安装后，使用以下命令连接到您以前创建的 SQL 数据库服务器︰

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest

    您将收到类似于下面的输出︰

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. 在`1>`提示下，输入以下命令行︰

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    当`GO`输入语句时，将计算了上述语句。 这将创建名为**mobiledata** ，将由 Sqoop 写到一个新表。

    使用以下方法来验证已创建表︰

        SELECT * FROM information_schema.tables
        GO

    您应该看到类似于下面的输出︰

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. 输入`exit`在`1>`提示退出 tsql 实用程序。

##<a name="create-the-job-definition"></a>创建作业定义

作业定义描述了在何处可以找到 workflow.xml，以及其他文件使用的工作流 （如 useooziewf.hql。)有关在工作流中使用的属性和关联的文件，它还定义的值。

1. 使用下面的命令以获取完整的 WASB 地址到默认存储。 这将使用在配置文件中在一段时间︰

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    这将返回类似于以下信息︰

        <name>fs.defaultFS</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>

    保存**wasbs://mycontainer@mystorageaccount.blob.core.windows.net**值，因为将在接下来的步骤中使用。

2. 使用以下命令获取群集 headnode 的 FQDN。 这将使用 JobTracker 地址的群集。 这将使用在配置文件中在一段时间︰

        hostname -f

    这将返回类似于以下内容的信息︰

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    用于 JobTracker 的端口为 8050，所以完整的地址，要用于 JobTracker 也为**hn0 CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**。

1. 使用以下方法来创建 Oozie 作业定义配置︰

        nano job.xml

2. 一旦 nano 编辑器打开时，使用以下内容作为该文件的内容︰

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
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
            <name>hiveScript</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * 替换所有实例的**wasbs://mycontainer@mystorageaccount.blob.core.windows.net**以前接收到的值。

    > [AZURE.WARNING] WASB 的完整，必须使用容器和存储帐户作为路径的一部分。 使用短格式 (wasbs: / /) 将导致 RunHiveScript 操作失败时启动作业。

    * 您收到了前面的 JobTracker/ResourceManager 地址替换**JOBTRACKERADDRESS** 。

    * 替换为您的登录名为 HDInsight 群集**YourName** 。

    * **服务器名**、 **adminLogin**和**adminPassword**替换 SQL Azure 数据库的信息。

    此文件中的信息大部分用于填充使用 workflow.xml 或 ooziewf.hql 文件 （如 ${nameNode}。） 中的值

    > [AZURE.NOTE] **Oozie.wf.application.path**条目定义了在何处可以找到 workflow.xml 文件，其中包含的工作流运行此作业。

2. 使用 Ctrl-X，然后**Y**和**enter 键**来保存该文件。

##<a name="submit-and-manage-the-job"></a>提交和管理作业

下列步骤将使用 Oozie 命令来提交和管理群集上的 Oozie 工作流。 Oozie 命令是对[Oozie REST API，](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html)友好的界面。

> [AZURE.IMPORTANT] 使用 Oozie 命令时，您必须使用 FQDN HDInsight headnode。 才从群集中，可以使用该 FQDN 或群集在 Azure 虚拟网络，来自同一网络上的其他计算机。

1. 使用以下方法来获取指向 Oozie 服务的 URL:

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    这将返回类似于以下值︰

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

    **Http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie**的部分是要使用 Oozie 命令时使用的 URL。

2. 使用下面的 url，创建环境变量，因此您不必键入每个命令︰

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    与更早收到替换 URL。

3. 使用以下方法来提交作业︰

        oozie job -config job.xml -submit

    这从**job.xml**加载作业信息并将它提交给 Oozie，但不会运行。

    一旦完成该命令，则应返回作业的 ID。 例如， `0000005-150622124850154-oozie-oozi-W`。 这将用于管理作业。

4. 查看使用下面的命令的作业的状态。 输入返回前一个命令的作业 ID:

        oozie job -info <JOBID>

    这将返回类似于以下内容的信息。

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasbs:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    此作业的状态为`PREP`，它指示它已提交，但尚未启动。

4. 使用以下方法来开始该作业︰

        oozie job -start JOBID

    如果您在此命令后检查状态，它将在运行状态下，和作业中的操作会返回信息。

5. 一旦成功完成该任务，您可以验证数据已生成并导出到 SQL 数据库表使用以下命令︰

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    在`1>`提示，请输入以下命令︰

        SELECT * FROM mobiledata
        GO

    您应该收到类似于以下的信息︰

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Oozie 命令的详细信息，请参阅[Oozie 命令行工具](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html)。

##<a name="oozie-rest-api"></a>Oozie REST API，

Oozie REST API，使您可以生成自己使用 Oozie 的工具。 HDInsight 使用 Oozie REST API，具体信息如下︰

* **URI**: REST API，可以通过在群集外`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **身份验证**︰ 必须给 API 使用群集 HTTP 帐户 （管理员） 和密码进行身份验证。 例如︰

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

使用 Oozie REST API 的详细信息，请参阅[Oozie Web 服务 API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html)。

##<a name="oozie-web-ui"></a>Oozie Web 用户界面

Oozie Web 用户界面提供群集 Oozie 作业的状态的基于 web 的视图。 它允许您在作业和作业日志中查看作业状态、 作业定义、 配置、 操作的图形。 您还可以查看在一个作业中的操作的详细信息。

若要访问 Oozie Web 用户界面，请使用以下步骤︰

1. 创建到 HDInsight 群集的 SSH 隧道。 有关如何执行此操作的信息，请参阅[使用 SSH 隧道 Ambari web 用户界面、 ResourceManager、 JobHistory、 NameNode，Oozie 和其他的 web 用户界面的访问](hdinsight-linux-ambari-ssh-tunnel.md)。

2. 一旦已经创建了一个隧道，在 web 浏览器中打开 Ambari web 用户界面。 Ambari 网站的 URI 是**https://CLUSTERNAME.azurehdinsight.net**。 **群集名称**替换基于 Linux 的 HDInsight 群集的名称。

3. 从该页的左侧，选择**Oozie**，然后**快速链接**，并最后**Oozie Web 用户界面**。

    ![菜单的图像](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Oozie Web 用户界面默认为显示正在运行的工作流作业。 若要查看所有工作流作业，请选择**所有的作业**。

    ![显示所有作业](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. 选择一个作业以查看有关作业的更多信息。

    ![作业信息](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. 作业信息选项卡中，您可以查看基本的作业信息，以及作业中的单个操作。 使用顶部的选项卡可以查看作业日志的作业定义，作业配置访问或查看定向非循环图 (DAG) 的作业。

    * **作业日志**︰ 选择**GetLogs**按钮以获得所有日志作业，或使用字段中**输入的搜索筛选器**来筛选日志

        ![作业日志](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: DAG 是在工作流执行的数据路径的图形化概况

        ![作业 DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. 从**作业信息**选项卡中选择一种操作将调出操作的信息。 例如，选择**RunHiveScript**操作。

    ![操作信息](./media/hdinsight-use-oozie-linux-mac/action.png)

8. 您可以看到措施，包括指向**控制台 URL**，它可以用于查看有关作业的 JobTracker 信息的详细的信息。

##<a name="scheduling-jobs"></a>作业级排产

协调员可以指定开始、 结束和作业的发生频率，这样他们可以安排特定时间。

若要定义工作流时间表，请使用以下步骤︰

1. 使用以下方法来创建一个名为**coordinator.xml**的新文件︰

        nano coordinator.xml

    使用以下内容作为该文件的内容︰

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    请注意，使用`${...}`在作业定义中的值将被替换的变量。 变量包括︰

    * **${coordFrequency}**: 正在运行的作业的实例之间的时间
    * **${coordStart}**: 作业开始时间
    * **${coordEnd}**: 作业结束时间
    * **${coordTimezone}**: 协调作业 （通常由使用 UTC） 没有夏时制节约时间是固定的时区中。 此时区被称为"Oozie 处理时区"
    * **${wfPath}**: workflow.xml 的路径

2. 使用 Ctrl-X，然后**Y**和**enter 键**来保存该文件。

3. 使用以下方法来将其复制到工作目录中为此作业︰

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. 使用以下方法来修改**job.xml**文件︰

        nano job.xml

    进行以下更改︰

    * 更改`<name>oozie.wf.application.path</name>`到`<name>oozie.coord.application.path</name>`。 这会指示 Oozie 要运行协调员的文件，而不是工作流文件

    * 将以下内容，这将是集添加指向的位置的 workflow.xml 在 coordinator.xml 中使用的变量︰

            <property>
              <name>workflowPath</name>
              <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        在 job.xml 文件中的其他项目中使用的值替换为**mycontainer**和**mystorageaccount**的值。

    * 添加以下定义的开始、 结束时和频率使用的 coordinator.xml 文件︰

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        这些设置于 2015 年 6 月 25 日 12:00 开始的时间，于 2015 年 6 月 27 日，结束时间为每日运行此作业的时间间隔 (以分钟为单位的频率是这样 24 小时 x 60 分钟 = 1440年分钟。)最后，将时区设置为 UTC。

5. 使用 Ctrl-X，然后**Y**和**enter 键**来保存该文件。

6. 若要运行作业，请使用下面的命令︰

        oozie job -config job.xml -run

    这会提交并开始作业。

7. 如果您访问 Oozie Web 用户界面，并选择**协调作业**选项卡，则应类似于下面这样的信息︰

    ![协调员作业选项卡](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    请注意**下一步的具体化**入口中;这是在下一次运行作业时。

8. 类似于早期的工作流作业，在 web 用户界面中选择的工作项将显示有关信息作业︰

    ![协调员作业信息](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    请注意，这只表明成功运行的作业，而不是单个操作计划工作流中。 若要查看所选择的一个**行动**项。 这将显示类似于早期的工作流作业中检索信息。

    ![操作信息](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##<a name="troubleshooting"></a>故障排除

当解决问题时 Oozie 作业，Oozie UI 是非常有用的因为它允许您可以轻松地查看两个 Oozie 日志，以及链接到 JobTracker MapReduce 任务，如配置单元查询日志。 一般情况下，进行故障排除的模式应该是︰

1. Oozie Web 用户界面中查看该作业。

2. 如果有错误或失败特定的操作，选择**错误消息**字段中是否失败时提供了详细信息，请参阅操作。

3. 如果可用，请使用 URL 的操作以查看更多详细信息 （如 JobTracker 日志） 的操作。

以下是您可能遇到的特定错误和如何解决这些问题。

###<a name="ja009-cannot-initialize-cluster"></a>JA009︰ 无法初始化群集

**症状**︰ 作业状态将更改为**挂起**。 作业的详细信息将显示为**START_MANUAL**的 RunHiveScript 状态。 选择操作将显示以下错误消息︰

    JA009: Cannot initialize Cluster. Please check your configuration for map

**原因**︰ 在**job.xml**文件中使用 WASB 地址不包含存储容器或存储帐户名称。 WASB 地址格式必须为`wasbs://containername@storageaccountname.blob.core.windows.net`。

**解决方法**︰ 更改作业所用的 WASB 地址。

###<a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie 不允许模拟&lt;用户 >

**症状**︰ 作业状态将更改为**挂起**。 作业的详细信息将显示为**START_MANUAL**的 RunHiveScript 状态。 选择操作将显示以下错误消息︰

    JA002: User: oozie is not allowed to impersonate <USER>

**原因**︰ 当前的权限设置不允许 Oozie 模拟指定的用户帐户。

**解决方法**︰ 允许 Oozie 模拟**用户**组中的用户。 使用`groups USERNAME`若要查看的用户帐户所在的组。 如果用户不是**用户**组的成员，请使用以下命令将用户添加到组︰

    sudo adduser USERNAME users

> [AZURE.NOTE] 它可能需要几分钟才会认识到 HDInsight，用户将被添加到组。

###<a name="launcher-error-sqoop"></a>启动程序错误 (Sqoop)

**症状**︰ 作业状态将更改**为**。 作业详细信息会显示**错误**RunSqoopExport 状态。 选择操作将显示以下错误消息︰

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**原因**︰ Sqoop 不能加载访问数据库所需的数据库驱动程序。

**解决方法**︰ 当 Oozie 作业中使用 Sqoop，您必须包括作业使用的其他资源 （如 workflow.xml) 的数据库驱动。

也必须引用包含数据库驱动程序从存档`<sqoop>...</sqoop>`workflow.xml 一节。

例如，对于本文档中工作，将使用以下步骤︰

1. 将 sqljdbc4.1.jar 文件复制到 /tutorials/useoozie 目录中︰

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar

2. 修改要在新行上面添加以下 workflow.xml `</sqoop>`:

        <archive>sqljdbc41.jar</archive>

##<a name="next-steps"></a>下一步行动
在本教程中，您学习了如何定义 Oozie 工作流以及如何运行 Oozie 作业。 若要了解有关使用 HDInsight 的详细信息，请参阅下列文章︰

- [使用基于时间的 Oozie 协调器与 HDInsight][hdinsight-oozie-coordinator-time]
- [上载数据中 HDInsight 的 Hadoop 作业][hdinsight-upload-data]
- [使用 Hadoop 在 HDInsight Sqoop][hdinsight-use-sqoop]
- [使用 Hadoop HDInsight 上配置单元][hdinsight-use-hive]
- [使用 Hadoop HDInsight 上的小猪][hdinsight-use-pig]
- [HDInsight 为开发 Java MapReduce 程序][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]: storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
