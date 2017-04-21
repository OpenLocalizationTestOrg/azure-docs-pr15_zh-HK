<properties
    pageTitle="HBase 教程︰ 开始使用 HBase 中 Hadoop |Microsoft Azure"
    description="按照此 HBase 教程来开始使用 Hadoop 在 HDInsight 中使用 Apache HBase。 从 HBase 外壳程序中创建的表和查询它们使用配置单元。"
    keywords="apache hbase，hbase，hbase 外壳，hbase 教程"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase 教程︰ 开始使用 HDInsight 中的基于 Windows 的 Hadoop 使用 Apache HBase

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

了解如何在 HDInsight 中创建 HBase 群集、 创建 HBase 表和查询的表使用 Apache 配置单元。 HBase 的一般信息，请参阅[HDInsight HBase 概述][hdinsight-hbase-overview]。

此文档中的信息是特定于基于 Windows HDInsight 群集。 基于 Windows 群集的信息，请使用页面顶部的选项卡上选择器切换。

> [AZURE.NOTE] HBase （0.98.0 版） 上的基于 Windows 的 HDInsight 功能仅适用于与 HDInsight 3.1 群集一起使用 （基于 Apache Hadoop 和 YARN 2.4.0）。 版本信息，请参阅[由 HDInsight 提供的 Hadoop 群集版本中的新增功能？][hdinsight-versions]

## <a name="before-you-begin"></a>在开始之前

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

本 HBase 教程之前，您必须具有以下︰

- **订阅 Microsoft Azure**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- **工作站**与 Visual Studio 2013年或更高版本︰ 有关说明，请参阅[安装 Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx)。

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>创建 HBase 群集

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**若要通过使用 Azure 门户创建一个 HBase 群集**

1. 登录到[Azure 的门户网站][azure-management-portal]。
2. 单击**新建**或**+**在左上角，，然后单击**数据 + 分析**， **HDInsight**。
3. 输入以下值︰

    - **群集名称**-输入一个名称来标识该群集。
    - **群集类型**-选择**HBase**。
    - **群集操作系统**-选择**窗口**。  用于创建基于 Linux 的 HBase 群集，请参阅[HBase 教程︰ 开始使用 Hadoop HDInsight (Linux) 中使用 Apache HBase](hdinsight-hbase-tutorial-get-started-linux.md)。
    - **版本**的选择 HBase 版本。
    - **预订**-选择您用于创建该群集的 Azure 订购。
    - **资源组**-创建一个新的 Azure 的资源组，或选择一个现有。 有关详细信息，请参阅[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)
    - **凭据**-对于基于 Windows 群集，您可以创建群集用户 （a.k.a HTTP 用户，HTTP web 服务用户） 和远程桌面用户。 单击**启用远程桌面**添加远程桌面用户凭据。 下一节需要 RDP。
    - **数据源**-创建一个新的 Azure 存储帐户，或选择一个现有的 Azure 存储帐户用作群集的默认文件系统。 默认存储帐户位置确定群集位置的位置。 默认的存储帐户和群集必须共同找到在同一个数据中心。
    - **节点定价层**-选择区域 HBase 群集的服务器的数量

        > [AZURE.WARNING] 对于 HBase 服务的高可用性，您必须创建的群集至少包含**三个**节点。 这可以确保，如果一个节点出现故障，HBase 数据区域内的其他节点上可用。

        > 如果学习 HBase，始终选择 1 群集大小，和降低的成本每次使用后删除群集。

    - **可选配置**-配置 Azure 虚拟网络配置脚本操作并添加更多存储帐户。

4. 单击**创建**。

>[AZURE.NOTE] HBase 群集被删除后，您可以创建另一个 HBase 群集使用相同的默认存储帐户和默认 blob 容器。 将选取 HBase 表在初始群集中创建新群集。 为了避免不一致的情况，我们建议您禁用 HBase 表，然后删除群集。

## <a name="create-tables-and-insert-data"></a>创建表并插入数据

目前，有两种方式访问 HBase。 本节介绍如何使用 HBase 外壳。 下一节介绍如何使用.NET SDK。

对于大多数人来说，数据以表格格式显示︰

![hdinsight hbase 表格式数据][img-hbase-sample-data-tabular]

HBase 是 BigTable 的实现，在相同的数据如下所示︰

![hdinsight hbase bigtable 数据][img-hbase-sample-data-bigtable]

下一步的过程完成后，这会使更有意义。  

**使用 HBase 外壳程序**

1. 使用 RDP 连接到 HDInsight 您 HBase 的群集。 RDP 的说明，请参阅[管理 Hadoop 群集中使用 Azure 门户网站 HDInsight][hdinsight-manage-portal]。
2. 在 RDP 会话，请单击位于桌面上的**Hadoop 命令行**快捷方式。
3. 打开 HBase 外壳︰

        cd %HBASE_HOME%\bin
        hbase shell

4. 创建两个列家族与 HBase:

        create 'Contacts', 'Personal', 'Office'
        list
5. 插入一些数据︰

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase 外壳][img-hbase-shell]

6. 获取单个行

        get 'Contacts', '1000'

    您将看到相同的结果，与使用扫描命令，因为只有一行。

    Hbase 表架构的更多信息，请参见[HBase 架构设计简介][hbase-schema]。 更多的 HBase 命令，请参阅[Apache HBase 参考指南][hbase-quick-start]。


6. 外壳程序退出

        exit

**将批量数据装载到联系人 HBase 表**

HBase 包括数据加载到表中的几种方法。 有关详细信息，请参阅[大容量加载](http://hbase.apache.org/book.html#arch.bulk.load)。


示例数据文件已上载到公钥 blob 容器， wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt。 数据文件的内容是︰

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

您可以创建一个文本文件和如果您希望将文件上载到您自己的存储帐户。 有关说明，请参阅[Hadoop 作业中 HDInsight 的数据上载][hdinsight-upload-data]。

> [AZURE.NOTE] 此过程将使用最后一个过程中创建的联系人 HBase 表。

1. 在 RDP 会话，请单击位于桌面上的**Hadoop 命令行**快捷方式。
2. 更改目录︰

        cd %HBASE_HOME%\bin

3. 运行下面的命令来转换数据文件 StoreFiles 和存储在由 Dimporttsv.bulk.output 指定的相对路径︰

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. 运行下面的命令以将 /example/data/storeDataFileOutput HBase 表的数据上载︰

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. 您可以打开 HBase 外壳程序中，并使用扫描命令可以列出该表的内容。



## <a name="use-hive-to-query-hbase-tables"></a>使用查询 HBase 表配置单元

您可以查询通过使用配置单元 HBase 中存储的数据。 本节创建一个配置单元表映射到 HBase 表并使用它来查询 HBase 表中的数据。

**若要打开群集面板**

1. 浏览到**https://<HDInsight Cluster Name>.azurehdinsight.net/**。
5. 输入的 Hadoop 用户帐户的用户名和密码。 默认的用户名是**admin** ，密码是您在创建过程中输入的内容。 打开新的浏览器选项卡。
6. 单击**配置单元编辑器**顶部的页。 配置单元编辑器如下所示︰

    ![HDInsight 群集的仪表板。][img-hdinsight-hbase-hive-editor]

**运行配置单元查询**

1. 在配置单元编辑器输入下面的 HiveQL 脚本并单击**提交**以创建配置单元映射到 HBase 表的表。 请确保您创建使用 HBase 外壳程序之前运行此语句引用先前在本教程中的示例表。

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    等到**完成**的**状态**更新。

2. 在配置单元的编辑器输入下面的 HiveQL 脚本，然后单击**提交**。 配置单元查询查询 HBase 表中的数据︰

        SELECT count(*) FROM hbasecontacts;

4. 若要检索配置单元查询的结果，请单击**作业会话**窗口中的**查看详细信息**链接，当运行该作业完成后。 将只有一个作业输出文件，因为您将一条记录放入 HBase 表。




**浏览输出文件**

1. 在查询控制台中，单击**文件浏览器**。
2. 单击用作 HBase 群集的默认文件系统的 Azure 存储帐户。
3. 单击 HBase 群集名称。 默认 Azure 存储帐户容器使用的群集名称。
4. 单击**用户**，然后单击**管理**。 （这是 Hadoop 用户名）。
6. 单击作业名称匹配时选择配置单元查询运行的时间**最后一次修改**的时间。
4. 单击**标准输出**。 保存文件并使用记事本打开该文件。 将会有一个输出文件。

    ![HDInsight HBase 配置单元编辑器文件浏览器][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>使用 HBase REST API，.NET 客户端库

您必须从 GitHub 的.net 下载 HBase REST API，客户端库和生成项目，以便可以使用 HBase.NET SDK。 下面的过程包含此任务的说明。

1. 创建一个新的 C# Visual Studio Windows 桌面控制台应用程序。
2. 通过单击**工具**打开 NuGet 程序包管理器控制台 > **NuGet 程序包管理器** > **程序包管理器控制台**。
3. 在控制台中运行下面的 NuGet 命令︰

        Install-Package Microsoft.HBase.Client

5. 将以下**using**语句添加在文件的顶部︰

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. 用以下内容替换**Main**函数︰

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. 在**Main**函数中设置前三个变量。
8. 按**f5 键**以运行该应用程序。

## <a name="check-cluster-status"></a>检查群集状态

HDInsight 在 HBase 随 Web UI 用于监视群集。 使用 Web 用户界面，您可以请求统计或有关区域的信息。

若要打开 Web 用户界面，您必须 RDP 到该群集，然后单击 HMaster 信息 Web 用户界面快捷在桌面上，或在 web 浏览器中使用以下 URL:

    http://zookeeper[0-2]:60010/master-status

在高可用性群集中，可以找到指向当前活动 HBase 主节点承载 Web 用户界面。

##<a name="delete-the-cluster"></a>删除群集
为了避免不一致的情况，我们建议您禁用 HBase 表，然后删除群集。
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>下一步是什么？
在 HDInsight 本 HBase 教程中，您学习了如何创建一个 HBase 群集以及如何创建表并查看 HBase 外壳从这些表中的数据。 您还学习了如何使用 HBase 表和如何使用 HBase C# REST Api 来创建一个 HBase 表，从表中检索数据中的数据的配置单元查询。

有关详细信息，请参阅︰

- [HDInsight HBase 概述][hdinsight-hbase-overview]。
HBase 是建立在大量的非结构化和 semistructured 数据提供随机访问和强一致性的 Hadoop Apache，开源，NoSQL 数据库。
- [在 Azure 虚拟网络上创建 HBase 群集][hdinsight-hbase-provision-vnet]。
通过虚拟网络集成，HBase 群集可以部署到与应用程序相同的虚拟网络，以便应用程序可以与 HBase 直接通信。
- [在 HDInsight 配置 HBase 复制](hdinsight-hbase-geo-replication.md)。 了解如何在两个 Azure 数据中心配置 HBase 复制。
- [分析 Twitter 主观意图与 HBase 中 HDInsight][hbase-twitter-sentiment]。
了解如何通过 HDInsight 在 Hadoop 群集中使用 HBase 做到实时[观点分析](http://en.wikipedia.org/wiki/Sentiment_analysis)的重要数据。

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
