<properties 
    pageTitle="并行使用 SQL 表进行分区的批量数据导入 |Microsoft Azure" 
    description="使用 SQL 表进行分区的并行批量数据导入" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>使用 SQL 表进行分区的并行批量数据导入

本文档介绍如何构建快速并行批量导入到 SQL Server 数据库的数据的分区的表。 对于大数据加载/转让到 SQL 数据库，将数据导入到 SQL 数据库和随后的查询可以提高使用_分区表和视图_。 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>创建新的数据库和文件组一套

- [创建新的数据库](https://technet.microsoft.com/library/ms176061.aspx)（如果不存在）
- 将数据库文件组添加到数据库将保留分区的物理文件

  注意︰ 可以[创建数据库](https://technet.microsoft.com/library/ms176061.aspx)如果新或[更改数据库](https://msdn.microsoft.com/library/bb522682.aspx)如果数据库已经存在

- 为每个数据库的文件组中添加一个或多个文件 （根据需要）

 > [AZURE.NOTE] 指定其中包含此分区数据的目标文件组和文件组数据的存储位置的物理数据库文件名称。
 
下面的示例创建一个新数据库具有三种非主文件组和日志组，包含在每一个物理文件。 在 SQL Server 实例配置为在默认的 SQL Server 数据文件夹，创建数据库文件。 有关默认文件位置的详细信息，请参阅[文件位置的默认实例和命名实例的 SQL Server](https://msdn.microsoft.com/library/ms143547.aspx)。

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>创建已分区的表

创建已分区的表根据映射到在先前步骤中创建的数据库文件组的数据架构。 批量导入到已分区表的数据时，记录将分布在分区方案，根据文件组中，如下所述。

**若要创建分区表，您需要︰**

- [创建分区函数](https://msdn.microsoft.com/library/ms187802.aspx)定义的值/边界包含在每个单个分区表，例如，以按月限制分区范围 (有些\_日期时间\_字段) 在 2013 年中︰

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [创建分区方案](https://msdn.microsoft.com/library/ms179854.aspx)将分区函数中的每个分区范围映射到物理文件组，如︰

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  提示︰ 要验证根据函数方案的每个分区中起作用的范围，请运行下面的查询︰

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [创建已分区的表](https://msdn.microsoft.com/library/ms174979.aspx)（s） 根据为您的数据的架构，并指定用于划分表，例如分区方案和约束字段︰

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

有关详细信息，请参阅[创建分区表和索引](https://msdn.microsoft.com/library/ms188730.aspx)。


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>批量导入的每个单个分区表的数据

- 您可以使用 BCP、 批量插入，或其他方法，例如[SQL Server 迁移向导](http://sqlazuremw.codeplex.com/)。 提供的示例使用 BCP 方法。

- [改变数据库](https://msdn.microsoft.com/library/bb522682.aspx)事务日志记录方案更改为 BULK_LOGGED 尽量减少开销记录，例如︰

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- 若要加快加载数据，启动并行的批量导入操作。 加快大容量导入到 SQL Server 数据库大数据的提示，请参阅[负载 1 TB，在不到 1 小时](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx)。

下面的 PowerShell 脚本是一种并行数据装载使用 BCP。

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>创建索引以优化联接和查询性能

- 如果您将来自多个表中提取数据进行建模，联接键以提高连接性能上创建索引。

- [创建索引](https://technet.microsoft.com/library/ms188783.aspx)（群集或非群集） 设定为每个分区中，同一个文件组的例如︰

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
或者，

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] 您可以选择创建索引之前批量导入数据。 批量导入之前创建索引将减慢数据加载。


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>高级分析功能流程和技术操作示例

使用公共数据集 Cortana 分析流程的端到端演练示例，请参见[Cortana 分析过程中的操作︰ 使用 SQL Server](machine-learning-data-science-process-sql-walkthrough.md)。
 
