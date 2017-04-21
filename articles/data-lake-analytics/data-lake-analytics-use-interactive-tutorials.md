<properties 
   pageTitle="了解数据湖分析和 U SQL Azure 门户交互式教程使用 |Azure" 
   description="快速入门学习数据湖分析和 U SQL。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>使用 Azure 数据湖分析交互式教程

Azure 门户提供了交互式教程为您着手湖泊分析数据。 本文演示了如何通过分析网站日志的教程。


>[AZURE.NOTE]如果您想要通过使用 Visual Studio 将同一教程，请参阅[分析网站日志使用数据湖分析](data-lake-analytics-analyze-weblogs.md)。
>更多交互式教程，可以添加到门户。


其他的教程，请参阅︰

- [有关数据使用 Azure 门户的湖泊分析入门](data-lake-analytics-get-started-portal.md)
- [有关数据使用 Azure PowerShell 的湖泊分析入门](data-lake-analytics-get-started-powershell.md)
- [数据使用.NET SDK 的湖泊分析入门](data-lake-analytics-get-started-net-sdk.md)
- [开发数据湖工具用于 Visual Studio U SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md) 

**系统必备组件**

在开始本教程之前，您必须具有以下︰

- **数据湖分析帐户**。  请参阅[开始使用 Azure 数据湖分析使用 Azure 门户](data-lake-analytics-get-started-portal.md)。

##<a name="create-data-lake-analytics-account"></a>创建数据湖分析帐户 

您可以运行任何作业之前，您必须具有数据湖分析帐户。

每个数据湖分析帐户具有[Azure 数据湖存储](../data-lake-store/data-lake-store-overview.md)帐户依赖关系。  此帐户被称为默认数据湖存储帐户。  事先或创建数据湖分析帐户时，您可以创建数据湖存储帐户。 在本教程中，您将创建与分析客户数据湖存储帐户

**若要创建一个数据湖分析帐户**

1. 登录到[Azure 的门户](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute)。
2. 单击**Microsoft Azure**中打开 StartBoard 的左上角。
3. 单击**市场上**拼贴。  
3. 在**所有**刀片式服务器，然后按**enter 键**上的搜索框中键入**Azure 数据湖分析**。 列表中，您应看到**Azure 数据湖分析**。
4. 从列表中单击**Azure 数据湖分析**。
5. 单击底部的刀片式服务器的**创建**。
6. 键入或选择以下设置︰

    ![Azure 数据湖分析门户刀片](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **名称**︰ 命名分析帐户。
    - **数据湖商店**︰ 每个数据湖分析帐户有相关的数据湖存储帐户。 数据湖分析帐户和相关数据湖存储帐户必须位于相同的 Azure 数据中心。 按照说明创建新的数据湖存储帐户，或选择一个现有。
    - **订阅**︰ 选择 Azure 用于分析客户的预订。
    - **资源组**。 选择一个现有的 Azure 资源组或新建一个。 应用程序通常组成许多组件，例如 web 应用程序、 数据库、 数据库服务器、 存储和第三方服务。 Azure 资源管理器 (ARM) 使您能够使用您的应用程序作为一个组，称为 Azure 资源组中的资源。 可以部署、 更新、 监视或删除单个、 协调操作中的应用程序的所有资源。 您的部署使用的模板，该模板可以适合不同的环境，如测试、 过渡和生产。 您可以通过查看整个组的汇总的成本为您的组织澄清计费。 有关详细信息，请参阅[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)。 
    - **位置**。 选择数据湖分析帐户 Azure 数据中心。 
7. 选择**锁定到 Startboard**。 这都需要遵循本教程。
8. 单击**创建**。 它将带您到门户网站 StartBoard。 新的拼贴显示"部署 Azure 数据湖分析"标签添加到主页。 花费一些时间来创建一个数据湖分析帐户。 创建帐户时，门户将打开新的刀片式服务器上的帐户。

    ![Azure 数据湖分析门户刀片](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>运行网站日志分析的交互式教程

**若要打开网站日志分析的交互式教程**

1. 从门户中，从左侧的菜单打开 StartBoard 中单击**Microsoft Azure** 。
2. 单击链接到数据湖分析客户的拼贴。
3. **基础**栏中单击**资源管理器的交互式教程**。

    ![数据湖分析交互式教程](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. 如果您看到橙色警告说"样本不设置，单击..."，然后单击**复制示例数据**，可以将示例数据复制到默认数据湖存储帐户。 交互式教程需要运行数据。
5. 从**交互式教程**刀片式服务器，请单击**网站日志分析**。 门户网站将在新的门户刀片打开教程。
5. 单击**1 介绍**，然后按照说明进行操作

##<a name="see-also"></a>请参见

- [Microsoft Azure 数据湖分析的概述](data-lake-analytics-overview.md)
- [有关数据使用 Azure 门户的湖泊分析入门](data-lake-analytics-get-started-portal.md)
- [有关数据使用 Azure PowerShell 的湖泊分析入门](data-lake-analytics-get-started-powershell.md)
- [开发数据湖工具用于 Visual Studio U SQL 脚本](data-lake-analytics-data-lake-tools-get-started.md)
- [分析网站日志使用 Azure 数据湖分析](data-lake-analytics-analyze-weblogs.md)
