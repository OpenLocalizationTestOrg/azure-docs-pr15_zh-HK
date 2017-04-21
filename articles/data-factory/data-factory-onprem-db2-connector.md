<properties 
    pageTitle="将数据从 DB2 移动 |Azure 数据工厂" 
    description="了解如何将数据从使用 Azure 数据工厂的 DB2 数据库移动" 
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

# <a name="move-data-from-db2-using-azure-data-factory"></a>将数据从 DB2 使用 Azure 数据工厂
这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂移到另一个数据的 DB2 中存储的数据。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动提供副本的活动和受支持的数据存储区的组合。

数据工厂支持连接到内部 DB2 源使用数据管理网关。 请查看[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章以了解有关数据管理网关和网关设置的分步指导。 

> [AZURE.NOTE]
使用网关连接到 DB2，即使承载于 Azure IaaS Vm。 如果您试图连接到承载于云的 DB2 实例，也可以在 IaaS VM 安装网关实例。

数据工厂目前支持仅移动数据从 DB2 到其他数据存储区而不是其他数据存储到 DB2。 

## <a name="installation"></a>安装 

数据管理网关提供的内置的 DB2 驱动程序支持以下功能︰ 

- SQLAM 9 / 10 / 11
- DB2 LUW (Linux、 Unix，Windows) 的
- DB2 为 z/OS 和 DB2 的我 (亦即为 / 400)

因此，您不再需要手动安装驱动程序从 DB2 复制数据时。

> [AZURE.NOTE] 请参阅故障排除连接/网关版的提示[疑难解答网关问题](data-factory-data-management-gateway.md#troubleshoot-gateway-issues)相关的问题。 

## <a name="copy-data-wizard"></a>复制数据向导
创建管线从 DB2 数据库复制数据的最简便方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 显示如何将数据从 DB2 数据库和 Azure Blob 存储复制它们。 但是，数据可复制到任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>示例︰ 将 DB2 数据复制到 Azure Blob

此示例演示如何将数据从内部 DB2 数据库复制到 Azure Blob 存储。 但是，数据可以复制**直接**对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  
 
此示例具有以下数据工厂实体︰

1.  链接的类型[OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。 
3.  [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。 
5.  [管线](data-factory-create-pipelines.md)与使用[RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。 

该示例将数据从 DB2 数据库中的查询结果到 Azure 的 blob 每小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，安装并配置数据管理网关。 在[内部部署位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)的文章的说明。

**链接的 DB2 服务︰**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }


**Azure Blob 存储链接的服务︰**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }

**DB2 输入数据集︰**

该示例假定您创建了一个表"MyTable"在 DB2 中，它包含名为"时间戳"时间系列数据的列。

设置为"外部": 真，通知数据工厂服务此数据集外部数据工厂并不由数据工厂中的活动。 请注意，**类型**设置为**RelationalTable**。 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
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


**Azure Blob 的输出数据集︰**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。

    {
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**管线与复制活动︰**

管线包含被配置为使用的输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**RelationalSource** ，**接收器**类型设置为**BlobSink**。 指定的**查询**属性从订单表中选择数据的 SQL 查询。

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>链接的 DB2 服务属性

下表提供了 JSON 元素特定于 DB2 的链接服务的说明。 

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- | 
| 类型 | 类型属性必须设置为︰ **OnPremisesDB2** | 是的 |
| 服务器 | DB2 服务器的名称。 | 是的 |
| 数据库 | DB2 数据库的名称。 | 是的 |
| 架构 | 在数据库中的架构的名称。 架构名称是区分大小写。 | 不 |
| authenticationType | 用于连接到 DB2 数据库的身份验证类型。 可能的值包括︰ 匿名的基本的和窗口。 | 是的 |
| 用户名 | 如果您使用基本或 Windows 身份验证，请指定用户名。 | 不 |
| 密码 | 指定对您指定的用户名的用户帐户的密码。 | 不 |
| gatewayName | 数据工厂服务连接到内部 DB2 数据库应使用的网关的名称。 | 是的 |

有关设置在部署 DB2 数据源的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security)。 


## <a name="db2-dataset-type-properties"></a>DB2 数据集类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和 JSON 数据集策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

TypeProperties 节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 为数据集的类型 （其中包括 DB2 数据集） 的 RelationalTable typeProperties 部分具有以下属性。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- | 
| 表名 | 链接的服务指的 DB2 数据库实例中的表的名称。 表名是区分大小写。 | 否 （如果指定了**RelationalSource** ，**查询**） |

## <a name="db2-copy-activity-type-properties"></a>DB2 复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。 

该活动的 typeProperties 部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

对于复制活动，当源类型**RelationalSource** （其中包括 DB2） 以下属性部分提供了 typeProperties:


| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------- | -------------- |
| 查询 | 使用自定义查询中读取数据。 | SQL 查询字符串。 例如︰ `"query": "select * from "MySchema"."MyTable""`。 | 否 （如果指定了**数据集**的**表名**）|

> [AZURE.NOTE] 架构和表的名称是区分大小写。 括起来的名称""（双引号） 的查询中。  

**示例︰**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>DB2 的类型映射
如[数据移动活动](data-factory-data-movement-activities.md)文章中提到，复制活动执行自动类型转换来水池下面 2 步方法类型的源类型︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

在将数据移到 DB2，以下映射都习惯从 DB2 类型的.NET 类型。

DB2 数据库类型 | .NET Framework 类型 
----------------- | ------------------- 
SmallInt | Int16
整数 | Int32
BigInt | Int64
现实 | 一个
双 | 双
浮点型 | 双
十进制 | 十进制
DecimalFloat | 十进制
数字 | 十进制
日期 | 日期时间
时间 | 时间跨度
时间戳 | 日期时间
Xml | Byte]
一个字符 | 字符串
VarChar | 字符串
LongVarChar | 字符串
DB2DynArray | 字符串
二元 | Byte]
低级 | Byte]
LongVarBinary | Byte]
图形 | 字符串
VarGraphic | 字符串
LongVarGraphic | 字符串
Clob | 字符串
Blob | Byte]
DbClob | 字符串
SmallInt | Int16
整数 | Int32
BigInt | Int64
现实 | 一个
双 | 双
浮点型 | 双
十进制 | 十进制
DecimalFloat | 十进制
数字 | 十进制
日期 | 日期时间
时间 | 时间跨度
时间戳 | 日期时间
Xml | Byte]
一个字符 | 字符串


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。


