<properties
    pageTitle="Azure 移动应用程序 (Xamarin.Forms) 为启用脱机同步 |Microsoft Azure"
    description="了解如何使用 Xamarin.Forms 应用程序中的缓存和同步脱机数据应用程序服务移动应用程序"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>启用脱机同步您的 Xamarin.Forms 移动应用程序

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>概述
本教程介绍 Xamarin.Forms Azure 移动应用程序的脱机同步功能。 脱机同步允许最终用户与移动应用程序，查看、 添加或修改数据--即使没有网络连接。 更改存储在本地数据库中。 设备重新联机后，这些更改的同步完成与远程服务。

本教程基于移动应用程序时完成本教程 [创建 Xamarin iOS 应用程序] 创建 Xamarin.Forms 快速入门解决方案。 Xamarin.Forms 的快速入门解决方案包含的代码来支持脱机同步，只需启用。 在本教程中，您可以更新快速入门办法开启 Azure 移动应用程序的脱机功能。 我们还突出显示应用程序中的脱机特定代码。 如果您不使用下载快速入门解决方案，必须向项目添加数据访问扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器 SDK Azure 移动应用程序的][1]。

若要了解有关脱机同步功能的详细信息，请参阅主题[在 Azure 移动应用程序的脱机数据同步][2]。

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>启用脱机同步快速入门解决方案中的功能

脱机同步代码包含在项目中使用 C# 的预处理器指令。 当**脱机\_同步\_启用**符号定义，这些代码路径包括在生成。 用于 Windows 的应用程序，您还必须安装 SQLite 平台。

1. 在 Visual Studio 中，右击解决方案 >**解决方案...管理 NuGet 程序包**，然后搜索并安装**Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet 程序包的解决方案中的所有项目。

2. 在解决方案资源管理器中，从与**便携式**名称，即可移植类库项目中的项目打开 TodoItemManager.cs 文件再取消注释下面的预处理器指令︰

        #define OFFLINE_SYNC_ENABLED

4. （可选）为了支持 Windows 设备，安装下面的 SQLite 运行时包之一︰

    * **Windows 8.1 运行时︰**安装[Windows 8.1 的 SQLite][3]。
    * **Windows Phone 8.1:**安装[Windows Phone 8.1 的 SQLite][4]。
    * **通用的 Windows 平台**安装[通用的 Windows 通用的 SQLite][5]。

    尽管快速入门不包含一个通用的窗口项目，Xamarin 窗体支持通用的 Windows 平台。

5. （可选）在每个 Windows 应用程序项目中，用鼠标右键单击**引用** > **添加引用...**，展开**Windows**文件夹 >**扩展**。
    启用相应**SQLite 的 Windows** SDK 以及**Visual C++ 2013年运行 Windows** SDK。
    每一种 Windows 平台，SQLite SDK 名称略有不同。

## <a name="review-the-client-sync-code"></a>检查客户端的同步代码

这里是教程中的代码中已包含的内容的简要概述`#if OFFLINE_SYNC_ENABLED`指令。 脱机同步功能是可移植类库项目中的 TodoItemManager.cs 项目文件中。 该功能的概念性概述，请参阅[脱机数据同步在 Azure 移动应用程序][2]。

* 表操作之前，必须先初始化本地存储区。 **TodoItemManager**类构造函数中初始化本地存储数据库时使用下面的代码︰

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    此代码将创建新的本地 SQLite 数据库使用**MobileServiceSQLiteStore**类。

    **DefineTable**方法与所提供的类型中的字段相匹配的本地存储区中创建一个表。  该类型不一定包括在远程数据库中的所有列。 很可能来存储列的一个子集。

* 在**TodoItemManager**的**todoTable**字段是**IMobileServiceSyncTable**类型而不是**IMobileServiceTable**。 此类使用所有的本地数据库的创建、 读取、 更新和删除 (CRUD) 表操作。 这些更改会被推送到移动应用程序的后端通过**IMobileServiceSyncContext**调用**PushAsync**时，您决定。 同步上下文有助于保留通过跟踪并推送更改调用**PushAsync**时，客户端应用程序已修改的所有表中的表关系。

    下面的**SyncAsync**方法调用与移动应用程序的后端的同步︰

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    此示例使用默认同步处理程序处理的简单错误。 实际的应用程序可以通过使用自定义的**IMobileServiceSyncHandler**实现处理网络条件和服务器冲突等的各种错误。

## <a name="offline-sync-considerations"></a>脱机同步的注意事项

在示例中，启动时和一个同步请求时才调用**SyncAsync**方法。  要启动 Android 或 iOS 应用程序中的同步，往下拉动的项目列表;对于 Windows，请使用**同步**按钮。 在实际应用中，您可以进行同步触发器的网络状态更改时。

已挂起的表执行请求时本地更新跟踪前面的上下文推送的拉操作自动触发器的上下文。 当刷新、 添加，并完成此示例中的项目时，您可以省略显式**PushAsync**调用。

中提供的代码中，远程 TodoItem 表中的所有记录进行都查询，但也可以通过将查询 id 和查询传递给**PushAsync**筛选记录。 有关详细信息，请参阅部分*增量同步*[脱机数据]同步在 Azure 移动应用程序[2]。

## <a name="run-the-client-app"></a>运行客户端应用程序

现在已启用了脱机同步，客户端应用程序至少运行一次来填充本地存储数据库的每个平台上。 以后，模拟脱机的情况下，应用程序处于脱机状态时修改本地存储区中的数据。

## <a name="update-the-sync-behavior-of-the-client-app"></a>更新的客户端应用程序的同步行为

在此部分中，修改客户端项目来模拟脱机的情况下，通过使用您的后端无效的应用程序的 URL。 或者，您可以关闭网络连接通过将您的设备移动到"飞机模式。  当添加或更改数据的项目时，这些更改保存在本地存储，但不是在重新建立连接之前同步到后端数据存储区。

1. 在解决方案资源管理器中，从**可移植**项目打开 Constants.cs 项目文件并更改值的`ApplicationURL`指向无效的 URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. 从**可移植**项目，打开 TodoItemManager.cs 文件，然后在**SyncAsync**中的**try...catch**块添加**捕获**的**异常**的基类。 此**catch**块异常消息写入控制台中，按如下所述︰

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. 生成并运行客户端应用程序。  添加一些新的项目。 请注意的异常记录在每次尝试同步与后端的控制台。 直到他们可以推送到移动的后端，仅在本地存储区中存在这些新项目。 客户端应用程序的行为就像它连接到后端，支持所有创建，读取，更新，删除 (CRUD) 操作。

4. 关闭应用程序并重新启动它以验证您所创建的新项目将保存到本地存储区。

5. （可选）使用 Visual Studio 可以查看 SQL Azure 数据库表格，查看后端数据库中的数据未发生更改。

    在 Visual Studio 中，打开**服务器资源管理器**。 导航到您的数据库在**Azure**->**SQL 数据库**。 右击数据库并选择**SQL Server 对象资源管理器中打开**。 现在您可以浏览 SQL 数据库表及其内容。

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>更新的客户端应用程序重新连接您移动的后端

在这一部分，重新连接到模拟回来到联机状态的应用程序的移动后端应用程序。 执行的刷新笔势时，数据同步到您的移动后端。

1. 重新打开 Constants.cs。 更正`applicationURL`以指向正确的 URL。

2. 重新生成并运行客户端应用程序。 应用程序尝试启动后与移动应用程序的后端同步。 请确认没有异常在调试控制台登录。

3. （可选）查看已更新的数据使用 SQL Server 对象资源管理器或其他工具如 Fiddler 或[把邮递员弄][6]。 注意到已在后端数据库和本地存储之间同步数据。

    注意数据库和本地存储之间同步数据时被中断您的应用程序添加的项。

## <a name="additional-resources"></a>其他资源

* [在 Azure 的移动应用程序的脱机数据同步][2]
* [Azure 的移动应用程序.NET SDK 如何][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md