<properties 
    pageTitle="将数据从数据工厂使用 MongoDB 移动 |Microsoft Azure" 
    description="了解如何将数据从使用 Azure 数据工厂 MongoDB 数据库移动。" 
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
    ms.date="08/04/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mongodb-using-azure-data-factory"></a>移动数据从 MongoDB 使用 Azure 数据工厂

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将数据从一个内部 MongoDB 数据库移动到另一个数据存储区。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动提供复制活动和复制活动支持的源/接收器数据存储组合。

数据工厂服务支持连接到内部 MongoDB 源使用数据管理网关。 数据管道移动数据设置网关，请参阅[数据管理网关](data-factory-data-management-gateway.md)文，了解有关数据管理网关和[移动数据从内部来云](data-factory-move-data-between-onprem-and-cloud.md)的文章的分步说明。 

> [AZURE.NOTE] 您需要使用网关连接到 MongoDB，即使承载于 Azure IaaS Vm。 如果要连接到的 MongoDB 承载在云实例，也可以在 IaaS VM 安装网关实例。

数据工厂目前支持仅移动数据从 MongoDB 到其他数据存储，而不是将数据从其他数据存储区移动到 MongoDB。

## <a name="prerequisites"></a>系统必备组件
对于 Azure 数据工厂服务，以便能够连接到内部 MongoDB 数据库，您必须安装以下组件︰ 

- 数据管理网关 2.0 或更高承载数据库的同一台计算机上或在单独的计算机，以避免争用资源的数据库。 数据管理网关是一种安全且管理方式连接到云服务的内部数据源软件。 请参阅[数据管理网关](data-factory-data-management-gateway.md)文章有关数据管理网关的详细信息。
  
    当您安装网关时，它将自动安装 Microsoft MongoDB ODBC 驱动程序用来连接到 MongoDB。 

## <a name="copy-data-wizard"></a>复制数据向导
创建管线 MongoDB 数据库中的数据复制到任何受支持的接收数据存储的最简单方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 它们显示了如何将数据从 MongoDB 数据库复制到 Azure Blob 存储。 但是，数据可复制到任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。

## <a name="sample-copy-data-from-mongodb-to-azure-blob"></a>示例︰ 将数据从 MongoDB 复制到 Azure Blob
此示例演示如何将数据从内部 MongoDB 数据库复制到 Azure Blob 存储。 但是，数据可以复制**直接**对任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。  
 
此示例具有以下数据工厂实体︰

1.  链接的类型[OnPremisesMongoDb](#linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [MongoDbCollection](#dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[MongoDbSource](#copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从 MongoDB 数据库中查询结果对 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，安装程序按照提供的说明[数据管理网关](data-factory-data-management-gateway.md)文章中的数据管理网关。 

**MongoDB 链接服务**

    { 
        "name": "OnPremisesMongoDbLinkedService", 
        "properties": 
        { 
            "type": "OnPremisesMongoDb", 
            "typeProperties": 
            { 
                "authenticationType": "<Basic or Anonymous>", 
                "server": "< The IP address or host name of the MongoDB server >",  
                "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>", 
                "username": "<username>", 
                "password": "<password>",
               "authSource": "< The database that you want to use to check your credentials for authentication. >",
                "databaseName": "<database name>",
                "gatewayName": "<mygateway>"
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

**MongoDB 输入数据集**设置为"外部":"真正的"通知数据工厂服务表外部数据工厂并不由数据工厂中的活动。
    
    {
         "name":  "MongoDbInputDataset",
        "properties": { 
            "type": "MongoDbCollection", 
            "linkedServiceName": "OnPremisesMongoDbLinkedService", 
            "typeProperties": { 
                "collectionName": "<Collection name>"   
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
                "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**MongoDbSource** ，**接收器**类型设置为**BlobSink**。 **查询**属性指定的 SQL 查询选择过去小时要复制的数据。
    
    {
        "name": "CopyMongoDBToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "MongoDbSource",
                            "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MongoDbInputDataset"
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
                    "name": "MongoDBToAzureBlob"
                }
            ],
            "start": "2016-06-01T18:00:00Z",
            "end": "2016-06-01T19:00:00Z"
        }
    }


## <a name="linked-service-properties"></a>链接的服务属性

下表提供了 JSON 元素特定于**OnPremisesMongoDB**链接服务的说明。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- | 
| 类型 | 类型属性必须设置为︰ **OnPremisesMongoDb** | 是的 |
| 服务器 | MongoDB 服务器 IP 地址或主机名。 | 是的 | 
| 端口 | MongoDB 服务器用来侦听客户端连接的 TCP 端口。 | 可选，默认值︰ 27017 |
| authenticationType | 基本的、 或匿名。 | 是的 | 
| 用户名 | 若要访问 MongoDB 的用户帐户。 | 是 （如果使用基本身份验证）。|
| 密码 | 用户的密码。 |   是 （如果使用基本身份验证）。 | 
| authSource | 您要用来检查您的凭据进行身份验证的 MongoDB 数据库名称。 | 可选 （如果使用基本身份验证）。 默认︰ 使用管理员帐户并指定使用 '数据库名称' 属性的数据库。 |  
| 数据库名称 | 您要访问的 MongoDB 数据库名称。 | 是的 |
| gatewayName | 网关访问数据存储区的名称。 | 是的 | 
| encryptedCredential | 通过网关加密的凭据。 | 可选 |


有关设置数据源的内部 MongoDB 的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security)。

## <a name="dataset-type-properties"></a>数据集的类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和 JSON 数据集策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

**TypeProperties**节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 类型**MongoDbCollection**的 typeProperties 部分数据集具有以下属性︰

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- |
| 集合名称 | 在 MongoDB 数据库集合的名称。 | 是的 | 

## <a name="copy-activity-type-properties"></a>复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。 

该活动的**typeProperties**部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

当源类型**MongoDbSource**以下属性是在 typeProperties 部分中︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- |
| 查询 | 使用自定义查询中读取数据。 | SQL-92 查询字符串。 例如︰ 选择 * 从 MyTable。 | 否 （如果指定**集合名称**的**数据集**） | 

## <a name="schema-by-data-factory"></a>数据工厂架构
Azure 数据工厂服务通过使用最新的 100 个文档集合中推断 MongoDB 集合中的架构。 如果这些 100 个文档不包含完整的架构，则可能在复制操作期间忽略某些列。 

## <a name="type-mapping-for-mongodb"></a>MongoDB 的类型映射

如[数据移动活动](data-factory-data-movement-activities.md)文章中提到，复制活动执行自动类型转换来水池下面 2 步方法类型的源类型︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

在将数据移到 MongoDB 以下映射用于从 MongoDB 类型对.NET 类型。

| MongoDB 类型 | .NET Framework 类型 |
| ------------------- | ------------------- | 
| 二元 | Byte] |
| 布尔值 | 布尔值 |
| 日期 | 日期时间 |
| NumberDouble | 双 |
| NumberInt | Int32 |
| NumberLong | Int64 |
| 对象 Id | 字符串 |
| 字符串 | 字符串 |
| UUID | Guid | 
| 对象 | 重新标准化到平面化具有嵌套分隔符"_"列 |

> [AZURE.NOTE]  
> 若要了解有关使用虚拟表的数组的支持，请参阅下面的[支持使用虚拟表的复杂类型](#support-for-complex-types-using-virtual-tables)的部分。 

目前，不支持以下 MongoDB 数据类型︰ 密钥的 DBPointer、 JavaScript、 最大值/最小值，正则表达式、 符号、 时间戳、 未定义

## <a name="support-for-complex-types-using-virtual-tables"></a>使用虚拟表的复杂类型的支持
Azure 数据工厂使用内置的 ODBC 驱动程序连接到并从 MongoDB 数据库复制数据。 对于如数组或对象在文档的不同类型的复杂类型，该驱动程序重新将数据标准化为相应的虚拟表。 特别是，如果表中包含这些列，驱动程序会产生下面的虚拟表︰

-   **基础表**，其中包含与真实表除了复杂类型列相同的数据。 基本表作为它所代表的实际表使用相同的名称。
-   对于每个复杂类型列，它可以扩展嵌套的数据**的虚拟表**。 使用真实表、 分隔符"_"和数组或对象的名称的名称进行命名的虚拟表。

虚拟表，请参阅启用驱动程序来访问非规范化的数据的实际表中的数据。 请参见示例部分详细信息的下面。 您可以通过查询和连接虚拟表访问 MongoDB 数组的内容。

可以使用[复制向导](data-factory-data-movement-activities.md#data-factory-copy-wizard)来直观地查看包括虚拟表，MongoDB 数据库中的表的列表，并预览中的数据。 此外可以构造查询中复制向导和验证以查看结果。

### <a name="example"></a>示例

例如，下面的"ExampleTable"是 MongoDB 表具有某一列使用的对象的数组中每个单元格 – 发票和某一列使用标量类型 – 额定值的数组。 

_id | 客户名称 | 发票 | 服务级别 | 额定值
--- | ------------- | -------- | ------------- | -------
1111 | ABC | [{invoice_id:"123"项目:"烤面包机"、 价格:"456"折扣:"0.2"}，{invoice_id:"124"项目:"烤箱"，价格:"1235"折扣:"0.2"}] | 银牌 | [5，6]
2222 | XYZ | [{invoice_id:"135"项目:"冰箱"、 价格:"12543"折扣:"0.0"}] | 金牌 | [1，2]

该驱动程序将生成多个虚拟表来表示单个表。 第一个虚拟目录是基表名为"ExampleTable"，如下所示。 基础表包含原始表的所有数据，但数组中的数据已被省略，并且在虚拟表中展开。

_id | 客户名称 | 服务级别
--- | ------------- | -------------
1111 | ABC | 银牌
2222 | XYZ | 金牌

下表显示表示在该示例中的原始阵列的虚拟表。 这些表包含下列信息︰ 

- 返回到原始主关键字列对应于原始数组 （通过 _id 列） 的行引用
- 相对值的原始数组中的数据的位置 
- 展开的数据数组中的每个元素

表"ExampleTable_Invoices":

_id | ExampleTable_Invoices_dim1_idx | invoice_id | 项目 | 价格 | 折扣
--- | -------------- | ---------- | ---- | ----- | --------
1111 | 0 | 123 | 烤箱 | 456 | 0.2
1111 | 1 | 124 | 烤箱 | 1235 | 0.2
2222 | 0 | 135 | 冰箱 | 12543 | 0.0

表"ExampleTable_Ratings":

_id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings
--- | ------------- | -------------
1111 | 0 | 5
1111 | 1 | 6
2222 | 0 | 1
2222 | 1 | 2

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。

## <a name="next-steps"></a>下一步行动
请参阅[移动内部和云之间的数据](data-factory-move-data-between-onprem-and-cloud.md)创建数据管道的逐步骤说明文章将数据从本地数据存储区移动到 Azure 数据存储区。 

