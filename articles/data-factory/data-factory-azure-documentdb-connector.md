<properties 
    pageTitle="将数据移动到介质/DocumentDB |Microsoft Azure" 
    description="了解如何将数据移动到或从 Azure DocumentDB 集合并使用 Azure 数据工厂" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>DocumentDB 使用 Azure 数据工厂之间转移数据

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂移到 Azure DocumentDB 从另一个数据的数据存储和将数据从 DocumentDB 移动到另一个数据存储区。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动提供副本的活动和受支持的数据存储区的组合。

下面的示例演示如何在 Azure DocumentDB 和 Azure Blob 存储复制数据。 但是，数据可以复制**直接**从任何源对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  

> [AZURE.NOTE] 在的内部部署/Azure IaaS 数据中的数据复制到 Azure DocumentDB 存储，并与数据管理网关 2.1 版及以上 （反之亦然） 支持。

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>示例︰ 将数据从 DocumentDB 复制到 Azure Blob

下面的示例演示︰

1. 链接的类型[DocumentDb](#azure-documentdb-linked-service-properties)的服务。
2. 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。 
3. [DocumentDbCollection](#azure-documentdb-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。 
4. 类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4. [管线](data-factory-create-pipelines.md)与使用[DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据复制到 Azure Blob Azure DocumentDB。 在这些示例中使用的 JSON 属性详见以下示例部分。

**Azure DocumentDB 链接的服务︰**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob 存储链接的服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure 文档数据库输入数据集︰**

该示例假定您已经命名 Azure DocumentDB 数据库中的**人**的集合。
 
设置为"外部":"true"，并指定 externalData 策略信息 Azure 数据工厂服务表外部数据工厂并不产生数据工厂中的活动。

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure Blob 的输出数据集︰**

将数据复制到新的斑点斑点小时粒度与反映特定日期时间每隔 1 小时与路径。

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

在 DocumentDB 数据库中的人集合示例 JSON 文档︰ 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB 支持通过分层的 JSON 文档中使用语法类似 SQL 的查询文档。 

示例︰ 选择 Person.PersonId，Person.Name.First AS 名字 Person.Name.Middle 作为称谓，Person.Name.Last AS 姓氏人

下面的管线将数据复制到 Azure 的 blob DocumentDB 数据库中的用户集合。 为复制活动输入的部分和输出指定的数据集。  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>示例︰ 将 Azure Blob 数据复制到 Azure DocumentDB

下面的示例演示︰

1. 链接的类型[DocumentDb](#azure-documentdb-linked-service-properties)的服务。
2. 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3. [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4. 类型[DocumentDbCollection](#azure-documentdb-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。 
4. [管线](data-factory-create-pipelines.md)与使用[BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)和[DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties)的复制活动。


此示例从 Azure 将数据复制到 Azure DocumentDB 的 blob。 在这些示例中使用的 JSON 属性详见以下示例部分。

**Azure Blob 存储链接的服务︰**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DocumentDB 链接的服务︰**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob 输入数据集︰**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure 的 DocumentDB 的输出数据集︰**

该示例将数据复制到一个名为"人"。

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

下面的管线将 Azure Blob 数据复制到 DocumentDB 中的人集合。 为复制活动输入的部分和输出指定的数据集。 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
如果示例 blob 输入的内容作为 

    1,John,,Doe

然后在 DocumentDB 的 JSON 输出将为︰

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB 是 NoSQL 商店的 JSON 文档，允许嵌套的结构的位置。 Azure 数据工厂使用户能够表示层次结构，通过**nestingSeparator**，即"。" 在此示例中。 使用分隔符，复制活动将生成三个子级元素"名称"对象第一次，中间名和姓，根据"Name.First"、"Name.Middle"和"Name.Last"中的表定义。

## <a name="azure-documentdb-linked-service-properties"></a>Azure DocumentDB 链接服务属性

下表提供了 JSON 元素特定于 Azure DocumentDB 链接服务的说明。 

| **属性** | **说明** | **必填** |
| -------- | ----------- | --------- |
| 类型 | 类型属性必须设置为︰ **DocumentDb** | 是的 |
| 连接字符串 | 指定连接到 Azure DocumentDB 数据库所需的信息。 | 是的 |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure DocumentDB 数据集的类型属性

部分和属性可用于定义数据集的完整列表请参阅文章[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和 JSON 数据集的策略部分是类似于所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。)。
 
TypeProperties 节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 为数据集类型**DocumentDbCollection**的 typeProperties 部分具有以下属性。

| **属性** | **说明** | **必填** |
| -------- | ----------- | -------- |
| 集合名称 | DocumentDB 文档集合的名称。 | 是的 |


示例︰

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>数据工厂架构
对于如 DocumentDB 架构的数据存储，数据工厂服务按以下方式之一来推断架构︰  

1.  如果数据集定义中使用的**结构**属性指定的数据结构，数据工厂服务将采用这种结构为架构。 在这种情况下，如果某一行不包含列的值，将为它提供一个空值。
2.  如果不通过数据集定义中使用的**结构**属性指定的数据结构，数据工厂服务将使用第一行数据中推断架构。 在这种情况下，如果第一行中不包含完整的架构，某些列将在复制操作的结果丢失。

因此，对于无架构的数据源，最好是指定使用**结构**属性的数据的结构。

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure DocumentDB 复制活动类型属性

部分和可用于定义活动属性的完整列表请参阅文章[创建管线](data-factory-create-pipelines.md)。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。
 
**注意︰**复制活动采用一个输入，并生成一个输出。

另一方面该活动的 typeProperties 部分中可用的属性随每种活动类型和复制活动对于他们因种源和接收器。

如果复制活动当源类型**DocumentDbCollectionSource**以下属性部分提供了**typeProperties** :

| **属性** | **说明** | **允许的值** | **必填** |
| ------------ | --------------- | ------------------ | ------------ |
| 查询 | 指定要从中读取数据的查询。 | 查询支持 DocumentDB 的字符串。 <br/><br/>示例︰`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | 不 <br/><br/>如果未指定，则执行的 SQL 语句︰`select <columns defined in structure> from mycollection` 
| nestingSeparator | 指示文档嵌套的特殊字符 | 任何字符。 <br/><br/>DocumentDB 是 NoSQL 商店的 JSON 文档，允许嵌套的结构的位置。 Azure 数据工厂使用户能够表示层次结构，通过 nestingSeparator，即"。" 在上面的示例中。 使用分隔符，复制活动将生成三个子级元素"名称"对象第一次，中间名和姓，根据"Name.First"、"Name.Middle"和"Name.Last"中的表定义。 | 不

**DocumentDbCollectionSink**支持以下属性︰

| **属性** | **说明** | **允许的值** | **必填** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | 指示该嵌套的文档的源列名称中的特殊字符，则需要。 <br/><br/>例如上面︰`Name.First`表在输出中生成以下的 JSON 结构，该结构在 DocumentDB 文档中︰<br/><br/>"Name": {<br/>  "第一":"约翰"<br/>}, | 用于分隔的嵌套级别的字符。<br/><br/>默认值是`.`（圆点）。 | 用于分隔的嵌套级别的字符。 <br/><br/>默认值是`.`（圆点）。 | 不 | 
| writeBatchSize | 向 DocumentDB 服务并行请求创建的文档数。<br/><br/>通过使用此属性复制到从 DocumentDB 的数据时，您可以微调性能。 由于发送 DocumentDB 到更多的并行请求增加 writeBatchSize 时，您可能会更好的性能。 但是您需要避免限制，可能会引发的错误消息:"申请率很大"。<br/><br/>带宽限制是由很多因素，包括大小的文档，在文档中的期数、 索引策略目标集合等决定的。为复制操作，可用于更好的集合 (如 S3) 具有最大可用吞吐量 （2500 申请单位/秒）。 | 整数 | 无 (默认值︰ 10000) |
| writeBatchTimeout | 请等待该操作完成之前超时时间。 | 时间跨度<br/><br/> 示例:"00: 30:00"（30 分钟）。 | 不 |
 
## <a name="appendix"></a>附录
1. **问题︰**  
   没有复制活动支持更新现有记录的？

    **答案是︰**  
   号

2. **问题︰**  
   如何尚为 DocumentDB 处理副本重试复制记录吗？

    **答案是︰**  
   如果复制操作尝试插入一条记录具有相同的 ID 记录具有的"ID"字段，则复制操作将引发错误。  
 
3. **问题︰** 
   数据工厂是否支持有[区域或基于散列的数据分区](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)？ 

    **答案是︰** 
   号 
4. **问题︰** 
   可以指定多个表的 DocumentDB 集合吗？
    
    **答案是︰** 
   号 这一次，可以指定只有一个集合。
     
## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。
