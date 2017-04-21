<properties
    pageTitle="启用脱机同步您与移动应用程序的通用 Windows 平台 (UWP) 应用程序 |Azure 应用程序服务"
    description="了解如何在您通用 Windows 平台 (UWP) 的应用程序中使用 Azure 移动到缓存和同步脱机数据应用程序。"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>使您的 Windows 应用程序的脱机同步

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>概述

本教程展示如何将离线支持添加到使用 Azure 的移动应用程序的后端一个通用 Windows 平台 (UWP) 应用程序。 脱机同步允许最终用户与移动应用程序，查看、 添加或修改数据-即使没有网络连接。 更改存储在本地数据库中。 设备重新联机后，这些更改的同步完成与远程的后端。

在本教程中，您可以更新 UWP 应用程序项目，从教程[创建一个 Windows 应用程序]来支持 Azure 移动应用程序的脱机功能。 如果不使用下载快速入门服务器项目，您必须向项目添加数据访问扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

若要了解有关脱机同步功能的详细信息，请参阅主题[在 Azure 移动应用程序的脱机数据同步]。

## <a name="requirements"></a>要求

本教程需要以下先决条件︰

* Visual Studio 2013 年运行 Windows 8.1 或更高版本。
* [创建一个 Windows 应用程序][创建一个 windows 应用程序]完成。
* [SQLite azure 移动服务的存储区][sqlite store nuget]
* [SQLite 通用 Windows 平台开发](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>更新客户端应用程序支持脱机功能

Azure 的移动应用程序脱机功能允许您与本地数据库进行交互，当您在脱机的情况下。 若要在您的应用程序中使用这些功能，您可以初始化[SyncContext] [synccontext]到本地存储区。 然后，您通过[IMobileServiceSyncTable][IMobileServiceSyncTable]接口的表引用。 SQLite 用作本地存储在设备上。

1. 安装[通用的 Windows 平台运行的 SQLite 时](http://sqlite.org/2016/sqlite-uwp-3120200.vsix)。

2. 在 Visual Studio，打开 NuGet 程序包管理器 UWP 的应用程序项目[创建一个 Windows 应用程序]教程中完成的。
    搜索并安装**Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet 程序包。

4. 在解决方案资源管理器中，用鼠标右键单击**引用** > **添加引用...** > **通用 Windows** > **扩展**，然后启用**通用 Windows 平台的 SQLite**和**Visual C++ 2015年运行时用于通用 Windows 平台的应用程序**。

    ![添加 SQLite UWP 引用][1]

5. 打开 MainPage.xaml.cs 文件，请取消注释`#define OFFLINE_SYNC_ENABLED`定义。

6. 在 Visual Studio 中，请按**F5**键以重新生成并运行客户端应用程序。 应用程序的工作方式相同启用脱机同步之前一样。 不过，现在可以在脱机情况下使用的数据填充本地数据库。

## <a name="update-sync"></a>更新应用程序断开连接后端

在本节中，您向您的移动应用程序后端来模拟脱机的情况下断开的连接。 当您添加数据项时，则异常处理程序将告诉您应用程序处于脱机模式。 在此状态下，新项目添加到本地存储区中，将同步到移动应用程序的后端，推下一次运行处于连接状态时。

1. 编辑共享项目中的 App.xaml.cs。 注释掉**MobileServiceClient**的初始化，然后添加下面的行，它使用一个无效的移动应用程序的 URL:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    此外可以通过禁用 wifi 和蜂窝移动通信网络设备上的演示脱机行为或使用飞机模式。

2. 按**f5 键**以生成并运行应用程序。 请注意您在应用程序启动时刷新失败的同步。

3. 输入新条目，请注意，推入将失败，并[CancelledByNetworkError]状态每次单击**保存**。 但是，新的 todo 项存在本地存储区中，直到它们可以推送到移动应用程序的后端。  在生产应用程序中，如果这些异常，则禁止显示客户端应用程序行为与它仍然连接到移动应用程序的后端。

4. 关闭应用程序并重新启动它以验证您所创建的新项目将保存到本地存储区。

5. （可选）在 Visual Studio 中，打开**服务器资源管理器**。 导航到您的数据库在**Azure**->**SQL 数据库**。 右击数据库并选择**SQL Server 对象资源管理器中打开**。 现在您可以浏览 SQL 数据库表及其内容。 验证后端数据库中的数据未发生更改。

6. （可选）使用其他工具，如 Fiddler 或把邮递员弄来查询您移动的后端，在窗体中使用 GET 查询`https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`。

## <a name="update-online-app"></a>更新应用程序重新连接您的移动应用程序后端

在本节中，您重新连接到移动应用程序的后端应用程序。 这些更改模拟应用程序的网络重新连接。

当您首次运行应用程序时，`OnNavigatedTo`事件处理程序调用`InitLocalStoreAsync`。 此方法进而调用`SyncAsync`同步您的本地存储后端数据库。 应用程序尝试启动时同步。

1. 打开共享项目中，App.xaml.cs 和取消注释您上次初始化`MobileServiceClient`使用的正确移动应用程序的 URL。

2. 按**F5**键以重新生成并运行该应用程序。 应用程序同步本地更改与 Azure 的移动应用程序的后端使用推送和请求操作时`OnNavigatedTo`执行事件处理程序。

3. （可选）查看已更新的数据使用 SQL Server 对象资源管理器或类似 Fiddler 其余工具。 请注意数据已 Azure 的移动应用程序的后端数据库和本地存储之间同步。

4. 在应用程序中，单击以完成它们在本地存储区中的几个项目旁边的复选框。

  `UpdateCheckedTodoItem`调用`SyncAsync`与移动应用程序的后端的每个同步已完成项目。 `SyncAsync`调用推 / 拉。 但是，**只要执行对客户端已更改、 表拉推始终会自动执行**。 此行为可确保本地存储和关系中的所有表都保持一致。 这种现象可能会导致意外的推。  这种行为的详细信息，请参阅[脱机数据同步在 Azure 移动应用程序]。


##<a name="api-summary"></a>API 摘要

若要支持移动服务的脱机功能，我们可以使用[IMobileServiceSyncTable]接口并初始化[MobileServiceClient.SyncContext] [synccontext]与本地的 SQLite 数据库。 在脱机状态下，普通的 CRUD 操作，移动应用程序的工作应用程序对本地存储区执行操作时仍处于连接状态。 以下方法用于与服务器同步的本地存储区︰

*  **[PushAsync]**由于此方法是[IMobileServicesSyncContext]的成员，在所有表的更改会被推送到后端。 仅包含本地更改的记录发送到服务器中。

* **[PullAsync] ** 
   [IMobileServiceSyncTable]从开始拉。 当表中的修订，隐式强制运行，以确保本地存储和关系中的所有表都保持一致。 *PushOtherTables*参数控制是否在上下文中的其他表已被按入隐式推。 *查询*参数采用[IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
  或 OData 查询字符串，可以筛选返回的数据。 *QueryId*参数用于定义增量同步。 有关详细信息，请参阅 [脱机数据同步在 Azure 移动应用程序](app-service-mobile-offline-data-sync.md#how-sync-works)。

* **[PurgeAsync]**您的应用程序应定期调用此方法以清除陈旧数据从本地存储区。 如果需要清除尚未同步的任何更改，请使用*强制*参数。

有关这些概念的详细信息，请参阅[脱机数据同步在 Azure 移动应用程序](app-service-mobile-offline-data-sync.md#how-sync-works)。

## <a name="more-info"></a>更多的信息

以下主题提供有关脱机同步功能的移动应用程序的其他背景信息︰

* [在 Azure 的移动应用程序的脱机数据同步]
* [Azure 的移动应用程序.NET SDK 如何][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[在 Azure 的移动应用程序的脱机数据同步]: app-service-mobile-offline-data-sync.md
[创建一个 windows 应用程序]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
