<properties
    pageTitle="为您的 Azure 移动应用程序 (Cordova) 启用脱机同步 |Microsoft Azure"
    description="了解如何使用 Cordova 应用程序中的缓存和同步脱机数据应用程序服务移动应用程序"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>启用脱机同步 Cordova 移动应用程序

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

本教程介绍 Cordova Azure 移动应用程序的脱机同步功能。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。 更改存储在本地数据库中;设备重新联机后，这些更改的同步完成与远程服务。

本教程基于移动应用程序时完成本教程[Apache Cordova 快速开始]创建 Cordova 快速入门解决方案。 在本教程中，您将更新快速入门解决方案添加脱机功能的 Azure 移动应用程序。 我们还将突出显示应用程序中脱机特定的代码。

若要了解有关脱机同步功能的详细信息，请参阅主题[在 Azure 移动应用程序的脱机数据同步]。 API 用法的详细信息，请参阅插件中的[自述]文件。

## <a name="add-offline-sync-to-the-quickstart-solution"></a>添加到快速启动解决方案脱机同步

必须将脱机同步代码添加到应用程序。 脱机同步要求 cordova sqlite 存储插件，获取时自动添加到您的应用程序的 Azure 移动应用程序插件包含在项目中。 快速入门项目包括两个这些插件。

1. 在 Visual Studio 解决方案资源管理器中打开 index.js，然后将下面的代码

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    使用此代码︰

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. 接下来，将下面的代码︰

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    使用此代码︰

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    前面的代码添加初始化本地存储并定义匹配的列的值使用的本地表中您 Azure 后端。 （您不需要在此代码中包含的所有列值。）

    您可以通过调用**getSyncContext**获取同步上下文的引用。 同步上下文有助于跟踪和推送客户端应用程序调用时**推入**已修改的所有表中的更改保留表关系。

3. 向您的移动应用程序应用程序 URL 更新应用程序 URL。

4. 接下来，将这段代码︰

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    使用此代码︰

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    前面的代码中初始化同步上下文，然后调用 getSyncTable （而不是可获取） 来获取本地表的引用。

    此代码使用所有的本地数据库的创建、 读取、 更新和删除 (CRUD) 表操作。

    本示例中执行简单的错误处理的同步冲突。 实际的应用程序将处理的各种错误，如网络条件、 服务器冲突和其他人。 有关代码示例，请参阅[脱机同步的示例]。

5. 接下来，添加此函数执行实际的同步。

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    您决定何时将更改推送到移动应用程序的后端，通过客户端使用**syncContext**对象调用**推**。 例如，您可以在应用程序中，例如新的同步按钮，按钮事件处理程序添加对**syncBackend**的调用也可以调用**addItemHandler**函数进行同步时将新项添加。

##<a name="offline-sync-considerations"></a>脱机同步的注意事项

在示例中， **syncContext**的**push**方法只能登录的回调函数在应用程序启动时调用。  在实际应用中，还可以让网络状态更改时或手动触发此同步功能。

当拉已挂起的本地更新跟踪的表执行的上下文时，该抽取操作将自动触发前面的上下文推送。 当刷新，添加并完成项目在此示例中，可以省略显式**强制**调用，因为它可能是冗余 （第一个请查看[自述文件]以了解当前功能状态）。

中提供的代码中，todoItem 远程表中的所有记录进行都查询，但也可以通过将查询 id 和查询传递给**push**筛选记录。 有关详细信息，请参见*增量同步*[脱机数据同步在 Azure 移动应用程序]中的部分。

## <a name="optional-disable-authentication"></a>（可选）禁用身份验证

如果您没有已将设置身份验证并不想设置身份验证之前测试脱机同步、 注释掉回调函数对于登录，但让内部代码取消注释的回调函数。

代码应如下所示注释掉登录行之后。

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

现在，应用程序将与同步 Azure 的后端运行的应用程序，而不是当您登录时。

## <a name="run-the-client-app"></a>运行客户端应用程序

现在已启用了脱机同步，您现在可以运行客户端应用程序至少一次来填充本地存储数据库的每个平台上。 以后，将模拟脱机的情况下也可以修改本地存储中的数据，该应用程序处于脱机状态时。

## <a name="optional-test-the-sync-behavior"></a>（可选）测试同步行为

在本节中，您将修改客户端项目来模拟脱机的情况下，通过使用您的后端无效的应用程序的 URL。 当添加或更改数据的项目时，这些更改将保存在本地存储，但不是同步到后端数据存储区，直到重新建立连接时。

1. 在解决方案资源管理器中，打开 index.js 项目文件和应用程序 URL 更改为指向无效的 URL，如下所示︰

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. 在 index.html，更新 CSP`<meta>`元素具有相同的 URL 无效。

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. 生成并运行客户端应用程序，并注意当尝试登录后同步与后端应用程序在控制台中记录一个异常。 所添加的任何新项目将只在本地存储区中存在，直到他们可以推送到移动的后端。 客户端应用程序的行为就象连接到后端，支持所有创建，读取，更新，删除 (CRUD) 操作。

4. 关闭应用程序并重新启动它以验证您所创建的新项目将保存到本地存储区。

5. （可选）使用 Visual Studio 可以查看 SQL Azure 数据库表格，查看后端数据库中的数据未发生更改。

    在 Visual Studio 中，打开**服务器资源管理器**。 导航到您的数据库在**Azure**->**SQL 数据库**。 右击数据库并选择**SQL Server 对象资源管理器中打开**。 现在您可以浏览 SQL 数据库表及其内容。


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>（可选）测试到您移动的后端重新连接

在本节中将重新连接到模拟回来到联机状态的应用程序的移动后端应用程序。 当您登录后时，将向您的移动后端同步数据。

1. 重新打开 index.js 和更正应用程序 URL 指向正确的 URL。

2. 重新打开 index.html 并更正应用程序 URL 在 CSP 中`<meta>`元素。

3. 重新生成并运行客户端应用程序。 应用程序尝试登录后同步与移动应用程序的后端。 请确认没有异常在调试控制台登录。

4. （可选）查看已更新的数据使用 SQL Server 对象资源管理器或类似 Fiddler 其余工具。 注意到已在后端数据库和本地存储之间同步数据。

    注意数据库和本地存储之间同步数据时被中断您的应用程序添加的项。

## <a name="additional-resources"></a>其他资源

* [在 Azure 的移动应用程序的脱机数据同步]

* [云盖︰ Azure 的移动服务在脱机同步]\(注︰ 视频移动服务但脱机同步以类似的方式在 Azure 移动应用程序中的工作\)

* [Apache Cordova visual Studio 工具]

## <a name="next-steps"></a>下一步行动

* 看一看更高级的脱机同步功能，例如[脱机同步示例]中的冲突解决方法
* 查看脱机同步 API 参考中[自述]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova 快速入门]: app-service-mobile-cordova-get-started.md
[自述文件]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[脱机同步示例]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[在 Azure 的移动应用程序的脱机数据同步]: app-service-mobile-offline-data-sync.md
[在移动服务 Azure 云盖︰ 脱机同步]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Apache Cordova visual Studio 工具]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
