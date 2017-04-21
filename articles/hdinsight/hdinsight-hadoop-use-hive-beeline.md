<properties
   pageTitle="使用 Beeline 处理 HDInsight (Hadoop) 上的配置单元 |Microsoft Azure"
   description="了解如何使用 SSH 连接到 HDInsight，在 Hadoop 群集，然后使用 Beeline 以交互方式提交配置单元查询。 Beeline 是通过 JDBC 使用 HiveServer2 实用程序。"
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>使用 Hadoop Beeline 与 HDInsight 中的配置单元

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

在本文中，您将学习如何使用安全外壳协议 (SSH) 连接到一个基于 Linux 的 HDInsight 群集，并再以交互方式提交配置单元查询使用[Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell)命令行工具。

> [AZURE.NOTE] Beeline 使用 JDBC 连接到的配置单元。 在配置单元中使用 JDBC 的详细信息，请参阅[配置单元在 Azure HDInsight 使用的配置单元的 JDBC 驱动程序的连接](hdinsight-connect-hive-jdbc-driver.md)。

##<a id="prereq"></a>系统必备组件

若要完成本文中的步骤操作，您需要︰

* 基于 linux * 的 Hadoop，HDInsight 群集上。

* SSH 客户端。 Linux 和 Unix 中，Mac OS 应附带的 SSH 客户端。 Windows 用户必须下载客户端，如[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

##<a id="ssh"></a>用 SSH 连接

通过连接到的完全合格的域名称 (FQDN) 的 HDInsight 群集使用 SSH 命令。 FQDN 将使用的名称的群集，然后**。 azurehdinsight.net**。 例如，以下将连接到名为**myhdinsight**的群集︰

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**如果您提供 SSH 身份验证的证书密钥**的创建 HDInsight 群集时，您可能需要指定客户机系统上的私有密钥的位置︰

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**如果您提供的密码 SSH 身份验证**创建 HDInsight 群集时，您将需要提供密码提示时。

在 HDInsight 中使用 SSH 的详细信息，请参阅[使用 SSH 基于 Linux 的 Hadoop HDInsight 从 Linux、 OS X 和 Unix 上使用](hdinsight-hadoop-linux-use-ssh-unix.md)。

###<a name="putty-windows-based-clients"></a>PuTTY （基于 Windows 的客户端）

Windows 不提供内置的 SSH 客户端。 我们建议使用**PuTTY**，这可以从[http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)下载。

使用 PuTTY 的详细信息，请参阅[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)。

##<a id="beeline"></a>使用 Beeline 命令

1. 建立连接后，使用以下方法来启动 Beeline:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    这将启动 Beeline 客户端，然后连接到 JDBC url。 在这里，`localhost`以来在群集中，这两种头节点上运行的 HiveServer2 和我们正在 Beeline 直接在主 headnode 上使用。
    
    完成此命令后，您将到达`jdbc:hive2://localhost:10001/>`提示。

3. Beeline 命令通常开头`!`字符，例如`!help`显示帮助。 但是`!`往往可以省略。 例如，`help`也适用。

    如果您查看的帮助，您会看到`!sql`，用于执行 HiveQL 语句。 但是，HiveQL 所以通常使用，则可以省略前面`!sql`。 下面两个语句具有完全相同的结果;显示当前可通过配置单元的表︰
    
        !sql show tables;
        show tables;
    
    应在新群集上，列出一个表︰ __hivesampletable__。

4. 使用以下方法来显示 hivesampletable 的架构︰

        describe hivesampletable;
        
    这将返回以下信息︰
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    此表中显示的列。 虽然我们无法执行某些查询这些数据，让我们改为创建一个新表来演示如何将数据加载到配置单元，并应用一种架构。
    
5. 输入以下语句，创建名为**log4jLogs** ，使用 HDInsight 群集所附带的示例数据的新表︰

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    这些语句执行以下操作︰

    * **除去表**-删除表和数据文件，以防表已存在。
    * **创建外部表**-在配置单元中创建一个新的外部表。 外部表只能在配置单元中存储的表定义。 将数据留在原来的位置。
    * **行格式**的说明配置单元设置数据的格式。 在这种情况下，由空格分隔每个日志中的字段。
    * **存储作为文本文件位置**-告诉配置单元数据的存储 （示例中的数据目录），并且它将存储为文本。
    * **选择**选择所有列**t4**其中包含值**[错误]**的行数。 随着三个包含此值的行，这应该返回值为**3** 。
    * **像 '%.log' INPUT__FILE__NAME** -告诉配置单元，我们只应从以结尾的文件返回的数据。 日志。 通常情况下，因此只需具有相同架构的数据相同文件夹中查询配置单元时，用时不过此示例日志文件与其他数据格式存储。

    > [AZURE.NOTE] 在预期基础数据更新的外部源，例如自动化的数据上载过程中，或者通过 MapReduce 的另一个操作，但却总是希望配置单元查询要使用的最新数据时，应使用外部表。
    >
    > 删除外部表 does**不**删除数据，仅表定义。
    
    此命令的输出应类似如下︰
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. 若要退出 Beeline，使用`!quit`。

##<a id="file"></a>运行 HiveQL 文件

Beeline 还可以用于运行包含 HiveQL 语句的文件。 使用以下步骤来创建一个文件，然后使用 Beeline 运行。

1. 使用下面的命令创建一个名为__query.hql__的新文件︰

        nano query.hql
        
2. 一旦编辑器中打开时，使用以下内容作为该文件的内容。 此查询将创建名为**errorLogs**的新内部表︰

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    这些语句执行以下操作︰

    * 如果不存在，则**创建表如果不存在**-创建一个表。 由于不使用**外部**关键字，则这种内部表，它存储在配置单元数据仓库和完全由配置单元。
    * **存储 AS ORC**的优化行纵栏式 (ORC) 格式存储的数据。 这是用于存储配置单元数据的高度优化和有效格式。
    * **插入覆盖...选择**-从**log4jLogs**表中包含**[错误]**，选择行，然后**errorLogs**表中插入数据。
    
    > [AZURE.NOTE] 与外部表，除去内部的表将删除基础数据。
    
3. 若要保存该文件，使用__Ctrl__+ ___X__，然后输入__Y__和最后__输入__。

4. 使用以下方法来使用 Beeline 运行该文件。 __主机名__替换为管理员帐户的密码与之前获得的头节点，和__密码__名称︰

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] `-i`参数启动 Beeline、 运行语句在 query.hql 文件中，并保留在 Beeline 在`jdbc:hive2://localhost:10001/>`提示。 您还可以运行一个文件使用`-f`参数，它返回到大扫除后已处理的文件。

5. 若要验证已创建的**errorLogs**表，请使用下面的语句从**errorLogs**返回的所有行︰

        SELECT * from errorLogs;

    三个数据的应返回的行，所有在列 t4 的包含**[错误]** :
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>有关 Beeline 连接的详细信息

在此步骤中记录的使用`localhost`连接到 HiveServer2 群集 headnode 上运行。 同时您还可以使用主机名或 headnode 的完全限定的域名称的那些需要额外的步骤，处理 （步骤查找主机名或 FQDN）。 使用`localhost`headnode 从使用 Beeline 时就足够。

如果您有边节点在群集中，安装 Beeline，您需要使用主机名或 FQDN 的 headnode 连接。

如果有 Beeline 之外您的群集客户机上安装，则可以连接使用下面的命令。 __群集名称__替换 HDInsight 群集的名称。 替换 （HTTP 登录） 使用管理员帐户的密码__密码__。

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

请注意，参数 URI 不同于直接在 headnode 上或从边缘节点在群集中运行时。 这是因为从互联网连接到群集使用公用网关路由在端口 443 上的通信量。 此外，几个其他服务公开通过公共网关端口 443 上, 这样的 URI 是不同于直接连接时。 从 internet 连接时还必须通过提供密码来验证会话。

##<a id="summary"></a><a id="nextsteps"></a>下一步行动

如您所见，Beeline 命令提供了一种以交互方式运行 HDInsight 群集配置单元查询简便方法。

HDInsight 中的配置单元的一般信息︰

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上︰

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

如果您使用 Tez 配置单元，请参阅以下文档以供调试信息︰

* [在基于 Windows 的 HDInsight 上使用 Tez 用户界面](hdinsight-debug-tez-ui.md)

* [使用基于 Linux 的 HDInsight Ambari Tez 视图](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

