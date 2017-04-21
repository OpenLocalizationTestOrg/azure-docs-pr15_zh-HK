<properties 
    pageTitle="移动数据的数据管理网关 |Microsoft Azure"
    description="设置数据网关内部和云之间移动数据。 使用在 Azure 数据工厂数据管理网关移动数据。" 
    keywords="数据网关、 数据集成、 移动数据网关凭据"
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
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>内部来源和数据管理网关与云之间移动数据
本文概述了内部数据存储和使用数据工厂的云数据存储之间的数据集成。 它建立在[数据移动活动](data-factory-data-movement-activities.md)文章和其他数据工厂核心概念文章︰[数据集](data-factory-create-datasets.md)和[管道](data-factory-create-pipelines.md)。 

## <a name="data-management-gateway"></a>数据管理网关
在内部计算机以启用移动到从内部数据存储区的数据上，必须安装数据管理网关。 网关可以安装在数据存储区的同一台计算机上或另一台计算机上，只要该网关能够连接到数据存储区。 

> [AZURE.IMPORTANT] 请参阅[数据管理网关](data-factory-data-management-gateway.md)文章有关数据管理网关的详细信息。   

下面的演练演示如何使用将数据从本地**SQL Server**数据库移动到 Azure blob 存储管道创建的数据工厂。 作为演练的一部分，安装和配置您的计算机上的数据管理网关。 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>演练︰ 部署在将数据复制到云
  
## <a name="create-data-factory"></a>创建数据工厂
在此步骤中，您使用 Azure 门户创建名为**ADFTutorialOnPremDF**的 Azure 数据工厂实例。 

1.  登录到[Azure 的门户](https://portal.azure.com)。 
2.  单击**+ 新建**，单击**智能 + 分析**，单击**数据工厂**。

    ![新-> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. 在**新数据工厂**刀片式服务器，输入名称**ADFTutorialOnPremDF** 。

    ![将添加到 Startboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Azure 数据工厂的名称必须是全局唯一的。 如果您收到错误︰**数据工厂名称"ADFTutorialOnPremDF"是不可用**的更改数据工厂 (例如，yournameADFTutorialOnPremDF) 的名称，然后尝试重新创建。 在本教程中执行其余步骤时使用此名称来代替 ADFTutorialOnPremDF。
    > 
    > 可作为将来因而变得公开可见的**DNS**名称注册数据工厂的名称。
3. 选择**Azure 订阅**所需数据工厂创建的位置。 
4.  选择现有**资源组**或创建资源组。 本教程中，创建一个名为的资源组︰ **ADFTutorialResourceGroup**。 
5.  在**新数据工厂**刀片式服务器，请单击**创建**。

    > [AZURE.IMPORTANT] 若要创建数据工厂实例，您必须订阅/资源分组级别的[数据工厂参与者](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor)角色的成员。 
11. 创建过程完成后，您会看到**数据工厂**刀片式服务器，如下图所示︰

    ![数据工厂主页](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>创建网关
5. 在**数据工厂**刀片式服务器，请单击**作者和部署**图块以启动**编辑器**的数据工厂。

    ![编写和部署图块](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  在数据工厂编辑器中，单击**按钮。.多个**在工具栏上，然后单击**新的数据网关**。 或者，您可以在树视图中右击**数据网关**，然后单击**新数据网关**。 

    ![在工具栏上的新数据网关](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. 在**创建**刀片式服务器， **adftutorialgateway**输入该**名称**，并单击**确定**。    

    ![创建网关刀片式服务器](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. 在**配置**刀片式服务器，请单击**直接在此计算机上安装**。 此操作将网关安装程序包的下载、 安装、 配置和注册的计算机上的网关。  

    > [AZURE.NOTE] 
    > 使用 Internet Explorer 或 Microsoft ClickOnce 兼容的 web 浏览器。
    > 
    > 如果您使用的铬，请转到[Chrome web 存储区](https://chrome.google.com/webstore/)，用"ClickOnce"关键字搜索，请选择一个 ClickOnce 扩展中，并将其安装。 
    >  
    > 执行相同的操作 （安装加载项） 的 firefox。 单击**打开的菜单**工具栏上的按钮 （在右上角的**三条水平线**），单击**附件**、"ClickOnce"关键字搜索、 选择一个 ClickOnce 扩展中，并安装。    

    ![网关的配置刀片式服务器](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    这种方法是最简单的方法 （一次单击） 下载、 安装、 配置和注册网关在一个单步执行。 您可以看到您的计算机上安装**Microsoft 数据管理网关配置管理器**应用程序。 您还可以在文件夹中找到可执行文件**ConfigManager.exe** : **C:\Program 数值数据管理 Gateway\2.0\Shared**。

    此外可以下载和此刀片式服务器中使用的链接手动安装网关并注册它使用**新的密钥**文本框中显示的项。
    
    有关网关的所有详细信息请参阅[数据管理网关](data-factory-data-management-gateway.md)文章。

    >[AZURE.NOTE] 您必须是本地计算机上的管理员来安装和配置数据管理网关成功。 您可以将其他用户添加到**数据管理网关用户**本地 Windows 组。 此组的成员可以使用数据管理网关配置管理器工具配置网关。 

5. 等待几分钟或者等待，直到您看到以下消息︰

    ![网关安装成功](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. 启动您的计算机上的**数据管理网关配置管理器**应用程序。 在**搜索**窗口中，键入**数据管理网关**来实现此功能。 您还可以在文件夹中找到可执行文件**ConfigManager.exe** : **C:\Program 数值数据管理 Gateway\2.0\Shared** 

    ![网关配置管理器](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. 确认，您会看到`adftutorialgateway is connected to the cloud service`消息。 底部的状态栏显示**已连接到云服务**以及一个**绿色的复选标记**。

    在**主页**选项卡上，您还可以执行以下操作︰ 
    - **注册**密钥从 Azure 门户网站通过使用注册按钮的网关。 
    - **停止**数据管理网关主机服务网关计算机上运行。 
    - **计划的更新**安装在一天中的特定时间。 
    - 网关是**最后一次更新**时的视图。
    - 指定网关的更新可以安装的时间。 

8. 切换到**设置**选项卡。 在**证书**部分中指定的证书用于加密/解密内部数据存储在门户网站上指定的凭据。 （可选）单击**更改**以改用您自己的证书。 默认情况下，网关使用由数据工厂服务自动生成的证书。

    ![网关证书配置](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    您还可以执行以下操作的**设置**选项卡上︰ 
    - 查看或导出证书所使用的网关。
    - 更改 HTTPS 终结点使用的网关。    
    - 设置 HTTP 代理服务器使用的网关。   
9. （可选）切换到**诊断**选项卡，选中**启用详细日志记录**选项，如果您想要启用详细日志记录，可以用来解决与网关的任何问题。 在**应用程序**和服务日志的**事件查看器**中找不到日志记录信息 -> **数据管理网关**节点。 

    ![诊断标签](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    您还可以在**诊断**选项卡中执行以下操作︰ 
    
    - 内部数据源使用的网关使用**测试连接**部分。
    - 单击**查看日志**以查看事件查看器窗口中的数据管理网关日志。 
    - 单击**发送日志**将具有日志的最后一个七天的 zip 文件上载到 Microsoft 有助于进行故障排除的问题。 
10. 在**诊断**选项卡中的**测试连接**部分中，选择**SqlServer**的数据存储区的类型、 输入数据库服务器，数据库的名称的名称、 指定的身份验证类型、 输入用户名称和密码，和单击**测试**以测试是否可以连接到数据库的网关。 
11. 切换到 web 浏览器中，然后在**Azure 的门户网站**，**新的数据网关**刀片式服务器上**配置**刀片式服务器，然后单击**确定**。
6. 您应该看到在左边的树视图下**数据网关**为**adftutorialgateway** 。  如果单击此链接，您应该看到相关联的 JSON。 
    

## <a name="create-linked-services"></a>创建链接的服务 
在此步骤中，您将创建两个链接的服务︰ **AzureStorageLinkedService**和**SqlServerLinkedService**。 **SqlServerLinkedService**链接到数据工厂内部 SQL Server 数据库和 Azure blob 存储的**AzureStorageLinkedService**链接的服务链接。 从本地 SQL Server 数据库的数据复制到 Azure blob 存储本演练中稍后创建管线。 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>为内部部署 SQL Server 数据库中添加链接的服务
1.  在**数据工厂编辑器**中，单击工具栏上的**新数据存储**并选择**SQL Server**。 

    ![新链接的 SQL Server 服务](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  在**JSON 编辑器**在右侧，请执行以下步骤︰ 
    1. 对于**gatewayName**中，指定**adftutorialgateway**。 
    2. 在**连接字符串**中，请执行以下步骤︰ 
        1. 对于**服务器名**，请输入承载 SQL Server 数据库的服务器的名称。
        2. 对于**数据库名称**，请输入数据库的名称。
        3. 单击工具栏上的**加密**按钮。 此下载并启动该凭据管理器应用程序。
        
            ![凭据管理器应用程序](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. 在**设置凭据**对话框中指定身份验证类型、 用户名称和密码，并单击**确定**。 如果连接不成功，加密的凭据存储在 JSON 中并关闭该对话框。 
        6. 关闭空的浏览器选项卡启动对话框中，如果没有自动关闭，回到 Azure 的门户网站选项卡。 
  
            网关计算机上，这些凭据**加密**使用数据工厂服务拥有的证书。 如果您想要使用的证书，而是与数据管理网关相关联，请参阅[安全设置凭据](#set-credentials-and-security)。    
    1.  若要将链接的 SQL Server 服务部署的命令栏上，单击**部署**。 您应该看到在树视图中的链接的服务。 
        
        ![在树视图中的链接的 SQL Server 服务](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>添加链接的 Azure 存储帐户服务
 
1. 在**数据工厂编辑器**中，单击命令栏上的**新数据存储**，单击**Azure 存储**。
2. 输入**帐户名**Azure 存储帐户的名称。
3. Azure 存储帐户的键输入**帐户密码**。
4. 单击**部署**来部署**AzureStorageLinkedService**。
   
 
## <a name="create-datasets"></a>创建数据集
在此步骤中，您创建输入和输出数据集表示的复制操作的输入和输出数据 (本地 SQL Server 数据库 = > Azure blob 存储)。 之前创建数据集，请执行以下步骤 （详细的步骤如下列表）︰

- 创建名为**emp**作为链接的服务添加到数据工厂的 SQL Server 数据库中的表并插入到表中的示例条目数。
- 创建名为**adftutorial** Azure blob 存储帐户作为链接的服务添加到数据工厂中的 blob 容器。

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>SQL Server 内部准备教程

1. 在您指定的本地 SQL Server 数据库链接的服务 (**SqlServerLinkedService**)，使用下面的 SQL 脚本在数据库中创建了**emp**表。

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. 向表中插入一些示例︰ 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>创建输入数据集

1. 在**数据工厂编辑器**中，单击**按钮。.多个**在命令栏上，单击**新的数据集**，单击**SQL Server 表**。 
2.  在右窗格中 JSON 替换下面的文本︰
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    请注意以下几点︰ 

    - **类型**被设置为**SqlServerTable**。
    - **表名**设置为**emp**。
    - **linkedServiceName**设置为**SqlServerLinkedService** （您必须创建此链接的服务在本演练前面部分中）。。
    - 对于另一条管道在 Azure 数据工厂则不会生成输入数据集，您必须设置**外部**为**true**。 它表示的输入的数据产生在 Azure 数据工厂服务。 您可以选择指定在**策略**部分中使用的**externalData**元素的任何外部数据策略。    

    有关 JSON 属性的详细信息，请参阅[移动到从 SQL Server 的数据](data-factory-sqlserver-connector.md)。
2. 若要部署此数据集的命令栏上，单击**部署**。  


### <a name="create-output-dataset"></a>创建输出数据集

1.  在**数据工厂编辑器**中，在命令栏上，单击**新的数据集**并单击**Azure Blob 存储**。
2.  在右窗格中 JSON 替换下面的文本︰ 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    请注意以下几点︰ 
    
    - **类型**被设置为**AzureBlob**。
    - **linkedServiceName**设置为**AzureStorageLinkedService** （已在步骤 2 中创建此链接的服务）。
    - **采用文件夹路径**设置为**adftutorial/outfromonpremdf** outfromonpremdf 所在的文件夹中的 adftutorial 容器。 如果不存在，则创建**adftutorial**容器。 
    - **可用性**设置为**每小时**（设置为**小时**的**频率**和**时间间隔**设置为**1**）。  数据工厂服务生成 SQL Azure 数据库中**emp**表中输出数据切片每隔一小时。 

    如果不指定**文件名**的**输出表**，**采用文件夹路径**中生成的文件被命名为以下格式︰ 数据。<Guid>.txt (例如:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt。)。

    要设置**采用文件夹路径**和**文件名** **SliceStart**时间动态变化，请使用 partitionedBy 属性。 在以下示例中，采用文件夹路径使用年份、 月份和日期的 SliceStart （正在处理切片的开始时间） 和文件名使用来自 SliceStart 小时。 例如，如果一块被生成为 2014年-10-20T08:00:00，文件夹名称设置为 wikidatagateway/wikisampledataout/2014年/10/20 和文件名设置为 08.csv。 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    有关 JSON 属性的详细信息，请参阅[移动到从 Azure Blob 存储的数据](data-factory-azure-blob-connector.md)。
2.  若要部署此数据集的命令栏上，单击**部署**。 确认您看到在树视图中的两个数据集。  

## <a name="create-pipeline"></a>创建管道
在此步骤中，创建**管道**与一个**复制活动**将**EmpOnPremSQLTable**用作输入和输出为**OutputBlobTable** 。

1.  在数据工厂编辑器中，单击**按钮。.多个**，然后单击**新管道**。 
2.  在右窗格中 JSON 替换下面的文本︰ 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
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
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > **启动**属性的值替换为当前的日期和**最终**值与第二天。

    请注意以下几点︰
 
    - 在活动部分中，则只将其**类型**设置为**复制**的活动。
    - 活动的**输入**设置为**EmpOnPremSQLTable** ，**输出**为活动设置为**OutputBlobTable**。
    - **TypeProperties**部分中， **SqlSource**指定为**源类型**并将**BlobSink**指定为**接收器类型**。
    - SQL 查询`select * from emp` **SqlSource**的**sqlReaderQuery**属性指定。

     同时开始和结束 datetimes 必须为[ISO 格式](http://en.wikipedia.org/wiki/ISO_8601)。 例如︰ 2014年-10-14T16:32:41Z。 **结束**时间是可选的但我们在本教程中使用它。 
    
    如果不指定**end**属性的值，它将计算为"**开始 + 48 小时**"。 要无限期地运行管道， **end**属性的值指定**9/9/9999** 。 
    
    要定义数据切片处理基于**可用性**属性为每个 Azure 数据工厂数据集定义的持续时间。
    
    在示例中，有 24 数据扇区每小时产生每个数据片。     
2. 单击**部署**命令栏中部署数据集上 （表是一个矩形的数据集）。 确认显示的管道**管线**节点下的树视图中。  
5. 现在，单击两次以关闭刀片式服务器才能取回**数据工厂**刀片式服务器的**ADFTutorialOnPremDF** **X** 。

**祝贺您 ！** 已成功创建 Azure 数据工厂，链接的服务、 数据集和管道并安排管道。

#### <a name="view-the-data-factory-in-a-diagram-view"></a>在关系图视图中查看数据工厂 
1. 在**Azure 的门户网站**中，请单击**图**图块**ADFTutorialOnPremDF**数据工厂的主页上。 :

    ![图表链接](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. 您应该看到类似于下图的图表︰

    ![关系图视图](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    您可以放大、 缩小、 放大到 100%，缩放以适合、 自动定位管道和数据集，并显示沿袭信息 （突出显示选定项目的上游和下游项目）。  您可以双击某个对象 （输入/输出数据集或管道） 来查看它的属性。 

## <a name="monitor-pipeline"></a>监视器管线
在此步骤中，您可以使用 Azure 门户来监视在 Azure 数据工厂中正在运行的内容。 您可以使用 PowerShell cmdlet 来监视数据集和管道。 有关监视的详细信息，请参阅[监视和管理管道](data-factory-monitor-manage-pipelines.md)。

5. 在图中，双击**EmpOnPremSQLTable**。  

    ![EmpOnPremSQLTable 快讯](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. 请注意，所有数据切片上处于**准备好**状态因为管道持续时间 （开始时间与结束时间） 是在过去。 也是因为您在 SQL Server 数据库中插入数据，并且有所有的时间。 确认，没有切片显示在底部**扇区问题**部分。 若要查看所有切片，请单击**请参阅更多**的扇区列表的底部。 
7. 现在，在**数据集**刀片式服务器，单击**OutputBlobTable**。

    ![OputputBlobTable 快讯](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. 单击列表中的任何数据切片，您应该会看到**数据切片**刀片式服务器。 您会看到活动运行切片。 您会看到只有一个通常运行的活动。  

    ![数据切片刀片式服务器](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    如果切片未处于**就绪**状态，您可以看到的上游片时，还没有准备好，并阻碍当前片从**上游都没有准备好的切片**列表中执行。

10. 单击**运行活动**从列表底部**运行活动的详细信息**，请参阅。

    ![活动运行详细信息刀片式服务器](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    您会看到如吞吐量、 工期和用于传输数据的网关信息。 
11. 单击**X** ，直到您关闭所有刀片式服务器 
12. 回到家乡刀片式服务器的**ADFTutorialOnPremDF**。
14. （可选）**管线**单击**ADFTutorialOnPremDF**，和钻取 （**进行**） 的表输入或输出数据集 （**生产**）。
15. 使用工具，例如使用的工具，如[Microsoft 存储资源管理器](http://storageexplorer.com/)验证 blob/文件将创建每个小时。

    ![Azure 存储资源管理器](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>下一步行动

- 请参阅有关数据管理网关的所有详细信息的[数据管理网关](data-factory-data-management-gateway.md)文章。
- 请参阅以了解有关如何使用复制活动来将数据从一个源数据存储区移动到接收器数据存储区[的数据复制到 Azure SQL Azure Blob](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) 。 
