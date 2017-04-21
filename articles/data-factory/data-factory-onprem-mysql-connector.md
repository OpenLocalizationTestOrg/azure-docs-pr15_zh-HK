<properties 
    pageTitle="将数据从 MySQL 移动 |Azure 数据工厂" 
    description="了解如何将数据从使用 Azure 数据工厂的 MySQL 数据库移动。" 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mysql-using-azure-data-factory"></a>移动数据从 MySQL 使用 Azure 数据工厂

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将从 MySQL 到另一个数据存储的数据移动。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动提供副本的活动和受支持的数据存储区的组合。

数据工厂服务支持连接到内部部署 MySQL 源使用数据管理网关。 请查看[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章以了解有关数据管理网关和网关设置的分步指导。 

> [AZURE.NOTE] 即使 MySQL 数据库位于 Azure IaaS 虚拟机 (VM) 中，网关是必需的。 只要该网关可以连接到数据库，可以作为数据存储同一个 VM 或一个不同的虚拟机上安装网关。  

数据工厂目前支持仅移动数据从 MySQL 到其他数据存储，而不是将数据从其他数据存储区移动到 MySQL。

## <a name="installation"></a>安装 
对于数据管理网关连接到 MySQL 数据库，您需要为数据管理网关在同一系统上安装[MySQL 连接器/Net Microsoft Windows 的 6.6.5](http://go.microsoft.com/fwlink/?LinkId=278885) 。

> [AZURE.NOTE] 请参阅故障排除连接/网关版的提示[疑难解答网关问题](data-factory-data-management-gateway.md#troubleshoot-gateway-issues)相关的问题。 

## <a name="copy-data-wizard"></a>复制数据向导
创建管线将 MySQL 数据库中的数据复制到任何受支持的接收数据存储的最简单方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于创建管线。 完成演练提供分步说明，请参见[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)的文章。 数据可以复制到任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>示例︰ 将 MySQL 数据复制到 Azure Blob
此示例演示如何将数据从本地 MySQL 数据库复制到 Azure Blob 存储。 但是，数据可以复制**直接**对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  

> [AZURE.IMPORTANT] 本示例提供了 JSON 段。 它不包含用于创建数据工厂的分步指导。 请参阅分步指导文章[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)。 
 
此示例具有以下数据工厂实体︰

1.  链接的类型[OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从一个 MySQL 数据库中的查询结果对 blob 每小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，设置数据管理网关。 在[内部部署位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)的文章的说明。 

**MySQL 链接服务**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**MySQL 的输入数据集**

该示例假定您创建了一个表"MyTable"在 MySQL 中，它包含名为"timestampcolumn"的时间系列数据的列。

设置为"外部":"真正的"通知数据工厂服务表外部数据工厂并不由数据工厂中的活动。
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }



**Azure Blob 输出数据集**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

管线包含被配置为使用的输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**RelationalSource** ，**接收器**类型设置为**BlobSink**。 **查询**属性指定的 SQL 查询选择过去小时要复制的数据。
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>MySQL 链接服务属性

下表提供了 JSON 元素特定于 MySQL 链接服务的说明。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- | 
| 类型 | 类型属性必须设置为︰ **OnPremisesMySql** | 是的 |
| 服务器 | MySQL 服务器的名称。 | 是的 |
| 数据库 | MySQL 数据库的名称。 | 是的 | 
| 架构  | 在数据库中的架构的名称。 | 不 | 
| authenticationType | 用于连接到 MySQL 数据库的身份验证类型。 可能的值包括︰ 匿名的基本的和窗口。 | 是的 | 
| 用户名 | 如果您使用基本或 Windows 身份验证，请指定用户名。 | 不 | 
| 密码 | 指定对您指定的用户名的用户帐户的密码。 | 不 | 
| gatewayName | 数据工厂服务连接到内部部署 MySQL 数据库应使用的网关的名称。 | 是的 |

有关设置为内部部署 MySQL 数据源凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security)。

## <a name="mysql-dataset-type-properties"></a>MySQL 数据集类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和 JSON 数据集策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

**TypeProperties**节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 TypeProperties 部分类型**RelationalTable** （其中包括 MySQL 数据集） 的数据集具有以下属性

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- |
| 表名 | 链接的服务指的 MySQL 数据库实例中的表的名称。 | 否 （如果指定了**RelationalSource** ，**查询**） | 

## <a name="mysql-copy-activity-type-properties"></a>MySQL 中复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表的策略都将显示为所有类型的活动。 

该活动的**typeProperties**部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

当复制活动中的源类型**RelationalSource** （其中包括 MySQL） 以下属性是在 typeProperties 部分中︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- |
| 查询 | 使用自定义查询中读取数据。 | SQL 查询字符串。 例如︰ 选择 * 从 MyTable。 | 否 （如果指定了**数据集**的**表名**） | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>MySQL 的类型映射

如[数据移动活动](data-factory-data-movement-activities.md)文章中提到，复制活动执行自动类型转换来水池下面两个方法的类型的源类型︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

在将数据移到 MySQL，以下映射用于从 MySQL 类型对.NET 类型。

| MySQL 数据库类型 | .NET Framework 类型 |
| ------------------- | ------------------- | 
| 无符号的 bigint | 十进制 |
| bigint | Int64 |
| 位 | 十进制 |
| blob | Byte] |
| 布尔值 |  布尔值 | 
| 字符 | 字符串 |
| 日期 | 日期时间 |
| 日期时间 | 日期时间 |
| 十进制 | 十进制 |
| 双精度 | 双 |
| 双精度 | 双 |
| 枚举 | 字符串 |
| 浮点型 | 一个 |
| 无符号整型 | Int64 |
| int | Int32 |
| 无符号的整数 | Int64 |
| 整数 | Int32 | 
| 长的低级 | Byte] |
| 长 varchar | 字符串 |
| longblob | Byte] |
| 长文本 | 字符串 | 
| mediumblob |  Byte] | 
| mediumint 无符号 | Int64 |
| mediumint | Int32 | 
| mediumtext | 字符串 |
| 数字 | 十进制 |
| 现实 |  双 |
| 设置 | 字符串 |
| 无符号 smallint | Int32 |
| smallint | Int16 |
| 文本 | 字符串 |
| 时间 | 时间跨度 |
| 时间戳 | 日期时间 |
| tinyblob | Byte] |
| tinyint 无符号 |  Int16 | 
| tinyint | Int16 |
| tinytext | 字符串 | 
| varchar | 字符串 | 
| 年份 | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。

