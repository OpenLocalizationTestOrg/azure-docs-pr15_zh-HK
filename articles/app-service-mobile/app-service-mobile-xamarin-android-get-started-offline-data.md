<properties
    pageTitle="为您的 Azure 移动应用程序 (Xamarin Android) 启用脱机同步"
    description="了解如何使用 Xamarin Android 应用程序中的缓存和同步脱机数据应用程序服务移动应用程序"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>启用脱机同步您的 Xamarin.Android 移动应用程序

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>概述

本教程介绍 Xamarin.Android Azure 移动应用程序的脱机同步功能。 脱机同步允许最终用户与移动应用程序，查看、 添加或修改数据--即使没有网络连接。 更改存储在本地数据库中。
设备重新联机后，这些更改的同步完成与远程服务。

在本教程中，您将更新从教程[创建一个 Xamarin Android 应用程序]支持脱机功能的 Azure 移动应用程序客户端项目。 如果不使用下载快速入门服务器项目，您必须向项目添加数据访问扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

若要了解有关脱机同步功能的详细信息，请参阅主题[在 Azure 移动应用程序的脱机数据同步]。

## <a name="update-the-client-app-to-support-offline-features"></a>更新客户端应用程序支持脱机功能

Azure 的移动应用程序脱机功能允许您与本地数据库进行交互，当您在脱机的情况下。 若要在您的应用程序中使用这些功能，您可以初始化到本地存储区[SyncContext] 。 然后，通过 [IMobileServiceSyncTable] [IMobileServiceSyncTable] 界面表引用。 SQLite 用作本地存储在设备上。

1. 在 Visual Studio，打开 NuGet 程序包管理器[创建一个 Xamarin Android 应用程序]教程中已完成的项目中。  搜索并安装**Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet 程序包。

2. 打开 ToDoActivity.cs 文件，请取消注释`#define OFFLINE_SYNC_ENABLED`定义。

3. 在 Visual Studio 中，请按**F5**键以重新生成并运行客户端应用程序。 应用程序的工作方式相同启用脱机同步之前一样。 不过，现在可以在脱机情况下使用的数据填充本地数据库。

## <a name="update-sync"></a>更新应用程序断开连接后端

在本节中，您向您的移动应用程序后端来模拟脱机的情况下断开的连接。 当您添加数据项时，则异常处理程序将告诉您应用程序处于脱机模式。 在此状态下，新项目添加到本地存储区中，并强制执行处于连接状态时同步到移动应用程序的后端。

1. 编辑共享项目中的 ToDoActivity.cs。 更改**applicationURL**指向无效的 URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    您可以通过禁用 wifi 和蜂窝移动通信网络设备或使用飞机模式来演示脱机行为。

2. 按**f5 键**以生成并运行应用程序。 请注意您在应用程序启动时刷新失败的同步。

3. 输入新条目，请注意，推入失败 [CancelledByNetworkError] 状态每次单击**保存**。 但是，新的 todo 项存在本地存储区中，直到它们可以推送到移动应用程序的后端。  在生产应用程序中，如果这些异常，则禁止显示客户端应用程序行为与它仍然连接到移动应用程序的后端。

4. 关闭应用程序并重新启动它以验证您所创建的新项目将保存到本地存储区。

5. （可选）在 Visual Studio 中，打开**服务器资源管理器**。 导航到您的数据库在**Azure**->**SQL 数据库**。 右击数据库并选择**SQL Server 对象资源管理器中打开**。 现在您可以浏览 SQL 数据库表及其内容。 验证后端数据库中的数据未发生更改。

6. （可选）使用其他工具，如 Fiddler 或把邮递员弄来查询您移动的后端，在窗体中使用 GET 查询`https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`。

## <a name="update-online-app"></a>更新应用程序重新连接您的移动应用程序后端

在这一部分，重新连接到移动应用程序的后端应用程序。 当您首次运行应用程序时，`OnCreate`事件处理程序调用`OnRefreshItemsSelected`。 此方法调用`SyncAsync`同步您的本地存储后端数据库。

1. 打开共享项目中，ToDoActivity.cs 和恢复的**applicationURL**属性所做的更改。

2. 按**F5**键以重新生成并运行该应用程序。 应用程序同步本地更改与 Azure 的移动应用程序的后端使用推送和请求操作时`OnRefreshItemsSelected`方法执行。

3. （可选）查看已更新的数据使用 SQL Server 对象资源管理器或类似 Fiddler 其余工具。 请注意数据已 Azure 的移动应用程序的后端数据库和本地存储之间同步。

4. 在应用程序中，单击以完成它们在本地存储区中的几个项目旁边的复选框。

  `CheckItem`调用`SyncAsync`与移动应用程序的后端的每个同步已完成项目。 `SyncAsync`调用推 / 拉。 **每当执行对客户端已更改、 表拉推始终会自动执行**。 这样可以确保关系以及本地存储区中的所有表都保持一致。 这种现象可能会导致意外的推。 这种行为的详细信息，请参阅[脱机数据同步在 Azure 移动应用程序]。

## <a name="review-the-client-sync-code"></a>检查客户端的同步代码

当您已完成本教程[创建一个 Xamarin Android 应用程序]下载的 Xamarin 客户端项目包含支持使用一个本地的 SQLite 数据库的脱机同步的代码。 下面是教程 c 代码中已包含的内容的简要概述。 该功能的概念性概述，请参阅[脱机数据同步在 Azure 移动应用程序]。

* 表操作之前，必须先初始化本地存储区。 本地存储数据库初始化时`ToDoActivity.OnCreate()`执行`ToDoActivity.InitLocalStoreAsync()`。 此方法创建一个本地的 SQLite 数据库使用`MobileServiceSQLiteStore`Azure 移动应用程序客户端 SDK 提供的类。

    `DefineTable`方法与所提供的类型中的字段相匹配的本地存储区中创建一个表`ToDoItem`在这种情况下。 该类型不一定包括在远程数据库中的所有列。 很可能来存储列的子集。

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* `toDoTable`的成员`ToDoActivity`的`IMobileServiceSyncTable`而不是键入`IMobileServiceTable`。 IMobileServiceSyncTable 指导所有创建、 读取、 更新和删除 (CRUD) 到本地存储数据库的表操作。

    您决定何时更改会被推送到 Azure 的移动应用程序的后端通过调用`IMobileServiceSyncContext.PushAsync()`。 同步上下文有助于跟踪和推送客户端应用程序已修改时的所有表中的更改保留表关系`PushAsync`调用。

    提供的代码调用`ToDoActivity.SyncAsync()`每当刷新 todoitem 列表 todoitem 已添加或已完成的同步。 在每个本地更改后的代码同步。

    在提供的代码中，所有记录在远程`TodoItem`查询表，但它也是可以通过查询 id 筛选记录和查询为`PushAsync`。 有关详细信息，请参见*增量同步*[脱机数据同步在 Azure 移动应用程序]中的部分。

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>其他资源

* [在 Azure 的移动应用程序的脱机数据同步]
* [Azure 的移动应用程序.NET SDK 如何][8]

<!-- URLs. -->
[创建一个 Xamarin Android 应用程序]: ../app-service-mobile-xamarin-android-get-started.md
[在 Azure 的移动应用程序的脱机数据同步]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[创建一个 Xamarin Android 应用程序]: app-service-mobile-xamarin-android-get-started.md
[在 Azure 的移动应用程序的脱机数据同步]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
