<properties 
   pageTitle="数据湖存储和使用 Sqoop 的 SQL Azure 数据库之间复制数据 |Microsoft Azure"
   description="使用 Sqoop 将 SQL Azure 数据库和数据湖存储之间的数据复制" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>数据湖存储和使用 Sqoop 的 SQL Azure 数据库之间复制数据

了解如何使用 Apache Sqoop 来导入和导出 SQL Azure 数据库和数据湖存储之间的数据。
 

## <a name="what-is-sqoop"></a>Sqoop 是什么？

大数据应用程序是处理非结构化和半结构化数据，如日志文件和文件的自然之选。 但是，也可能有需要处理结构化的数据存储在关系数据库中。

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html)是设计关系数据库和一个大数据存储库中的，如湖数据存储之间传输数据的工具。 可以使用关系数据库管理系统 (RDBMS) 如 SQL Azure 数据库的数据导入数据湖存储。 可以然后转换和分析使用大数据工作负载的数据，然后回 RDBMS 中导出数据。 在本教程中，您使用 Azure SQL 数据库为您关系数据库从导入/导出。
 

## <a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
- **启用 Azure 的订阅**数据湖存储公共预览。 请参阅[说明](data-lake-store-get-started-portal.md#signup)。 
- 有权访问数据湖存储帐户**azure HDInsight 群集**。 请参阅[创建与数据湖商店 HDInsight 群集](data-lake-store-hdinsight-hadoop-use-portal.md)。 这篇文章假定您已经有数据湖存储访问 HDInsight Linux 群集。
- **SQL azure 数据库**。 说明如何创建，请参阅[创建 SQL Azure 数据库](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>请与视频快速学习吗？

[观看这段视频](https://mix.office.com/watch/1butcdjxmu114)如何将 Azure 存储 Blob 数据湖存储之间的数据复制使用 DistCp。

## <a name="create-sample-tables-in-the-azure-sql-database"></a>在 SQL Azure 数据库中创建示例表

1. 启动时，SQL Azure 数据库中创建两个示例表。 使用[SQL Server 管理 Studio](../sql-database/sql-database-connect-query-ssms.md)或 Visual Studio 连接到 SQL Azure 数据库，然后运行以下查询。

    **创建表 1**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **创建表 2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. 在**表 1**中，将添加一些示例数据。 将**表 2**保留为空。 我们会将数据从**表 1**导入数据湖存储区。 然后，我们将数据从存储中导出数据湖到**表 2**。 运行下面的代码段。

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Sqoop 从访问 HDInsight 群集使用到湖边的数据存储

HDInsight 群集已有可用的 Sqoop 软件包。 如果您已配置 HDInsight 群集数据湖存储区用作附加存储空间，可用于 Sqoop （无需任何配置更改） 导入/导出关系数据库 （在此示例中，SQL Azure 数据库） 之间的数据和数据湖存储帐户。 

1. 在本教程中，我们假定您创建 Linux 群集，因此您应该使用 SSH 连接到群集。 请参阅[连接到一个基于 Linux 的 HDInsight 群集](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)。

2. 验证是否可以从群集中访问数据湖存储帐户。 在 SSH 提示符下运行以下命令︰

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    这将提供数据湖存储帐户的文件/文件夹的列表。

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>将数据从 SQL Azure 数据库导入数据湖存储区

3. 定位到 Sqoop 包可用的目录。 通常情况下，这将为`/usr/hdp/<version>/sqoop/bin`。 

4. **表 1**中的数据导入数据湖存储帐户。 使用下面的语法︰

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    请注意， **sql 数据库服务器名称**的占位符表示正在运行的 SQL Azure 数据库服务器的名称。 **sql 数据库名称**的占位符表示实际的数据库名称。

    例如，

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. 确认数据已被转移到数据湖存储帐户。 运行以下命令︰

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    您应看到下面的输出。

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    每个**部件-m-** *文件对应于源表中的行*的*表 1**。您可以查看该部件的内容-m-*文件来验证。


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>将数据从数据湖存储区导出到 SQL Azure 数据库

6. 从数据湖存储帐户的将数据导出到空表，**表 2**，SQL Azure 数据库中。 使用以下语法。

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    例如，

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. 确认数据已上载到 SQL 数据库表。 使用[SQL Server 管理 Studio](../sql-database/sql-database-connect-query-ssms.md)或 Visual Studio 连接到 SQL Azure 数据库，然后运行下面的查询。

        
        SELECT * FROM TABLE2

    这应该有下面的输出。

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>请参见

- [将数据复制到数据湖商店 Azure 存储 Blob](data-lake-store-copy-data-azure-storage-blob.md)
- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)
- [与数据湖商店使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
