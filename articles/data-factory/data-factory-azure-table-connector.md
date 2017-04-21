<properties 
    pageTitle="将数据移动到或从 Azure 表 |Microsoft Azure" 
    description="了解如何移动到中使用 Azure 数据工厂的 Azure 表存储的数据。" 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Azure 表使用 Azure 数据工厂之间转移数据

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂移到从 Azure 表的数据与其他数据存储区。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动和受支持的数据存储组合提供复制活动。

## <a name="copy-data-wizard"></a>复制数据向导
创建管线将复制到从 Azure 表存储的数据的最简便方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 它们说明了 Azure 表存储和 Azure Blob 数据库之间复制数据。 但是，数据可以复制**直接**从任何源对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>示例︰ 将 Azure 表数据复制到 Azure Blob

下面的示例演示︰

1.  链接的类型的服务的[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) （用于表和 blob）。
2.  [AzureTable](#azure-table-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
3.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。 
3.  [管线](data-factory-create-pipelines.md)与使用[AzureTableSource](#azure-table-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。 

该示例将复制每小时属于默认分区对 blob Azure 表中的数据。 在这些示例中使用的 JSON 属性详见以下示例部分。

**链接的 azure 存储服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure 数据工厂支持两种类型的链接的 Azure 存储服务︰ **AzureStorage**和**AzureStorageSas**。 为第一个，指定连接字符串，其中包含帐户密码并为更高的一个，您指定的共享访问签名 (SAS) Uri。 有关详细信息请参阅[链接服务](#linked-services)部分。  

**Azure 表输入数据集︰**

该示例假设您已 Azure 表中创建一个表"MyTable"。
 
设置为"外部":"真正的"通知数据工厂服务数据集外部数据工厂并不由数据工厂中的活动。

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Azure Blob 的输出数据集︰**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**管线与复制活动︰**

管线包含被配置为使用的输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**AzureTableSource** ，**接收器**类型设置为**BlobSink**。 **AzureTableSourceQuery**属性指定的 SQL 查询选择数据从默认分区每小时复制。

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },              
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }
                }
             ]  
        }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>示例︰ 将 Azure Blob 数据复制到 Azure 表

下面的示例演示︰

1.  链接的类型的服务的[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) （用于表和 blob）
3.  [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureTable](#azure-table-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。 
4.  [管线](data-factory-create-pipelines.md)与使用[BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)和[AzureTableSink](#azure-table-copy-activity-type-properties)的复制活动。 


时间序列数据从 Azure 斑点到 Azure 的示例复制表每小时。 在这些示例中使用的 JSON 属性详见以下示例部分。

**（对于 Azure 表和 Blob） 链接的 azure 存储服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure 数据工厂支持两种类型的链接的 Azure 存储服务︰ **AzureStorage**和**AzureStorageSas**。 为第一个，指定连接字符串，其中包含帐户密码并为更高的一个，您指定的共享访问签名 (SAS) Uri。 有关详细信息请参阅[链接服务](#linked-services)部分。 

**Azure Blob 输入数据集︰**

数据挑选新的斑点从每小时 (频率︰ 小时、 间隔︰ 1)。 Blob 的文件夹路径和文件名称动态地计算基于切片所处理的开始时间。 文件夹路径使用的年、 月和日部分的开始时间和文件的名称使用的小时部分的开始时间。 "外部": 设置"为 true"，通知数据工厂服务数据集外部数据工厂并不由数据工厂中的活动。
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
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
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Azure 表输出数据集︰**

该示例将数据复制到名为"MyTable"Azure 表中的表。 正如预期的 Blob CSV 文件包含相同的列数创建 Azure 表。 新行被添加到表的每个小时。 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**管线与复制活动︰**

管线包含被配置为使用的输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**BlobSource** ，**接收器**类型设置为**AzureTableSink**。 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
              }
            },
            "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },                      
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

## <a name="linked-services"></a>链接的服务
有两种类型的链接可用于链接到 Azure 数据工厂的 Azure blob 存储的服务。 它们是︰ **AzureStorage**链接服务和**AzureStorageSas**链接服务。 链接的 Azure 存储服务提供到 Azure 存储全局访问的数据工厂。 而 Azure 存储 SA （共享访问签名） 链接服务将提供到 Azure 存储限制/时间限制访问的数据工厂。 有这两种链接服务没有其他差别。 选择适合您的需要的链接的服务。 以下各节提供有关这两个链接服务更多详细信息。

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure 表数据集的类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和 JSON 数据集策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

TypeProperties 节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 为数据集类型**AzureTable**的**typeProperties**部分具有以下属性。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- |
| 表名 | 链接的服务指的 Azure 表数据库实例中的表的名称。 | 是的。 而 azureTableSourceQuery 指定表名，则表中的所有记录被都复制到目标。 如果还指定了 azureTableSourceQuery，满足查询表中的记录被复制到目标。 |

### <a name="schema-by-data-factory"></a>数据工厂架构
对于类似 Azure 表架构的数据存储，数据工厂服务按以下方式之一来推断架构︰

1.  如果数据集定义中使用的**结构**属性指定的数据结构，数据工厂服务将采用这种结构为架构。 在这种情况下，如果某一行不包含列的值，为它提供一个空值。
2. 如果不通过数据集定义中使用的**结构**属性指定的数据结构，数据工厂将使用第一行数据中推断架构。 在这种情况下，如果第一行中不包含完整的架构，复制操作的结果中缺失的某些列。

因此，对于无架构的数据源，最好是指定使用**结构**属性的数据的结构。

## <a name="azure-table-copy-activity-type-properties"></a>Azure 表复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出数据集和策略都可用于所有类型的活动。 

该活动的 typeProperties 部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

**AzureTableSource**在 typeProperties 部分支持以下属性︰

属性 | 说明 | 允许的值 | 必填
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | 使用自定义查询中读取数据。 | Azure 表的查询字符串。 请参阅下一节中的示例。 | 不。 而 azureTableSourceQuery 指定表名，则表中的所有记录被都复制到目标。 如果还指定了 azureTableSourceQuery，满足查询表中的记录被复制到目标。
azureTableSourceIgnoreTableNotFound | 指示是否允许存在异常的表不存在。 | 真<br/>假 | 不 |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery 示例

如果 Azure 表中的列是字符串类型︰ 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

如果 datetime 类型的 Azure 表中的列︰ 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink**在 typeProperties 部分支持以下属性︰


属性 | 说明 | 允许的值 | 必填  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | 默认分区值可以使用的接收器。 | 一个字符串值。 | 不 
azureTablePartitionKeyName | 指定的值用作分区键的列的名称。 如果未指定，将 AzureTableDefaultPartitionKeyValue 用作分区键。 | 列名称。 | 不 |
azureTableRowKeyName | 指定的列的值将用作行键的列的名称。 如果未指定，请为每个行使用 GUID。 | 列名称。 | 不  
azureTableInsertType | 要将数据插入到 Azure 表的模式。<br/><br/>此属性控制是否输出表中具有匹配的分区和行键的现有行具有它们的值替换或合并。 <br/><br/>若要了解有关这些设置 （合并和替换） 的工作原理，请参阅[插入或合并实体](https://msdn.microsoft.com/library/azure/hh452241.aspx)和[插入或替换实体](https://msdn.microsoft.com/library/azure/hh452242.aspx)主题。 <br/><br> 在行级别，而不是表级别应用此设置，任何选项删除不存在输入中输出表中的行。 | 合并 （默认值）<br/>替换 | 不 
writeBatchSize | 命中的 writeBatchSize 或 writeBatchTimeout，则可以向 Azure 表中插入数据。 | 整数 （行数）| 无 (默认值︰ 10000) 
writeBatchTimeout | 将数据插入到 Azure 表，当点击 writeBatchSize 或 writeBatchTimeout | 时间跨度<br/><br/>示例:"00: 20:00"（20 分钟） | 无 （默认为存储客户端的默认超时值 90 秒）

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
将源列映射到目的列中使用转换器 JSON 属性，才能使用 azureTablePartitionKeyName 作为目标列。

在以下示例中，DivisionID 的源列映射到目的列中︰ DivisionID。  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

DivisionID 被指定为分区键。 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Azure 表的类型映射

如[数据移动活动](data-factory-data-movement-activities.md)文章中提到，复制活动执行自动类型转换来水池下面两个方法的类型的源类型。

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

当将数据移动到和 Azure 表中，下面的[映射定义 Azure 表服务](https://msdn.microsoft.com/library/azure/dd179338.aspx)使用从 Azure 表 OData 类型为.NET 类型，反之亦然。 

| OData 数据类型 | .NET 类型 | 详细信息 |
| --------------- | --------- | ------- |
| Edm.Binary | byte] | 字节数组的最多为 64 KB。 |
| Edm.Boolean | 布尔值 | 一个布尔值。 |
| Edm.DateTime | 日期时间 | 表示为协调世界时 (UTC) 的 64 位值。 从午夜 12:00，公元 1601 年 1 月 1 日开始的受支持的日期范围 （公元） UTC。 结束范围于 12 月 31 日，9999。 |
| Edm.Double | 双精度 | 64 位浮点值。 |
| Edm.Guid | Guid | 128 位的全局唯一标识符。 |
| Edm.Int32 | Int32 或 int | 一个 32 位整数。 |
| Edm.Int64 | Int64 或 long 类型的值 | 一个 64 位整数。 |
| Edm.String | 字符串 | UTF 16 编码的值。 字符串值可能达 64 KB。 |

### <a name="type-conversion-sample"></a>类型转换示例

下面的示例是将数据从 Azure Blob 复制到 Azure 表，使用类型转换。 

假设采用 CSV 格式的 Blob 数据集包含三列。 其中之一是使用日期是星期几的缩写法语名称的自定义 datetime 格式的日期时间列。 

定义以及类型定义的列，如下所示的 Blob 源数据集。
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

到.NET 类型从 Azure 表 OData 类型给定类型映射，将定义 Azure 表中的表格与下面的架构。 

**Azure 表架构︰**

列名称 | 类型
----------- | --------
用户 id | Edm.Int64
名称 | Edm.String 
lastlogindate | Edm.DateTime

接下来，如下所述定义 Azure 表数据集。 您不需要"结构"部分指定的类型信息，因为在基础数据存储区中已指定的类型信息。

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

在这种情况下，自动数据工厂类型转换，以将数据从 Blob 移到 Azure 表时使用"fr-fr"区域性的自定义 datetime 格式包括日期时间字段。



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>性能和调整  
若要了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素，请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)。







