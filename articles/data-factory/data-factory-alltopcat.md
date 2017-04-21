<properties
    pageTitle="数据工厂服务的所有主题 |Microsoft Azure"
    description="Azure 服务的所有主题的表命名为 http://azure.microsoft.com/documentation/articles/、 标题和说明中存在的数据工厂。"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Azure 数据工厂服务的所有主题

本主题列出了每个主题的 Azure**数据工厂**服务直接应用。 您可以通过使用**Ctrl + F**，若要查找当前感兴趣的主题搜索此关键字的网页。




## <a name="new"></a>新增功能

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 1 | [移动数据使用 Azure 数据工厂从 Amazon Redshift](data-factory-amazon-redshift-connector.md) | 了解如何将数据从使用 Azure 数据工厂的 Amazon Redshift 移动。 |
| 2 | [移动数据从 Amazon 简单存储服务使用 Azure 数据工厂](data-factory-amazon-simple-storage-service-connector.md) | 了解如何将数据从 Amazon 简单存储服务 (S3) 使用 Azure 数据工厂。 |
| 3 | [Azure 数据工厂-复制向导](data-factory-azure-copy-wizard.md) | 了解有关如何使用数据工厂 Azure 复制向导支持的数据源的数据复制到接收器。 |
| 4 | [教程︰ 创建使用数据工厂 REST API，您第一个 Azure 数据工厂](data-factory-build-your-first-pipeline-using-rest-api.md) | 在本教程中，您可以创建使用数据工厂 REST API 示例 Azure 数据工厂管线。 |
| 5 | [教程︰ 创建管线与使用.NET API 的复制活动](data-factory-copy-activity-tutorial-using-dotnet-api.md) | 在本教程中，您创建 Azure 数据工厂管道与复制活动使用.NET API。 |
| 6 | [教程︰ 创建管线与使用 REST API 的复制活动](data-factory-copy-activity-tutorial-using-rest-api.md) | 在本教程中，您创建 Azure 数据工厂管道与复制活动使用 REST API。 |
| 7 | [数据工厂复制向导](data-factory-copy-wizard.md) | 了解有关如何使用数据工厂复制向导支持的数据源的数据复制到接收器。 |
| 8 | [数据管理网关](data-factory-data-management-gateway.md) | 设置数据网关内部和云之间移动数据。 使用在 Azure 数据工厂数据管理网关移动数据。 |
| 9 | [将数据从使用 Azure 数据工厂内部卡桑德拉数据库](data-factory-onprem-cassandra-connector.md) | 了解如何将数据从使用 Azure 数据工厂内部卡桑德拉数据库移动。 |
| 10 | [移动数据从 MongoDB 使用 Azure 数据工厂](data-factory-on-premises-mongodb-connector.md) | 了解如何将数据从使用 Azure 数据工厂 MongoDB 数据库移动。 |
| 11 | [将数据从队伍移动通过使用 Azure 数据工厂](data-factory-salesforce-connector.md) | 了解如何使用 Azure 数据工厂的销售队伍从移动数据。 |


## <a name="updated-articles-data-factory"></a>更新的文章，数据工厂

本部分列出了文章，其中更新了最近，大或巨大，其中已更新。 对于每个更新的文章，显示添加的减价文本的粗糙段。 在**2016年-10-05**到**2016年-08-22**日期范围内更新了文章。

| &nbsp; | 文章 | 已更新的文本、 代码段 | 更新时间 |
| --: | :-- | :-- | :-- |
| 12 | [Azure 数据工厂-.NET API 更改日志](data-factory-api-change-log.md) | 这篇文章中的特定版本到 Azure 数据工厂 SDK 提供有关更改的信息。 您可以找到有关此处 Azure 数据工厂 (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)**版本 4.11.0**的新增功能的新的 NuGet 程序包: / 已添加以下链接的服务类型:-OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx)-AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx)-AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / 已添加了以下的数据集类型:-MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx)-AmazonS3Dataset (https://msdn.microsoft.com/library/mt765112.aspx) / 已添加了以下复制源类型:-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx)**版本 4.10.0** / 绑定到已添加以下可选属性:-滑雪 | 2016-09-22 |
| 13 | [使用 Azure 数据工厂的 Azure 斑点之间转移数据](data-factory-azure-blob-connector.md) |  / copyBehavior / BlobSource 或文件系统源时定义的复制行为。  / **PreserveHierarchy:**保留在目标文件夹中的文件层次结构。 源代码文件到源文件夹的相对路径等同于目标文件的目标文件夹的相对路径。br /.br /。**FlattenHierarchy:**在第一级的目标文件夹是源文件夹中的所有文件。 目标文件具有自动生成的名称。 启动 /.br /。**MergeFiles: （默认）**将合并到一个文件的源文件夹中所有文件。 如果指定了文件/Blob 名称，合并的文件的名称将指定的名称;否则，将自动生成的文件的名称。  / 不 / **BlobSource**还支持这两个属性用于向后兼容性。 / **treatEmptyAsNull**︰ 指定是否将空值为 null 或空字符串。 / **skipHeaderLineCount** -指定需要跳过的行数。 仅当输入数据集使用绑定是适用。 同样， **BlobSink**支持日 | 2016-09-28 |
| 14 | [创建预测管道使用 Azure 机器学习活动](data-factory-azure-ml-batch-execution-activity.md) | **Web 服务需要多个输入**如果 web 服务接受多个输入，而不是使用**webServiceInput**使用**webServiceInputs**属性。 **WebServiceInputs**所引用的数据集还必须包含在所活动的**输入**。 在 Azure ML 实验，web 服务的输入和输出端口和全局参数具有可自定义的默认名称 （"输入 1"，"输入 2"）。 用于 webServiceInputs、 webServiceOutputs 和 globalParameters 设置的名称必须完全匹配试验中的名称。 您可以在批处理执行帮助页上以验证预期的映射您 Azure ML 终结点查看示例请求负载。    {"name":"PredictivePipeline"，"属性": {"说明":"使用 AzureML 模型"，"活动": {"name":"MLActivity"，"类型":"AzureMLBatchExecution"，"说明":"批次输入的预测分析，""输入": {"名称":"inputDataset1"}，{"名称":"inputDatase | 2016-09-13 |
| 15 | [复制活动性能和优化指南](data-factory-copy-activity-performance.md) | 1。**建立一个基线**。 在开发阶段，测试通过对具有代表性的数据示例复制活动的管道。 数据工厂切片模型 (数据-工厂-计划-和-execution.md time-series-datasets-and-data-slices) 可用于限制您所使用的数据量。  通过**监视和管理应用程序**收集执行时间和性能特征。 数据工厂主页上选择**监视和管理**。 在树视图中，选择**输出数据集**。 在**活动窗口**列表中，选择运行复制活动。 **活动窗口**列出复制活动持续时间，并将复制的数据的大小。 **活动窗口资源管理器**中列出了吞吐量。 若要了解有关应用程序的详细信息，请参阅显示器使用和管理 Azure 数据工厂管道的监视和管理应用程序 (数据-工厂-显示器-管理-app.md)。  ! 运行详细信息 (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn 的活动 | 2016-09-27 |
| 16 | [教程︰ 创建管线与使用 Visual Studio 的复制活动](data-factory-copy-activity-tutorial-using-visual-studio.md) |   请注意以下几点:-数据集**类型**设置为**AzureBlob**。     - **linkedServiceName**设置为**AzureStorageLinkedService**。 您在步骤 2 中创建此链接的服务。     -**采用文件夹路径**设置为**adftutorial**容器。 您还可以指定一个 blob 中使用的**文件名**属性的文件夹的名称。 由于并不指定的 blob 名称，从该容器中的所有 blob 数据被认为是作为输入的数据。    格式**类型**设置为**格式**的文本文件 ΓÇô**名字字段**和**姓氏**ΓÇô 分隔逗号字符 (**columnDelimiter**) 中有两个字段-**可用性**设置为**每小时**（**频率**设置为**小时**和**间隔**设置为**1**）。 因此，数据工厂寻找输入数据每隔一小时的 blob 容器 (**adftutorial**) 所指定的根文件夹中。  如果没有指定**文件名**用于**输入**数据集，从输入的文件夹 (**采用文件夹路径**) 的所有文件/blob 都是 consid | 2016-09-29 |
| 17 | [创建、 监视和管理使用数据工厂.NET SDK 的 Azure 数据工厂](data-factory-create-data-factories-programmatically.md) | 记下应用程序 ID 和密码 （客户端），在本演练中使用它。 **获取 Azure 订阅和租户 Id**如果您没有 PowerShell 您计算机上安装最新版本，请遵循说明如何安装和配置 Azure PowerShell (.../ powershell-安装 configure.md） 文章以安装它。 1。 启动 Azure PowerShell 并运行以下命令 2。 运行以下命令，并输入用户名和密码用于登录到 Azure 的门户。         登录 AzureRmAccount，如果您有一个与此帐户关联的 Azure 订阅，您不需要执行以下两个步骤。 3。 运行以下命令来查看该帐户的所有订阅。       获得 AzureRmSubscription 4。 运行下面的命令以选择想要使用的订阅。 Azure 订阅的名称替换**NameOfAzureSubscription** 。       获得 AzureRmSubscription-SubscriptionName NameOfAzureSubscription / 集 AzureRmCo | 2016-09-14 |
| 18 | [管线和 Azure 数据工厂中的活动](data-factory-create-pipelines.md) |       "开始":"2016年-07-12T00:00:00Z"，"结束":"2016年-07-13T00:00:00Z"}} 请注意以下几点︰ 在活动部分，有是只能有一个活动的**类型**设置为**复制**。 / 活动输入设置为**InputDataset** ，输出为活动设置为**OutputDataset**。 在**typeProperties**部分中， **BlobSource**指定为源类型和**SqlSink**指定为接收器类型。 完成创建此管线的演练，请参见教程︰ Blob 存储中的数据复制到 SQL 数据库 (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。 **示例转换管道**在下面的示例管线中，没有一个活动类型的**活动**部分的**HDInsightHive** 。 在此示例中，HDInsight 配置单元活动 (数据-工厂-配置单元-activity.md) 的运行在 Azure HDInsight Hadoop 群集配置单元脚本文件转换从 Azure Blob 存储的数据。  {"name":"TransformPipeline"，"p | 2016-09-27 |
| 19 | [转换数据在 Azure 数据工厂](data-factory-data-transformation-activities.md) | 数据工厂支持可以将添加到以下数据转换活动分别是管线 (数据-工厂-创建-pipelines.md) 或链与另一个活动。 .  AZURE。注意︰ 对于演练提供分步说明，请参阅创建管线配置单元转换 (data-factory-build-your-first-pipeline.md) 的文章。 **HDInsight 配置单元活动**数据工厂管线中的 HDInsight 配置单元活动执行您自己的配置单元查询或按需基于 Windows/Linux HDInsight 群集。 请参阅配置单元活动 (数据-工厂-配置单元-activity.md) 文章有关此活动的详细信息。 **HDInsight 小猪活动**数据工厂管线中的 HDInsight 豚活动执行自己的猪的查询或按需基于 Windows/Linux HDInsight 群集。 猪的活动 (数据-工厂-猪-activity.md) 参见有关此活动的详细信息。 **HDInsight MapReduce 活动**在 y 轴上的数据工厂管线中的 HDInsight MapReduce 活动执行 MapReduce 程序 | 2016-09-26 |
| 20 | [数据工厂计划编制和执行](data-factory-scheduling-and-execution.md) | 已成功地运行了 CopyActivity1 和 Dataset2 是可用时，才会运行 CopyActivity2。 下面是示例管道 JSON: {"名称":"ChainActivities"，"属性": {"说明":"运行序列中的活动，""活动": {"类型":"复制"、"typeProperties": {"源": {"类型":"BlobSource"}，"接收器": {"类型":"BlobSink"，"copyBehavior":"PreserveHierarchy"，"writeBatchSize": 0，"writeBatchTimeout":"00: 00:00"}}，"输入": {"名称":"Dataset1"}，"输出": {"名称":"Dataset2"}，"策略": {"超时":"01: 00:00"}，"计划": {"频率":"小时"、"间隔": 1}、"姓名":"CopyFromBlob1ToBlob2"，"说明":"将数据从一个 blob 复制到另一个"}，{"类型":"复制"、"typeProperties": {"源": {"类型":"BlobSource"}，"接收器": {"类型":"BlobSink"，"writeBatchSize": 0，"writeBatchTimeout":"00: 00:00"}}， | 2016-09-28 |





## <a name="tutorials"></a>教程

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 21 | [教程︰ 创建用于处理数据使用 Hadoop 群集您第一个渠道](data-factory-build-your-first-pipeline.md) | 本 Azure 数据工厂教程演示如何创建和安排处理数据使用 Hadoop 群集配置单元脚本的数据工厂。 |
| 22 | [教程︰ 构建第一个 Azure 数据工厂使用 Azure 资源管理器模板](data-factory-build-your-first-pipeline-using-arm.md) | 在本教程中，您将创建使用 Azure 资源管理器模板示例 Azure 数据工厂管线。 |
| 23 | [教程︰ 构建第一个 Azure 数据工厂使用 Azure 门户](data-factory-build-your-first-pipeline-using-editor.md) | 在本教程中，您创建示例 Azure 数据工厂管线使用数据工厂编辑器在 Azure 的门户。 |
| 24 | [教程︰ 构建第一个使用 Azure PowerShell Azure 数据工厂](data-factory-build-your-first-pipeline-using-powershell.md) | 在本教程中，您将创建示例 Azure 数据工厂管线使用 Azure PowerShell。 |
| 25 | [使用 Microsoft Visual Studio 教程︰ 生成第一个 Azure 数据工厂](data-factory-build-your-first-pipeline-using-vs.md) | 在本教程中，您将创建示例 Azure 数据工厂管线使用 Visual Studio。 |
| 26 | [教程︰ 创建管线与使用 Azure 门户复制活动](data-factory-copy-activity-tutorial-using-azure-portal.md) | 在本教程中，您 Azure 数据工厂管道与复制活动通过数据工厂编辑器中创建 Azure 的门户。 |
| 27 | [教程︰ 使用复制活动使用 Azure PowerShell 创建管线](data-factory-copy-activity-tutorial-using-powershell.md) | 在本教程中，您创建 Azure 数据工厂管道与复制活动使用 Azure PowerShell。 |
| 28 | [教程︰ 创建管线与使用 Visual Studio 的复制活动](data-factory-copy-activity-tutorial-using-visual-studio.md) | 在本教程中，您创建 Azure 数据工厂管道与复制活动通过使用 Visual Studio。 |
| 29 | [Blob 存储中的数据复制到 SQL 数据库使用数据工厂](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | 本教程展示如何在 Azure 数据工厂管道中使用复制活动 Blob 存储中的数据复制到 SQL 数据库。 |
| 30 | [教程︰ 创建管线与使用数据工厂复制向导复制活动](data-factory-copy-data-wizard-tutorial.md) | 在本教程中，您创建 Azure 数据工厂管道与复制活动使用复制向导支持的数据工厂 |



## <a name="data-movement"></a>数据移动

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 31 | [使用 Azure 数据工厂的 Azure 斑点之间转移数据](data-factory-azure-blob-connector.md) | 了解如何将 blob 数据复制在 Azure 数据工厂。 使用我们的示例︰ 如何 Azure Blob 存储和 SQL Azure 数据库之间复制数据。 |
| 32 | [Azure 数据湖存储区使用 Azure 数据工厂之间转移数据](data-factory-azure-datalake-connector.md) | 了解如何将数据移到从 Azure 数据湖存储区使用 Azure 数据工厂 |
| 33 | [DocumentDB 使用 Azure 数据工厂之间转移数据](data-factory-azure-documentdb-connector.md) | 了解如何将数据移动到或从 Azure DocumentDB 集合并使用 Azure 数据工厂 |
| 34 | [使用 Azure 数据工厂的 Azure SQL 数据库之间转移数据](data-factory-azure-sql-connector.md) | 了解如何移动到从 Azure 使用 Azure 数据工厂的 SQL 数据库的数据。 |
| 35 | [SQL Azure 的数据仓库使用 Azure 数据工厂之间转移数据](data-factory-azure-sql-data-warehouse-connector.md) | 了解如何将数据移到从 SQL Azure 的数据仓库使用 Azure 数据工厂 |
| 36 | [Azure 表使用 Azure 数据工厂之间转移数据](data-factory-azure-table-connector.md) | 了解如何移动到中使用 Azure 数据工厂的 Azure 表存储的数据。 |
| 37 | [复制活动性能和优化指南](data-factory-copy-activity-performance.md) | 了解当您使用复制活动影响在 Azure 数据工厂中移动数据的性能的关键因素。 |
| 38 | [通过使用复制活动移动数据](data-factory-data-movement-activities.md) | 了解如何在数据工厂管道中移动数据︰ 云存储之间和内部存储和云存储之间的数据迁移。 使用复制活动。 |
| 39 | [发行说明，了解数据管理网关](data-factory-gateway-release-notes.md) | 数据管理网关文题目发行说明 |
| 40 | [将数据从使用 Azure 数据工厂内部 HDFS 移动](data-factory-hdfs-connector.md) | 了解如何将数据从使用 Azure 数据工厂内部 HDFS 移动。 |
| 41 | [监视和管理新的监视和管理应用程序使用 Azure 数据工厂管线](data-factory-monitor-manage-app.md) | 了解如何使用监视和管理应用程序来监视和管理 Azure 数据工厂和管道。 |
| 42 | [内部来源和数据管理网关与云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md) | 设置数据网关内部和云之间移动数据。 使用在 Azure 数据工厂数据管理网关移动数据。 |
| 43 | [移动数据从 OData 源使用 Azure 数据工厂](data-factory-odata-connector.md) | 了解如何将数据从使用 Azure 数据工厂的 OData 源移动。 |
| 44 | [移动数据从 ODBC 数据存储使用 Azure 数据工厂](data-factory-odbc-connector.md) | 了解如何将数据从 ODBC 数据存储使用 Azure 数据工厂移动。 |
| 45 | [将数据从 DB2 使用 Azure 数据工厂](data-factory-onprem-db2-connector.md) | 了解如何将数据从使用 Azure 数据工厂的 DB2 数据库移动 |
| 46 | [通过使用 Azure 数据工厂和本地文件系统中移动数据](data-factory-onprem-file-system-connector.md) | 了解如何通过使用 Azure 数据工厂内部文件系统之间来回移动数据。 |
| 47 | [移动数据从 MySQL 使用 Azure 数据工厂](data-factory-onprem-mysql-connector.md) | 了解如何将数据从使用 Azure 数据工厂的 MySQL 数据库移动。 |
| 48 | [将数据移到从内部 Oracle 使用 Azure 数据工厂](data-factory-onprem-oracle-connector.md) | 了解如何移动 Oracle 数据库即为来自内部使用 Azure 数据工厂。 |
| 49 | [将数据从 PostgreSQL 使用 Azure 数据工厂](data-factory-onprem-postgresql-connector.md) | 了解如何将数据从使用 Azure 数据工厂 PostgreSQL 数据库移动。 |
| 50 | [将数据从 Sybase 使用 Azure 数据工厂](data-factory-onprem-sybase-connector.md) | 了解如何将数据从使用 Azure 数据工厂的 Sybase 数据库移动。 |
| 51 | [将数据从 Teradata 使用 Azure 数据工厂](data-factory-onprem-teradata-connector.md) | 允许您将数据从 Teradata 数据库移动数据工厂服务了解 Teradata 连接器 |
| 52 | [移动数据并从 SQL Server 部署或 IaaS （Azure 虚拟机） 上使用 Azure 数据工厂](data-factory-sqlserver-connector.md) | 了解如何移动到从 SQL Server 数据库的内部部署或使用 Azure 数据工厂 Azure VM 中的数据。 |
| 53 | [将数据从 Web 表源使用 Azure 数据工厂](data-factory-web-table-connector.md) | 了解如何将数据从内部移动表格在网页上使用 Azure 数据工厂。 |



## <a name="data-transformation"></a>数据转换

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 54 | [创建预测管道使用 Azure 机器学习活动](data-factory-azure-ml-batch-execution-activity.md) | 描述如何创建创建预测管道使用 Azure 数据工厂和 Azure 机器学习 |
| 55 | [计算链接的服务](data-factory-compute-linked-services.md) | 了解有关计算 enviornments，您可以使用在 Azure 数据工厂管线转换/处理数据。 |
| 56 | [使用数据工厂和批次的处理大规模数据集](data-factory-data-processing-using-batch.md) | 描述如何通过使用 Azure 批次的并行处理能力处理大量 Azure 数据工厂管道中的数据。 |
| 57 | [转换数据在 Azure 数据工厂](data-factory-data-transformation-activities.md) | 了解如何转换数据或使用 Hadoop，机器学习或 Azure 数据湖分析 Azure 数据工厂中的流程数据。 |
| 58 | [Hadoop 流活动](data-factory-hadoop-streaming-activity.md) | 了解如何使用 Hadoop 流活动在 Azure 数据工厂上请求/您自己的 HDInsight 群集上运行 Hadoop 流程序。 |
| 59 | [配置单元活动](data-factory-hive-activity.md) | 了解如何使用该配置单元活动在 Azure 数据工厂上请求/您自己的 HDInsight 群集上运行配置单元查询。 |
| 60 | [调用数据工厂的 MapReduce 节目](data-factory-map-reduce.md) | 了解如何通过从 Azure 数据工厂 Azure HDInsight 群集上运行 MapReduce 程序来处理数据。 |
| 61 | [猪的活动](data-factory-pig-activity.md) | 了解如何使用猪的活动在 Azure 数据工厂上请求/您自己的 HDInsight 群集上运行 Pig 脚本。 |
| 62 | [调用数据工厂从触发程序](data-factory-spark.md) | 了解如何调用在 Azure 数据工厂使用 MapReduce 活动触发程序。 |
| 63 | [SQL Server 存储过程活动](data-factory-stored-proc-activity.md) | 了解如何使用 SQL Server 存储过程活动来调用数据工厂管线从存储的过程的 SQL Azure 数据库或 Azure SQL 数据仓库中。 |
| 64 | [在 Azure 数据工厂管道中使用自定义活动](data-factory-use-custom-activities.md) | 了解如何创建自定义活动并在 Azure 数据工厂管线中使用它们。 |
| 65 | [从 Azure 数据工厂运行 Azure 数据湖分析 U SQL 脚本](data-factory-usql-activity.md) | 了解如何通过在 Azure 数据湖分析计算服务上运行 U SQL 脚本中处理数据。 |



## <a name="samples"></a>示例

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 66 | [Azure 数据工厂-示例](data-factory-samples.md) | 在 Azure 数据工厂服务提供有关装运的示例的详细信息。 |



## <a name="use-cases"></a>使用案例

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 67 | [客户案例研究](data-factory-customer-case-studies.md) | 了解如何我们的一些客户已经使用 Azure 数据工厂。 |
| 68 | [使用案例 — 客户分析](data-factory-customer-profiling-usecase.md) | 了解如何使用 Azure 数据工厂创建数据驱动的工作流 （管道） 来分析游戏的客户。 |
| 69 | [使用案例 — 产品建议](data-factory-product-reco-usecase.md) | 了解有关使用 Azure 数据工厂以及其他服务实现的用例。 |



## <a name="monitor-and-manage"></a>监视和管理

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 70 | [监视和管理 Azure 数据工厂管线](data-factory-monitor-manage-pipelines.md) | 了解如何使用 Azure 门户和 Azure PowerShell 来监视和管理 Azure 数据工厂和已创建的管线。 |



## <a name="sdk"></a>SDK

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 71 | [Azure 数据工厂-.NET API 更改日志](data-factory-api-change-log.md) | 介绍重大更改、 新增功能、 缺陷修补程序等等...在 Azure 数据工厂的.NET API 的特定版本。 |
| 72 | [创建、 监视和管理使用数据工厂.NET SDK 的 Azure 数据工厂](data-factory-create-data-factories-programmatically.md) | 了解如何以编程方式创建、 监视和管理 Azure 数据工厂使用数据工厂 SDK。 |
| 73 | [Azure 数据工厂开发人员参考](data-factory-sdks.md) | 了解不同的方法来创建、 监视和管理 Azure 数据工厂 |



## <a name="miscellaneous"></a>杂项

| &nbsp; | 标题 | 说明 |
| --: | :-- | :-- |
| 74 | [Azure 数据工厂-常见问题](data-factory-faq.md) | Azure 数据工厂有关的常见问题。 |
| 75 | [Azure 数据工厂-函数和系统变量](data-factory-functions-variables.md) | 提供 Azure 数据工厂函数和系统变量的列表 |
| 76 | [Azure 数据工厂的命名规则](data-factory-naming-rules.md) | 描述数据工厂实体的命名规则。 |
| 77 | [解决数据工厂的问题](data-factory-troubleshoot.md) | 了解如何解决使用 Azure 数据工厂的问题。 |

