<properties     
    pageTitle="Azure 数据工厂-示例" 
    description="在 Azure 数据工厂服务提供有关装运的示例的详细信息。" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure 数据工厂-示例

## <a name="samples-on-github"></a>在 GitHub 上的示例
[GitHub Azure DataFactory 存储库](https://github.com/azure/azure-datafactory)包含一些示例，帮助您快速量产 Azure 数据工厂服务与 （或） 修改脚本，并在自己的应用程序中使用它。 Samples\JSON 文件夹包含 JSON 段为常见方案。

| 示例 | 说明 |
| :----- | :---------- | 
| [ADF 演练](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | 此示例提供的端到端演练来处理日志文件使用 Azure 数据工厂可以从日志文件中的数据求助的见解。 <br/><br/>在本演练中，数据工厂管道收集的示例日志，流程和丰富与引用数据日志数据和转换的数据，以评估最近启动的市场营销活动的有效性。 |
| [JSON 示例](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | 此示例提供示例 JSON 的常见方案。 | 
| [Http 数据下载程序示例](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | 此示例展示了下载的数据从 HTTP 端点到 Azure Blob 存储使用.NET 的自定义活动。 |
| [跨应用程序域点净活动示例](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | 此示例允许您创作到 ADF 启动程序 （例如，WindowsAzure.Storage v4.3.0，Newtonsoft.Json v6.0.x 等） 所使用的程序集版本不受约束的自定义.NET 活动。 |
| [运行 R 脚本](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  本示例包含可用于调用 RScript.exe 数据工厂自定义活动。 此示例仅适用于已 R 安装在它自己 （不按需） HDInsight 群集。 |
| [调用 HDInsight Hadoop 群集上的触发作业](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | 此示例演示如何使用 MapReduce 活动来调用触发程序。 触发程序只需将数据从一个 Azure Blob 容器复制到另一个。 |
| [使用 twitter 使用 Azure 机器学习批评分活动分析](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | 此示例演示如何使用 AzureMLBatchScoringActivity 来调用执行 twitter 观点分析，评分，预测等 Azure 机器学习模型。 |
| [使用自定义活动的 twitter 分析](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  此示例演示如何使用自定义的.NET 活动来调用执行 twitter 观点分析，评分，预测等 Azure 机器学习模型。 |
| [参数化的管道 Azure 的机器学习](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | 该示例提供用于部署评分和重新培训每个都有不同的区域参数区域列表来源一个 parameters.txt 文件，其中包含此示例的位置的 N 管道端到端 C# 代码。 | 
| [引用数据刷新的 Azure 流分析作业](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  此示例演示如何同时使用 Azure 数据工厂和 Azure 流分析可以使用引用数据运行查询并设置为引用数据，按计划刷新。 |
| [与内部 Hortonworks Hadoop 混合管道](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | 该示例使用一个内部的 Hadoop 群集作为计算目标就像 HDInsight 基于云的 Hadoop 群集，则可以添加其他计算目标数据工厂中运行作业。 |
| [JSON 转换工具](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | 此工具允许您将 JSONs 转换在 2015年-07-01-预览最新或 2015年-07-01-预览 （默认值） 之前的版本中。 |  
| [U SQL 示例输入的文件](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  此文件是 U SQL 活动使用的示例文件。 | 

## <a name="azure-resource-manager-templates"></a>Azure 的资源管理器模板
可以为数据工厂在 Github 上找到以下 Azure 资源管理器模板。 

| 模板 | 说明 |
| -------- | ----------- | 
| [从 Azure Blob 存储复制到 SQL Azure 数据库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | 部署此模板创建管线将从指定的 Azure blob 存储的数据复制到 SQL Azure 数据库 Azure 数据工厂 |    
| [从队伍到 Azure Blob 存储复制](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | 部署此模板创建管线将从指定的销售帐户的数据复制到 Azure blob 存储 Azure 数据工厂。 |    
| [通过 Azure HDInsight 群集上运行配置单元脚本转换数据](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | 部署此模板创建使用 Azure HDInsight Hadoop 群集上运行的示例配置单元脚本转换数据管道的 Azure 数据工厂。 | 


## <a name="samples-in-azure-portal"></a>在 Azure 门户中的示例
您可以使用数据工厂的主页上**样本管道**铺到数据工厂部署样本管道并在其关联的实体 （数据集和链接的服务）。 

1. 数据工厂创建或打开现有的数据工厂。 请参阅[使用 Azure 数据工厂入门](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory)步骤创建的数据工厂。
2. 在数据工厂**数据工厂**刀片式服务器，单击**样本管道**铺。

    ![示例的管线图块](./media/data-factory-samples/SamplePipelinesTile.png)

2. 在**样本管道**刀片式服务器，请单击要部署**示例**。 
    
    ![样品的管线刀片式服务器](./media/data-factory-samples/SampleTile.png)

3. 指定配置设置的示例。 例如，您 Azure 存储帐户用户名和帐户密码、 SQL Azure 服务器名称、 数据库、 用户 ID 和密码等。 

    ![刀片式服务器示例](./media/data-factory-samples/SampleBlade.png)

4. 使用指定的配置设置完之后，请单击**创建**创建/部署示例管道和管道使用的链接的服务/表。
5. 您可以单击**样本管道**刀片式服务器的前面的示例图块上看到部署状态。

    ![部署状态](./media/data-factory-samples/DeploymentStatus.png)

6. 当看到**部署已成功**消息示例图块上时，关闭**示例管道**刀片式服务器。  
5. **数据工厂**刀片式服务器，您可以看到链接的服务、 数据集和管线被添加到您的数据工厂。  

    ![数据工厂刀片式服务器](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>在 Visual Studio 中的示例

### <a name="prerequisites"></a>系统必备组件

您必须在您的计算机上安装下列程序︰ 

- Visual Studio 2013年或 Visual Studio 2015
- 下载为 Visual Studio 2013年或 Visual Studio 2015 Azure SDK。 导航到[Azure 下载页](https://azure.microsoft.com/downloads/)，然后单击**VS 2013**或**VS 2015**的**.NET**中。
- Visual Studio 为下载最新的 Azure 数据工厂插件︰ [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5)或[VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005)。 如果您使用的 Visual Studio 2013年，还可以通过执行以下步骤更新插件︰ 单击**工具**菜单上 -> **扩展和更新** -> **在线** -> **Visual Studio 库** -> **Microsoft Azure 数据工厂工具 Visual Studio** -> **更新**。

### <a name="use-data-factory-templates"></a>使用数据工厂模板

1. 单击**文件**菜单上，指向**新建**，单击**项目**。 
2. 在**新建项目**对话框中，请执行以下步骤︰ 
    1. 选择**模板**下的**DataFactory** 。 
    2. 在右窗格中选择**数据工厂模板**。 
    3. 为项目输入一个**名称**。 
    4. 选择项目**位置**。 
    5. 单击**确定**。 

    ![新建项目对话框](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. 在**数据工厂模板**对话框中，从**用例模板**部分中，选择示例模板，然后单击**下一步**。 以下步骤将指导您完成使用**客户分析**模板。 步骤是类似的其他示例。 

    ![数据工厂模板对话框](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. **数据工厂配置**对话框中，在**数据工厂基础**页上单击**下一步**。
8. 在**配置数据工厂**页上，请执行以下步骤︰ 
    1. 选择**创建新的数据工厂**。 您还可以选择**使用现有的数据工厂**。
    2. 数据工厂输入一个**名称**。
    3. 选择要在其中创建数据工厂**Azure 的订阅**。 
    4. 选择数据工厂的**资源组**。
    5. **地区**选择**美国西部**、**东部美国**或**北欧**。
    6. 单击**下一步**。 
9. 在**配置数据存储**页中，指定一个现有的**SQL Azure 数据库**和**Azure 存储帐户**（或） 创建数据库存储并单击下一步。 
10. 在**计算配置**页面中，选择默认值，并单击**下一步**。 
11. 在**摘要**页上，检查所有的设置，并单击**下一步**。 
12. 在**部署状态**页中，等待，直到完成部署，然后单击**完成**。
13. 在解决方案资源管理器中，右击项目并单击**发布**。 
19. 如果查看**Microsoft 客户在登录**对话框中，输入您的凭据具有 Azure 的订阅的帐户，然后单击**登录**。
20. 您应看到下面的对话框︰

    ![发布对话框](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. 在**配置数据工厂**页中，请执行以下步骤︰ 
    1. 确认**使用现有数据工厂**中的该选项。
    2. 选择使用该模板时，您必须选择**数据工厂**。 
    6. 单击**下一步**要切换到**发布项**页。 （按**选项卡**移出到名称字段如果**下一步**按钮将被禁用）。 
23. 在**发布项目**页中，确保所有数据工厂，实体被选中，然后单击**下一步**要切换到**摘要**页。     
24. 检查摘要并单击**下一步**启动部署过程并查看**部署状态**。
25. 在**部署状态**页中，您应该看到部署过程的状态。 部署完成后，请单击完成。 

有关使用 Visual Studio 创作数据工厂实体并将其发布到 Azure 的详细信息，请参阅[构建您的第一个数据工厂 (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) 。          