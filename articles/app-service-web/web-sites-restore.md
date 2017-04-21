<properties 
    pageTitle="还原在 Azure 应用程序" 
    description="了解如何从备份中还原您的应用程序。" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>还原在 Azure 应用程序

本文介绍了如何还原以前备份 （请参阅[备份应用程序在 Azure](web-sites-backup.md)） 的[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)中的应用程序。 您可以将您的应用程序与它链接的数据库 （SQL 数据库或 MySQL） 按需恢复到以前的状态，或创建新应用程序基于一个原始应用程序的备份。 创建新的应用程序到最新版本并行运行可用于 A / B 测试。

从备份中还原可供在**标准**和**特优**层中运行的应用程序。 有关扩展您的应用程序的信息，请参阅[在 Azure 应用程序扩展](web-sites-scale.md)。 **特优**层使更多的比**标准**层要执行的每日备份。

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>从现有的备份恢复应用程序

1. 在 Azure 门户应用程序**设置**刀片式服务器，请单击显示**备份**刀片式服务器的**备份**。 然后单击命令栏中的**立即还原**。 
    
    ![选择立即还原][ChooseRestoreNow]

3. 在**还原**刀片式服务器，首先选择备份的源。 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    **应用程序备份**选项显示的当前应用程序中，所有现有的备份，您可以方便地选择一个。 
    **存储**选项允许您选择从任何现有的 Azure 存储帐户和您的订阅中的容器的任何备份 ZIP 文件。 
    如果您正在尝试还原备份的其他应用程序，使用**存储**选项。

4. 然后，在**还原目标**指定应用程序还原的目标位置。

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] 如果选择**覆盖**，您当前的应用程序中的所有现有数据都将被清除。 单击**确定**之前，请确保它是完全要做。
    
    您可以选择**现有应用程序**将还原到同一资源组中的其他应用程序的应用程序备份。 使用此选项之前，应已创建另一个应用程序使用镜像到一个备份应用程序中定义的数据库配置资源组中。 
    
5. 单击**确定**。

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>下载或从存储帐户中删除备份
    
1. 从主的 Azure 门户网站，**浏览**刀片式服务器选择**存储帐户**。
    
    将显示您现有的存储帐户的列表。 
    
2. 选择包含您想要下载或删除的备份的存储帐户。
    
    将显示刀片式服务器的存储帐户。

3. 在存储 accountn 刀片式服务器，选择所需的容器
    
    ![视图容器][ViewContainers]

4. 选择要下载或删除的备份文件。

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. 这取决于您希望单击**下载**或**删除**。  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>监视恢复操作
    
1. 要查看有关的成功或失败的应用程序还原操作的详细信息，请导航到**审核日志**刀片式服务器在 Azure 的门户。 
    
    **审核日志**刀片式服务器显示所有您的运营，以及级别、 状态、 资源和时间的详细信息。
    
2. 向下滚动以查找所需的还原操作，请单击以选中它。

刀片式服务器的详细信息将显示与还原操作相关的可用信息。
    
## <a name="next-steps"></a>下一步行动

您还可以备份和还原应用程序服务使用 REST API 的应用程序 （请参见[使用其他备份和还原应用程序服务的应用程序](websites-csm-backup.md)）。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
