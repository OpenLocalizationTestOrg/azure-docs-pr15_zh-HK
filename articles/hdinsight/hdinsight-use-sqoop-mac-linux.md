<properties
    pageTitle="在基于 Linux 的 HDInsight 中使用 Hadoop Sqoop |Microsoft Azure"
    description="了解如何运行 Sqoop 的导入和导出 SQL Azure 数据库 HDInsight 群集上基于 Linux 的 Hadoop 之间。"
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>使用 Hadoop 在 HDInsight (SSH) Sqoop

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

了解如何使用 Sqoop 来导入和导出基于 Linux 的 HDInsight 群集 SQL Azure 数据库或 SQL Server 数据库之间。

> [AZURE.NOTE] 这篇文章中的步骤使用 SSH 连接到基于 Linux 的 HDInsight 群集。 Windows 客户端可以使用 Azure PowerShell 和 HDInsight.NET SDK 基于 Linux 的群集上使用 Sqoop。 使用制表符选择器来打开这些项目。

##<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- **在 HDInsight 的 Hadoop 群集**和__SQL Azure 数据库__︰ 在此文档中的步骤基于群集创建和数据库使用[创建群集和 SQL 数据库](hdinsight-use-sqoop.md#create-cluster-and-sql-database)文档。 如果您已经有一个 HDInsight 群集和 SQL 数据库，可以替换那些在本文档中使用的值。
- **工作站**︰ SSH 客户端的计算机。

##<a name="install-freetds"></a>安装 FreeTDS

1. 使用 SSH 连接到基于 Linux 的 HDInsight 群集。 连接时要使用的地址是`CLUSTERNAME-ssh.azurehdinsight.net`，端口为`22`。

    使用 SSH 连接到 HDInsight 的详细信息，请参阅以下文档︰

    * **Linux、 Unix 或 OS X 客户端**︰[连接到基于 Linux 的 HDInsight 从 Linux、 OS X 或 Unix 群集](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)，请参阅

    * **Windows 客户端**︰[连接到基于 Linux 的 HDInsight 从 Windows 群集](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)，请参阅

3. 使用下面的命令来安装 FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS 将在几个步骤中用来连接到 SQL 数据库。

##<a name="create-the-table-in-sql-database"></a>在 SQL 数据库中创建表

> [AZURE.IMPORTANT] 如果您正在使用 HDInsight 群集和使用[创建群集和 SQL 数据库](hdinsight-use-sqoop.md)中的步骤创建的 SQL 数据库，忽略此节中的步骤，因为作为该文档中的步骤创建数据库和表。

1. 通过 SSH 连接到 HDInsight，使用下面的命令将在这些步骤的其余部分中使用的表连接到 SQL 数据库服务器和博客︰

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    您将收到类似于下面的输出︰

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. 在`1>`提示下，输入以下命令行︰

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    当`GO`输入语句时，将计算了上述语句。 首先，创建了**mobiledata**表，然后聚集的索引添加到它 （所必需的 SQL 数据库）。

    使用以下方法来验证已创建表︰

        SELECT * FROM information_schema.tables
        GO

    您应该看到类似于下面的输出︰

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. 输入`exit`在`1>`提示退出 tsql 实用程序。

##<a name="sqoop-export"></a>Sqoop 导出

3. 通过 SSH 连接到 HDInsight，se 下面的命令以验证 Sqoop，可以看到您的 SQL 数据库︰

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    这应返回的数据库，包括您在前面创建的**sqooptest**数据库的列表。

4. 使用下面的命令从**hivesampletable**到**mobiledata**表中导出数据︰

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    这将指示连接到 SQL 数据库， **sqooptest**数据库，并将数据从导出的 Sqoop **wasbs: / 配置单元/仓库/hivesampletable/** （ *hivesampletable*，物理文件） 到**mobiledata**表。

5. 在命令完成后，使用以下连接到使用 TSQL 数据库︰

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    建立连接后，可以使用以下语句以验证到**mobiledata**表中导出数据︰

        SELECT * FROM mobiledata
        GO

    您应该看到表中数据的列表。 类型`exit`退出 tsql 实用程序。

##<a name="sqoop-import"></a>Sqoop 导入

1. 使用以下方法来导入数据从 SQL 数据库中的**mobiledata**表上时，将为**wasbs: / 教程/usesqoop/importeddata/**在 HDInsight 目录︰

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    导入的数据必须由制表符分隔的字段，将新行字符的终止线。

2. 完成导入后，在新目录中使用下面的命令给出数据的列表︰

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>使用 SQL Server

您还可以使用 Sqoop 来导入和导出数据从 SQL Server，或者在您的数据中心在 Azure 托管的虚拟机上。 使用 SQL 数据库和 SQL Server 之间的差异包括︰

* HDInsight 和 SQL Server 必须位于相同的 Azure 虚拟网络

    > [AZURE.NOTE] HDInsight 支持仅基于位置的虚拟网络，并且它目前不能使用基于相似性组的虚拟网络。

    当您在您的数据中心中使用 SQL Server 时，则必须为*站点到站点*或*点到站点*配置虚拟网络。

    > [AZURE.NOTE] 为虚拟网络中**点到网站**，SQL Server 必须在 VPN 客户端配置的应用程序，可从 Azure 的虚拟网络配置**仪表板**。

    Azure 虚拟网络的详细信息，请参阅[虚拟网络概述](../virtual-network/virtual-networks-overview.md)。

* SQL Server 必须配置为允许 SQL 身份验证。 有关详细信息，请参阅[选择身份验证模式](https://msdn.microsoft.com/ms144284.aspx)

* 您可能需要配置为接受远程连接的 SQL Server。 有关详细信息，请参阅[如何解决连接到 SQL Server 数据库引擎](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* 您必须在 SQL Server 使用**SQL Server 管理 Studio**或**tsql**之类的实用程序创建的**sqooptest**数据库-使用 Azure CLI 的步骤仅适用于 SQL Azure 数据库

    要创建**mobiledata**表的 TSQL 语句相近所使用的 SQL 数据库中，除了创建 clusterd 索引-这不是必需的 SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* 当从 HDInsight 连接到 SQL Server，您可能需要使用 SQL Server 的 IP 地址，除非您已配置了域名系统 (DNS) 来解析 Azure 虚拟网络上的名称。 例如︰

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>限制

* 批量导出-基于与 Linux 的 HDInsight、 Sqoop 连接器用于将数据导出到 Microsoft SQL Server 或 SQL Azure 数据库当前不支持大容量插入。

* 属性的基于 Linux 的 HDInsight，当使用`-batch`开关执行插入时，Sqoop 将执行多个而不是批处理插入操作的插入。

##<a name="next-steps"></a>下一步行动

现在您已了解如何使用 Sqoop。 若要了解详细信息，请参阅︰

- [Oozie 使用 HDInsight][hdinsight-use-oozie]︰ 在 Oozie 工作流中使用 Sqoop 操作。
- [航班延迟使用分析数据 HDInsight][hdinsight-analyze-flight-data]︰ 使用配置单元来分析飞行延迟数据，然后使用 Sqoop 将数据导出到 SQL Azure 数据库。
- [将数据上传到 HDInsight][hdinsight-upload-data]︰ 找到其他方法，用于将数据上载到 HDInsight/Azure Blob 存储。



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
