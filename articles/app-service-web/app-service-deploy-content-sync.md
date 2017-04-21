<properties
    pageTitle="将内容从云文件夹同步到 Azure 应用程序服务"
    description="了解如何部署您的应用程序到 Azure 应用程序服务内容同步通过从云文件夹。"
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>将内容从云文件夹同步到 Azure 应用程序服务

本教程展示了如何通过将您的内容从收存箱和 OneDrive 等流行的云存储服务部署到[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)。 

## <a name="overview"></a>内容同步部署概述

按需内容同步部署电源[Kudu 部署引擎](https://github.com/projectkudu/kudu/wiki)与应用程序服务集成在一起。 在[Azure 门户](https://portal.azure.com)中，可以在云存储中指定一个文件夹、 与您的应用程序代码和在该文件夹中的内容并通过单击一个按钮同步到应用程序服务。 内容同步利用 Kudu 为生成和部署过程。 
    
## <a name="contentsync"></a>如何启用内容同步部署
若要启用从[Azure 门户](https://portal.azure.com)的内容同步，请执行以下步骤︰

1. 在您的应用程序在 Azure 门户的刀片式服务器，请单击**设置** > **部署源**。 单击**选择源**，然后选择**OneDrive**或**收存箱**作为部署的来源。 

    ![内容同步](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] 由于 Api 基础差异，这一次不支持**OneDrive 的业务**。 

2. 完成授权工作流启用应用程序服务访问特定的预定义指定的路径为 OneDrive 或收存箱的所有应用程序服务内容的存储位置。  
    之后授权应用程序服务平台将提供选项以内容文件夹下创建文件夹的指定内容的路径，或选择现有内容文件夹在此指定的内容的路径下。 在用的应用程序服务同步您云存储帐户下指定的内容路径如下所示︰  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **收存箱**︰`Dropbox\Apps\Azure`

3. 在初始内容同步后内容同步可以启动按需从 Azure 的门户。 部署的历史是**部署**刀片式服务器可用。

    ![部署历史记录](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
收存箱部署的详细信息，请参阅[部署从收存箱](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx)。 


