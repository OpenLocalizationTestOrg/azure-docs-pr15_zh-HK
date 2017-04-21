<properties
    pageTitle="使用索引器的 Azure 搜索连接 DocumentDB |Microsoft Azure"
    description="这篇文章演示了如何使用 Azure 搜索索引器与 DocumentDB 作为数据源。"
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>使用索引器的 Azure 搜索连接 DocumentDB

如果您希望在您的 DocumentDB 数据实现很好的搜索体验，DocumentDB 使用 Azure 搜索索引器 ！ 在本文中，我们将介绍如何将 Azure DocumentDB Azure 搜索与集成而无需编写任何代码来维护基础索引结构 ！

对此进行设置，您必须[设置 Azure 搜索帐户](../search/search-create-service-portal.md)（不需要升级到标准搜索），，然后调用[Azure 搜索 REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)来创建一个 DocumentDB**的数据源**，该数据源的**索引器**。

在订单发送请求与 REST Api 进行交互，您可以使用[把邮递员弄](https://www.getpostman.com/)， [Fiddler](http://www.telerik.com/fiddler)或您喜欢的任何工具。


##<a id="Concepts"></a>Azure 搜索索引器概念

Azure 搜索支持创建和管理数据源 （包括 DocumentDB），并针对这些数据源操作的索引器。

**数据源**指定哪些数据需要被索引，凭据来访问数据和策略以便启用 Azure 搜索来有效地识别数据 （例如，修改或删除您的集合内的文档） 中的更改。 数据源被指独立的资源，以便它可以由多个索引器。

**索引器**描述了数据流动方式从数据源到目标搜索索引。 您应计划创建一个索引器，为每个目标索引和数据源组合。 虽然可以有多个索引器写入到同一个索引，索引器可以只写入到单个索引中。 索引器是用来︰

- 执行一次性复制的数据填充索引。
- 同步的时间表数据源中的更改的索引。 该计划是索引器定义的一部分。
- 调用索引根据需要按需更新。

##<a id="CreateDataSource"></a>步骤 1︰ 创建数据源

发出 HTTP POST 请求来创建新的数据源在 Azure 搜索服务中，包括了以下的请求标头。

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

`api-version`是必需的。 有效值包括`2015-02-28`或更高版本。 若要查看所有受支持的搜索 API 版本[在 Azure 搜索 API 版本](../search/search-api-versions.md)，请访问。

请求的正文中包含的数据源定义，其中应该包含以下字段︰

- **名称**︰ 选择代表您的 DocumentDB 数据库的任何名称。

- **类型**︰ 使用`documentdb`。

- **凭据**︰

    - **连接字符串**︰ 所需。 按以下格式指定到 Azure DocumentDB 数据库的连接信息︰`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **容器**︰

    - **名称**︰ 所需。 指定 DocumentDB 集合索引的 id。

    - **查询**︰ 可选。 您可以指定一个查询来拼合到 Azure 搜索可编制索引的平面架构的一个任意的 JSON 文档。

- **dataChangeDetectionPolicy**︰ 可选。 请参阅[数据变化检测策略](#DataChangeDetectionPolicy)的下面。

- **dataDeletionDetectionPolicy**︰ 可选。 请参阅[数据删除检测策略](#DataDeletionDetectionPolicy)下面。

请参见下面[的示例请求正文](#CreateDataSourceExample)。

###<a id="DataChangeDetectionPolicy"></a>获取已更改的文档

数据更改检测策略的目的是有效地识别已更改的数据项目。 目前，唯一受支持的策略就是`High Water Mark`策略使用`_ts`上次修改的时间戳属性提供的 DocumentDB-指定，如下所示︰

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

您还需要添加`_ts`在投影中和`WHERE`为您的查询子句。 例如︰

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>捕获已删除的文档

从源表中删除行时, 您应从搜索索引以及删除这些行。 数据删除检测策略的目的是有效地识别出已删除的数据项。 目前，唯一受支持的策略就是`Soft Delete`策略 （删除标记用的某种标志），指定如下︰

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] 您将需要包括 SELECT 子句中的 softDeleteColumnName 属性，如果您使用的自定义投影。

###<a id="CreateDataSourceExample"></a>请求正文示例

下面的示例创建一个数据源与自定义的查询和策略的提示︰

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>响应

如果数据源已成功创建，您将收到一个 HTTP 201 创建响应。

##<a id="CreateIndex"></a>步骤 2︰ 创建索引

如果您还没有，创建目标 Azure 搜索索引。 从[Azure 门户用户界面](../search/search-create-index-portal.md)或通过[创建索引 API](https://msdn.microsoft.com/library/azure/dn798941.aspx)，您可以执行此操作。

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


请确保目标索引架构兼容的架构源 JSON 文档或输出的自定义查询投影。

>[AZURE.NOTE] 对于已分区的集合，默认文档键是 DocumentDB 的`_rid`属性，获取重命名为`rid`Azure 搜索中。 此外，DocumentDB 的`_rid`值中包含在 Azure 搜索键中无效的字符因此，`_rid`的值为 Base64 编码。

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>图 a: JSON 数据类型和 Azure 的搜索数据类型之间的映射

| JSON 数据类型|   兼容的目标索引字段类型|
|---|---|
|布尔值|Edm.Boolean Edm.String|
|看起来像整数数字|Edm.Int32，Edm.Int64，Edm.String|
|数字看起来像浮动点|Edm.Double Edm.String|
|字符串|Edm.String|
|数组的基元类型如"a"、"b"，"c" |Collection(Edm.String)|
|外观上像日期的字符串| Edm.DateTimeOffset Edm.String|
|GeoJSON 对象如 {"类型":"点"，"坐标": [长，lat]} | Edm.GeographyPoint |
|其他的 JSON 对象|N/A|

###<a id="CreateIndexExample"></a>请求正文示例

下面的示例创建索引的 id 和描述字段︰

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>响应

如果成功创建索引时，您会收到一个 HTTP 201 创建响应。

##<a id="CreateIndexer"></a>步骤 3︰ 创建索引

通过使用 HTTP POST 请求与以下标头可以在 Azure 搜索服务中新的索引器。

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

请求的正文中包含索引定义中，应包含以下字段︰

- **名称**︰ 所需。 该索引器的名称。

- **名称**︰ 所需。 现有数据源的名称。

- **targetIndexName**︰ 所需。 现有索引的名称。

- **计划**︰ 可选。 请参阅[索引日程安排](#IndexingSchedule)下。

###<a id="IndexingSchedule"></a>按计划运行索引器

索引器可以选择指定一个计划。 如果存在一个计划，该索引器将按照日程安排定期运行。 计划有以下特性︰

- **间隔**︰ 所需。 运行指定的时间间隔或索引器的周期持续时间值。 允许的最小间隔为 5 分钟;最长为一天。 必须为 XSD"dayTimeDuration"值 （有限[ISO 8601 工期](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration)值的一部分） 将其格式化。 这样的模式是︰ `P(nD)(T(nH)(nM))`。 示例︰`PT15M`每隔 15 分钟，为`PT2H`的每 2 小时。

- **开始时间**︰ 所需。 指定索引器时应启动 UTC 日期时间运行。

###<a id="CreateIndexerExample"></a>请求正文示例

下面的示例创建将数据从所引用的集合的索引器`myDocDbDataSource`数据源到`mySearchIndex`索引的计划，在 UTC 2015 年 1 月 1 日开始，每小时运行。

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>响应

如果已成功创建了索引，您将收到一个 HTTP 201 创建响应。

##<a id="RunIndexer"></a>步骤 4︰ 运行索引器

除了按计划定期运行，索引器，也可以调用按需通过发出以下 HTTP POST 请求︰

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>响应

如果成功调用索引器，则会收到一个 HTTP 202 接受响应。

##<a name="GetIndexerStatus"></a>第 5 步︰ 获取索引器状态

您可以发出 HTTP GET 请求来检索索引器的当前状态和执行历史记录︰

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>响应

您将看到包含索引器的总体健康状态、 上次索引器调用，以及最新的索引器调用的历史记录的信息 （如果存在） 响应正文以及返回一个 HTTP 200 确定响应。

响应应类似于以下形式︰

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

多 50 最近已完成执行，以反向时间顺序排在 （这样的最新执行最先响应中） 包含执行历史记录。

##<a name="NextSteps"></a>下一步行动

祝贺您 ！ 您学习了如何使用索引器的 DocumentDB Azure 搜索与集成 Azure DocumentDB。

 - 若要了解如何 Azure DocumentDB，请参阅[DocumentDB 服务页面](https://azure.microsoft.com/services/documentdb/)。

 - 若要了解如何 Azure 搜索有关的详细信息，请参阅[搜索服务页面](https://azure.microsoft.com/services/search/)。
