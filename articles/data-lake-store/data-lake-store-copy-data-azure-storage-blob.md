<properties
   pageTitle="Azure 存储 Blob 中的数据复制到数据湖存储 |Microsoft Azure"
   description="使用 AdlCopy 工具将从 Azure 存储 Blob 数据复制到湖边的数据存储"
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>将数据复制到数据湖商店 Azure 存储 Blob

> [AZURE.SELECTOR]
- [使用 DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [使用 AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

Azure 数据湖商店提供了一个命令行工具， [AdlCopy](http://aka.ms/downloadadlcopy)，可从以下来源复制数据︰

* 从 Azure 存储入湖存储数据的 Blob。 您不能使用 AdlCopy 将数据从数据湖存储区复制到 Azure 存储 blob。

* 间两个 Azure 数据湖存储帐户。 

此外，可以使用 AdlCopy 工具在两个不同的模式︰

* **独立**，该工具在使用数据湖存储资源执行任务。

* **使用数据湖分析帐户**，其中分配给数据湖分析帐户单位用于执行复制操作。 您可能希望查找以可预测的方式执行复制任务时请使用此选项。

##<a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

- 某些数据使用**azure 存储 Blob**容器。

- **Azure 数据湖分析帐户 （可选）** -请参阅[开始使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)有关如何创建数据湖存储帐户的说明。

- **AdlCopy 工具**。 从[http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy)上安装 AdlCopy 工具。

## <a name="syntax-of-the-adlcopy-tool"></a>AdlCopy 工具的语法

使用下面的语法使用 AdlCopy 工具

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

在语法中的参数如下所述︰

| 选项    | 说明                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| 来源    | 在 Azure 存储 blob 中指定的源数据的位置。 源可以是 blob 容器、 斑点或其他数据湖存储帐户。                                                                                                                                                                                                                                                                                                 |
| 目标      | 指定要复制到的数据湖存储目标。                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | 指定的 Azure 存储 blob 源的存储访问键。 这是必需的只有当源为 blob 容器或 blob。                                                                                                                                                                                                                                                                                                                                                 |
| 帐户   | **可选**。 如果您想要使用 Azure 数据湖分析帐户运行复制作业，请使用此选项。 如果在语法中使用 /Account 选项，但未指定数据湖分析帐户，AdlCopy 将使用默认帐户来运行该作业。 而且，如果您使用此选项，您必须添加的源 (Azure 存储 Blob) 和目标 （Azure 数据湖商店） 作为数据源数据湖分析帐户。  |
| 单位     |  指定将用于复制作业的数据湖分析单位的数目。 此选项是必需的如果您使用**/Account**选项指定数据湖分析帐户。
| 模式   | 指定指示哪些 blob 或要复制的文件的正则表达式模式。 AdlCopy 使用区分大小写的匹配。 不指定任何模式时使用的默认模式是复制所有项目。 不支持指定多个文件模式。                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>使用 （作为独立） AdlCopy Azure 存储 blob 中的数据复制

1. 打开命令提示窗口，然后定位到 AdlCopy 的安装的目录，通常`%HOMEPATH%\Documents\adlcopy`。

2. 运行下面的命令以将特定 blob 从源容器复制到数据湖存储︰

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    例如︰

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] 上面的语法指定要复制到的文件夹中的数据湖存储帐户的文件。 AdlCopy 工具创建一个文件夹，如果不存在指定的文件夹名。

    系统将提示您输入身份凭证必须依据数据湖存储帐户的 Azure 订阅。 您将看到类似于下面的输出︰

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. 您还可以从一个容器复制所有 blob 数据湖存储帐户使用下面的命令︰

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    例如︰

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>作为 AdlCopy （独立） 将数据从其他数据湖存储帐户复制

此外可以使用 AdlCopy 来将两个数据湖存储帐户之间的数据复制。

1. 打开命令提示窗口，然后定位到 AdlCopy 的安装的目录，通常`%HOMEPATH%\Documents\adlcopy`。

2. 运行下面的命令以将特定文件复制到另一个数据湖存储帐户。

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    例如︰

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] 上面的语法指定要复制到的文件夹中的目标数据湖存储帐户的文件。 AdlCopy 工具创建一个文件夹，如果不存在指定的文件夹名。

    系统将提示您输入身份凭证必须依据数据湖存储帐户的 Azure 订阅。 您将看到类似于下面的输出︰

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. 以下命令复制源数据湖存储帐户中的特定文件夹到一个文件夹中的目标数据湖存储帐户所有文件。

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>使用 AdlCopy （带数据湖分析帐户） 复制数据

您可以使用数据湖分析帐户运行 AdlCopy 作业 Azure 存储 blob 数据复制到湖边的数据存储。 千兆字节和万亿字节，范围要移动的数据时，通常将使用此选项，并且您想更好的和可预测性能的吞吐量。

要使用 AdlCopy 数据湖分析帐户从 Azure 存储 Blob 复制，必须将源 (Azure 存储 Blob) 添加作为数据源数据湖分析帐户。 将其他数据源添加到数据湖分析帐户的说明，请参阅[管理数据湖分析客户数据源](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources)。

>[AZURE.NOTE] 如果要使用数据湖分析帐户的源复制从 Azure 数据湖存储帐户，您不需要数据湖分析帐户相关联的数据湖存储帐户。 要数据湖分析帐户相关联的源存储的要求是仅当源是 Azure 存储帐户。

运行下面的命令将从 Azure 存储 blob 复制到使用数据湖分析帐户数据湖存储帐户︰

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

例如︰

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


同样，运行下面的命令将从 Azure 存储 blob 复制到使用数据湖分析帐户数据湖存储帐户︰

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>使用 AdlCopy 将使用模式匹配的数据复制

在本节中，您将学习如何使用 AdlCopy 将数据复制到来源 （在以下示例中，我们使用 Azure 存储 Blob） 到使用模式匹配的目标数据湖存储帐户。 例如，可以使用下面的步骤将由源块的具有.csv 扩展名的所有文件复制到目标。

1. 打开命令提示窗口，然后定位到 AdlCopy 的安装的目录，通常`%HOMEPATH%\Documents\adlcopy`。

2. 运行下面的命令以从源容器 blob 数据湖存储到特定复制 *.csv 扩展名的所有文件︰

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    例如︰

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>帐单

* 如果您使用 AdlCopy 工具作为独立您将货款出口成本移动数据，如果源 Azure 存储帐户不在同一区域作为数据湖存储区。

* 如果您使用数据湖分析帐户使用 AdlCopy 工具，将应用标准[数据湖分析记帐费率](https://azure.microsoft.com/pricing/details/data-lake-analytics/)。

## <a name="considerations-for-using-adlcopy"></a>使用 AdlCopy 的注意事项

* AdlCopy （对于 1.0.5 版），支持共同拥有超过上千个文件和文件夹的源中的数据复制。 但是，如果遇到拷贝大数据集问题，您可以分发到不同的子文件夹的文件/文件夹而使用路径对这些子文件夹作为源。

## <a name="next-steps"></a>下一步行动

- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)
- [与数据湖商店使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
