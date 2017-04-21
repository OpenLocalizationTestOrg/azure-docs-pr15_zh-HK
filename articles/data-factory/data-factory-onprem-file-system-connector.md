<properties
    pageTitle="文件系统之间转移数据 |Microsoft Azure"
    description="了解如何通过使用 Azure 数据工厂内部文件系统之间来回移动数据。"
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
    ms.date="09/01/2016"
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>通过使用 Azure 数据工厂和本地文件系统中移动数据

这篇文章概括介绍了如何使用 Azure 数据工厂复制活动以内部文件系统之间移动数据。 数据存储区可以用作源或接收器的内部文件系统的列表，请参见[支持的源和接收器](data-factory-data-movement-activities.md#supported-data-stores)。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动提供副本的活动和受支持的数据存储区的组合。

数据工厂支持连接与数据管理网关通过内部文件系统。 若要了解有关数据管理网关和网关设置的分步说明，请参阅[移动内部源和与数据管理网关云间的数据](data-factory-move-data-between-onprem-and-cloud.md)。

> [AZURE.NOTE]
> 数据管理网关，除了任何其他二进制文件不需要被安装为从本地文件系统与通信。
>
> 有关连接/网关相关的问题进行故障排除的提示，请参阅[疑难解答网关问题](data-factory-data-management-gateway.md#troubleshoot-gateway-issues)。

## <a name="linux-file-share"></a>Linux 文件共享

执行以下两个步骤用于 Linux 文件共享链接文件服务器服务︰

- 在 Linux 服务器上安装[Samba](https://www.samba.org/) 。
- 安装和配置 Windows 服务器上的数据管理网关。 不支持在 Linux 服务器上安装数据管理网关。

## <a name="copy-wizard"></a>复制向导
创建将数据复制到本地文件系统与管线的最简单方法是使用复制向导。 快速的演练，请参见[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)。

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)、 [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 演示如何复制数据从本地文件系统和 Azure Blob 存储它们。 但是，您可以复制数据*直接*从任何来源于在 Azure 数据工厂中使用复制活动列入[支持源和接收器](data-factory-data-movement-activities.md#supported-data-stores)的接收器。


## <a name="sample-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>示例︰ 将数据从本地文件系统复制到 Azure Blob 存储

此示例演示如何将数据从本地文件系统复制到 Azure Blob 存储。

此示例具有以下数据工厂实体︰

- 链接的类型[OnPremisesFileServer](data-factory-onprem-file-system-connector.md#onpremisesfileserver-linked-service-properties)的服务。
- 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
- [文件](data-factory-onprem-file-system-connector.md#on-premises-file-system-dataset-type-properties)类型输入的[数据集](data-factory-create-datasets.md)。
- 类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
- [管线](data-factory-create-pipelines.md)与使用[FileSystemSource](data-factory-onprem-file-system-connector.md#file-share-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

下面的示例将复制时间序列数据从本地文件系统到 Azure Blob 存储每隔一小时。 这些示例中使用的 JSON 属性各节所述样本之后。

作为第一步，设置数据管理网关根据中[移动内部源和与数据管理网关云间数据](data-factory-move-data-between-onprem-and-cloud.md)的说明。

**内部链接的文件服务器服务︰**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

我们建议使用**encryptedCredential**属性而不使用**用户 id**和**密码**属性。 有关此链接的服务的详细信息，请参阅[文件服务器链接服务](#onpremisesfileserver-linked-service-properties)。

**Azure 存储链接的服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**内部文件系统输入数据集︰**

数据获取新文件从每隔 1 小时。 采用文件夹路径和文件名属性确定基于扇区的开始时间。  

设置`"external": "true"`，通知数据工厂数据集外部数据工厂，并不由数据工厂中的活动。

    {
      "name": "OnpremisesFileSystemInput",
      "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
          "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
          ]
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

**Azure Blob 存储输出数据集︰**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 文件夹路径使用的年、 月、 日和小时部分的开始时间。

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**复制活动︰**

管线包含被配置为使用输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**FileSystemSource**，和**接收器**类型设置为**BlobSink**。

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T19:00:00",
        "description":"Pipeline for copy activity",
        "activities":[  
          {
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "OnpremisesFileSystemInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "FileSystemSource"
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

## <a name="sample-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>示例︰ 将数据从 SQL Azure 数据库复制到本地文件系统

下面的示例演示︰

- 链接的类型 AzureSqlDatabase 的服务。
- 链接的类型 OnPremisesFileServer 的服务。
- AzureSqlTable 类型的输入数据集。
- 类型文件的输出数据集。
- 使用 SqlSource 和 FileSystemSink 的复制活动的管道。

示例时间序列数据从复制 SQL Azure 表到内部文件系统每隔 1 小时。 这些示例中使用的 JSON 属性各节所述样本之后。

**SQL azure 链接的服务︰**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**内部链接的文件服务器服务︰**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

我们建议使用**encryptedCredential**属性而不使用**用户 id**和**密码**属性。 有关此链接的服务的详细信息，请参阅[文件系统链接服务](#onpremisesfileserver-linked-service-properties)。

**Azure SQL 输入数据集︰**

该示例假定您已创建一个表"MyTable"在 Azure SQL，它包含名为"timestampcolumn"为时间序列的数据列。

设置``“external”: ”true”``，通知数据工厂数据集外部数据工厂，并不由数据工厂中的活动。

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
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

**内部文件系统输出数据集︰**

每隔一小时数据被复制到新文件中。 基于扇区的开始时间决定采用文件夹路径和文件名的 blob。

    {
      "name": "OnpremisesFileSystemOutput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
          "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
          ]
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

**管线与复制活动︰**

管线包含被配置为使用输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**SqlSource**，和**接收器**类型设置为**FileSystemSink**。 **SqlReaderQuery**属性指定的 SQL 查询选择过去小时要复制的数据。


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T20:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "OnpremisesFileSystemOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "FileSystemSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 3,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

## <a name="on-premises-file-server-linked-service-properties"></a>内部链接的文件服务器服务属性

您可以链接到与内部部署文件服务器链接服务 Azure 数据工厂的内部文件系统。 下表提供的 JSON 元素特定于内部部署文件服务器链接服务的描述。

属性 | 说明 | 必填
-------- | ----------- | --------
类型 | 确保类型属性设置为**OnPremisesFileServer**。 | 是的
主机 | 指定要复制的文件夹的根目录路径。 使用转义字符 '\' 特殊字符在字符串中。 有关示例，请参见[示例链接服务和数据集的定义](#sample-linked-service-and-dataset-definitions)。 | 是的
用户 id | 指定有权访问该服务器的用户的 ID。 | 否 （如果选择了 encryptedCredential）
密码 | 指定的密码的用户 (用户 id)。 | 否 （如果您选择 encryptedCredential
encryptedCredential | 指定您可以通过运行新建 AzureRmDataFactoryEncryptValue cmdlet 的加密的凭据。 | 否 （如果您选择以纯文本格式指定用户 id 和密码）
gatewayName | 指定连接到内部文件服务器数据工厂应使用的网关的名称。 | 是的

有关设置本地文件系统数据源的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security)。

### <a name="sample-linked-service-and-dataset-definitions"></a>示例链接的服务和数据集定义
方案 | 链接的服务定义中的主机 | 采用数据集定义中的文件夹路径
-------- | --------------------------------- | --------------------- |
数据管理网关计算机上的本地文件夹︰ <br/><br/>示例︰ d:\\ \*或 D:\folder\subfolder\\* | D:\\ \\ （对于数据管理网关 2.0 及更高版本） <br/><br/> 本地主机 （对于数据管理网关 2.0 比早期版本） | .\\\\ 或文件夹\\\\子文件夹 （仅适用于数据管理网关 2.0 及更高版本） <br/><br/>D:\\ \\或 d:\\\\文件夹\\\\子文件夹 （低于 2.0 网关版）
远程共享的文件夹中︰ <br/><br/>示例︰ \\ \\myserver\\共享\\\*或\\ \\myserver\\共享\\文件夹\\子文件夹\\* | \\\\\\\\myserver\\\\共享 | .\\\\ 或文件夹\\\\子文件夹


**若要查找一个网关的版本︰**

1. 您的计算机上启动[数据管理网关配置管理器](data-factory-data-management-gateway.md#data-management-gateway-configuration-manager)。
2. 切换到**帮助**选项卡。

> [AZURE.NOTE] 我们建议该[网关到数据管理网关 2.0 或更高版本升级](data-factory-data-management-gateway.md#update-data-management-gateway)到利用最新的功能和修复程序。

**示例︰ 使用用户名和密码以纯文本格式**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

**示例︰ 使用 encryptedcredential**

    {
      "Name": " OnPremisesFileServerLinkedService ",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "D:\\",
          "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
          "gatewayName": "mygateway"
        }
      }
    }

## <a name="on-premises-file-system-dataset-type-properties"></a>内部文件系统数据集的类型属性

部分可用于定义数据集的属性的完整列表，请参阅[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和 JSON 数据集策略的部分是为所有数据集的类型相似。

TypeProperties 部分是不同的每种类型的数据集。 它提供了信息，如位置和数据存储区中的数据的格式。 类型**文件共享**数据集的 typeProperties 部分具有以下属性︰

属性 | 说明 | 必填
-------- | ----------- | --------
采用文件夹路径 | 指定的子文件夹路径。 使用转义字符 ' \' 特殊字符在字符串中。 有关示例，请参见[示例链接服务和数据集的定义](#sample-linked-service-and-dataset-definitions)。<br/><br/>可将此属性与**partitionBy**具有基于片上的文件夹路径合并的开始/结束日期-时间。 | 是的
文件名 | 在**采用文件夹路径**指定的文件的名称，如果您想要特定文件夹中的文件引用的表。 如果未指定此属性的任何值，表指向该文件夹中的所有文件。<br/><br/>当输出数据集未指定文件名时，所生成的名称是文件的以下面的格式︰ <br/><br/>`Data.<Guid>.txt`(示例︰ Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) | 不
partitionedBy | PartitionedBy 可用于指定时间序列数据的动态采用文件夹路径/文件名。 例如，采用参数化数据每小时的文件夹路径。 | 不
格式 | 支持以下格式类型︰**绑定**、 **AvroFormat**、 **JsonFormat**、 **OrcFormat**和**ParquetFormat**。 将格式下的**类型**属性设置为下列值之一。 有关详细信息，请参阅[指定格式](#specifying-textformat)，[指定 AvroFormat](#specifying-avroformat)、[指定 JsonFormat](#specifying-jsonformat)、[指定 OrcFormat](#specifying-orcformat)，并[指定 ParquetFormat](#specifying-parquetformat) 。 如果您想要复制的文件-是之间的基于文件的存储 （二进制副本），则可以跳过这两个输入和输出数据集定义中的格式部分。 | 不
过滤 | 指定的筛选器可用于选择采用文件夹路径中的文件，而不是所有文件的子集。 <br/><br/>允许值为︰ `*` （多个字符） 和`?`（单个字符）。<br/><br/>示例 1:"过滤":"*.log"<br/>示例 2:"过滤": 2014-1-？。txt"<br/><br/>请注意该过滤是适用于输入文件共享数据集。 | 不
| 压缩 | 指定的类型和级别的数据压缩。 支持的类型包括**GZip**以及**Deflate**， **BZip2**。 受支持的级别是**最佳**和**最快**。 目前， **AvroFormat**或**OrcFormat**中的数据不支持压缩设置。 有关详细信息，请参阅[支持的压缩](#compression-support)部分。 | 不 |


> [AZURE.NOTE] 不能同时使用文件名和过滤。

### <a name="using-partitionedby-property"></a>使用 partitionedBy 属性

如在上一节中所述，可以使用 partitionedBy 中指定的动态采用文件夹路径和文件名的时间系列数据。 您可以使用数据工厂宏和系统变量 SliceStart 和 SliceEnd，指示给定的数据片的逻辑时间段这样做。

若要了解在时间序列数据集上的更多详细信息，调度和切片，请参阅[创建数据集](data-factory-create-datasets.md)、[计划和执行](data-factory-scheduling-and-execution.md)，以及[创建管线](data-factory-create-pipelines.md)。

#### <a name="sample-1"></a>示例 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy":
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

在此示例中，{切片} 替换为数据工厂系统变量 SliceStart 中的格式 (YYYYMMDDHH) 的值。 SliceStart 是指切片的开始时间。 采用文件夹路径是不同的每个扇区。 例如︰ wikidatagateway/wikisampledataout/2014100103 或 wikidatagateway/wikisampledataout/2014100104。

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

在此示例中，年、 月、 日和时间 SliceStart 提取到不同采用文件夹路径和文件名属性使用的变量。

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="file-share-copy-activity-type-properties"></a>文件共享复制活动类型属性

**FileSystemSource**支持以下属性︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- |
| 递归 | 指示数据只能从指定的文件夹或子文件夹是否为只读以递归方式。 | True，则为 False （默认值）| 不 |

**FileSystemSink**支持以下属性︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- |
| copyBehavior | 如果源为 BlobSource 或文件系统，定义的复制行为。 | **PreserveHierarchy:**将保留在目标文件夹中的文件层次结构。 即，源文件到源文件夹的相对路径是与目标文件的目标文件夹的相对路径相同。<br/><br/>**FlattenHierarchy:**在第一级的目标文件夹中创建的源文件夹中的所有文件。 使用自动生成的名称创建目标文件。<br/><br/>**MergeFiles:**将合并到一个文件的源文件夹中所有文件。 如果指定的文件名称/blob 名称，则合并的文件的名称是指定的名称。 否则，将自动生成的文件名。 | 不 |

### <a name="recursive-and-copybehavior-examples"></a>递归和 copyBehavior 示例
本节描述递归和 copyBehavior 属性值的不同组合的复制操作的结果行为。

递归值 | copyBehavior 值 | 结果行为
--------- | ------------ | --------
真 | preserveHierarchy | 源文件夹为 Folder1 具有下面的结构<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件 3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>目标文件夹为 Folder1 创建与源相同的结构︰<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件 3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5  
真 | flattenHierarchy | 源文件夹为 Folder1 具有下面的结构<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件 3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>目标为 Folder1 创建具有以下结构︰ <br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;file1 自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;自动生成的名称，为文件 2 的<br/>&nbsp;&nbsp;&nbsp;&nbsp;自动生成的名称，为文件 3 的<br/>&nbsp;&nbsp;&nbsp;&nbsp;自动生成的名称为 File4 的<br/>&nbsp;&nbsp;&nbsp;&nbsp;自动生成的名称为 File5 的
真 | mergeFiles | 源文件夹为 Folder1 具有下面的结构<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件 3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>目标为 Folder1 创建具有以下结构︰ <br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + 文件 3 + File4 + 5 文件内容合并到一个文件中使用自动生成的文件名。
假 | preserveHierarchy | 源文件夹为 Folder1 具有下面的结构<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件 3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>目标文件夹为 Folder1 创建具有以下结构︰<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/><br/>与文件 3、 File4 和 File5 的 Subfolder1 不领取。
假 | flattenHierarchy | 源文件夹为 Folder1 具有下面的结构<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件 3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>目标文件夹为 Folder1 创建具有以下结构︰<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;file1 自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;自动生成的名称，为文件 2 的<br/><br/>与文件 3、 File4 和 File5 的 Subfolder1 不领取。
假 | mergeFiles | 源文件夹为 Folder1 具有下面的结构<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件 3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>目标文件夹为 Folder1 创建具有以下结构︰<br/><br/>为 Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;文件 1 + File2 内容合并到一个文件中使用自动生成的文件名。<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 自动生成的名称<br/><br/>与文件 3、 File4 和 File5 的 Subfolder1 不领取。


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>性能和调整
 若要了解有关影响的数据移动 （复制活动） 在 Azure 数据工厂和各种方法优化其性能的关键因素，请参阅[复制活动性能和优化指南](data-factory-copy-activity-performance.md)。
