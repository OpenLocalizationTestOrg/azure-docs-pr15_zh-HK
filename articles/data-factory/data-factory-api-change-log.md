<properties 
    pageTitle="数据工厂-.NET API 更改日志 |Microsoft Azure" 
    description="介绍重大更改、 新增功能、 缺陷修补程序等等...在 Azure 数据工厂的.NET API 的特定版本。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure 数据工厂-.NET API 更改日志 
这篇文章中的特定版本到 Azure 数据工厂 SDK 提供有关更改的信息。 您可以找到最新的 NuGet 程序包 Azure 数据工厂为[此处](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>版本 4.11.0
新增功能︰

- 已添加以下服务链接的类型︰
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- 已添加了以下的数据集类型︰ 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- 已添加了下面的复制源类型︰
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>版本 4.10.0
- 绑定到已添加以下可选属性︰
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- 已添加以下服务链接的类型︰
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- 已添加了以下的数据集类型︰
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- 已添加了下面的复制源类型︰
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- 将[WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx)属性添加到 AzureMLBatchExecutionActivity 
    - 启用传递多个 web 服务输入到 Azure 机器学习实验


## <a name="version-491"></a>4.9.1 版

### <a name="bug-fix"></a>Bug 修复

- 否决 WebApi 基于[WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx)的身份验证。

## <a name="version-490"></a>版本 4.9.0

### <a name="feature-additions"></a>新增功能

- 将[EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx)和[StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx)属性添加到 CopyActivity 中。 有关此功能的详细信息，请参阅[分步副本](data-factory-copy-activity-performance.md#staged-copy)。


### <a name="bug-fix"></a>Bug 修复

- 介绍[ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx)方法，采用[ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx)实例的重载。
- 将[WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx)和[WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx)标记为可选的 CopySink。

## <a name="version-480"></a>版本 4.8.0

### <a name="feature-additions"></a>新增功能
- 以下可选属性已添加到复制活动类型启用的复制性能调整︰
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>版本 4.7.0

### <a name="feature-additions"></a>新增功能
* 添加新的 StorageFormat 类型[OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx)类型优化的行纵栏 (ORC) 格式复制文件。
* 将[AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx)和 PolyBaseSettings 属性添加到 SqlDWSink 中。
    * 能够使用 PolyBase 将数据复制到 SQL 数据仓库。

## <a name="version-461"></a>4.6.1 版本

### <a name="bug-fixes"></a>错误修复
* 解决了 HTTP 请求列出活动窗口。
    * 从请求有效负载中移除资源组的名称和数据工厂名称。

## <a name="version-460"></a>版本 4.6.0

### <a name="feature-additions"></a>新增功能

- [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)中添加以下属性︰
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [数据集](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)中添加以下属性︰
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- 添加新[StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx)类型[JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx)类型来定义其数据是 JSON 格式的数据集。 

## <a name="version-450"></a>4.5.0

### <a name="feature-additions"></a>新增功能
* 添加的[活动窗口的列表操作](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx)。
    * 添加的方法以检索与基于实体类型 （即，数据工厂、 数据集、 管道和活动） 的筛选器的活动窗口。
* 已添加以下服务链接的类型︰ 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx) [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* 已添加了以下的数据集类型︰ 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx) [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* 已添加了下面的复制源类型︰  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>版本 4.4.0

### <a name="feature-additions"></a>新增功能

- 下面的链接的服务类型已添加作为数据源和接收器的复制活动︰
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx)。 有关的概念信息和示例，请参见[Azure 存储 SA 链接服务](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service)。 

## <a name="version-430"></a>4.3.0 版本

### <a name="feature-additions"></a>新增功能

- 下面链接的服务类型庇护所被添加为复制活动的数据源︰
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx)。 有关的概念信息和示例，请参阅[移动 HDFS 数据工厂使用的数据](data-factory-hdfs-connector.md)。 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx)。 有关的概念信息和示例，请参阅[移动数据从 ODBC 数据存储使用 Azure 数据工厂](data-factory-odbc-connector.md)。 

## <a name="version-420"></a>4.2.0 版

### <a name="feature-additions"></a>新增功能

- 已添加了下列新的活动类型︰ [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx)。 有关活动的详细信息，请参阅[更新 Azure ML 模型使用更新资源活动](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity)。
- 已添加新的可选属性[updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx)到[AzureMLLinkedService 类](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx)。 
- [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx)和[LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx)属性已被添加到[DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx)类。 
- 允许配置的客户端调用数据工厂服务的超时值。 


## <a name="version-410"></a>版本 4.1.0

### <a name="feature-additions"></a>新增功能
* 已添加以下服务链接的类型︰ 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* 已添加了以下的活动类型︰ 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* 已添加了以下的数据集类型︰ 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* 已添加以下源和接收器的复制活动类型︰
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>版本 4.0.1 版

### <a name="breaking-changes"></a>重大更改
下面的类已被重命名。 新名称是原始类的名称之前 4.0.0 释放。 
 
在 4.0.0 命名 | 4.0.1 版中的名称
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>4.0.0 版

### <a name="breaking-changes"></a>重大更改



- 下面的类接口已被重命名。

| 旧名称 | 新名称 |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| 表 | [数据集](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- **列表**方法立即返回分页的结果。 如果响应包含非空的**NextLink**属性，客户端应用程序将需要继续直到所有页将都返回取下一个页面。  下面是一个示例︰ 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **列表**管线 API 返回只管道而不是完整的详细信息的摘要。 例如，在管线汇总活动只能包含名称和类型。

### <a name="feature-additions"></a>新增功能
- [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx)类支持两个新属性， **SliceIdentifierColumnName**和**SqlWriterCleanupScript**，支持幂等复制到 Azure SQL 数据仓库。 [Azure SQL 数据仓库](data-factory-azure-sql-data-warehouse-connector.md)项目，具体来说，[机制 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1)和[机制 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2)节，有关这些属性的详细信息，请参阅。

- 现在，我们支持作为复制活动的一部分，针对 SQL Azure 数据库和 Azure SQL 数据仓库的源运行存储的过程。 [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx)和[SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx)类具有以下特点︰ **SqlReaderStoredProcedureName**和**StoredProcedureParameters**。 有关这些属性的详细信息，请参阅 Azure.com 一[SQL Azure 数据库](data-factory-azure-sql-connector.md#sqlsource)和[Azure SQL 数据仓库](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource)文章。  