<properties 
    pageTitle="教程︰ 创建使用 Visual Studio 的复制活动与管道 |Microsoft Azure" 
    description="在本教程中，您创建 Azure 数据工厂管道与复制活动通过使用 Visual Studio。" 
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
    ms.topic="get-started-article" 
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>教程︰ 创建管线与使用 Visual Studio 的复制活动
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [复制向导](data-factory-copy-data-wizard-tutorial.md)
- [Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure 的资源管理器模板](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API，](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


本教程演示如何创建并监控使用 Visual Studio Azure 数据工厂。 数据工厂中的管道使用复制活动 Azure Blob 存储 SQL Azure 数据库之间复制数据。

以下是本教程的一部分执行的步骤︰

1. 创建两个链接的服务︰ **AzureStorageLinkedService1**和**AzureSqlinkedService1**。 

    AzureStorageLinkedService1 链接 Azure 存储和 AzureSqlLinkedService1 链接到数据工厂的 SQL Azure 数据库︰ **ADFTutorialDataFactoryVS**。 管道的输入的数据驻留在 Azure blob 存储 blob 容器中，输出数据存储在 SQL Azure 数据库中的表。 因此，您将添加下列两个数据存储为链接服务到数据工厂。
2. 创建两个数据集︰ **InputDataset**和**OutputDataset**，这表示数据存储区中存储的输入/输出数据。 

    对于 InputDataset，您可以指定包含源数据的 blob 的 blob 容器。 对于 OutputDataset，您可以指定存储输出数据的 SQL 表。 您还可以指定其他属性，如结构、 可用性和策略。
3. 创建名为**ADFTutorialPipeline**的 ADFTutorialDataFactoryVS 中的管道。 

    管道已将输入数据从 Azure 斑点到输出 SQL Azure 表**复制活动**。 复制活动在 Azure 数据工厂执行数据移动。 该活动是由全局可用的服务，可将各种数据存储安全、 可靠和可扩展的方式之间的数据复制的供电。 请参阅[数据移动活动](data-factory-data-movement-activities.md)文章复制活动有关的详细信息。 
4. 创建名为**VSTutorialFactory**的数据工厂。 部署数据工厂和所有数据工厂实体 （链接的服务、 表和管道）。    

## <a name="prerequisites"></a>系统必备组件

1. [教程概述](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)文章阅读，完成**先决条件**步骤。 
2. 您必须是**Azure 订阅的管理员**能够将数据工厂实体发布到 Azure 数据工厂。  
3. 您必须在您的计算机上安装下列程序︰ 
    - Visual Studio 2013年或 Visual Studio 2015
    - 下载为 Visual Studio 2013年或 Visual Studio 2015 Azure SDK。 导航到[Azure 下载页](https://azure.microsoft.com/downloads/)，然后单击**VS 2013**或**VS 2015**的**.NET**中。
    - Visual Studio 为下载最新的 Azure 数据工厂插件︰ [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5)或[VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005)。 此外可以通过执行以下步骤来更新插件︰ 单击**工具**菜单上 -> **扩展和更新** -> **在线** -> **Visual Studio 库** -> **Microsoft Azure 数据工厂工具 Visual Studio** -> **更新**。

## <a name="create-visual-studio-project"></a>创建 Visual Studio 项目 
1. 启动**Visual Studio 2013年**。 单击**文件**，指向**新建**，单击**项目**。 您应该看到**新建项目**对话框。  
2. 在**新建项目**对话框中，选择**DataFactory**模板，并单击**空数据工厂项目**。 如果您看不到 DataFactory 模板，关闭 Visual Studio 安装 Visual Studio 2013，Azure SDK，重新打开 Visual Studio。  

    ![新建项目对话框](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. 为项目、**位置**和**解决方案**名称输入一个**名称**，并单击**确定**。

    ![解决方案资源管理器](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>创建链接的服务
链接的服务链接的数据存储区或计算到 Azure 数据工厂服务。 请参阅[支持的数据存储](data-factory-data-movement-activities.md##supported-data-stores-and-formats)，以便所有源和接收器支持的复制活动。 [计算链接的服务](data-factory-compute-linked-services.md)数据工厂支持的计算服务的列表，请参阅。 在本教程中，您不使用任何计算服务。 

在此步骤中，您将创建两个链接的服务︰ **AzureStorageLinkedService1**和**AzureSqlLinkedService1**。 AzureStorageLinkedService1 链接服务链接 Azure 存储帐户和 AzureSqlLinkedService 链接到数据工厂的 SQL Azure 数据库︰ **ADFTutorialDataFactory**。 

### <a name="create-the-azure-storage-linked-service"></a>创建链接的 Azure 存储服务

4. 在解决方案资源管理器中右击**链接服务**，指向**添加**，并单击**新的项目**。      
5. 在**添加新项**对话框中，从列表中选择**Azure 存储链接服务**并单击**添加**。 

    ![新链接的服务](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. 更换`<accountname>`和`<accountkey>`* 使用 Azure 存储帐户和它的键的名称。 

    ![Azure 存储链接服务](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. 将**AzureStorageLinkedService1.json**文件保存。

> 有关 JSON 属性的详细信息，请参阅[将与 Azure Blob 数据移动](data-factory-azure-blob-connector.md#azure-storage-linked-service)。

### <a name="create-the-azure-sql-linked-service"></a>创建链接的 Azure SQL 服务

5. 再次右击**解决方案资源管理器**中的**链接服务**节点上，指向**添加**，并单击**新项**。 
6. 这一次，选择**Azure SQL 链接服务**，然后单击**添加**。 
7. 在**AzureSqlLinkedService1.json 文件**中，替换`<servername>`， `<databasename>`， `<username@servername>`，和`<password>`具有名称的 SQL Azure 服务器、 数据库、 用户帐户和密码。    
8.  将**AzureSqlLinkedService1.json**文件保存。 

> [AZURE.NOTE]
> 有关 JSON 属性的详细信息，请参阅[移动与 SQL Azure 数据库的数据](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)。

## <a name="create-datasets"></a>创建数据集
在前面的步骤中，您创建了链接的服务**AzureStorageLinkedService1**和**AzureSqlLinkedService1**来链接到数据工厂的 Azure 存储帐户和 SQL Azure 数据库︰ **ADFTutorialDataFactory**。 在此步骤中，您可以定义两个数据集 （ **InputDataset**和**OutputDataset** ） 表示通过 AzureStorageLinkedService1 和 AzureSqlLinkedService1 分别引用数据存储区中存储的输入/输出数据。 对于 InputDataset，您可以指定包含源数据的 blob 的 blob 容器。 对于 OutputDataset，您可以指定存储输出数据的 SQL 表。

### <a name="create-input-dataset"></a>创建输入数据集
在此步骤中，您将创建名为**InputDataset**指向 blob 容器由**AzureStorageLinkedService1**链接服务 Azure 存储中的数据集。 表是一个矩形的数据集，数据集现在支持的唯一类型。 

9. 右击**解决方案资源管理器**中的**表**，指向**添加**，并单击**新的项目**。
10. 在**添加新项**对话框中，选择**Azure 斑点**，并单击**添加**。   
10. JSON 文本替换为下面的文本并保存的**AzureBlobLocation1.json**文件。 

        {
          "name": "InputDataset",
          "properties": {
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
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     请注意以下几点︰ 
    
    - 数据集**类型**设置为**AzureBlob**。
    - **linkedServiceName**设置为**AzureStorageLinkedService**。 您在步骤 2 中创建此链接的服务。
    - **采用文件夹路径**设置为**adftutorial**容器。 您还可以指定一个 blob 中使用的**文件名**属性的文件夹的名称。 由于并不指定的 blob 名称，从该容器中的所有 blob 数据被认为是作为输入的数据。  
    - 格式**类型**设置**格式**为
    - 在用逗号字符 (**columnDelimiter**) 分隔的文本文件 –**名字字段**和**姓氏**--有两个字段 
    - **可用性**设置为**每小时**（**频率**设置为**小时**和**间隔**设置为**1**）。 因此，数据工厂寻找输入数据每隔一小时的 blob 容器 (**adftutorial**) 所指定的根文件夹中。 
    
    如果没有指定**文件名**用于**输入**数据集，从输入的文件夹 (**采用文件夹路径**) 的所有文件/blob 被都视为输入。 如果在 JSON 中指定一个文件名，只指定的文件/blob 被认为是 asn 输入。
 
    如果不指定**文件名**的**输出表**，**采用文件夹路径**中生成的文件被命名为以下格式︰ 数据。&lt;Guid\&gt;。txt (示例︰ Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt。)。

    要设置**采用文件夹路径**和**文件名** **SliceStart**时间动态变化，请使用**partitionedBy**属性。 在以下示例中，采用文件夹路径使用年份、 月份和日期的 SliceStart （正在处理切片的开始时间） 和文件名使用来自 SliceStart 小时。 例如，如果一块被生成为 2016年-09-20T08:00:00，文件夹名称设置为 wikidatagateway/wikisampledataout/2016年/09/20 和文件名设置为 08.csv。 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [AZURE.NOTE]
> 有关 JSON 属性的详细信息，请参阅[将与 Azure Blob 数据移动](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)。

### <a name="create-output-dataset"></a>创建输出数据集
在此步骤中，您将创建一个名为**OutputDataset**的输出数据集。 此数据集指向由**AzureSqlLinkedService1**表示 SQL Azure 数据库中的 SQL 表。 

11. 再次右击**解决方案资源管理器**中的**表**，指向**添加**，单击**新建项目**。
12. 在**添加新项**对话框中，选择**SQL Azure**中，并单击**添加**。 
13. JSON 文本替换为下面的 JSON 并保存的**AzureSqlTableLocation1.json**文件。

        {
          "name": "OutputDataset",
          "properties": {
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
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     请注意以下几点︰ 
    
    - 数据集**类型**设置为**AzureSQLTable**。
    - **linkedServiceName**设置为**AzureSqlLinkedService** （您将创建在步骤 2 中此链接的服务）。
    - **表名**设置为**emp**。
    - 有三个列 – **ID**、**名字**和**姓氏**– emp 表在数据库中。 ID 是标识列，因此您需要在此处指定只有**名字字段**和**姓氏**。
    - **可用性**设置为**每小时**（设置为**小时**的**频率**和**时间间隔**设置为**1**）。  数据工厂服务生成 SQL Azure 数据库中**emp**表中输出数据切片每隔一小时。

> [AZURE.NOTE]
> 有关 JSON 属性的详细信息，请参阅[移动与 SQL Azure 数据库的数据](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)。

## <a name="create-pipeline"></a>创建管道 
到目前为止已经创建输入/输出链接的服务和表。 现在，管线创建与要复制的**复制活动**从 Azure 数据 blob 到 SQL Azure 数据库。 


1. **管线**在**解决方案资源管理器**中用鼠标右键单击，指向**添加**，单击**新项**。  
15. 在**添加新项**对话框中选择**复制数据管道**并单击**添加**。 
16. 替换为下面的 JSON 的 JSON 并保存的**CopyActivity1.json**文件。
            
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

    请注意以下几点︰

    - 在活动部分是其**类型**设置为**复制**只能有一个活动。
    - 活动的输入设置为**InputDataset** ，输出为活动设置为**OutputDataset**。
    - **TypeProperties**部分中， **BlobSource**指定为源类型并将**SqlSink**指定为接收器类型。

    **启动**属性的值替换为当前的日期和**最终**值与第二天。 您可以指定只的日期部分并跳过日期时间的时间部分。 例如，"2016年-02-03"，相当于"2016年-02-03T00:00:00Z"
    
    同时开始和结束 datetimes 必须为[ISO 格式](http://en.wikipedia.org/wiki/ISO_8601)。 例如︰ 2016年-10-14T16:32:41Z。 **结束**时间是可选的但我们在本教程中使用它。 
    
    如果不指定**end**属性的值，它将计算为"**开始 + 48 小时**"。 要无限期地运行管道，请指定**9999-09-09** **end**属性的值。
    
    在前面的示例中，有 24 数据扇区每小时产生每个数据片。

## <a name="publishdeploy-data-factory-entities"></a>发布/部署数据工厂实体
在此步骤中，您发布数据工厂实体 （链接的服务、 数据集和管道） 之前创建。 您还可以指定新数据工厂创建存放这些实体的名称。  

18. 在解决方案资源管理器中，右击项目并单击**发布**。 
19. 如果查看**Microsoft 客户在登录**对话框中，输入您的凭据具有 Azure 的订阅的帐户，然后单击**登录**。
20. 您应看到下面的对话框︰

    ![发布对话框](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. 在配置数据的工厂页中，请执行以下步骤︰ 
    1. 选择**创建新的数据工厂**。
    2. 对于**名称**中输入**VSTutorialFactory** 。  
    
        > [AZURE.IMPORTANT]  
        > Azure 数据工厂的名称必须是全局唯一的。 如果在发布时，您会收到有关数据工厂的名称错误，，更改数据工厂 (例如，yournameVSTutorialFactory)，请尝试重新发布的名称。 请参阅[数据工厂的命名规则](data-factory-naming-rules.md)命名规则对数据工厂项目的主题。     
    3.选择 Azure 订阅**订阅**字段。
     
        > [AZURE.IMPORTANT]如果看不到任何订阅，请确保您登录使用的帐户是管理员或 co-管理预订。  
    4.选择数据工厂创建的**资源组**。 5。 选择**区域**的数据工厂。 仅数据工厂服务支持的地区所示的下拉列表。
6。 单击**下一步**要切换到**发布项**页。
    
        ![配置数据工厂页](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. 在**发布项目**页中，确保所有数据工厂，实体被选中，然后单击**下一步**要切换到**摘要**页。
    
    ![发布项目页](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. 检查摘要并单击**下一步**启动部署过程并查看**部署状态**。

    ![发布摘要页](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. 在**部署状态**页中，您应该看到部署过程的状态。 部署完成后，请单击完成。 
    ![部署状态页，](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) ，请注意以下几点︰ 

- 如果您收到错误消息:"**此订阅未注册使用 Microsoft.DataFactory 命名空间**"，执行下列操作之一并再次尝试发布︰ 

    - 在 Azure PowerShell，运行以下命令以注册数据工厂提供程序。 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        您可以运行以下命令，以确认数据工厂注册提供程序。 
    
            Get-AzureRmResourceProvider
    - 使用 Azure 订阅到[Azure 门户](https://portal.azure.com)登录并导航到数据工厂刀片式服务器 （或） 在 Azure 的门户网站中创建的数据工厂。 此操作会自动注册为您的提供程序。
-   可作为将来因而变得公开可见的 DNS 名称注册数据工厂的名称。

> [AZURE.IMPORTANT] 若要创建数据工厂实例，您需要是管理员/co-管理员的 Azure 的订阅

## <a name="summary"></a>摘要
在本教程中，您创建了 Azure 数据工厂要复制到 SQL Azure 数据库斑点从 Azure 数据。 使用 Visual Studio 创建数据工厂，链接的服务、 数据集和管线。 下面是您在本教程中执行的高级步骤︰  

1.  创建 Azure**数据工厂**。
2.  创建**链接的服务**︰
    1. 要保存输入的数据将 Azure 存储帐户链接**Azure 存储**链接服务。    
    2. 若要链接 SQL Azure 数据库保存的输出数据链接**SQL Azure**服务。 
3.  创建**数据集**的描述输入的数据和输出数据管道。
4.  作为源和接收器作为**SqlSink** **BlobSource** **复制活动**创建**管线**。 


## <a name="use-server-explorer-to-view-data-factories"></a>使用服务器资源管理器来查看数据工厂

1. 在**Visual Studio**中，单击**视图**菜单上，单击**服务器资源管理器**。
2. 在服务器资源管理器窗口中，展开**Azure** ，展开**数据工厂**。 如果看到**登录到 Visual Studio**，输入**帐户**与 Azure 订阅，然后单击**继续**。 输入**密码**，然后单击**登录**。 Visual Studio 将尝试获取有关您的订阅中的所有 Azure 数据工厂信息。 您看到此**数据工厂任务列表**窗口中的操作的状态。
    ![服务器资源管理器](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. 您可以的数据工厂，右键单击，并选择导出到新的项目来创建基于现有的数据工厂 Visual Studio 项目的数据工厂。
    ![VS 项目导出数据工厂](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Visual Studio 为更新数据工厂工具
更新 Visual Studio Azure 数据工厂工具，请执行以下步骤︰

1. 单击**工具**菜单上，选择**扩展和更新**。 
2. 在左窗格中选择**更新程序**，然后选择**Visual Studio 库**。
4. 选择**Visual Studio 的 Azure 数据工厂工具**，然后单击**更新**。 如果看不到此条目，则已经有最新版本的工具。 

您在本教程中创建的说明如何使用 Azure 的门户来监视管道和数据集，请参阅[监视数据集和管道](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline)。

## <a name="see-also"></a>请参见
| 主题 | 说明 |
| :---- | :---- |
| [数据移动活动](data-factory-data-movement-activities.md) | 本文提供了您在本教程中使用复制活动的详细的信息。 |
| [安排和执行](data-factory-scheduling-and-execution.md) | 本文介绍了 Azure 数据工厂应用程序模型的调度和执行方面。 |
| [管道](data-factory-create-pipelines.md) | 这篇文章可以帮助您理解管线和 Azure 数据工厂中的活动 |
| [数据集](data-factory-create-datasets.md) | 这篇文章将帮助您了解在 Azure 数据工厂的数据集。
| [监视和管理管道使用监视应用程序](data-factory-monitor-manage-app.md) | 本文介绍了如何监视、 管理和调试使用监视和管理应用程序的管道。 
