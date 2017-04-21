<properties
   pageTitle="将大量的数据上载到数据湖存储脱机使用 |Microsoft Azure"
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
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>对于脱机复制数据到数据湖商店使用 Azure 进出口服务

在本文中，您将学习有关如何复制大量的数据集 (> 200 GB) 到使用脱机副本的方法，像[Azure 导入/导出服务](../storage/storage-import-export-service.md)Azure 数据湖商店。 具体而言，作为本文中示例中使用的文件是 339,420,860,416 字节即约 319 GB 磁盘上。 让我们来调用此文件 319GB.tsv。

Azure 的导入/导出服务使您能够安全地到 Azure blob 存储送货到 Azure 数据中心的硬磁盘驱动器的传输大量的数据。

## <a name="prerequisites"></a>系统必备组件

在开始这篇文章之前，您必须具有以下︰

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

- **Azure 存储帐户**。

- **Azure 数据湖分析帐户 （可选）** -请参阅[开始使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)有关如何创建数据湖存储帐户的说明。


## <a name="preparing-the-data"></a>准备数据

使用导入/导出服务之前, 我们必须中断要转移**到副本，则小于 200 GB**大小的数据文件。 这是因为导入工具不适用于超过 200 GB 的文件。 遵守这一点，在本教程中我们将文件拆分为 100 GB 字节的块。 你可以很容易地使用[Cygwin](https://cygwin.com/install.html)。 Cygwin 支持 Linux 命令使用户可以轻松地执行此操作。 对于我们的示例中，我们使用下面的命令。

    split -b 100m 319GB.tsv

拆分操作创建具有以下名称的文件。

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>数据准备磁盘

按照说明[使用 Azure 导入/导出服务](../storage/storage-import-export-service.md)（部分下的**准备您的驱动器**） 来准备您的硬盘驱动器。 下面是如何准备驱动器的整体流程。

1. 获得满足要求 Auzre 导入/导出服务使用的硬盘。

2. 标识的数据将 Azure 存储帐户复制一次 si 运送到 Azure 数据中心。

3. 使用[Azure 导入/导出工具](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409)，命令行实用程序。 下面是有关如何使用该工具的示例代码段。

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    有关如何使用**Azure 导入/导出工具**的更多示例代码段，请参阅[使用 Azure 导入/导出服务](../storage/storage-import-export-service.md)。

4. 上面的命令在指定的位置创建一个日志文件。 您将使用此日志文件从[Azure 经典门户](https://manage.windowsazure.com)创建导入作业。

## <a name="create-an-import-job"></a>创建导入作业

现在，您可以创建导入作业 （在**创建导入作业**部分） 在[使用 Azure 导入/导出服务](../storage/storage-import-export-service.md)使用的说明。 有关此导入作业时，与其他详细信息，还提供准备磁盘驱动器时创建的日志文件。

## <a name="physically-ship-the-disks"></a>以物理方式提供磁盘

现在实际运送至 Azure 数据中心的数据通过复制到 Azure 存储 Blob 创建导入作业时提供的磁盘。 此外，在创建作业时，如果您选择提供跟踪信息以后，您现在可以回到导入作业，更新跟踪号码。

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>将数据复制到 Azure 数据湖商店 Azure 存储 Blob

一旦导入作业的状态显示已完成，您可以验证是否已指定 Azure 存储 Blob 中的可用数据。 然后可以使用多种方法将该数据从 Azure 存储 Blob 移到 Azure 数据湖存储区。 上载数据的所有可用选项，请参阅[Ingesting 数据读入数据湖存储区](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store)。

在本节中，我们为您提供可用于创建将数据复制 Azure 数据工厂管道的 JSON 定义。 您可以使用这些从[Azure 门户](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)的 JSON 定义。

### <a name="source-linked-service-azure-storage-blob"></a>源链接服务 (Azure 存储 Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>目标链接服务 （Azure 数据湖存储区）

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>输入的数据集
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>输出数据集
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>管道 （复制活动）
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
有关如何使用 Azure 数据工厂从 Azure 存储 Blob 和 Azure 数据湖存储移动数据的详细信息，请参阅[移动到 Azure 数据湖存储区使用 Azure 数据工厂 Azure 存储 Blob 中的数据](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)。

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>重新构造 Azure 数据湖存储中的数据文件

我们启动了 319 GB 大小并分解其较小的文件以便使用 Azure 导入/导出服务将传输的文件。 现在，数据是 Azure 数据存储中存储湖，我们可以 reconstrcut 为其原始大小的文件。 下面的 Azure PowerShell cmldts 可用于执行此操作。

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>下一步行动

- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)
- [与数据湖商店使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [与数据湖商店使用 Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
