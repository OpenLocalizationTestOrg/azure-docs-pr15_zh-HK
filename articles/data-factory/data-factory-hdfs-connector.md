<properties 
    pageTitle="将数据从内部 HDFS 移动 |Azure 数据工厂" 
    description="了解如何将数据从使用 Azure 数据工厂内部 HDFS 移动。" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>将数据从使用 Azure 数据工厂内部 HDFS 移动
这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将从内部 HDFS 数据移动到另一个数据存储区。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章概要介绍了数据移动提供副本的活动和受支持的数据存储区的组合。

数据工厂目前支持仅移动数据从内部 HDFS 到其他数据存储，而不是将数据从其他数据存储区移动到内部 HDFS。


## <a name="enabling-connectivity"></a>启用连接
数据工厂服务支持连接到内部 HDFS 数据管理网关使用。 请查看[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章以了解有关数据管理网关和网关设置的分步指导。 使用网关连接到 HDFS，即使它位于 Azure IaaS VM 中。 

虽然您可以在同一本地计算机或作为 HDFS Azure VM 安装网关，我们建议您在单独的计算机/Azure IaaS 虚拟机上安装网关。 有一台计算机上的网关可以减少资源争用和提高性能。 在网关安装在单独的计算机上时，计算机应该能够访问与 HDFS 机器。 


## <a name="copy-data-wizard"></a>复制数据向导
创建管线从内部 HDFS 中复制数据的最简便方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 它们显示了如何从内部 HDFS 数据复制到 Azure Blob 存储。 但是，数据可复制到任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>示例︰ 将内部 HDFS 数据复制到 Azure Blob

此示例演示如何从内部 HDFS 数据复制到 Azure Blob 存储。 但是，数据可以复制**直接**对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  
 
此示例具有以下数据工厂实体︰

1.  链接的类型[OnPremisesHdfs](#hdfs-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [文件](#hdfs-dataset-type-properties)类型输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[FileSystemSource](#hdfs-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从内部 HDFS 到 Azure 的 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，设置数据管理网关。 [内部部署位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章中的说明操作。 

**HDFS 链接服务**此示例使用 Windows 身份验证。 请参见针对不同类型的身份验证，您可以使用[HDFS 链接服务](#hdfs-linked-service-properties)一节。 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }

**Azure 存储链接服务**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**HDFS 输入数据集**此数据集引用 HDFS 文件夹 DataTransfer/UnitTest /。 管道在此文件夹中的所有文件都复制到目的地。 

设置为"外部":"真正的"通知数据工厂服务数据集外部数据工厂并不由数据工厂中的活动。
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure Blob 输出数据集**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**管线与复制活动**

管线包含被配置为使用这些输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**FileSystemSource** ，**接收器**类型设置为**BlobSink**。 **查询**属性指定的 SQL 查询选择过去小时要复制的数据。
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>HDFS 链接服务属性

下表提供了描述特定于 HDFS 的 JSON 元素链接服务。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- | 
| 类型 | 类型属性必须设置为︰ **Hdfs** | 是的 | 
| Url | 对 HDFS 的 URL | 是的 |
| encryptedCredential | [新 AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx)输出的访问凭据。 | 不 |
| 用户名 | 用户名为 Windows 身份验证。 | 是 （对于 Windows 身份验证）
| 密码 | Windows 身份验证的密码。 | 是 （对于 Windows 身份验证）
| authenticationType | Windows，或匿名。 | 是的 |
| gatewayName | 连接到 HDFS 数据工厂服务应使用的网关的名称。 | 是的 |   

有关设置内部 HDFS 的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security)。

### <a name="using-anonymous-authentication"></a>使用匿名身份验证

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>使用 Windows 身份验证
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>HDFS 数据集类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和 JSON 数据集策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

**TypeProperties**节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 类型 （其中包括 HDFS 数据集） 的**文件共享**数据集的 typeProperties 部分具有以下属性

属性 | 说明 | 必填
-------- | ----------- | --------
采用文件夹路径 | 指向文件夹的路径。 示例︰`myfolder`<br/><br/>使用转义字符 '\' 特殊字符在字符串中。 例如︰ 指定文件夹的 folder\subfolder，\\\\子文件夹和 d:\samplefolder，用于指定 d:\\\\相应的访问。<br/><br/>可将此属性与**partitionBy**具有基于片上的文件夹路径合并的开始/结束日期-时间。 | 是的
文件名 | 在**采用文件夹路径**指定的文件的名称，如果您想要特定文件夹中的文件引用的表。 如果未指定此属性的任何值，表指向该文件夹中的所有文件。<br/><br/>当输出数据集未指定文件名时，所生成的文件的名称将在下面的格式︰ <br/><br/>数据。<Guid>.txt (例如:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | 不
partitionedBy | partitionedBy 可用于指定动态采用文件夹路径，用于时间序列数据的文件名。 采用参数化数据每小时示例︰ 文件夹路径。 | 不
过滤 | 指定的筛选器可用于选择采用文件夹路径中的文件，而不是所有文件的子集。 <br/><br/>允许值为︰ `*` （多个字符） 和`?`（单个字符）。<br/><br/>示例 1:`"fileFilter": "*.log"`<br/>示例 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**注意**︰ 过滤是适用于输入文件共享数据集 | 不
| 格式 | 支持以下格式类型︰**绑定**、 **AvroFormat**、 **JsonFormat**、 **OrcFormat**和**ParquetFormat**。 将格式下的**类型**属性设置为下列值之一。 请参阅[指定格式](#specifying-textformat)，[指定 AvroFormat](#specifying-avroformat)、[指定 JsonFormat](#specifying-jsonformat)、[指定 OrcFormat](#specifying-orcformat)，并[指定 ParquetFormat](#specifying-parquetformat)部分以了解详细信息。 如果您想要复制的文件-是之间的基于文件的存储 （二进制副本），则可以跳过这两个输入和输出数据集定义中的格式部分。 | 不 
| 压缩 | 指定的类型和级别的数据压缩。 支持的类型包括︰ **GZip**， **Deflate**， **BZip2**并受支持的级别是︰**最佳**和**最快**。 目前， **AvroFormat**或**OrcFormat**中的数据不支持压缩设置。 有关详细信息，请参阅[压缩支持](#compression-support)部分。  | 不 |



> [AZURE.NOTE] 不能同时使用文件名和过滤。


### <a name="using-partionedby-property"></a>使用 partionedBy 属性

如在上一节中所述，您可以指定动态采用文件夹路径，文件名与 partitionedBy 的时间系列数据。 您可以使用数据工厂宏和系统变量 SliceStart，SliceEnd，用于指示给定的数据切片的逻辑时间段这样做。 

了解更多有关时间系列数据集，调度和切片，请参阅文章[创建数据集](data-factory-create-datasets.md)、[计划与执行](data-factory-scheduling-and-execution.md)、 以及[创建管线](data-factory-create-pipelines.md)。 

#### <a name="sample-1"></a>示例 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

在此示例中 {切片} 替换指定的格式 (YYYYMMDDHH) 中的数据工厂系统变量 SliceStart 的值。 SliceStart 是指切片的开始时间。 采用文件夹路径是不同的每个扇区。 例如︰ wikidatagateway/wikisampledataout/2014100103 或 wikidatagateway/wikisampledataout/2014100104。

#### <a name="sample-2"></a>示例 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

在此示例中，年、 月、 日和时间 SliceStart 提取到不同采用文件夹路径和文件名属性所使用的变量。

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>HDFS 复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。 

该活动的 typeProperties 部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

对于复制活动，当源类型**FileSystemSource**以下属性中提供了 typeProperties 部分中︰

**FileSystemSource**支持以下属性︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- |
| 递归 | 指示数据只能从指定的文件夹或子文件夹是否为只读以递归方式。 | True，则为 False （默认值）| 不 |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。

