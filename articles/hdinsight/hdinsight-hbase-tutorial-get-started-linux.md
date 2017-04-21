<properties
    pageTitle="HBase 教程︰ 开始使用 Hadoop 的基于 Linux 的 HBase 簇 |Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase 教程︰ 开始使用 HDInsight 中的基于 Linux 的 Hadoop 使用 Apache HBase 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

了解如何在 HDInsight 中创建一个 HBase 群集、 创建 HBase 表和查询表使用配置单元。 HBase 的一般信息，请参阅[HDInsight HBase 概述][hdinsight-hbase-overview]。

此文档中的信息是特定于基于 Linux 的 HDInsight 群集。 基于 Windows 群集的信息，请使用页面顶部的选项卡上选择器切换。

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>系统必备组件

本 HBase 教程之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
- [安全的 Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md)。 
- [卷曲](http://curl.haxx.se/download.html)。

### <a name="access-control-requirements"></a>访问控制要求

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>创建 HBase 群集

下面的过程使用 Azure 资源管理器模板来创建一个版本 3.4 的基于 Linux 的 HBase 群集和 Azure 存储帐户相关的默认值。 若要了解该过程和其他群集创建方法中使用的参数，请参阅[HDInsight 中的基于创建 Linux 的 Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)。

1. 请单击下面的图像可以在 Azure 的门户打开该模板。 该模板位于公钥 blob 容器中。 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. 从**自定义部署**刀片式服务器，输入以下命令︰

    - **订阅**︰ 选择 Azure 订购，将用于创建群集。
    - **资源组**︰ 创建新的 Azure 资源管理组，或者使用一个现有。
    - **位置**︰ 指定资源组中的位置。 
    - **群集名称**︰ 输入，您将创建 HBase 群集的名称。
    - **群集登录名和密码**︰ 默认登录名是**管理员**。
    - **SSH 的用户名和密码**︰ 默认的用户名是**sshuser**。  您可以重命名它。
     
    其他参数是可选的。  
    
    每个群集有 Azure Blob 存储帐户依赖项。 删除群集后，数据将保留在存储帐户。 群集默认存储帐户名称是使用"存储"附加的群集名称。 它是硬编码的模板变量部分中。
        
3. **我同意的条款和条件如以上所述**，选择，然后单击**采购**。 大约需要 20 分钟来创建一个群集。


>[AZURE.NOTE] HBase 群集被删除后，您可以创建另一个 HBase 群集使用相同的默认 blob 容器。 将选取 HBase 表在初始群集中创建新群集。 为了避免不一致的情况，我们建议您禁用 HBase 表，然后删除群集。

## <a name="create-tables-and-insert-data"></a>创建表并插入数据

可以使用 SSH 来连接到 HBase 群集，然后使用 HBase 外壳程序创建 HBase 表，插入数据和查询数据。 使用 SSH 的信息，请参阅[使用 SSH 上从 Linux、 Unix 或 OS X HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-unix.md)和[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md)。
 

对于大多数人来说，数据以表格格式显示︰

![HDInsight HBase 表格式数据][img-hbase-sample-data-tabular]

HBase 是 BigTable 的实现，在相同的数据如下所示︰

![HDInsight HBase bigtable 数据][img-hbase-sample-data-bigtable]

下一步的过程完成后，它将更有意义。  


**使用 HBase 外壳程序**

1. 从 SSH，运行以下命令︰

        hbase shell

4. 创建两列家族与 HBase:

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

    您将使用扫描命令，因为只有一行看到相同的结果。

    HBase 表架构的更多信息，请参见[HBase 架构设计简介][hbase-schema]。 更多的 HBase 命令，请参阅[Apache HBase 参考指南][hbase-quick-start]。

6. 外壳程序退出

        exit



**将批量数据装载到联系人 HBase 表**

HBase 包括数据加载到表中的几种方法。  有关详细信息，请参阅[大容量加载](http://hbase.apache.org/book.html#arch.bulk.load)。


示例数据文件已上载到公钥 blob 容器， *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*。  数据文件的内容是︰

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

1. 从 SSH，运行下面的命令来转换数据文件 StoreFiles 和存储在由 Dimporttsv.bulk.output 指定的相对路径:。  如果您是在 HBase 外壳程序中，使用 exit 命令退出。

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. 运行下面的命令以将 /example/data/storeDataFileOutput HBase 表的数据上载︰

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. 您可以打开 HBase 外壳程序中，并使用扫描命令可以列出该表的内容。



## <a name="use-hive-to-query-hbase"></a>使用查询 HBase 配置单元

您可以通过使用配置单元查询 HBase 表中的数据。 本节创建一个配置单元表映射到 HBase 表并使用它来查询 HBase 表中的数据。

1. 打开**PuTTY**，并连接到该群集。  请参阅前面步骤中的说明进行操作。
2. 打开配置单元外壳。

       hive
3. 运行下面的 HiveQL 脚本，以创建一个映射到 HBase 表的配置单元表。 请确保您已经使用 HBase 外壳程序之前运行此语句引用先前在本教程中的示例表。

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. 运行下面的 HiveQL 脚本，以查询 HBase 表中的数据︰

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>使用 HBase REST Api 使用卷曲

> [AZURE.NOTE] 当卷曲或任何其他通讯的其余部分使用 WebHCat，您必须通过 HDInsight 群集管理员提供的用户名和密码验证请求。 此外必须使用群集名称作为一部分用于向服务器发送请求统一资源标识符 (URI)。
>
> 本节中的命令，**用户名**替换用户验证到群集中，并替换**密码**与用户帐户的密码。 **群集名称**替换您的群集的名称。
>
> REST API，通过[基本身份验证](http://en.wikipedia.org/wiki/Basic_access_authentication)被安全。 始终应通过使用安全 HTTP (HTTPS)，以帮助确保您的凭据被安全地发送到服务器的请求。

1. 从命令行，使用下面的命令验证可以连接到 HDInsight 群集︰

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    您应该收到类似下面的响应︰

        {"status":"ok","version":"v1"}

    在此命令中使用的参数如下所示︰

    * **-u** -的用户名称和密码用于对请求进行身份验证。
    * **-G** -表示，这是一个 GET 请求。

2. 使用下面的命令可以列出现有的 HBase 表︰

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. 使用下面的命令来创建新的 HBase 表与列的两个系列︰

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    以 JSon 格式提供了架构。

4. 使用下面的命令来插入一些数据︰

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    必须 base64 编码-d 开关中指定的值。  在示例中︰

    - MTAwMA = =: 1000年
    - UGVyc29uYWw6TmFtZQ = =: 个人︰ 名称
    - Sm9obiBEb2xl: John Dole

    [假行键](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single)允许您插入多个 （批） 的值。

5. 使用下面的命令以获取一行︰

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

HBase 其余部分有关详细信息，请参阅[Apache HBase 参考指南](https://hbase.apache.org/book.html#_rest)。

## <a name="check-cluster-status"></a>检查群集状态

HDInsight 在 HBase 随 Web UI 用于监视群集。 使用 Web 用户界面，您可以请求统计或有关区域的信息。

此外可以使用 SSH 隧道本地请求，如 web 请求，向 HDInsight 群集。 如同它有是在 HDInsight 群集的头节点上生成，然后将所请求资源路由请求。 有关详细信息，请参阅[使用 SSH 在从 Windows HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel)。

**建立一个 SSH 隧道会话**

1. 打开**PuTTY**。  
2. 如果在创建过程中创建用户帐户时，您可以提供 SSH 密钥，您必须执行以下步骤，选择对群集进行身份验证时使用的专用密钥︰

    在**类别**中，扩展**连接**，展开**SSH**，选择**身份验证**。 最后，单击**浏览**并选择.ppk 文件，其中包含您的私钥。

3. 在**类别**中，单击**会话**。
4. 从基本选项 PuTTY 会话屏幕中，输入以下值︰

    - **主机名**︰ SSH HDInsight 服务器的主机名 （或 IP 地址） 的字段的地址。 SSH 地址为您的群集名称，则**-ssh.azurehdinsight.net**。 例如， *mycluster-ssh.azurehdinsight.net*。
    - **端口**︰ 22。 Ssh 上主的 headnode 端口是 22。  
5. 在对话框左侧的**类别**部分中，展开**连接**， **SSH**，展开，然后单击**隧道**。
6. 在控制 SSH 端口转发表单选项中提供以下信息︰

    - **源端口**--您希望转发客户端上的端口。 例如，9876。
    - **动态**-启用动态路由的 SOCKS 代理。
7. 单击**添加**以添加设置。
8. 单击**打开**对话框的底部来打开 SSH 连接。
9. 出现提示时，登录到使用 SSH 客户的服务器中。 这会建立 SSH 会话并启用隧道。

**若要查找使用 Ambari zookeepers 的 FQDN**

1. 浏览到 https://<ClusterName>.azurehdinsight.net/。
2. 两次输入您的群集用户帐户凭据。
3. 从左侧的菜单中，单击**zookeeper**。
4. 单击汇总列表中的三个**ZooKeeper 服务器**链接之一。
5. 将复制**的主机名**。 例如，zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net。

**配置客户端程序 (Firefox)，并检查群集状态**

1. 打开 Firefox。
2. 单击**打开的菜单**按钮。
3. 单击**选项**。
4. 单击**高级**，单击**网络**，然后单击**设置**。
5. 选择**手动代理服务器配置**。
6. 输入以下值︰

    - **Socks 主机**︰ 本地主机
    - **端口**︰ 使用相同的端口中 Putty SSH 隧道配置。  例如，9876。
    - **SOCKS v5**: （已选中）
    - **远程 DNS**: （已选中）
7. 单击**确定**以保存所做的更改。
8. 浏览到 http://&lt;ZooKeeper 的 FQDN >: 60010/主状态。

在高可用性群集中，您会发现一个指向当前活动 HBase 主节点承载 Web 用户界面。

##<a name="delete-the-cluster"></a>删除群集

为了避免不一致的情况，我们建议您禁用 HBase 表，然后删除群集。

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>下一步行动

在 HDInsight 本 HBase 教程中，您学习了如何创建一个 HBase 群集以及如何创建表并查看 HBase 外壳从这些表中的数据。 您还学习了如何使用配置单元查询 HBase 表中的数据以及如何使用 HBase C# REST Api 来创建一个 HBase 表，从表中检索数据。

若要了解详细信息，请参阅︰

- [HDInsight HBase 概述][hdinsight-hbase-overview]: HBase 是 Apache，开源，NoSQL 数据库建立在 Hadoop 提供大量的非结构化和 semistructured 数据随机存取和强一致性。


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
