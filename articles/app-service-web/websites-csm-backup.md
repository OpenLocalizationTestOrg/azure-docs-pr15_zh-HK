<properties
    pageTitle="其余部分用于备份和还原应用程序服务的应用程序"
    description="了解如何使用 rest 风格的 API 调用来备份和还原在 Azure 应用程序服务的应用程序"
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
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>其余部分用于备份和还原应用程序服务的应用程序

> [AZURE.SELECTOR]
- [PowerShell](../app-service/app-service-powershell-backup.md)
- [REST API，](websites-csm-backup.md)

[应用程序服务的应用程序](https://azure.microsoft.com/services/app-service/web/)可以在 Azure 存储中的 blob 作为备份。 备份还可以包含应用程序的数据库。 如果曾经意外地删除应用程序，或者如果应用程序需要恢复到以前的版本，它可以从任何以前的备份还原它。 根据需要，随时可备份或备份安排在合适的时间间隔。

本文介绍如何备份和还原的 rest 风格的 API 请求的应用程序。 如果您想要创建和管理以图形方式通过 Azure 的门户应用程序备份，请参阅[备份在 Azure 应用程序服务 web 应用程序](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>入门教程
要发送其余请求，您需要知道您的应用程序**名称**、**资源组**和**订阅 id**。 可以通过单击您的应用程序的[Azure 的门户](https://portal.azure.com)**应用程序服务**刀片式服务器中找到此信息。 本文中的示例，我们将配置网站**backuprestoreapiexamples.azurewebsites.net**。 它将存储在默认的 Web WestUS 资源组并订阅与 ID 00001111-2222年-3333-4444-555566667777 上运行。

![示例网站信息][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>备份和还原 REST API，
我们现在将介绍如何使用 REST API 来备份和还原应用程序的几个示例。 每个示例包含 URL 和 HTTP 请求正文。 该示例 URL 包含包装在大括号内，例如，{订阅 id} 占位符。 这些占位符替换为您的应用程序的相应信息。 为了便于参考，下面是示例 Url 中显示的每个占位符的说明。

* 订购 id – Azure 订阅包含应用程序 ID
* 资源组的名称 – 包含应用程序的资源组的名称
* 名称 – 应用程序的名称
* 备份 id – 应用程序备份 ID

有关完整文档的 api，其中包括一些可选参数，可以在 HTTP 请求中，包括[Azure 资源管理器](https://resources.azure.com/)。

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>备份应用程序即需即装
若要立即备份应用程序，发送**POST**请求到**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**。

这里是 URL 如下所示使用我们的示例网站。 **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backup/**

提供一个 JSON 对象，您可以指定要用于存储备份的存储帐户的请求的正文中。 JSON 对象必须具有一个名为**storageAccountUrl**，其中包含[SAS URL](../storage/storage-dotnet-shared-access-signature-part-1.md)授予写访问权限保存备份的 blob 的 Azure 存储容器属性。 如果要备份您的数据库，您还必须提供一个包含名称、 类型和要备份的数据库的连接字符串的列表。

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

收到请求后立即开始应用程序的备份。 备份过程可能需要很长时间才能完成。 HTTP 响应包含一个 ID，您可以使用在另一个请求，以查看备份的状态。 还有一种我们备份请求的 HTTP 响应的正文。

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] 在 HTTP 响应日志属性中找不到错误消息。

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>计划自动备份
除了按需备份应用程序，还可以安排自动执行备份。

### <a name="set-up-a-new-automatic-backup-schedule"></a>设置自动备份计划
若要设置备份时间表，请求**放**到**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**。

这里是我们的示例网站 URL 的样子。 **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup**

请求正文必须指定备份配置一个 JSON 对象。 下面是一个示例使用所有必需的参数。

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

本示例配置应用程序来自动备份每隔七天。 参数**frequencyInterval**和**frequencyUnit**一起确定备份的发生频率。 对于**frequencyUnit**的有效值为**小时**和**天**。 例如，要备份应用程序每隔 12 小时，将 frequencyInterval 设置为 12 和 frequencyUnit 小时。

旧的备份自动从存储帐户。 您可以控制如何旧备份可能会通过将**retentionPeriodInDays**参数设置。 如果您希望始终必须至少一个备份保存，无论多久，设置**keepAtLeastOneBackup**为 true。

### <a name="get-the-automatic-backup-schedule"></a>获取自动备份计划
若要获取应用程序的备份配置，发送**POST**请求的 url **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**。

对于我们的示例站点的 URL 是**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**。

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>获取备份的状态
根据多大的应用程序是，备份可能需要一段时间才能完成。 备份可能也会失败，超时，或部分成功。 要查看应用程序的所有备份的状态，将**GET**请求发送到该 URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**。

若要查看特定备份的状态，请向 URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**发送 GET 请求。

这里是我们的示例网站 URL 的样子。 **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

响应正文中包含类似于以下示例的 JSON 对象。

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

备份的状态是枚举的类型。 下面是每个可能的状态。

* 0 – 正在进行︰ 备份已启动但尚未完成。
* 1-失败︰ 备份不成功。
* 2 – 成功︰ 已成功完成备份。
* 3 – 超时︰ 备份未完成的时间，已被取消。
* 4 – 创建︰ 备份请求排入队列，但尚未启动。
* 5 – 跳过︰ 备份没有继续进行，由于触发太多的备份计划。
* 6-PartiallySucceeded︰ 备份成功，但是一些文件没有备份，因为无法读取它们。 这通常是因为排它锁放置在文件上。
* 7-DeleteInProgress︰ 备份已请求被删除但尚未删除。
* 8-DeleteFailed︰ 无法删除备份。 因为 SA URL 用于创建备份已过期，则可能发生此错误。
* 9 – 删除︰ 已成功删除备份。

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>从备份中还原应用程序
如果您的应用程序已被删除，或者如果您想要恢复到以前的版本应用程序，您可以从备份中还原应用程序。 若要调用还原，发送**POST**请求的 url **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**。

这里是我们的示例网站 URL 的样子。 **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/restore**

在请求正文中，发送一个 JSON 对象，包含用于还原操作的属性。 下面是一个包含所有必需的属性的示例︰

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>恢复到新应用程序
有时您可能想要还原的备份，而不是覆盖现有的应用程序时创建新的应用程序。 若要执行此操作，更改请求 URL 指向要创建新的应用程序，并将 json 格式的**覆盖**属性更改为**false**。

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>删除应用程序的备份
如果您想要删除的备份，请向 URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**发送**删除**请求。

这里是我们的示例网站 URL 的样子。 **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>管理备份的 SAS URL
Azure 应用程序服务将尝试从使用 SAS URL 创建备份时提供的 Azure 存储中删除您的备份。 如果此 SAS URL 不再是有效的无法通过 REST API，删除备份。 但是，您可以更新与备份相关联的 url 发送**POST**请求 SAS URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**。

这里是我们的示例网站 URL 的样子。 **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/list**

在请求正文中，发送一个 JSON 对象，包含新的 SAS URL。 下面是一个示例。

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] 出于安全考虑，对于特定的备份发送 GET 请求时不返回与备份相关联的 SAS URL。 如果您想要查看与备份相关联的 SAS URL，与上面相同的 URL 发送 POST 请求。 请求正文中包含一个空的 JSON 对象。 来自服务器的响应包含该备份的信息，包括其 SAS URL 的所有。

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
