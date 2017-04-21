<properties 
    pageTitle="将数据从数据工厂使用 Amazon Redshift 移动 |Microsoft Azure" 
    description="了解如何将数据从使用 Azure 数据工厂的 Amazon Redshift 移动。" 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>移动数据使用 Azure 数据工厂从 Amazon Redshift

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将从 Amazon Redshift 到另一个数据存储的数据移动。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动提供复制活动和源/接收器的数据存储区列表。  

数据工厂目前支持仅移动数据从 Amazon Redshift 到其他数据存储，而不是将数据从其他数据存储区移动到 Amazon Redshift。

## <a name="prerequisites"></a>系统必备组件

- 如果将数据移动到本地数据存储区中，授权 Amazon Redshift 群集访问数据管理网关 （使用这台计算机的 IP 地址）。 有关说明，请参阅[授权访问群集的权限](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html)。 
- 如果要将数据移动到 Azure 数据存储区，请参阅使用 Microsoft Azure 数据中心的计算 IP 地址范围 （包括 SQL 范围） [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=41653)。 

## <a name="copy-data-wizard"></a>复制数据向导
创建管线从 Amazon Redshift 复制数据的最简便方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 它演示如何将数据从 Amazon Redshift 复制到 Azure Blob 存储。 但是，数据可复制到任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)。

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>示例︰ 将数据从 Amazon Redshift 复制到 Azure Blob
此示例演示如何将数据从 Amazon Redshift 数据库复制到 Azure Blob 存储。 但是，数据可以复制**直接**对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  
 
此示例具有以下数据工厂实体︰

- 链接的类型[AmazonRedshift](#linked-service-properties)的服务。
- 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
- [RelationalTable](#dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
- 类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
- [管线](data-factory-create-pipelines.md)与使用[RelationalSource](#copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从 Amazon Redshift 中的查询结果对 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

**Amazon Redshift 链接服务**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Amazon Redshift 输入数据集**

设置**"外部": 真**数据集外部数据工厂并不由数据工厂中的活动通知数据工厂服务。 在输入数据集不是通过管道中的一个活动上，此属性设置为 true。

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
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
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyAmazonRedshiftToBlob",
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
                            "name": "AmazonRedshiftInputDataset"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>链接的服务属性

下表提供了 JSON 元素特定于 Amazon Redshift 链接服务的说明。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- | 
| 类型 | 类型属性必须设置为︰ **AmazonRedshift**。 | 是的 |
| 服务器 | Amazon Redshift 服务器的 IP 地址或主机名。 | 是的 |
| 端口 | Amazon Redshift 服务器用来侦听客户端连接的 TCP 端口号。 | 否，默认值︰ 5439 |
| 数据库 | Amazon Redshift 数据库的名称。 | 是的 |
| 用户名 | 有权访问数据库的用户的名称。| 是的 |
| 密码 | 该用户帐户的的密码。| 是的 |


## <a name="dataset-type-properties"></a>数据集的类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

**TypeProperties**节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 TypeProperties 部分类型**RelationalTable** （其中包括 Amazon Redshift 数据集） 的数据集具有以下属性

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- |
| 表名 | 链接的服务指 Amazon Redshift 数据库中的表的名称。 | 否 （如果指定了**RelationalSource** ，**查询**） | 

## <a name="copy-activity-type-properties"></a>复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。 

该活动的**typeProperties**部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

当复制活动的源类型**RelationalSource** （其中包括 Amazon Redshift） 以下属性是在 typeProperties 部分中︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- |
| 查询 | 使用自定义查询中读取数据。 | SQL 查询字符串。 例如︰ 选择 * 从 MyTable。 | 否 （如果指定了**数据集**的**表名**） | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Amazon Redshift 的类型映射

如[数据移动活动](data-factory-data-movement-activities.md)文章中提到，复制活动执行自动类型转换来水池下面两个方法的类型的源类型︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

将数据移动到 Amazon Redshift，将以下映射从 Amazon Redshift 类型中使用.NET 类型中。

Amazon Redshift 类型 | .NET 的基类型
-------------------- | ---------------
SMALLINT | Int16
整数 | Int32
BIGINT | Int64
十进制 | 十进制
现实 | 一个
双精度 | 双
布尔值 | 字符串
一个字符 | 字符串
VARCHAR | 字符串
日期 | 日期时间
时间戳 | 日期时间
文本 | 字符串



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。

## <a name="next-steps"></a>下一步行动
请参阅以下文章︰ 
- 使用复制活动创建管线的指导[教程复制活动](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。 