<properties 
    pageTitle="分析与配置单元上的基于 Linux 的 HDInsight 航班延迟数据 |Microsoft Azure" 
    description="了解如何使用配置单元分析飞行数据的基于 Linux 的 HDInsight，然后将数据导出到 SQL 数据库中使用 Sqoop。" 
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

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>使用 HDInsight 中的配置单元分析航班延迟数据

了解如何使用基于 Linux 的 HDInsight 配置单元的航班延迟数据进行分析，然后将数据导出到 Azure SQL 数据库使用 Sqoop。

> [AZURE.NOTE] 虽然可以使用本文档的各个部分与基于 Windows 的 HDInsight 群集 （Python 和示例的配置单元），很多步骤是针对基于 Linux 的群集。 将使用基于 Windows 群集的步骤，请参阅[使用 HDInsight 中的配置单元分析航班延迟数据](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>系统必备组件

在开始本教程之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。

- __HDInsight 群集__。 有关创建新的基于 Linux 的 HDInsight 群集的步骤，请参阅[开始使用 Hadoop 使用 Linux 上的 HDInsight 中的配置单元](hdinsight-hadoop-linux-tutorial-get-started.md)。

- __SQL azure 数据库__。 您将使用 SQL Azure 数据库作为目标数据存储。 如果您没有一个 SQL 数据库，请参阅[SQL 数据库教程︰ 在几分钟内创建 SQL 数据库](../sql-database/sql-database-get-started.md)。

- __Azure CLI__。 如果您没有安装 Azure CLI，请参见[安装和配置 Azure CLI](../xplat-cli-install.md)执行更多步骤。

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>飞行数据下载

1. 浏览到[研究和创新的技术管理，局运输统计的][rita-website]。
2. 在页上，选择以下值︰

  	| 名称 | 值 |
  	| ---- | ---- |
  	| 筛选器年 | 2013 |
  	| 过滤周期 | 1 月 |
  	| 字段 | 年，FlightDate、 UniqueCarrier、 承运人、 FlightNum、 OriginAirportID、 来源、 OriginCityName、 OriginState、 DestAirportID、 目标、 DestCityName、 DestState、 DepDelayMinutes、 ArrDelay、 ArrDelayMinutes、 CarrierDelay、 WeatherDelay、 NASDelay、 SecurityDelay、 LateAircraftDelay。 清除所有其他字段 |

3. 单击**下载**。 

##<a name="upload-the-data"></a>上载数据

1. 使用下面的命令将该 zip 文件上传到 HDInsight 群集的头节点︰

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Zip 文件的名称替换__文件名__。 替换为 HDInsight 群集的 SSH 登录__用户名__。 群集名称替换 HDInsight 群集的名称。
    
    > [AZURE.NOTE] 如果您使用密码来验证您的 SSH 登录，系统将提示您输入密码。 如果您使用公用密钥，您可能需要使用`-i`参数指定的路径匹配的私钥。 例如`scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`。

2. 完成上载后，连接到使用 SSH 的群集︰

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    在 SSH 中使用基于 Linux 的 HDInsight 的详细信息，请参阅下列文章︰
    
    * [HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [在从 Windows HDInsight 基于 Linux 的 Hadoop 使用 SSH](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. 建立连接后，使用以下方法来解压缩.zip 文件︰

        unzip FILENAME.zip
    
    这将解压缩的大小大约为 60 MB 的.csv 文件。
    
4. 使用以下方法来创建一个新目录，在 WASB （HDInsight，使用的分布式的数据存储） 和复制文件︰

    hdfs dfs 的 mkdir-p /tutorials/flightdelays/data hdfs dfs-放入 FILENAME.csv/教程/flightdelays/数据 /
    
##<a name="create-and-run-the-hiveql"></a>创建并运行 HiveQL

使用以下步骤来将数据从 CSV 文件导入配置单元表名为__延迟__。

1. 使用以下方法来创建和编辑名为__flightdelays.hql__的新文件︰

        nano flightdelays.hql
        
    使用以下内容作为本文件的内容︰
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. 使用__Ctrl + X__，然后__Y__若要保存该文件。

3. 使用以下方法来开始配置单元并运行__flightdelays.hql__文件︰

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] 在此示例中，`localhost`以来您连接到已在运行 HiveServer2 HDInsight 群集的头节点使用。

4. 使用下面的命令来打开交互式 Beeline 会话︰

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. 当您收到`jdbc:hive2://localhost:10001/>`提示，使用以下方法来导入的航班延迟数据中检索数据。

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    这将检索城市遇到天气延迟，平均延迟时间，并列表并保存到`/tutorials/flightdelays/output`。 随后，Sqoop 将从该位置读取数据并将其导出到 SQL Azure 数据库。

6. 若要退出 Beeline，输入`!quit`在提示符处。

## <a name="create-a-sql-database"></a>创建 SQL 数据库

如果您已经有一个 SQL 数据库，您必须获取服务器的名称。 您可以在[Azure 门户网站](https://portal.azure.com)找到这选择__SQL 数据库__，然后在您想要使用的数据库的名称上筛选。 __服务器__列中列出的服务器名称。

如果没有 SQL 数据库，使用中的信息[SQL 数据库教程︰ 在几分钟内创建 SQL 数据库](../sql-database/sql-database-get-started.md)创建一个。 您将需要保存用于数据库的服务器名称。

##<a name="create-a-sql-database-table"></a>创建 SQL 数据库表

> [AZURE.NOTE] 有许多方法来连接到 SQL 数据库创建表。 下列步骤将从 HDInsight 群集使用[FreeTDS](http://www.freetds.org/) 。

1. 使用 SSH 连接到基于 Linux 的 HDInsight 群集，以及从 SSH 会话运行以下步骤。

3. 使用下面的命令来安装 FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. 一旦安装 FreeTDS 后，使用下面的命令来连接到 SQL 数据库服务器。 __服务器名称__替换 SQL 数据库服务器的名称。 为 SQL 数据库的登录更换__adminLogin__和__adminPassword__ 。 __数据库名称__替换为数据库的名称。

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    您将收到类似于下面的输出︰

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. 在`1>`提示下，输入以下命令行︰

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    当`GO`输入语句时，将计算了上述语句。 这将创建新的表名__的延迟__，为具有一个聚集索引 （所必需的 SQL 数据库）。

    使用以下方法来验证已创建表︰

        SELECT * FROM information_schema.tables
        GO

    您应该看到类似于下面的输出︰

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. 输入`exit`在`1>`提示退出 tsql 实用程序。
    
##<a name="export-data-with-sqoop"></a>导出数据时包含 Sqoop

2. 使用下面的命令以验证 Sqoop，可以看到您的 SQL 数据库︰

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    这应返回的数据库，包括先前创建的延迟表中的数据库的列表。

3. 使用下面的命令从 hivesampletable 到 mobiledata 表中导出数据︰

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    这将指示连接到 SQL 数据库中，数据库包含的延迟，并从 wasbs 中导出数据的 Sqoop: / / 教程/flightdelays/（其中我们存储更早版本，配置单元查询的输出中） 到输出延迟表。

4. 在命令完成后，使用以下连接到使用 TSQL 数据库︰

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    建立连接后，可以使用以下语句以验证到 mobiledata 表中导出数据︰
    
        SELECT * FROM delays
        GO

    您应该看到表中数据的列表。 类型`exit`退出 tsql 实用程序。

##<a id="nextsteps"></a>下一步行动

现在您了解了如何将文件上载到 Azure Blob 存储、 如何使用 Azure Blob 存储中的数据填充配置单元表、 如何运行配置单元查询和如何使用 Sqoop 来 HDFS 数据导出到 SQL Azure 数据库。 若要了解详细信息，请参阅下列文章︰

* [要开始使用 HDInsight][hdinsight-get-started]
* [使用 HDInsight 配置单元][hdinsight-use-hive]
* [HDInsight 使用 Oozie][hdinsight-use-oozie]
* [使用 HDInsight Sqoop][hdinsight-use-sqoop]
* [使用 HDInsight 的小猪][hdinsight-use-pig]
* [HDInsight 为开发 Java MapReduce 程序][hdinsight-develop-mapreduce]
* [开发 Python Hadoop 流为 HDInsight 的程序][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
