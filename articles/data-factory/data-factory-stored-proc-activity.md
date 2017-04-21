<properties 
    pageTitle="SQL Server 存储过程活动" 
    description="了解如何使用 SQL Server 存储过程活动来调用数据工厂管线从存储的过程的 SQL Azure 数据库或 Azure SQL 数据仓库中。" 
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
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL Server 存储过程活动
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)

您可以使用数据工厂[管线](data-factory-create-pipelines.md)中的 SQL Server 存储过程活动调用一个下面的数据存储区中的存储的过程︰ 


- SQL azure 数据库 
- SQL azure 数据仓库  
- 在您的企业或 Azure VM 中的 SQL Server 数据库。 您需要安装数据管理网关，承载数据库的同一台计算机上或在单独的计算机，以避免争用资源的数据库。 数据管理网关是连接的软件部署数据源中的数据源在 Azure Vm 破坏到云服务安全和管理的方式。 请参阅有关数据管理网关的详细信息的[内部和云之间的数据移动](data-factory-move-data-between-onprem-and-cloud.md)文章。 

本文基于[数据变换活动](data-factory-data-transformation-activities.md)文章，介绍了数据转换和受支持的转换活动的一般概述。

## <a name="walkthrough"></a>演练

### <a name="sample-table-and-stored-procedure"></a>示例表和存储的过程
1. 在使用 SQL Server 管理 Studio SQL Azure 数据库中创建下**表**或任何其他工具您感到满意。 Datetimestamp 列是日期和时间生成对应的 ID 的时候。 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Id 是唯一的标识和 datetimestamp 列是日期和时间生成对应的 ID 的时候。
    ![示例数据](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] 此示例使用 SQL Azure 数据库但 Azure SQL 数据仓库和 SQL Server 数据库中相同的方式工作。 
2. 创建以下**存储过程**插入到**sampletable**中的数据。

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **名称**和参数 （在本例中，日期时间） 的**大小写**必须与管道/活动 JSON 中指定的参数相匹配。 在存储的过程定义中，确保**@**可作为前缀的参数。
    
### <a name="create-a-data-factory"></a>创建数据工厂  
4. 登录到[Azure 的门户](https://portal.azure.com/)。 
5. 单击左侧菜单上的**新建**，单击**智能 + 分析**，单击**数据工厂**。
    
    ![新数据工厂](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  在**新数据工厂**刀片式服务器，输入名称**SProcDF** 。 Azure 数据工厂名称是**全局唯一**的。 您需要数据工厂的名称加上您的名字，能够成功创建工厂的前缀。

    ![新数据工厂](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  选择您的**Azure 的订阅**。 
4.  **资源组**，请执行以下步骤之一︰ 
    1.  单击**创建**，然后输入资源组的名称。
    2.  单击**使用现有的**并选择现有资源组。  
5.  选择数据工厂的**位置**。
6.  以便可以看到数据工厂操控板上的下一次登录的时，请选择**固定到仪表板**。 
6.  在**新数据工厂**刀片式服务器，请单击**创建**。
6.  您会看到在 Azure 门户的**仪表板**中创建的数据工厂。 数据工厂创建成功后，您会看到数据工厂页中，显示的内容的数据工厂。
    ![数据工厂主页](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>创建链接的 Azure SQL 服务  
创建数据工厂之后, 您可以创建链接到数据工厂的 SQL Azure 数据库链接的 Azure SQL 服务。 此数据库中包含 sampletable 表和 sp_sample 存储过程。

7.  单击**作者和部署**的**SProcDF**启动数据工厂编辑器**数据工厂**刀片上。
2.  单击命令栏上的**新数据存储**，并选择**SQL Azure 数据库**。 您应该看到 JSON 脚本用于在编辑器中创建链接的 Azure SQL 服务。 

    ![新的数据存储](media/data-factory-stored-proc-activity/new-data-store.png)
4. 在 JSON 脚本中，进行以下更改︰ 
    1. 更换**&lt;服务器名称&gt;**与 SQL Azure 数据库服务器的名称。
    2. 更换**&lt;'数据库名称'&gt;**与在其中创建表和存储的过程的数据库。
    3. 更换**&lt;username@servername**使用的用户帐户具有访问数据库的权限。
    4. 更换**&lt;密码&gt;**与用户帐户的密码。 

    ![新的数据存储](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. 若要将链接的服务部署的命令栏上，单击**部署**。 确认您看到在左边的树视图中的 AzureSqlLinkedService。 

    ![与链接服务的树视图](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>创建一个输出数据集
6. 单击**按钮。.多个**在工具栏上，单击**新的数据集**，然后单击**SQL Azure**。 命令栏和选择**SQL Azure**上**新数据集**。

    ![与链接服务的树视图](media/data-factory-stored-proc-activity/new-dataset.png)
7. 复制/粘贴以下 JSON 中的脚本到 JSON 编辑器。

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. 若要部署此数据集的命令栏上，单击**部署**。 确认您看到的树视图中的数据集。 

    ![树视图中的链接服务](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>与 SqlServerStoredProcedure 活动创建管线
现在，让我们与 SqlServerStoredProcedure 活动中创建管线。
 
9. 单击**按钮。.多个**命令栏，然后单击**新管道**。 
9. 复制/粘贴下面的 JSON 段。 **StoredProcedureName**设置为**sp_sample**。 名称和**日期时间**参数的大小写必须匹配的名称和大小写形式存储的过程定义中的参数。  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    如果您需要传递 null 作为参数，请使用语法:"param1": 空 （全部为小写）。 
9. 在部署管线工具栏上，单击**部署**。  

### <a name="monitor-the-pipeline"></a>监视管道

6. 单击**X**关闭数据工厂编辑器刀片式服务器并向后定位到数据工厂刀片式服务器，然后单击**关系图**。

    ![平铺关系图](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. 在**关系图视图**中，您将看到概述的管线，以及在本教程中使用的数据集。 

    ![平铺关系图](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. 在关系图视图中，双击数据集**sprocsampleout**。 您会看到就绪状态中的扇区。 应为五片，因为切片生成开始时间和结束时间从 JSON 之间每小时。

    ![平铺关系图](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. 当切片处于**就绪**状态时，运行*从 sampletable 中*选择*** 验证，数据被插入在表的存储过程的 SQL Azure 数据库查询。

    ![输出数据](./media/data-factory-stored-proc-activity/output.png)

    有关监视 Azure 数据工厂管道的详细信息，请参阅[监视管道](data-factory-monitor-manage-pipelines.md)。  

> [AZURE.NOTE] 在此示例中，SprocActivitySample 有任何输入。 如果您想要链接 （即，以前处理） 上游活动与此活动，上游活动的输出可以用作输入在此活动中。 在这种情况下，上游活动完成并上游活动的输出会出现 （在就绪状态），才会执行此活动。 输入不能直接用作存储的过程活动的参数

## <a name="json-format"></a>JSON 格式
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON 属性

属性 | 说明 | 必填
-------- | ----------- | --------
名称 | 活动名称 | 是的
说明 | 描述该活动的用途的文本 | 不
类型 | SqlServerStoredProcedure | 是的
输入 | 可选。 如果您指定输入数据集，它必须是可用 （处于就绪状态） 的要运行的存储的过程活动。 不能在存储过程中作为参数使用输入数据集。 它仅用于在启动存储的过程活动之前检查依赖项。 | 不
输出 | 您必须指定一个输出数据集的存储的过程活动。 输出数据集指定存储的过程活动**计划**（每小时、 每周、 每月等）。 <br/><br/>输出数据集必须使用 Azure SQL 数据库或 Azure SQL 数据仓库或 SQL Server 数据库中您想要运行的存储的过程引用**链接的服务**。 <br/><br/>输出数据集可以作为传递后续处理的管道中 （[链接活动](data-factory-scheduling-and-execution.md#chaining-activities)） 的另一个活动的存储过程的结果的方法。 但是，数据工厂不到此数据集自动写入存储过程的输出。 它是将写入输出数据集指向 SQL 表的存储的过程。 <br/><br/>在某些情况下，输出数据集可以是**虚拟数据集**，它只用来指定运行该存储的过程活动的日程安排。 | 是的
storedProcedureName | SQL Azure 数据库或 Azure SQL 数据仓库所表示的链接使用服务，则输出表中指定的存储过程的名称。 | 是的
storedProcedureParameters | 指定存储的过程参数的值。 如果您需要传递 null 作为参数，请使用语法:"参数 1": 空值 （所有小写）。 请参阅下面的示例以了解有关使用此属性。| 不

## <a name="passing-a-static-value"></a>将静态值传递 
现在，让我们考虑包含静态值称为文档示例的表中添加另一列命名为方案。

![示例数据 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

现在，通过从存储的过程活动方案参数和值。 前面的示例中的 typeProperties 部分类似于下面的代码段︰

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

