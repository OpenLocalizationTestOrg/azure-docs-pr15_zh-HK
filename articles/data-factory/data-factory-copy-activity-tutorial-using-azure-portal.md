<properties 
    pageTitle="教程︰ 创建使用 Azure 门户复制活动与管道 |Microsoft Azure" 
    description="在本教程中，您 Azure 数据工厂管道与复制活动通过数据工厂编辑器中创建 Azure 的门户。" 
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
    ms.date="09/16/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>教程︰ 创建管线与使用 Azure 门户复制活动
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [复制向导](data-factory-copy-data-wizard-tutorial.md)
- [Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure 的资源管理器模板](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API，](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)



本教程演示如何创建并监控使用 Azure 门户 Azure 数据工厂。 数据工厂中的管道使用复制活动 Azure Blob 存储 SQL Azure 数据库之间复制数据。

以下是本教程的一部分执行的步骤︰

步骤 | 说明
-----| -----------
[创建 Azure 数据工厂](#create-data-factory) | 在此步骤中，您将创建名为**ADFTutorialDataFactory**Azure 数据工厂。  
[创建链接的服务](#create-linked-services) | 在此步骤中，您将创建两个链接的服务︰ **AzureStorageLinkedService**和**AzureSqlLinkedService**。 <br/><br/>AzureStorageLinkedService 链接 Azure 存储和 AzureSqlLinkedService ADFTutorialDataFactory 的链接 SQL Azure 数据库。 输入的数据的管道位于 Azure blob 存储和输出数据中的 blob 容器存储在 SQL Azure 数据库中的表。 因此，您将添加下列两个数据存储为链接服务到数据工厂。      
[创建输入和输出数据集](#create-datasets) | 在前面的步骤中，您创建了指向包含输入/输出数据的数据存储的链接的服务。 在此步骤中，您可以定义两个数据集 （ **InputDataset**和**OutputDataset** ） 表示数据存储区中存储的输入/输出数据。 <br/><br/>InputDataset，指定包含 blob 与源数据和 OutputDataset 的 blob 容器，指定的 SQL 表中存储输出的数据。 您还可以指定其他属性，如结构、 可用性和策略。 
[创建管线](#create-pipeline) | 在此步骤中，您可以创建名为**ADFTutorialPipeline**的 ADFTutorialDataFactory 管道。 <br/><br/>**复制活动**添加到输出 SQL Azure 表斑点将输入数据从 Azure 的管道。 复制活动在 Azure 数据工厂执行数据移动。 它被采用全局可用的服务，可将各种数据存储安全、 可靠和可扩展的方式之间的数据复制。 请参阅[数据移动活动](data-factory-data-movement-activities.md)文章复制活动有关的详细信息。 
[监视器管线](#monitor-pipeline) | 在此步骤中，您使用 Azure 门户监控片的输入和输出表。

## <a name="prerequisites"></a>系统必备组件 
然后再执行本教程[教程概述](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)文章中列出了完整的先决条件。

## <a name="create-data-factory"></a>创建数据工厂
在此步骤中，您可以使用 Azure 门户创建名为**ADFTutorialDataFactory**Azure 数据工厂。

1.  之后登录到[Azure 的门户网站](https://portal.azure.com/)，单击**新建**，选择**智能 + 分析**，然后单击**数据工厂**。 

    ![新-> DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)  

6. 在**新数据工厂**刀片式服务器︰
    1. **名称**中输入**ADFTutorialDataFactory** 。 
    
        ![新数据工厂刀片式服务器](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)

        Azure 数据工厂的名称必须是**全局唯一**的。 如果您收到下面的错误，更改数据工厂 (例如，yournameADFTutorialDataFactory) 的名称，并尝试重新创建。 请参阅[数据工厂的命名规则](data-factory-naming-rules.md)命名规则对数据工厂项目的主题。
    
            Data factory name “ADFTutorialDataFactory” is not available  
     
        ![数据工厂名称不可用](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
    2. 选择 Azure**订阅**。
    3. 资源组，请执行以下步骤之一︰
        1. 选择**使用现有**，然后从下拉列表中选择现有资源组。 
        2. 选择**新建**，输入资源组的名称。   
    
            部分在本教程中的步骤假定您使用的名称︰ **ADFTutorialResourceGroup**资源组。 若要了解有关资源组，请参阅[使用资源组管理 Azure 资源](../azure-resource-manager/resource-group-overview.md)。  
    4. 选择数据工厂的**位置**。 仅数据工厂服务支持的地区所示的下拉列表。
    5. 选择**锁定到 Startboard**。     
    6. 单击**创建**。

        > [AZURE.IMPORTANT] 若要创建数据工厂实例，您必须订阅/资源分组级别的[数据工厂参与者](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor)角色的成员。
        >  
        >  可作为将来因而变得公开可见的 DNS 名称注册数据工厂的名称。              
9.  若要查看状态/通知邮件，单击工具栏上的贝尔图标。 

    ![通知消息](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
10. 创建过程完成后，您会看到**数据工厂**刀片式服务器，如图所示。

    ![数据工厂主页](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>创建链接的服务
链接的服务链接的数据存储区或计算到 Azure 数据工厂服务。 请参阅[支持的数据存储](data-factory-data-movement-activities.md##supported-data-stores-and-formats)，以便所有源和接收器支持的复制活动。 [计算链接的服务](data-factory-compute-linked-services.md)数据工厂支持的计算服务的列表，请参阅。 在本教程中，您不使用任何计算服务。 

在此步骤中，您将创建两个链接的服务︰ **AzureStorageLinkedService**和**AzureSqlLinkedService**。 AzureStorageLinkedService 链接服务链接 Azure 存储帐户和 AzureSqlLinkedService **ADFTutorialDataFactory**的链接 SQL Azure 数据库。 稍后在本教程中，将数据复制到 AzureSqlLinkedService 中的 SQL 表 AzureStorageLinkedService 中的 blob 容器创建管线。

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>创建链接的 Azure 存储帐户服务
1.  在**数据工厂**刀片式服务器，请单击**作者和部署**图块以启动**编辑器**的数据工厂。

    ![编写和部署图块](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
5. 在**编辑器**中，单击**新的数据存储**工具栏上的按钮，从下拉菜单中选择**Azure 存储**。 您应该看到在右边的窗格中创建链接的 Azure 存储服务的 JSON 模板。 

    ![编辑器中新建数据存储区按钮](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
6. 更换`<accountname>`和`<accountkey>`的帐户名称和 Azure 存储帐户的帐户密钥值。 

    ![编辑器 Blob 存储 JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png) 
6. 在工具栏上，单击**部署**。 现在，您应该看到在树视图已部署的**AzureStorageLinkedService** 。 

    ![编辑器 Blob 存储部署](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [AZURE.NOTE]
> 有关 JSON 属性的详细信息，请参阅[将与 Azure Blob 数据移动](data-factory-azure-blob-connector.md#azure-storage-linked-service)。

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>创建 SQL Azure 数据库链接的服务
1. 在**数据工厂编辑器**中，单击**新的数据存储**工具栏上的按钮，从下拉菜单中选择**SQL Azure 数据库**。 您应该看到在右边的窗格中创建链接的 Azure SQL 服务的 JSON 模板。
2. 更换`<servername>`， `<databasename>`， `<username>@<servername>`，和`<password>`具有名称的 SQL Azure 服务器、 数据库、 用户帐户和密码。 
3. 创建和部署**AzureSqlLinkedService**工具栏上，单击**部署**。
4. 请确认您在树视图中看到**AzureSqlLinkedService** 。 

> [AZURE.NOTE]
> 有关 JSON 属性的详细信息，请参阅[移动与 SQL Azure 数据库的数据](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)。

## <a name="create-datasets"></a>创建数据集
在前面的步骤中，您创建了链接的服务**AzureStorageLinkedService**和**AzureSqlLinkedService**来链接到数据工厂的 Azure 存储帐户和 SQL Azure 数据库︰ **ADFTutorialDataFactory**。 在此步骤中，您可以定义两个数据集 （ **InputDataset**和**OutputDataset** ） 表示通过 AzureStorageLinkedService 和 AzureSqlLinkedService 分别引用数据存储区中存储的输入/输出数据。 InputDataset，指定包含 blob 与源数据和 OutputDataset 的 blob 容器，指定的 SQL 表中存储输出的数据。 

### <a name="create-input-dataset"></a>创建输入数据集 
在此步骤中，您将创建名为**InputDataset**指向 blob 容器由**AzureStorageLinkedService**链接服务 Azure 存储中的数据集。

1. 数据工厂**编辑器**中单击**命令。.多个**，单击**新的数据集**，然后从下拉菜单中单击**Azure Blob 存储**。 

    ![新的数据集菜单](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. 在右窗格中 JSON 替换下面的 JSON 段︰ 

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
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
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
            ],
2. 工具栏来创建和部署**InputDataset**数据集上，单击**部署**。 确认您看到**InputDataset**在树视图中。

> [AZURE.NOTE]
> 有关 JSON 属性的详细信息，请参阅[将与 Azure Blob 数据移动](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)。

### <a name="create-output-dataset"></a>创建输出数据集
在此部分中的步骤，您可以创建一个名为**OutputDataset**的输出数据集。 此数据集指向由**AzureSqlLinkedService**表示 SQL Azure 数据库中的 SQL 表。 

1. 数据工厂**编辑器**中单击**命令。.多个**，单击**新的数据集**，并从下拉菜单中单击**SQL Azure** 。 
2. 在右窗格中 JSON 替换下面的 JSON 段︰

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
            "linkedServiceName": "AzureSqlLinkedService",
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

3. 工具栏来创建和部署**OutputDataset**数据集上，单击**部署**。 确认您看到**OutputDataset**在树视图中。 

> [AZURE.NOTE]
> 有关 JSON 属性的详细信息，请参阅[移动与 SQL Azure 数据库的数据](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)。

## <a name="create-pipeline"></a>创建管道
在此步骤中，您创建管线与一个**复制活动**，将**InputDataset**用作输入和输出为**OutputDataset** 。

1. 数据工厂**编辑器**中单击**命令。.多个**，然后单击**新管道**。 或者，您可以在树视图中右击**管线**并单击**新管道**。
2. 在右窗格中 JSON 替换下面的 JSON 段︰ 
        
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
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
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
    
4. 创建和部署**ADFTutorialPipeline**工具栏上，单击**部署**。 确认您看到在树视图中的管道。 
5. 现在，单击**X**关闭**编辑器**刀片式服务器。单击**X**再次以**ADFTutorialDataFactory**，请参阅**数据工厂**主页。

**祝贺您 ！** 已成功创建 Azure 数据工厂、 链接的服务、 表和管道并安排管道。   
 
### <a name="view-the-data-factory-in-a-diagram-view"></a>在关系图视图中查看数据工厂 
1. 在**数据工厂**刀片式服务器，单击**图表**。

    ![数据工厂刀片的平铺关系图](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. 您应该看到类似于下图的图表︰ 

    ![关系图视图](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)

    您可以放大、 缩小、 放大到 100%，以适应、 自动定位管道和表，并显示沿袭信息 （突出显示选定项目的上游和下游项目） 的显示比例。  您可以双击某个对象 （输入/输出表或管道） 来查看它的属性。 
3. **ADFTutorialPipeline**在关系图视图中用鼠标右键单击，然后单击**打开管道**。 

    ![开放的管道](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. 您应该看到活动的输入和输出数据集以及管道中的活动。 在本教程中，InputDataset 管线 （复制活动） 以及输入数据集作为输出数据集 OutputDataset 中都有只有一个活动。   

    ![打开管线视图](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. 单击痕迹导航中的左上角，以返回到关系图视图中的**数据工厂**。 关系图视图显示所有管线。 在此示例中，您只能创建一条管线。   
 

## <a name="monitor-pipeline"></a>监视器管线
在此步骤中，您可以使用 Azure 门户来监视在 Azure 数据工厂中正在运行的内容。 

### <a name="monitor-pipeline-using-diagram-view"></a>监视器使用关系图视图的管道

1. 单击**X**关闭**图**视图以查看数据工厂数据工厂主页。 如果您关闭 web 浏览器，请执行以下步骤︰ 
    2. 导航到[Azure 的门户](https://portal.azure.com/)。 
    2. 双击**ADFTutorialDataFactory** **Startboard**上 （或） 在左边的菜单中，单击**数据工厂**，ADFTutorialDataFactory 搜索。 
3. 您会看到计数和表和您在此刀片式服务器创建的管道的名称。

    ![包含名称的主页](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
4. 现在，单击**数据集**拼贴。
5. 在**数据集**刀片式服务器，请单击**InputDataset**。 此数据集是**ADFTutorialPipeline**的输入数据集。

    ![与 InputDataset 所选的数据集](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. 单击**按钮。.（省略号）**若要查看所有数据切片。

    ![所有输入数据切片](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  

    请注意，因为**emp.txt**文件 blob 容器中存在的所有时间截至当前时间的所有数据切片已都准备**就绪**︰ **adftutorial\input**。 确认，没有切片显示在底部**最近失败的切片**部分。

    **最近更新片**和**最近失败扇区**列表将按**上一次更新时间**。 
    
    过滤片工具栏上，单击**筛选**。  
    
    ![筛选器输入的切片](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. 关闭刀片式服务器，直到您看到**的数据集**刀片式服务器。 单击**OutputDataset**。 此数据集是**ADFTutorialPipeline**的输出数据集。

    ![数据集刀片式服务器](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
6. 您应该看到**OutputDataset**刀片式服务器，如下图所示︰

    ![表刀片式服务器](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
7. **已生成的数据切片截至当前时间的通知，他们准备好。** 没有切片显示在底部的**问题片**部分中。
8. 单击**按钮。.（省略号）**若要查看所有扇区。

    ![数据切片刀片式服务器](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
9. 单击列表中的任何数据切片，您应该会看到**数据切片**刀片式服务器。

    ![数据切片刀片式服务器](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
  
    如果切片未处于**就绪**状态，您可以看到的上游片时，还没有准备好，并阻碍当前片从**上游都没有准备好的切片**列表中执行。
11. 在**数据切片**刀片式服务器，您应该看到在底部列表中运行的所有活动。 单击**运行活动**刀片式服务器**活动运行的详细信息**，请参阅。 

    ![活动运行的详细信息](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. 单击**X**关闭所有刀片式服务器，直到您回到家乡刀片式服务器的**ADFTutorialDataFactory**。
14. （可选）单击主页上的**ADFTutorialDataFactory****管线**、**管线**刀片式服务器，请单击**ADFTutorialPipeline**和钻取输入的表 （**进行**） 或输出表 （**生产**）。
15. 启动**SQL Server 管理 Studio**、 连接到 Azure 的 SQL 数据库，并验证到**emp**表在数据库中插入行。

    ![sql 查询结果](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor--manage-app"></a>监视使用监视和管理应用程序的管道
您还可以使用监视器和管理应用程序来监视您的管线。 有关使用此应用程序的详细信息，请参阅[显示器和管理 Azure 数据工厂管线使用监视和管理应用程序](data-factory-monitor-manage-app.md)。

1. 单击**监视和管理**数据工厂的主页上的平铺。

    ![监控和管理平铺](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. 您应该看到**监视和管理应用程序**。 更改**开始时间**和**结束时间**以包括 (2016年-07-12) 开始和结束时间 (2016年-07-13) 的管道，并单击**应用**。 

    ![监视和管理应用程序](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. 活动窗口列表中选择**活动窗口**以查看有关它的详细信息。 
    ![活动窗口的详细信息](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>摘要 
在本教程中，您创建了 Azure 数据工厂要复制到 SQL Azure 数据库斑点从 Azure 数据。 Azure 门户用于创建数据工厂，链接的服务、 数据集和管线。 下面是您在本教程中执行的高级步骤︰  

1.  创建 Azure**数据工厂**。
2.  创建**链接的服务**︰
    1. 要保存输入的数据将 Azure 存储帐户链接**Azure 存储**链接服务。    
    2. 若要链接 SQL Azure 数据库保存的输出数据链接**SQL Azure**服务。 
3.  创建**数据集**描述输入的数据和输出数据管道。
4.  作为源和接收器作为**SqlSink** **BlobSource** **复制活动**创建**管线**。  


## <a name="see-also"></a>请参见
| 主题 | 说明 |
| :---- | :---- |
| [数据移动活动](data-factory-data-movement-activities.md) | 本文提供了您在本教程中使用复制活动的详细的信息。 |
| [安排和执行](data-factory-scheduling-and-execution.md) | 本文介绍了 Azure 数据工厂应用程序模型的调度和执行方面。 |
| [管道](data-factory-create-pipelines.md) | 这篇文章将帮助您了解管线和 Azure 数据工厂中的活动。 |
| [数据集](data-factory-create-datasets.md) | 这篇文章将帮助您了解在 Azure 数据工厂的数据集。
| [监视和管理管道使用监视应用程序](data-factory-monitor-manage-app.md) | 本文介绍了如何监视、 管理和调试使用监视和管理应用程序的管道。 


