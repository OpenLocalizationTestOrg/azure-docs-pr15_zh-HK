<properties
   pageTitle="将数据与 WASB 复制到使用 Distcp 数据湖存储 |Microsoft Azure"
   description="使用 Distcp 工具到湖边的数据存储到 Azure 存储 Blob 与复制数据"
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

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>使用 Distcp 将 Azure 存储 Blob 数据湖存储之间的数据复制

> [AZURE.SELECTOR]
- [使用 DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [使用 AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)


一旦您创建了 HDInsight 拥有群集的数据湖存储帐户的访问权限，可以使用 Hadoop 生态系统工具，例如 Distcp 将数据**与**HDInsight 群集存储 (WASB) 复制到数据湖存储帐户。 本文说明如何实现这一点。

##<a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
- **启用 Azure 的订阅**数据湖存储公共预览。 请参阅[说明](data-lake-store-get-started-portal.md#signup)。
- 有权访问数据湖存储帐户**azure HDInsight 群集**。 请参阅[创建与数据湖商店 HDInsight 群集](data-lake-store-hdinsight-hadoop-use-portal.md)。 请确保您为群集启用远程桌面。

## <a name="do-you-learn-fast-with-videos"></a>请与视频快速学习吗？

[观看这段视频](https://mix.office.com/watch/1liuojvdx6sie)如何将 Azure 存储 Blob 数据湖存储之间的数据复制使用 DistCp。

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>使用远程桌面 （Windows 群集） 或 SSH （Linux 群集） Distcp

HDInsight 群集是通过 Distcp 实用程序，用于将来自不同源的数据复制到 HDInsight 群集。 如果您已配置 HDInsight 群集数据湖存储区用作附加存储空间，Distcp 实用程序可以使用的的现成数据复制和数据湖存储帐户。 在这一节我们看如何使用 Distcp 实用程序。

1. 如果您有 Windows 群集，远程进入 HDInsight 群集具有数据湖存储帐户的访问权限。 有关说明，请参阅[连接到群集使用 RDP](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)。 从群集桌面，打开 Hadoop 命令行。

    如果您有一个 Linux 群集，使用 SSH 连接到群集。 请参阅[连接到一个基于 Linux 的 HDInsight 群集](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)。 在 SSH 提示符下运行命令。

3. 请验证您是否可以访问 Azure 存储 Blob (WASB)。 运行以下命令︰

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    这将提供存储 blob 中的目录列表。

4. 同样，验证是否可以从群集中访问数据湖存储帐户。 运行以下命令︰

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    这将提供数据湖存储帐户的文件/文件夹的列表。

5. 使用 Distcp 来将数据从 WASB 复制到数据湖存储帐户。

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    这会将 WASB 中**的/示例/数据/谷登**堡/文件夹中的内容复制到**/myfolder**数据湖存储帐户中。

6. 同样，可以使用 Distcp 来将数据从数据湖存储帐户复制到 WASB。

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    这会将**/myfolder**中的内容数据湖存储帐户中复制**到/示例/数据/谷登**堡/WASB 中的文件夹。

## <a name="see-also"></a>请参见

- [将数据复制到数据湖商店 Azure 存储 Blob](data-lake-store-copy-data-azure-storage-blob.md)
- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)
- [与数据湖商店使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
