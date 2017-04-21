<properties 
    pageTitle="将数万亿字节的数据加载到 SQL 数据仓库 |Microsoft Azure" 
    description="演示如何 1 TB 的数据可以加载到 Azure SQL 数据仓库在 15 分钟的 Azure 数据工厂" 
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
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>在 15 分钟的 Azure 数据工厂 [复制向导] 到 Azure SQL 数据仓库加载 1 TB
[Azure SQL 数据仓库](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)是一个基于云的、 向外扩展的数据库能够处理海量数据、 关系和非关系。  在大规模并行处理 (MPP) 的体系结构上构建，是为企业数据仓库工作负载优化 SQL 数据仓库。  它提供了云弹性，灵活地扩展存储和独立计算。

开始使用 Azure SQL 数据仓库是现在比以往任何时候都使用**Azure 数据工厂**更容易。  Azure 数据工厂是一个完全托管的基于云的数据集成服务，它可以用于填充 SQL 数据仓库的数据与您现有的系统，从而节省您的宝贵时间，同时评估 SQL 数据仓库和建立在它的上面分析解决方案。  以下是将数据加载到 SQL Azure 的数据仓库使用 Azure 数据工厂的主要优点︰

- **易于设置**︰ 5 步骤无需脚本使用直观的向导。
- **丰富的数据存储支持**︰ 内置支持一套丰富的内部和基于云的数据存储区。
- **安全和标准**︰ 数据传输通过 HTTPS 或 ExpressRoute，和全球服务是否存在可确保您的数据永远不会离开的地理边界
- **无与伦比的性能，通过使用 PolyBase** – 使用 Polybase 是最有效的方式将数据移动到 Azure SQL 数据仓库。 使用临时的 blob 功能，可以实现高负荷速度从所有类型的数据存储区除了 Azure Blob 存储，默认情况下支持 Polybase。

这篇文章演示了如何使用数据工厂复制向导在 15 分钟后，在超过 1.2 GBps 吞吐量的 1 TB 数据从 Azure Blob 存储加载到 Azure SQL 数据仓库。

本文提供了有关使用复制向导将数据移动到 Azure SQL 数据仓库中的分步说明。 

> [AZURE.NOTE] 请参阅[移动数据，SQL Azure 的数据仓库使用 Azure 数据工厂与](data-factory-azure-sql-data-warehouse-connector.md)文章有关一般信息功能的数据工厂中将数据移动到或从 Azure SQL 数据仓库。 
> 
> 您也可以生成使用 Azure 门户，Visual Studio，PowerShell，管道等。请参阅[教程︰ 将 Azure Blob 数据复制到 SQL Azure 数据库](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)快速演练的 Azure 数据工厂中使用复制活动的分步指导。  

## <a name="prerequisites"></a>系统必备组件
- Azure Blob 存储︰ 此试验用于存储 TPC H 测试数据集使用 Azure 存储 Blob (GRS)。  如果您没有一个 Azure 存储帐户，了解[如何创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)。
- [TPC H](http://www.tpc.org/tpch/)数据︰ 我们 TPC H 作为测试数据集。  要做到这一点，您需要使用`dbgen`从 TPC H 工具包，从而帮助您生成数据集。  您也可以下载源代码`dbgen`从[TPC 工具](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp)并编译它自己，或从[GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools)上下载的已编译的二进制文件。  使用以下命令以生成 1 TB 平面文件中的，运行 dbgen.exe`lineitem`跨 10 个文件表格跨页︰
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    现在将生成的文件复制到 Azure Blob。  如何做到这一点的[移动数据与使用 Azure 数据工厂内部文件系统](data-factory-onprem-file-system-connector.md)是指使用 ADF 副本。   
- Azure SQL 数据仓库︰ 本实验将数据加载到 SQL Azure 的数据仓库创建具有 6000 DWUs

    有关如何创建 SQL 数据仓库数据库的详细说明，请参阅[创建一个 Azure SQL 数据仓库](../sql-data-warehouse/sql-data-warehouse-get-started-provision/)。  若要到 SQL 数据仓库中使用 Polybase 获得最佳可能的负载性能，我们选择数据仓库单元 (DWUs) 的性能设置，即 6000 DWUs 中所允许的最大数量。

    > [AZURE.NOTE] 
    > 加载时从 Azure Blob，数据加载性能是 SQL 数据仓库配置的 DWUs 数成正比︰
    > 
    > 1 TB 载入 1000 DWU SQL 数据仓库采用 87 分钟 (~ 200MBps 吞吐量) 1 TB 载入 2000 DWU SQL 数据仓库采用 46 分钟 (~ 380MBps 的吞吐量) 装载 6000 到 1 TB DWU SQL 数据仓库采用 14 分钟 （~1.2GBps 吞吐量） 

    要使用 6000 DWUs 创建 SQL 数据仓库，将一直向右移动性能滑块︰

    ![性能滑块](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    现有数据库未配置为具有 6000 DWUs，您可以扩大它使用 Azure 的门户。  导航到 Azure 的门户网站中的数据库，如下图所示的**概述**面板中没有的**小数位数**按钮︰

    ![缩放按钮](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    单击**比例**按钮以打开下面的面板中，将滑块移动到最大值，然后单击**保存**按钮。

    ![缩放对话框](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    本实验将数据加载到 Azure SQL 数据仓库使用`xlargerc`的资源类。

    若要获得最佳可能的吞吐量，复制需要使用 SQL 数据仓库用户属于执行`xlargerc`的资源类。  了解如何通过以下[更改用户资源类示例](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example)。  

- 通过运行以下 DDL 语句 Azure SQL 数据仓库数据库中创建目标表的架构︰

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

与已完成的必备步骤，我们现在就可以配置使用复制向导复制活动。

## <a name="launch-copy-wizard"></a>启动复制向导

1.  登录到[Azure 的门户](https://portal.azure.com)。
2.  单击**+ 新建**从左上角，单击**智能 + 分析**，单击**数据工厂**。 
6. 在**新数据工厂**刀片式服务器︰
    1. **名称**中输入**LoadIntoSQLDWDataFactory** 。
        Azure 数据工厂的名称必须是全局唯一的。 如果您收到错误︰**数据工厂名称"LoadIntoSQLDWDataFactory"是不可用**的更改数据工厂 (例如，yournameLoadIntoSQLDWDataFactory) 的名称，然后尝试重新创建。 请参阅[数据工厂的命名规则](data-factory-naming-rules.md)命名规则对数据工厂项目的主题。  
     
    2. 选择 Azure**订阅**。
    3. 资源组，请执行以下步骤之一︰ 
        1. 选择**使用现有的**选择现有资源组。
        2. 选择**创建新**输入资源组的名称。
    3. 选择数据工厂的**位置**。
    4. 选择**固定到面板**底部的刀片式服务器的复选框。  
    5. 单击**创建**。
10. 创建过程完成后，您会看到**数据工厂**刀片式服务器，如下图所示︰

    ![数据工厂主页](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. 在数据工厂的主页上，单击**将数据复制**瓷砖启动**复制向导**。 

    > [AZURE.NOTE] 如果您查看 web 浏览器被卡住在"授权..."，禁用/取消选中**禁止第三方 cookie 和站点数据**设置 （或） 保持启用**login.microsoftonline.com**例外和创建然后尝试重新启动该向导。


## <a name="step-1-configure-data-loading-schedule"></a>步骤 1︰ 配置数据加载计划
第一步是配置数据装载计划。  

在**属性**页中︰
1. 输入**任务名称** **CopyFromBlobToAzureSqlDataWarehouse**
2. 选择**一次现在运行**的选项。   
3. 单击**下一步**。  

![复制向导-属性页面](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>步骤 2︰ 配置源
本节介绍的步骤来配置源︰ Azure Blob 包含 1 TB TPC H 行项的文件。

选择**Azure Blob 存储**数据存储，然后单击**下一步**。

![复制向导 — — 选择的源页](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

填写的 Azure Blob 存储帐户的连接信息并单击**下一步**。

![复制向导的源连接信息](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

选择包含 TPC H 行项目文件的**文件夹**，然后单击**下一步**。

![复制向导-选择输入的文件夹](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

在单击**下一步**，会自动检测文件格式设置。  检查以确保该列分隔符 | 而不是默认逗号，。  数据预览完毕之后，请单击**下一步**。

![复制向导-文件格式设置](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>步骤 3︰ 配置目标
本节说明如何配置目标︰ `lineitem` Azure SQL 数据仓库数据库中的表。

选择**Azure SQL 数据仓库**作为目标存储，然后单击**下一步**。

![复制向导-选择目标数据存储区](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

填写 Azure SQL 数据仓库的连接信息。  请确保您指定的用户的角色的成员`xlargerc`（请参见**先决条件**部分的详细说明），并单击**下一步**。 

![复制向导-目标连接信息](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

选择目标表，然后单击**下一步**。

![复制向导-表映射页](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

接受列映射的默认设置，然后单击**下一步**。

![复制向导-架构映射页](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>步骤 4︰ 性能设置

**Polybase 允许**在缺省情况下处于选中状态。  单击**下一步**。

![复制向导-架构映射页](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>步骤 5︰ 部署和监视负载结果
单击**完成**按钮进行部署。 

![复制向导的摘要页面](media/data-factory-load-sql-data-warehouse/summary-page.png)

部署完毕后，请单击`Click here to monitor copy pipeline`来监控运行进度的副本。

选择**活动窗口**列表中创建的副本管道。

![复制向导的摘要页面](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

您可以查看包括从源中读取和写入到目标、 工期和运行的平均吞吐量的数据卷的右面板中的**活动窗口资源管理器**中运行的详细信息的副本。

您可以看到从下面的屏幕快照，复制到数据仓库中 SQL Azure Blob 存储 1TB 花费 14 分钟内，有效地达到 1.22 GBps 吞吐量 ！

![复制向导-成功对话框](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>最佳做法
下面是运行 Azure SQL 数据仓库数据库的一些最佳做法︰

- 加载到 COLUMNSTORE 聚集索引时，请使用一个较大的资源类。
- 对于更有效的联接，请考虑使用而不是倒圆角法分配默认选择列希分发。
- 更快的加载速度，请考虑使用瞬态数据堆。
- 在完成加载 Azure SQL 数据仓库之后，请创建统计信息。

有关详细信息，请参阅[最佳操作 Azure SQL 数据仓库](../sql-data-warehouse/sql-data-warehouse-best-practices.md)。 

## <a name="next-steps"></a>下一步行动
- [数据工厂复制向导](data-factory-copy-wizard.md)— — 这篇文章提供了有关复制向导的详细信息。 
- [复制活动性能和调优指南 》](data-factory-copy-activity-performance.md)的这篇文章包含引用性能测量和优化指南。

