<properties
   pageTitle="学习如何使用数据湖存储区使用跨平台命令行界面 |Microsoft Azure"
   description="使用 Azure 的跨平台命令行来创建数据湖存储帐户并执行基本的操作"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>学习如何使用 Azure 数据湖存储区使用 Azure 命令行

> [AZURE.SELECTOR]
- [门户网站](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API，](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

了解如何使用 Azure 的命令行界面来创建 Azure 数据湖存储帐户并执行基本的操作，如创建文件夹，上载和下载的数据文件，请删除您的帐户，等等。有关数据湖商店的详细信息，请参阅[数据湖商店的概述](data-lake-store-overview.md)。

在 Node.js 实现 Azure CLI。 可以在任何支持 Node.js，包括 Windows、 Mac 和 Linux 的平台上使用它。 Azure CLI 是开源。 在<a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>情况下，源代码管理在 GitHub 中。 本文介绍如何只与数据湖商店使用 Azure CLI。 有关如何使用 Azure CLI 的一般指南，请参阅[如何使用 Azure CLI] [azure-command-line-tools]。


##<a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

- **Azure CLI** -请参阅[安装和配置 Azure CLI](../xplat-cli-install.md)的安装和配置信息。 请确保您重新启动计算机后安装 CLI。

## <a name="authentication"></a>身份验证

这篇文章与数据湖存储位置记录作为最终用户的用户使用更简单的身份验证方法。 对数据湖存储帐户和文件系统再受登录的用户的访问级别访问级别。 不过，有其他方法以及进行身份验证与数据湖商店，哪些**最终用户身份验证**或**身份验证服务的服务**。 有关说明和如何进行身份验证的详细信息，请参阅[身份验证数据湖存储区使用 Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)。

##<a name="login-to-your-azure-subscription"></a>登录到 Azure 订阅

1. 按照[连接到 Azure 订阅从 Azure 命令行界面 (Azure CLI)](../xplat-cli-connect.md)中记录的步骤，并连接到您的订阅使用`azure login`方法。

2. 列出与使用您的帐户相关联的订阅`azure account list`命令。

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    从上面的输出中，当前已启用**Azure-子-1** ，和其他订阅是**Azure-子-2**。 

3. 选择您想要在的订阅。 如果您想要在 Azure-子-2 预订下工作，使用`azure account set`命令。

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>创建一个 Azure 数据湖存储帐户

打开命令提示符、 外壳或终端会话，然后运行下面的命令。

2. 切换到 Azure 资源管理器模式下使用以下命令︰

        azure config mode arm


5. 创建新的资源组。 下面的命令，提供您想要使用的参数值。

        azure group create <resourceGroup> <location>

    如果位置名称中包含空格，请将它放在引号中。 例如"东亚美国 2"。

5. 创建数据湖存储帐户。

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>在湖边数据存储区中创建文件夹

您的 Azure 数据湖存储帐户管理和存储数据，可以创建文件夹。 使用下面的命令创建一个名为"mynewfolder"在数据湖存储区的根目录文件夹。

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

例如︰

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>将数据上载到数据存储区湖

直接位于根级别的数据湖商店或您在帐户中创建一个文件夹，您可以上载您的数据。 下面的代码段演示了如何将一些示例数据上载到您在上一节中创建的文件夹 (**mynewfolder**)。

如果您正在寻找一些示例数据上载，可以从[Azure 数据湖 Git 存储库](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)获取**急救数据**文件夹。 下载该文件并将其存储在您的计算机 （如 C:\sampledata） 上的本地目录\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

例如︰

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>数据湖存储区中的文件列表

使用以下命令列出数据湖存储帐户中的文件。

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

例如︰

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

此输出内容应类似如下︰

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>重命名、 下载和删除数据湖存储区中的数据

* **若要重命名的文件**，请使用以下命令︰

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    例如︰

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **下载文件**，请使用以下命令。 请确保您已指定的目标路径存在。

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    例如︰

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **若要删除某个文件**，请使用以下命令︰

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    例如︰

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    出现提示时，输入**Y**以删除该项目。

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>视图数据湖存储区中的文件夹的访问控制列表

使用下面的命令以查看数据湖存储文件夹上的 Acl。 在当前版本中，可以只在数据湖存储区的根目录上设置 Acl。 因此，下面的路径参数始终为根 （/）。

    azure datalake store permissions show <dataLakeStoreName> <path>

例如︰

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>删除数据湖存储帐户

使用下面的命令来删除数据湖存储帐户。

    azure datalake store account delete <dataLakeStoreAccountName>

例如︰

    azure datalake store account delete mynewdatalakestore

出现提示时，输入**Y**以删除该帐户。


## <a name="next-steps"></a>下一步行动

- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)
- [与数据湖商店使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
