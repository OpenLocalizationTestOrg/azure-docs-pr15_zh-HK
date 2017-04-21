<properties
    pageTitle="构建您的第一个数据工厂 (Visual Studio) |Microsoft Azure"
    description="在本教程中，您将创建示例 Azure 数据工厂管线使用 Visual Studio。"
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
    ms.topic="hero-article" 
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>使用 Microsoft Visual Studio 教程︰ 生成第一个 Azure 数据工厂
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-build-your-first-pipeline.md)
- [Azure 门户](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [资源管理器模板](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API，](data-factory-build-your-first-pipeline-using-rest-api.md)

在本文中，您可以使用 Microsoft Visual Studio 创建您的第一个 Azure 数据工厂。

## <a name="prerequisites"></a>系统必备组件
1. [教程概述](data-factory-build-your-first-pipeline.md)文章阅读，完成**先决条件**步骤。
2. **Azure 订阅的管理员**必须能够发布数据工厂实体从 Visual Studio 到 Azure 数据工厂。
3. 您必须在您的计算机上安装下列程序︰ 
    - Visual Studio 2013年或 Visual Studio 2015
    - 下载为 Visual Studio 2013年或 Visual Studio 2015 Azure SDK。 导航到[Azure 下载页](https://azure.microsoft.com/downloads/)，然后单击**VS 2013**或**VS 2015**的**.NET**中。
    - Visual Studio 为下载最新的 Azure 数据工厂插件︰ [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5)或[VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005)。 您还可以通过执行以下更新插件︰ 在菜单上，单击**工具** -> **扩展和更新** -> **在线** -> **Visual Studio 库** -> **Microsoft Azure 数据工厂工具 Visual Studio** -> **更新**。 
 
现在，让我们使用 Visual Studio 创建 Azure 数据工厂。 


## <a name="create-visual-studio-project"></a>创建 Visual Studio 项目 
1. 启动**Visual Studio 2013年**或**Visual Studio 2015年**。 单击**文件**，指向**新建**，单击**项目**。 您应该看到**新建项目**对话框。  
2. 在**新建项目**对话框中，选择**DataFactory**模板，并单击**空数据工厂项目**。   

    ![新建项目对话框](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. 为项目、**位置**和**解决方案**名称输入一个**名称**，并单击**确定**。

    ![解决方案资源管理器](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>创建链接的服务
数据工厂可以有一个或多个管道。 管道可以有一个或多个活动的。 例如，复制活动，以将数据从源复制到目标数据存储区和一个 HDInsight 配置单元活动运行配置单元脚本来转换输入的数据。 请参阅[支持的数据存储](data-factory-data-movement-activities.md##supported-data-stores-and-formats)，以便所有源和接收器支持的复制活动。 [计算链接的服务](data-factory-compute-linked-services.md)数据工厂支持的计算服务的列表，请参阅。 

在此步骤中，您将链接 Azure 存储帐户，并按需 Azure HDInsight 群集到数据工厂。 Azure 存储帐户在此示例中包含管道的输入和输出数据。 HDInsight 链接服务用于运行配置单元中管道在此示例中的活动指定的脚本。 确定哪些数据存储/计算服务在您的方案中使用，并通过创建链接的服务将这些服务链接到数据工厂。  

您指定的名称和数据工厂设置稍后发布数据工厂解决方案时。

#### <a name="create-azure-storage-linked-service"></a>创建链接的 Azure 存储服务
在此步骤中，您将 Azure 存储帐户链接到数据工厂。 对于本教程，您可以使用相同的 Azure 存储帐户存储输入/输出数据和 HQL 脚本文件。 

4. 在解决方案资源管理器中右击**链接服务**，指向**添加**，并单击**新的项目**。      
5. 在**添加新项**对话框中，从列表中选择**Azure 存储链接服务**并单击**添加**。 
3. **帐户名**和**accountkey**替换为 Azure 存储帐户和它的键的名称。 若要了解如何获取您的存储访问密钥，请参阅[查看、 复制和重新生成存储访问键](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure 存储链接服务](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. 将**AzureStorageLinkedService1.json**文件保存。

#### <a name="create-azure-hdinsight-linked-service"></a>创建链接的 Azure HDInsight 服务
在此步骤中，您将按需 HDInsight 群集链接到数据工厂。 HDInsight 群集自动在运行时创建并删除之后的指定时间内完成处理和空闲。 您可以使用 HDInsight 群集，而不是使用按需 HDInsight 群集。 有关详细信息，请参阅[计算链接服务](data-factory-compute-linked-services.md)。 

1. 在**解决方案资源管理器**中用鼠标右键单击**链接服务**，指向**添加**，单击**新项**。
2. 选择**HDInsight 上请求链接服务**，然后单击**添加**。 
3. **JSON**替换以下内容︰

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    下表提供了使用代码段中的 JSON 属性的说明︰
    
    属性 | 说明
    -------- | -----------
    版本 | 指定创建的版本 HDInsight 为 3.2。 
    ClusterSize | 指定 HDInsight 群集的大小。 
    TimeToLive | 指定 HDInsight 群集的空闲时间之前将其删除。
    linkedServiceName | 指定用于存储由 HDInsight 生成的日志的存储帐户

    请注意以下事项︰ 
    
    - 数据工厂创建与前面的 JSON 为您**的基于 Windows 的**HDInsight 群集。 您也可以让它创建一个**基于 Linux 的**HDInsight 群集。 有关详细信息，请参阅[按需 HDInsight 链接服务](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 
    - 您可以使用**HDInsight 群集**，而不是使用按需 HDInsight 群集。 有关详细信息，请参阅[HDInsight 链接的服务](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)。
    - HDInsight 群集指定 json 格式 (**linkedServiceName**) 的 blob 存储区中创建一个**默认容器**。 删除群集时，HDInsight 不会删除该容器。 这种现象是设计使然。 与按需 HDInsight 链接服务，在每次处理一个切片时，除非现有活动群集 (**timeToLive**) 创建 HDInsight 群集。 在进行处理时，会自动删除群集。
    
        处理多个切片，您会看到在 Azure blob 存储许多容器。 如果排除故障的作业不需要它们，可能要删除它们以降低存储成本。 这些容器的名称遵循某一模式:"adf**yourdatafactoryname**-**linkedservicename**datetimestamp"。 使用[Microsoft 存储浏览器](http://storageexplorer.com/)这样的工具删除在 Azure blob 存储容器。

    有关详细信息，请参阅[按需 HDInsight 链接服务](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 
4. 将**HDInsightOnDemandLinkedService1.json**文件保存。

## <a name="create-datasets"></a>创建数据集
在此步骤中，您将创建数据集来表示输入和输出配置单元处理的数据。 这些数据集是指先前在本教程中创建的**AzureStorageLinkedService1** 。 到 Azure 存储帐户和数据集的链接的服务点指定容器、 文件夹、 文件名存储中包含输入和输出数据。   

#### <a name="create-input-dataset"></a>创建输入数据集

1. 在**解决方案资源管理器**中右击**表**，指向**添加**，单击**新项**。 
2. 从列表中选择**Azure Blob** 、 更改的文件的名称为**InputDataSet.json**，并单击**添加**。
3. 用以下内容替换**JSON**在编辑器中︰ 

    在 JSON 段中，您将创建数据集称为**AzureBlobInput**表示输入的数据管道中的活动。 此外，指定输入的数据位于名为**adfgetstarted**的 blob 容器和名为**inputdata**的文件夹
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    下表提供了使用代码段中的 JSON 属性的说明︰

  	| 属性 | 说明 |
  	| :------- | :---------- |
  	| 类型 | 类型属性设置为 AzureBlob 因为数据驻留在 Azure blob 存储。 |  
  	| linkedServiceName | 是指您在前面创建的 AzureStorageLinkedService1。 |
  	| 文件名 | 此属性是可选的。 如果省略此属性，将选取采用文件夹路径中的所有文件。 在这种情况下，仅 input.log 进行处理。 |
  	| 类型 | 日志文件是以文本格式，因此我们使用格式。 | 
  	| columnDelimiter | 由逗号 （，） 分隔的日志文件中的列 |
  	| 频率/间隔 | 设置为月和间隔的频率为 1，这意味着，每月一次输入的切片是用。 | 
  	| 外部 | 此属性设置为 true，如果输入的数据不由数据工厂服务生成。 | 
      
    
3. 将**InputDataset.json**文件保存。 

 
#### <a name="create-output-dataset"></a>创建输出数据集
现在，您可以创建输出数据集来表示 Azure Blob 存储中存储的输出数据。 

1. 在**解决方案资源管理器**中右击**表**，指向**添加**，单击**新项**。 
2. 从列表中选择**Azure Blob** 、 更改的文件的名称为**OutputDataset.json**，并单击**添加**。 
3. 用以下内容替换**JSON**在编辑器中︰ 

    在 JSON 段中，您将创建数据集称为**AzureBlobOutput**，并指定该配置单元脚本生成的数据的结构。 此外，您可以指定结果存储在名为**adfgetstarted**的 blob 容器和名为**partitioneddata**的文件夹中。 **可用性**部分指定输出数据集，在每月的基础上产生。
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    请参阅**创建输入数据集**一节有关这些属性的说明。 您未设置外部属性输出数据集与数据集由数据工厂服务。

4. 将**OutputDataset.json**文件保存。


### <a name="create-pipeline"></a>创建管道
在此步骤中，您将创建第一个管道与**HDInsightHive**活动。 输入的扇区是每月可用 (频率︰ 月、 间隔︰ 1)、 每月生成输出切片和活动计划程序属性也设置为每月一次。 输出数据集和活动时间表的设置必须匹配。 目前，输出数据集是驱使计划，因此即使该活动不会产生任何输出，您必须创建一个输出数据集。 如果活动并不需要输入任何内容，则可以跳过创建输入数据集。 这一节的末尾介绍以下 JSON 中使用的属性。

1. 在**解决方案资源管理器**中用鼠标右键单击**管线**，指向**添加**，然后单击**新项目。** 
2. 从列表中，选择**配置单元转换管道**并单击**添加**。 
3. 下面的代码段替换**JSON** 。

    > [AZURE.IMPORTANT] **storageaccountname**替换为您的存储帐户的名称。

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    在 JSON 段中，您将创建管线的使用流程数据 HDInsight 群集上配置单元的单个活动组成。
    
    在 JSON 段中，您将创建管线的使用流程数据 HDInsight 群集上配置单元的单个活动组成。
    
    在 Azure 存储帐户 （由 scriptLinkedService，称为**AzureStorageLinkedService1**），并在容器**adfgetstarted****脚本**文件夹中存储的配置单元脚本文件**partitionweblogs.hql**。

    **定义**部分用于指定传递给配置单元脚本以配置单元配置值的运行时设置 (例如 ${hiveconf: inputtable}，{hiveconf:partitionedtable} $)。

    管道的**start**和**end**属性指定管道的有效期间。

    在 JSON 的活动中，可以指定配置单元脚本运行指定**linkedServiceName** — **HDInsightOnDemandLinkedService**的计算上。

    > [AZURE.NOTE] 有关示例中使用 JSON 属性的详细信息，请参阅[管线的剖析](data-factory-create-pipelines.md#anatomy-of-a-pipeline)。 
3. 将**HiveActivity1.json**文件保存。

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>添加为依赖项的 partitionweblogs.hql 和 input.log 

1. 用鼠标右键单击**解决方案资源管理器**窗口中的**依赖项**，指向**添加**，单击**现有项目**。  
2. 定位到**C:\ADFGettingStarted** ，并选择**partitionweblogs.hql**， **input.log**文件，然后单击**添加**。 从[教程概述](data-factory-build-your-first-pipeline.md)创建这两个文件作为系统必备组件的一部分。

下一步在发布解决方案时， **partitionweblogs.hql**文件上载到脚本文件夹中的**adfgetstarted** blob 容器。   

### <a name="publishdeploy-data-factory-entities"></a>发布/部署数据工厂实体

18. 在解决方案资源管理器中，右击项目并单击**发布**。 
19. 如果查看**Microsoft 客户在登录**对话框中，输入您的凭据具有 Azure 的订阅的帐户，然后单击**登录**。
20. 您应看到下面的对话框︰

    ![发布对话框](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. 在配置数据的工厂页中，执行下列操作︰ 
    1. 选择**创建新的数据工厂**。
    2. 数据工厂输入一个唯一的**名称**。 例如︰ **FirstDataFactoryUsingVS09152016**。 该名称必须是全局唯一的。  
    
    
        > [AZURE.IMPORTANT] 如果在发布时，您会收到错误**数据工厂名称"FirstDataFactoryUsingVS"是不可用**，则更改名称 (例如，yournameFirstDataFactoryUsingVS)。 请参阅[数据工厂的命名规则](data-factory-naming-rules.md)命名规则对数据工厂项目的主题。
3。 选择**订阅**字段的右侧订阅。
     
     
        > [AZURE.IMPORTANT] 如果看不到任何订阅，请确保您登录使用的帐户是管理员或 co-管理预订。  
        
    4. 选择数据工厂创建的**资源组**。 
    5. 选择**区域**的数据工厂。 
    6. 单击**下一步**要切换到**发布项**页。 （按**选项卡**移出到名称字段如果**下一步**按钮将被禁用）。 
23. 在**发布项目**页中，确保所有数据工厂，实体被选中，然后单击**下一步**要切换到**摘要**页。     
24. 检查摘要并单击**下一步**启动部署过程并查看**部署状态**。
25. 在**部署状态**页中，您应该看到部署过程的状态。 部署完成后，请单击完成。 

 
需要注意的要点︰ 

- 如果您收到错误消息:"**此订阅未注册使用 Microsoft.DataFactory 命名空间**"，执行下列操作之一并再次尝试发布︰ 

    - 在 Azure PowerShell，运行以下命令以注册数据工厂提供程序。 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        您可以运行以下命令，以确认数据工厂注册提供程序。 
    
            Get-AzureRmResourceProvider
    - 使用 Azure 的订购到[Azure 门户](https://portal.azure.com)登录并导航到数据工厂刀片式服务器 （或） 在 Azure 的门户网站中创建的数据工厂。 此操作会自动注册为您的提供程序。
-   可作为将来因而变得公开可见的 DNS 名称注册数据工厂的名称。
-   若要创建数据工厂实例，您需要是管理员或 co-管理 Azure 订阅的

 
## <a name="monitor-pipeline"></a>监视器管线

### <a name="monitor-pipeline-using-diagram-view"></a>监视器使用关系图视图的管道
6. 登录到[Azure 的门户网站](https://portal.azure.com/)，请执行下列操作︰
    1. 单击**更多的服务**，然后单击**数据工厂**。
        ![浏览数据工厂](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. 选择数据工厂的名称 (例如︰ **FirstDataFactoryUsingVS09152016**) 中的数据工厂。 
        ![选择数据工厂](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. 在数据工厂主页页中，单击**图表**。
  
    ![平铺关系图](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. 在关系图视图中，可以看到概述的管线，以及在本教程中使用的数据集。
    
    ![关系图视图](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. 若要查看在管道中的所有活动，用鼠标右键单击图表中的管道并单击打开管道。 

    ![打开管道菜单](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. 确认您看到在管道中的 HDInsightHive 活动。 
  
    ![打开管线视图](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    若要导航回前一视图，请在顶部的痕迹导航菜单中单击**数据工厂**。 
10. 在**关系图视图**中，双击数据集**AzureBlobInput**。 确认该切片处于**就绪**状态。 它可能需要几分钟的切片显示在就绪状态。 如果它不会发生后一段时间等待，，查看是否有放在适当容器 (adfgetstarted) 和文件夹 (inputdata) 的输入的文件 (input.log)。

    ![处于就绪状态的输入的切片](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. 单击**X**关闭**AzureBlobInput**刀片式服务器。 
12. 在**关系图视图**中，双击数据集**AzureBlobOutput**。 您将看到当前正在处理的扇区。

    ![数据集](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. 当处理完成后时，您会看到**就绪**状态中的扇区。

    > [AZURE.IMPORTANT] 创建一个按需 HDInsight 群集通常需要一段时间 （大约 20 分钟）。 因此，预计要花**大约 30 分钟**来处理切片的管线。  

    ![数据集](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. 当切片处于**就绪**状态时，检查**adfgetstarted**的输出数据的 blob 存储在容器中的**partitioneddata**文件夹。  
 
    ![输出数据](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. 单击切片**数据切片**刀片中有关它的详细信息，请参阅。

    ![数据切片详细信息](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. 单击运行活动**活动运行列表**中要查看详细信息**活动运行详细信息**窗口中运行 （配置单元活动在我们的方案） 的活动。   
    ![活动运行的详细信息](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    从日志文件中，可以查看已执行的配置单元查询和状态信息。 这些日志可用于解决任何问题。  
 

您在本教程中创建的说明如何使用 Azure 的门户来监视管道和数据集，请参阅[监视数据集和管道](data-factory-monitor-manage-pipelines.md)。

### <a name="monitor-pipeline-using-monitor--manage-app"></a>监视使用监视和管理应用程序的管道
您还可以使用监视器和管理应用程序来监视您的管线。 有关使用此应用程序的详细信息，请参阅[显示器和管理 Azure 数据工厂管线使用监视和管理应用程序](data-factory-monitor-manage-app.md)。

1. 单击监视器和管理平铺。

    ![监控和管理平铺](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. 您应查看监视和管理应用程序。 更改的**开始时间**和**结束时间**来匹配开始 (04-01-2016 12:00 AM) 时间和结束时间 (04-02-2016 上午 12:00) 的管道，并单击**应用**。

    ![监视和管理应用程序](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. 活动窗口列表中选择活动窗口以查看有关它的详细信息。 
    ![活动窗口的详细信息](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] 成功处理切片后，输入的文件被删除。 因此，如果您要重新运行该切片或再次执行该教程，将输入的文件 (input.log) 上载到 adfgetstarted 容器的 inputdata 文件夹中。
 

## <a name="use-server-explorer-to-view-data-factories"></a>使用服务器资源管理器来查看数据工厂

1. 在**Visual Studio**中，单击**视图**菜单上，单击**服务器资源管理器**。
2. 在服务器资源管理器窗口中，展开**Azure** ，展开**数据工厂**。 如果看到**登录到 Visual Studio**，输入**帐户**与 Azure 订阅，然后单击**继续**。 输入**密码**，然后单击**登录**。 Visual Studio 将尝试获取有关您的订阅中的所有 Azure 数据工厂信息。 您看到此**数据工厂任务列表**窗口中的操作的状态。

    ![服务器资源管理器](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. 您可以用鼠标右键单击数据工厂，并选择**到新项目中导出数据工厂**创建 Visual Studio 项目根据现有的数据工厂。

    ![导出数据工厂](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Visual Studio 为更新数据工厂工具

若要更新有关 Visual Studio Azure 数据工厂工具，执行下列操作︰

1. 单击**工具**菜单上，选择**扩展和更新**。
2. 在左窗格中选择**更新程序**，然后选择**Visual Studio 库**。
3. 选择**Visual Studio 的 Azure 数据工厂工具**，然后单击**更新**。 如果看不到此条目，则已经有最新版本的工具。 

## <a name="use-configuration-files"></a>使用配置文件
可以使用 Visual Studio 中的配置文件来配置链接服务/表/管线以不同的方式为每个环境的属性。 

请考虑下面的 JSON 定义 Azure 存储链接服务。 若要指定**连接字符串**值不同的帐户名和 accountkey 根据数据工厂实体部署到的环境 （测试/开发/生产）。 可以通过为每个环境中使用单独的配置文件来实现此行为。 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>添加配置文件
添加为每个环境配置文件，请执行以下步骤︰   

1. 右击 Visual Studio 解决方案中的数据工厂项目，指向**添加**，并单击**新项**。
2. 从左侧的已安装模板的列表中选择**配置**、 选择**配置文件**，配置文件中，输入一个**名称**，单击**添加**。

    ![添加配置文件](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. 以下面的格式添加配置参数及其值。

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    本示例配置链接的 Azure 存储服务和链接的 Azure SQL 服务的连接字符串属性。 请注意，指定名称的语法[JsonPath](http://goessner.net/articles/JsonPath/)。   

    如果 JSON 具有一个属性具有数组的值，如下面的代码所示︰  

        "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
        ],
    
    配置属性，如下面的配置文件 （使用从零开始编制索引） 中所示︰ 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>带有空格的属性名称
如果属性名称中有空格，则使用方括号 （数据库服务器名称） 下面的示例所示︰ 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>部署解决方案使用配置
当您要发布 Azure 数据工厂实体 VS 中时，可以指定您想要使用该发布操作的配置。 

若要发布 Azure 数据工厂项目使用配置文件中的实体︰   

1. 右键单击数据工厂项目，然后单击**发布**以查看**发布项目**对话框。 
2. 选择现有的数据工厂或在**配置数据工厂**页面上，创建一个数据工厂为指定的值并单击**下一步**。   
3. 在**发布项**页上︰ 请参见**选择部署配置**字段中的可用配置下拉列表。

    ![选择配置文件](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. 选择您想要使用，然后单击**下一步**的**配置文件**。 
5. 确认您查看**摘要**页中的 JSON 文件的名称，并单击**下一步**。 
6. 在部署操作完成后，请单击**完成**。 

在部署时，该配置文件中的值用于再到 Azure 数据工厂服务部署实体数据工厂实体的 JSON 文件中设置属性的值。   

## <a name="summary"></a>摘要 
在本教程中，您创建流程数据到 Azure 数据工厂通过 HDInsight hadoop 群集上运行脚本的配置单元。 在 Azure 门户中使用数据工厂编辑器，请执行以下步骤︰  

1.  创建 Azure**数据工厂**。
2.  创建两个**链接的服务**︰
    1.  若要将链接保存到数据工厂的输入/输出文件您 Azure 的 blob 存储的**azure 存储**链接服务。
    2.  **Azure HDInsight**拨号链接的服务链接到数据工厂的点播 HDInsight Hadoop 群集。 Azure 数据工厂创建 HDInsight Hadoop 群集中实时处理输入的数据并产生输出数据。 
3.  创建两个**数据集**，其中介绍了在管道中的 HDInsight 配置单元活动的输入和输出数据。 
4.  使用**HDInsight 配置单元**活动创建的**管道**。  


## <a name="next-steps"></a>下一步行动
在本文中，您创建管线与按需 HDInsight 群集运行的配置单元脚本转换活动 （HDInsight 活动）。 若要查看如何使用将数据复制到 Azure SQL Azure Blob 复制活动，请参阅[教程︰ Azure 的数据复制到 SQL Azure 的 blob](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。
  
## <a name="see-also"></a>请参见
| 主题 | 说明 |
| :---- | :---- |
| [数据转换活动](data-factory-data-transformation-activities.md) | 这篇文章列出了数据转换活动 （如您在本教程中使用的 HDInsight 配置单元转换） 的 Azure 数据工厂支持。 | 
| [安排和执行](data-factory-scheduling-and-execution.md) | 本文介绍了 Azure 数据工厂应用程序模型的调度和执行方面。 |
| [管道](data-factory-create-pipelines.md) | 这篇文章将帮助您了解管线和 Azure 数据工厂，以及如何使用它们来构建端到端方案或业务数据驱动的工作流中的活动。 |
| [数据集](data-factory-create-datasets.md) | 这篇文章将帮助您了解在 Azure 数据工厂的数据集。
| [监视和管理管道使用监视应用程序](data-factory-monitor-manage-app.md) | 本文介绍了如何监视、 管理和调试使用监视和管理应用程序的管道。 
