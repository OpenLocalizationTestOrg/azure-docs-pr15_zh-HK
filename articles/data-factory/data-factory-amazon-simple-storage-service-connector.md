<properties 
    pageTitle="将数据从数据工厂使用 Amazon 简单存储服务移动 |Microsoft Azure" 
    description="了解如何将数据从 Amazon 简单存储服务 (S3) 使用 Azure 数据工厂。" 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>移动数据从 Amazon 简单存储服务使用 Azure 数据工厂

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将从 Amazon 简单存储服务 (S3) 另一种数据存储的数据移动。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章概要介绍了数据移动和数据存储区支持的源接收器的列表提供复制活动。  

数据工厂目前支持仅移动数据从 Amazon S3 到其他数据存储，而不是将数据从其他数据存储区移动到 Amazon S3。

## <a name="required-permissions"></a>所需的权限

若要从 Amazon S3 中复制数据，请确保您已被授予以下权限︰

- **s3:GetObject**和**s3:GetObjectVersion**的 Amazon S3 对象操作
- **s3:ListBucket**和**s3:ListAllMyBuckets** （仅复制向导用于） Amazon S3 存储桶操作 

可以在[策略中指定的权限](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html)中找到 Amazon S3 带有详细信息的权限的完整列表。

## <a name="copy-data-wizard"></a>复制数据向导
创建管线从 Amazon S3 复制数据的最简便方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 它演示如何从 Amazon S3 的数据复制到 Azure Blob 存储。 但是，数据可复制到任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)。

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>示例︰ 将 Amazon S3 数据复制到 Azure Blob
此示例演示如何将数据复制到 Azure Blob 存储 Amazon S3。 但是，数据可以复制**直接**对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  
 
此示例具有以下数据工厂实体︰

- 链接的类型[AwsAccessKey](#linked-service-properties)的服务。
- 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
- [AmazonS3](#dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
- 类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
- [管线](data-factory-create-pipelines.md)与使用[FileSystemSource](#copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从 Amazon S3 到 Azure 的 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

**链接的 Amazon S3 服务**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
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

**Amazon 的 S3 输入数据集**

设置**"外部": 真**数据集外部数据工厂并不由数据工厂中的活动通知数据工厂服务。 在输入数据集不是通过管道中的一个活动上，此属性设置为 true。

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }



**Azure Blob 输出数据集**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

管线包含被配置为使用的输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**FileSystemSource** ，**接收器**类型设置为**BlobSink**。 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>链接的服务属性

下表提供对 Amazon S3 JSON 元素特定说明 (**AwsAccessKey**) 链接服务。

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | 秘密访问键 ID。 | 字符串 | 是的 |
| secretAccessKey | 秘密访问密钥本身。 | 加密密钥的字符串 | 是的 | 


## <a name="dataset-type-properties"></a>数据集的类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

**TypeProperties**节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 TypeProperties 部分类型**AmazonS3** （其中包括 Amazon S3 数据集） 的数据集具有以下属性

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------- | ------ | 
| bucketName | S3 存储桶名称。 | 字符串 | 是的 |
| 密钥 | S3 对象键。 | 字符串 | 不 | 
| 前缀 | S3 对象键的前缀。 选定的对象的键开始时使用此前缀。 仅当键为空时的应用。 | 字符串 | 不 | 
| 版本 | 如果启用了版本控制 S3 S3 对象的版本。 | 字符串 | 不 |  
| 格式 | 支持以下格式类型︰**绑定**、 **AvroFormat**、 **JsonFormat**、 **OrcFormat**、 **ParquetFormat**。 将格式下的**类型**属性设置为下列值之一。 请参阅[指定格式](#specifying-textformat)，[指定 AvroFormat](#specifying-avroformat)、[指定 JsonFormat](#specifying-jsonformat)、[指定 OrcFormat](#specifying-orcformat)，并[指定 ParquetFormat](#specifying-parquetformat)部分以了解详细信息。 如果您想要复制的文件-是之间的基于文件的存储 （二进制副本），则可以跳过这两个输入和输出数据集定义中的格式部分。| 不
| 压缩 | 指定的类型和级别的数据压缩。 支持的类型包括︰ **GZip**， **Deflate**， **BZip2**并受支持的级别是︰**最佳**和**最快**。 目前， **AvroFormat**或**OrcFormat**中的数据不支持的压缩设置。 有关详细信息，请参阅[压缩支持](#compression-support)部分。  | 不 |

> [AZURE.NOTE] bucketName + 键指定 S3 对象其中桶是 S3 对象的根容器，关键在于 S3 对象的完整路径的位置。

### <a name="sample-dataset-with-prefix"></a>带前缀的示例数据集

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>示例数据集 （与版本）

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>S3 的动态路径

在示例中，我们使用键和 bucketName 在 Amazon S3 数据集的属性的固定的值。 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

您可以通过使用如 SliceStart 的系统变量计算的密钥和 bucketName 在运行时动态的数据工厂。

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

要做到 Amazon S3 数据集的前缀属性相同。 有关受支持的函数和变量的列表，请参阅[数据工厂函数和系统变量](data-factory-functions-variables.md)。 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。 

该活动的**typeProperties**部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

**FileSystemSource** （其中包括 Amazon S3） 类型的复制活动的源时，以下属性是在 typeProperties 部分中︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- | 
| 递归 | 指定是否以递归方式列出 S3 对象的目录下。 | 真/假 | 不 | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。

## <a name="next-steps"></a>下一步行动
请参阅以下文章︰ 
- 使用复制活动创建管线的指导[教程复制活动](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。 
