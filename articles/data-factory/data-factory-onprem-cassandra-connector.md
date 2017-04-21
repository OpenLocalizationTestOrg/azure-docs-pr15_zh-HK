<properties 
    pageTitle="卡桑德拉使用数据工厂从移动数据 |Microsoft Azure" 
    description="了解如何将数据从使用 Azure 数据工厂内部卡桑德拉数据库移动。" 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>将数据从使用 Azure 数据工厂内部卡桑德拉数据库
这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂内部卡桑德拉数据库中的数据复制到任何数据存储区，在接收器的[支持源和接收器](data-factory-data-movement-activities.md#supported-data-stores)部分中的列。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章，概要介绍了数据移动提供副本的活动和受支持的数据存储区的组合。

数据工厂目前支持只移动数据库中数据的卡桑德拉到[支持接收器的数据存储](data-factory-data-movement-activities.md#supported-data-stores)，但不是将数据从其他数据存储到卡桑德拉数据库。

## <a name="prerequisites"></a>系统必备组件
对于 Azure 数据工厂服务，以便能够连接到内部卡桑德拉数据库，您必须安装以下程序︰ 

- 数据管理网关 2.0 或更高承载数据库的同一台计算机上或在单独的计算机，以避免争用资源的数据库。 数据管理网关是一种安全且管理方式连接到云服务的内部数据源软件。 请参阅有关数据管理网关的详细信息的[内部和云之间的数据移动](data-factory-move-data-between-onprem-and-cloud.md)文章。
  
    当您安装网关时，它将自动安装 Microsoft 卡桑德拉的 ODBC 驱动程序用来连接到卡桑德拉的数据库。 

> [AZURE.NOTE] 请参阅故障排除连接/网关版的提示[疑难解答网关问题](data-factory-data-management-gateway.md#troubleshoot-gateway-issues)相关的问题。 

## <a name="copy-data-wizard"></a>复制数据向导
创建管线卡桑德拉数据库中的数据复制到任何受支持的接收数据存储的最简单方法是使用复制数据向导。 请参阅[教程︰ 创建管线使用复制向导](data-factory-copy-data-wizard-tutorial.md)上创建管道使用复制数据向导快速演练。 

下面的示例提供了示例 JSON 定义可用于通过使用[Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)或[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)或[Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)创建管线。 它们显示了如何将数据复制到 Azure Blob 存储卡桑德拉数据库。 但是，数据可复制到任何接收器规定[此处](data-factory-data-movement-activities.md#supported-data-stores)在 Azure 数据工厂中使用复制活动。   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>示例︰ 将数据从卡桑德拉复制至 Blob
该示例将数据复制卡桑德拉数据库到 Azure 的 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 数据可以直接复制到任何在 Azure 数据工厂中使用复制活动[数据移动活动](data-factory-data-movement-activities.md#supported-data-stores)文章中所述的接收器。 

- 链接的类型[OnPremisesCassandra](#onpremisescassandra-linked-service-properties)的服务。
- 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
- [CassandraTable](#cassandratable-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
- 类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
- [管线](data-factory-create-pipelines.md)与使用[CassandraSource](#cassandrasource-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

**卡桑德拉链接服务**

本示例使用的**卡桑德拉**链接服务。 此链接服务支持的属性，请参见[卡桑德拉链接服务](#onpremisescassandra-linked-service-properties)部分。  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
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

**卡桑德拉的输入数据集**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
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

将设置为**true**的**外部**通知数据工厂服务，数据集是外部数据工厂并不由数据工厂中的活动。

**Azure Blob 输出数据集**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**管线与复制活动**

管线包含被配置为使用的输入和输出的数据集，并计划每小时运行一次的复制活动。 在管线 JSON 定义中，将**源**类型设置为**CassandraSource** ，**接收器**类型设置为**BlobSink**。 

RelationalSource 支持的属性的列表，请参见[RelationalSource 类型属性](#cassandrasource-type-properties)。 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
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
## <a name="onpremisescassandra-linked-service-properties"></a>OnPremisesCassandra 链接服务属性

下表提供了 JSON 元素特定于卡桑德拉链接服务的说明。

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- | 
| 类型 | 类型属性必须设置为︰ **OnPremisesCassandra** | 是的 |
| 主机 | 一个或多个 IP 地址或主机名的卡桑德拉的服务器。<br/><br/>指定以逗号分隔的 IP 地址或主机名，可以同时连接到所有的服务器列表。 | 是的 | 
| 端口 | 卡桑德拉服务器用来监听客户端连接的 TCP 端口。 | 否，默认值︰ 9042 |
| authenticationType | 基本的、 或匿名 | 是的 |
| 用户名 |指定的用户帐户的用户名。 | 是的如果将 authenticationType 设置为基本。 |
| 密码 | 指定的用户帐户的密码。  | 是的如果将 authenticationType 设置为基本。 |
| gatewayName | 用于连接到内部卡桑德拉数据库网关的名称。 | 是的 |
| encryptedCredential | 通过网关加密的凭据。 | 不 | 

## <a name="cassandratable-properties"></a>CassandraTable 属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 节如结构、 可用性和 JSON 数据集策略为所有的数据集类型 (Azure SQL，Azure blob，Azure 表，等等。) 相近。

**TypeProperties**节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 类型**CassandraTable**的 typeProperties 部分数据集具有以下属性

| 属性 | 说明 | 必填 |
| -------- | ----------- | -------- |
| 密钥空间 | 卡桑德拉的数据库中的架构的密钥空间的名称。 | 是 （如果未定义**CassandraSource** **查询**）。 |
| 表名 | 卡桑德拉的数据库中的表的名称。 | 是 （如果未定义**CassandraSource** **查询**）。 |


## <a name="cassandrasource-type-properties"></a>CassandraSource 类型属性
节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，例如名称、 说明、 输入和输出表和策略都可用于所有类型的活动。 

该活动的 typeProperties 部分中可用的属性在另一方面随每种活动类型。 对于复制活动，它们因种源和接收器。

当源类型**CassandraSource**，以下属性是在 typeProperties 部分中︰

| 属性 | 说明 | 允许的值 | 必填 |
| -------- | ----------- | -------------- | -------- |
| 查询 | 使用自定义查询中读取数据。 | CQL 查询或 SQL 92 的查询。 请参阅[CQL 参考](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html)。 <br/><br/>当使用 SQL 查询，指定**密钥空间 name.table 名称**来表示您要查询的表。 | 否 （如果定义了表名和数据集上的密钥空间）。  |
| consistencyLevel | 一致性级别指定复制副本数必须在将数据返回到客户端应用程序之前对读请求的响应。 卡桑德拉检查指定的副本以用于数据以满足的读的请求数。 | 一个、 两个、 三个，仲裁，所有，LOCAL_QUORUM，EACH_QUORUM、 LOCAL_ONE。 有关详细信息，请参阅[配置数据的一致性](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html)。 | 不。 默认值为 1。 |  


### <a name="type-mapping-for-cassandra"></a>卡桑德拉的类型映射
卡桑德拉的类型 | .Net 的基类型
--------------- | ---------------
ASCII | 字符串 
BIGINT | Int64
BLOB | Byte]
布尔值 | 布尔值
十进制 | 十进制
双 | 双
浮点型 | 一个
INET | 字符串
INT | Int32
文本 | 字符串
时间戳 | 日期时间
TIMEUUID | Guid
UUID | Guid
VARCHAR | 字符串
VARINT | 十进制

> [AZURE.NOTE]  
> 集合类型 （映射、 集、 列表等），请参阅[使用虚拟表的卡桑德拉的集合类型处理](#work-with-collections-using-virtual-table)部分。 
> 
> 不支持用户定义的类型。
> 
> 二进制和字符串列的长度的长度不能大于 4000。 

## <a name="work-with-collections-using-virtual-table"></a>处理使用虚拟表集合
Azure 数据工厂使用内置的 ODBC 驱动程序连接到和卡桑德拉数据库中的数据复制。 集合类型包括地图、 组和列表，该驱动程序重新将数据标准化为相应的虚拟表。 特别是，如果表包含集合中的任何列，驱动程序会产生下面的虚拟表︰

-   **基础表**，其中包含与真实表除了集合列相同的数据。 基本表作为它所代表的实际表使用相同的名称。
-   对于每个集合列，它可以扩展嵌套的数据**的虚拟表**。 使用真实表、"_vt_"分隔符和列的名称的名称进行命名的虚拟表所表示的集合。

虚拟表，请参阅启用驱动程序来访问非规范化的数据的实际表中的数据。 有关详细信息请参见示例部分。 您可以通过查询以及加入虚拟表来访问卡桑德拉集合的内容。

您可以利用[复制向导](data-factory-data-movement-activities.md#data-factory-copy-wizard)来直观地查看卡桑德拉数据库包括虚拟表，表的列表和预览中的数据。 此外可以构造查询中复制向导和验证以查看结果。

### <a name="example"></a>示例
例如，下面的"ExampleTable"是卡桑德拉的数据库表包含整数主键列命名为"pk_int"、 名为 value 的文本列、 列表列、 映射列中和 （名为"StringSet"） 的一组列。

pk_int | 值 | 列表 | 地图 |   StringSet
------ | ----- | ---- | --- | --------
1 | "示例值 1" | ["1", "2", "3"]  | {"S1":"a"、"S2":"b"。 | {"A", "B", "C"}
3 | "示例值 3" | ["100"、"101"、"102"、"105"] | {"S1":"t"} | {"A", "E"}

该驱动程序将生成多个虚拟表来表示单个表。 在虚拟表中的外键列引用在实际的表中，主键列和指明虚拟表中的行对应于哪些实际的表行。 

第一个虚拟目录是基表名为"ExampleTable"显示在下表中。 基础表包含相同的数据集合，此表省略和展开其他虚拟表中除原始数据库表。

pk_int | 值
------ | -----
1 | "示例值 1"
3 | "示例值 3"

下表显示了 renormalize 列表、 映射和 StringSet 列中的数据的虚拟表。 以"_index"或"_key"结尾的名称的列表示原始列表或图中的数据的位置。 以"_value"结尾的名称的列包含集合中的扩展的数据。

#### <a name="table-exampletablevtlist"></a>表"ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>表"ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | 一个
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>表"ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | 一个
1 | B
1 | C
3 | 一个
3 | 电子





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>性能和调整  
请参阅[复制活动性能及调优指南](data-factory-copy-activity-performance.md)，了解移动数据 （副本活动） 在 Azure 数据工厂，并对其优化的各种方法中影响性能的关键因素。
