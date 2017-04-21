<properties
    pageTitle="使用 PowerShell 来备份和还原应用程序服务的应用程序"
    description="了解如何使用 PowerShell 来备份和还原在 Azure 应用程序服务的应用程序"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>使用 PowerShell 来备份和还原应用程序服务的应用程序

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST API，](../app-service-web/websites-csm-backup.md)

了解如何使用 Azure PowerShell 来备份和还原[应用程序服务的应用程序](https://azure.microsoft.com/services/app-service/web/)。 Web 应用程序备份，包括要求和限制，有关详细信息，请参阅[备份在 Azure 应用程序服务 web 应用程序](../app-service-web/web-sites-backup.md)。

## <a name="prerequisites"></a>系统必备组件
若要使用 PowerShell 管理应用程序备份，您需要以下各项︰

- 允许读取和写入访问到 Azure 存储容器的**SAS URL** 。 SAS Url 的说明，请参阅[了解 SAS 模型](../storage/storage-dotnet-shared-access-signature-part-1.md)。 管理 Azure 存储使用 PowerShell 的示例，请参阅[使用 Azure 存储使用 Azure PowerShell](../storage/storage-powershell-guide-full.md) 。
- **一个数据库连接字符串**如果您想要您的 web 应用程序和数据库进行备份。

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>如何生成要使用的 web 应用程序备份 cmdlet 的 SAS URL
可以使用 PowerShell 生成 SAS 的 URL。 这里是如何生成一个可以使用本文中讨论的 cmdlet 的一个示例。

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>安装 Azure PowerShell 1.3.2 或更高版本

有关安装和使用 Azure PowerShell 的说明，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-install-configure.md)。

## <a name="create-a-backup"></a>创建备份

使用 New AzureRmWebAppBackup cmdlet 以创建 web 应用程序的备份。

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

这与自动生成的名称创建一个备份。 如果您想要为您的备份提供一个名称，使用 BackupName 的可选参数。

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

若要在备份中包括数据库，首先创建数据库备份设置使用 New AzureRmWebAppDatabaseBackupSetting cmdlet，然后提供新建 AzureRmWebAppBackup cmdlet 的数据库参数中的该设置。 数据库参数接受数组的数据库设置，使您可以备份多个数据库。

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>得到备份

获得 AzureRmWebAppBackupList cmdlet 返回数组的 web 应用程序的所有备份。 您必须提供 web 应用程序和其资源组的名称。

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

若要获得特定的备份，请使用 Get AzureRmWebAppBackup cmdlet。

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

您也可以到任何备份管理 cmdlet 以便管道 web 应用程序对象。

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>计划自动备份

您可以计划备份指定的时间间隔自动发生。 要配置备份时间表，请使用编辑 AzureRmWebAppBackupConfiguration cmdlet。 此 cmdlet 带多个参数︰

- **名称**的 web 应用程序的名称。
- **ResourceGroupName** -包含 web 应用程序的资源组的名称。
- **插槽**-可选。 Web 应用程序槽的名称。
- **StorageAccountUrl** -用于存储备份的 Azure 存储容器的 SAS URL。
- **FrequencyInterval** -数值应进行备份的频率。 必须是一个正整数。
- **FrequencyUnit** -频率应进行备份的时间单位。 选项为小时和天。
- **RetentionPeriodInDays** -在被自动删除之前应保存自动备份的天数。
- **开始时间**-可选。 自动备份开始时的时间。 如果此字段为 null，立即开始备份。 必须为日期时间。
- **数据库**-可选。 要备份的数据库 DatabaseBackupSettings 数组。
- **KeepAtLeastOneBackup** -可选切换参数。 如果提供一个备份应始终保持在存储帐户，无论其是多久。

下面是如何使用此 cmdlet 的示例。

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

若要获取当前备份时间表，使用 Get AzureRmWebAppBackupConfiguration cmdlet。 这可用于修改已配置的时间表。

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>从备份中还原一个 web 应用程序

若要从备份中还原一个 web 应用程序，使用还原 AzureRmWebAppBackup cmdlet。 若要使用此 cmdlet 的最简单方法是从 Get AzureRmWebAppBackup cmdlet 或获取 AzureRmWebAppBackupList cmdlet 检索备份对象中的管道。

备份对象之后，可以先将其输送到还原 AzureRmWebAppBackup cmdlet。 指定覆盖开关参数，以指示您要备份的内容覆盖您的 web 应用程序的内容。 如果备份中包含的数据库，以及还原这些数据库。

        $backup | Restore-AzureRmWebAppBackup -Overwrite

下面是如何使用还原 AzureRmWebAppBackup 通过指定所有参数的一个示例。

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>删除备份

要删除备份，请使用删除 AzureRmWebAppBackup cmdlet。 这将从您的存储帐户删除备份。 指定应用程序名称、 其资源组和您想要删除的备份 ID。

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

您还可以删除 AzureRmWebAppBackup cmdlet 将它删除到管道备份对象。

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
