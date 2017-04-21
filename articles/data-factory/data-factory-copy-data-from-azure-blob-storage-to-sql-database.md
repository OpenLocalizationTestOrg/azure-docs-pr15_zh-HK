<properties
    pageTitle="Blob 存储中的数据复制到 SQL 数据库 |Microsoft Azure"
    description="本教程展示如何在 Azure 数据工厂管道中使用复制活动 Blob 存储中的数据复制到 SQL 数据库。"
    Keywords="blob sql，blob 存储数据副本"
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
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Blob 存储中的数据复制到 SQL 数据库使用数据工厂 
> [AZURE.SELECTOR]
- [概述和系统必备组件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [复制向导](data-factory-copy-data-wizard-tutorial.md)
- [Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure 的资源管理器模板](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API，](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


在本教程中，使用管道来 Blob 存储中的数据复制到 SQL 数据库创建数据工厂。

复制活动在 Azure 数据工厂执行数据移动。 它被采用全局可用的服务，可将各种数据存储安全、 可靠和可扩展的方式之间的数据复制。 请参阅[数据移动活动](data-factory-data-movement-activities.md)文章复制活动有关的详细信息。  

> [AZURE.NOTE] 数据工厂服务的详细概述，请参见[Azure 数据工厂简介](data-factory-introduction.md)文章。

##<a name="prerequisites-for-the-tutorial"></a>本教程的先决条件
在开始本教程之前，您必须具有以下︰

- **Azure 的订阅**。  如果没有预订，您可以在几分钟创建免费的试用帐户。 [免费试用版](http://azure.microsoft.com/pricing/free-trial/)文章的详细信息，请参阅。
- **Azure 存储帐户**。 在本教程中作为**源**数据存储使用 blob 存储。 如果您没有 Azure 存储帐户，请参阅[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)文章中的步骤创建一个。
- **SQL azure 数据库**。 在本教程中作为**目标**数据存储使用 SQL Azure 数据库。 如果您没有在本教程中可以使用 SQL Azure 数据库，请参阅[如何创建和配置一个 Azure 的 SQL 数据库](../sql-database/sql-database-get-started.md)来创建一个。
- **SQL Server 2012年/2014年或 Visual Studio 2013年**。 若要创建一个示例数据库，并查看数据库中的结果数据，请使用 SQL Server 管理 Studio 或 Visual Studio。  

## <a name="collect-blob-storage-account-name-and-key"></a>收集 blob 存储帐户名称和密钥 
所需的帐户名和 Azure 存储帐户进行本教程中的帐户密码。 请注意，**帐户名称**和 Azure 存储帐户的**帐户项**下。

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 单击左侧菜单上的**多个服务**，然后选择**存储帐户**。

    ![浏览-存储帐户](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. 在**存储帐户**刀片式服务器，选择您想要在本教程中使用**Azure 存储帐户**。
4. 选择**访问键**在**设置**下的链接。
5.  单击**存储帐户名称**文本框旁边的**副本**（图像） 按钮，保存/粘贴它在某个位置 (例如︰ 在一个文本文件中)。
6. 重复上述步骤复制或下**key1**注意到。
    
    ![存储访问键](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. 单击**X**关闭所有刀片式服务器。

## <a name="collect-sql-server-database-user-names"></a>收集 SQL 服务器、 数据库、 用户名称
您需要的 SQL Azure 服务器、 数据库和用户做本教程中的名称。 记下您的 SQL Azure 数据库**服务器**、**数据库**和**用户**的名称。

1. 在**Azure 的门户网站**中，请单击左边的**更多的服务**和选择**SQL 数据库**。
2. 在**SQL 数据库刀片式服务器**，选择您想要在本教程中使用的**数据库**。 请注意下的**数据库名称**。  
3. 在**SQL 数据库**刀片式服务器，单击**设置**下的**属性**。
4. **服务器名称**和**服务器管理员登录**，记下的值。
5. 单击**X**关闭所有刀片式服务器。

## <a name="allow-azure-services-to-access-sql-server"></a>允许访问服务器 SQL Azure 服务 
确保****允许访问 Azure 服务**设置打开 SQL Azure 服务器，以便数据工厂服务可以访问您的 SQL Azure 服务器**。 要验证，并启用此设置，请执行以下步骤︰

1. 单击左侧的**更多服务**中枢并单击**SQL 服务器**。
2. 选择您的服务器，然后单击**设置**下的**防火墙**。 
4. **在**设置防火墙**刀片式服务器，单击**Azure 服务允许**访问。**
5. 单击**X**关闭所有刀片式服务器。

## <a name="prepare-blob-storage-and-sql-database"></a>准备 Blob 存储和 SQL 数据库 
现在，准备您的 Azure blob 存储和 SQL Azure 数据库教程，请执行以下步骤︰  

1. 启动记事本、 粘贴以下文本，并将其保存为**emp.txt**到**C:\ADFGetStarted**文件夹在您的硬盘上。

        John, Doe
        Jane, Doe

2. 若要创建**adftutorial**容器，将**emp.txt**文件上载到容器使用[Azure 存储浏览器](https://azurestorageexplorer.codeplex.com/)这样的工具。

    ![Azure 存储资源管理器。 Blob 存储 SQL 数据库之间复制数据](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. 使用以下 SQL 脚本在 SQL Azure 数据库中创建了**emp**表。  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **如果您有在您的计算机上安装的 SQL Server 2012年/2014年:**按照说明从[第 2 步︰ 连接到 SQL 数据库，使用 SQL Server 管理 Studio 管理 Azure SQL 数据库的](../sql-database/sql-database-manage-azure-ssms.md#Step2)项目连接到 SQL Azure 服务器并运行 SQL 脚本。 本文使用了[经典的 Azure 门户](http://manage.windowsazure.com)，不[新 Azure 的门户](https://portal.azure.com)，配置 SQL Azure 服务器的防火墙。

    如果您的客户端不允许访问该 SQL Azure 服务器，您需要配置您的 SQL Azure 服务器以允许访问您的计算机 （IP 地址） 的防火墙。 请参见[这篇文章](../sql-database/sql-database-configure-firewall-settings.md)的步骤来配置 SQL Azure 服务器的防火墙。

您已经完成了系统必备组件。 您可以创建使用以下方式之一的数据工厂。 单击某个选项卡顶部或下面的链接以执行本教程。     

- [Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API，](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [复制向导](data-factory-copy-data-wizard-tutorial.md)
