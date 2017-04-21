<properties
    pageTitle="为您的 Azure 移动应用程序 (Android) 启用脱机同步"
    description="了解如何使用应用程序服务移动应用程序到 Android 应用程序中的缓存和同步脱机数据"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>启用您 Android 的移动应用程序的脱机同步

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>概述

本教程介绍了 Android 的 Azure 移动应用程序的脱机同步功能。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。 更改存储在本地数据库中。 设备重新联机后，这些更改的同步完成与远程的后端。

如果这是您对 Azure 移动应用程序的第一体验，您应首先完成本教程[创建一个 Android 的应用程序]。 如果不使用下载快速入门服务器项目，您必须向项目添加数据访问扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

若要了解有关脱机同步功能的详细信息，请参阅主题[在 Azure 移动应用程序的脱机数据同步]。

## <a name="update-the-app-to-support-offline-sync"></a>更新应用程序支持脱机同步

与脱机同步，则为从读取和写入*同步表*（使用*IMobileServiceSyncTable*接口），这是属于您的设备上的**SQLite**数据库。

要推送和拉入 Azure 移动服务与设备之间的更改，请使用*同步上下文*(*MobileServiceClient.SyncContext*)，它使用本地数据库来存储本地数据进行初始化。

1. 在`TodoActivity.java`，注释掉现有定义的`mToDoTable`和取消注释的同步表版本︰

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. 在`onCreate`方法中，注释掉现有的初始化`mToDoTable`和取消注释此定义︰

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. 在`refreshItemsFromTable`注释掉的定义`results`和取消注释此定义︰

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. 注释掉的定义`refreshItemsFromMobileServiceTable`。

5. 取消注释的定义`refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. 取消注释的定义`sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>测试应用程序

在本节中，您测试使用 WiFi 的行为，然后关闭 WiFi 来创建脱机的情况。

添加数据项时，它们被保存在本地的 SQLite 存储，但不是同步到移动服务，直到您按下的**刷新**按钮。 其他应用程序可能会有不同的要求，有关数据需要同步，但出于演示目的，本教程中有用户显式请求它。

当您按该按钮时，将启动新的后台任务。 首先，它将对使用同步上下文，然后将所有更改数据从 Azure 到本地表的本地存储区所做的所有更改。

### <a name="offline-testing"></a>离线测试

1. 在*飞机模式下*放置设备或模拟器。 这将创建脱机的情况。

2. 添加一些*ToDo*项，或标记为已完成的一些项目。 请退出设备或模拟器 （或强制关闭应用程序），然后重新启动。 验证是否所做的更改已被保存在设备上因为他们持有本地 SQLite 存储区中。

3. 查看与 SQL 工具如*SQL Server 管理 Studio*或其他客户端如*Fiddler*或*把邮递员弄*Azure *TodoItem*表的内容。 验证新项_尚未与服务器同步_

    + Node.js 后端，转到[Azure 的门户网站](https://portal.azure.com/)和移动应用程序中后端请单击**简单表** > **TodoItem**来查看内容的`TodoItem`表。
    + 查看.NET 后端，使用*SQL Server 管理 Studio*，例如一个 SQL 工具或其他客户端如*Fiddler*或*把邮递员弄*的表内容。

4. 打开 WiFi 设备或模拟器中。 接下来，请按**刷新**按钮。

5. 在 Azure 门户再次查看 TodoItem 的数据。 现在应该显示新的和更改 TodoItems。

## <a name="additional-resources"></a>其他资源

* [在 Azure 的移动应用程序的脱机数据同步]

* [云盖︰ Azure 的移动服务在脱机同步]\(注︰ 视频移动服务但脱机同步以类似的方式在 Azure 移动应用程序中的工作\)


<!-- URLs. -->

[在 Azure 的移动应用程序的脱机数据同步]: app-service-mobile-offline-data-sync.md

[创建一个 Android 的应用程序]: app-service-mobile-android-get-started.md

[在移动服务 Azure 云盖︰ 脱机同步]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

