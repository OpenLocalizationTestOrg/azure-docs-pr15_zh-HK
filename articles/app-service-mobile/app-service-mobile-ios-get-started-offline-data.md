<properties
    pageTitle="为您的 Azure 移动应用程序 (iOS) 启用脱机同步"
    description="了解如何使用 iOS 应用程序中的缓存和同步脱机数据应用程序服务移动应用程序"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>启用 iOS 移动应用程序的脱机同步

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>概述

本教程介绍了 iOS Azure 移动应用程序的脱机同步功能。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。 更改存储在本地数据库中;设备重新联机后，这些更改的同步完成与远程的后端。

如果这是您对 Azure 移动应用程序的第一体验，您应首先完成本教程[创建的 iOS 应用程序]。 如果不使用下载快速入门服务器项目，您必须向项目添加数据访问扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

若要了解有关脱机同步功能的详细信息，请参阅主题[在 Azure 移动应用程序的脱机数据同步]。

## <a name="review-sync"></a>检查客户端的同步代码

您已经下载为教程[创建的 iOS 应用程序]客户端项目包含支持使用基于核心数据的本地数据库的脱机同步的代码。 这一节是教程的代码中已包含的内容的摘要。 该功能的概念性概述，请参阅[脱机数据同步在 Azure 移动应用程序]。

Azure 的移动应用程序的脱机数据同步同步功能允许最终用户无法访问网络时与本地数据库进行交互。 若要在您的应用程序中使用这些功能，您可以初始化同步上下文的`MSClient`和引用的本地存储区。 然后，引用表通过`MSSyncTable`接口。

1. 在**QSTodoService.m** (目标 C) 或**ToDoTableViewController.swift** (Swift)，注意到该类型的成员`syncTable`是`MSSyncTable`。 脱机同步使用此同步表接口而不是`MSTable`。 使用同步表时，所有操作转到本地存储，并只与显式的推送和请求操作远程端与同步。

    若要获取对同步表的引用，请使用`syncTableWithName`在`MSClient`。 若要删除脱机同步功能，请使用`tableWithName`相反。

2. 表操作之前，必须先初始化本地存储区。 以下是相关代码。 
    
    **目标 C**:
    
    在`QSTodoService.init`方法︰
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    在`ToDoTableViewController.viewDidLoad`方法︰
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    这将创建使用接口的本地存储区`MSCoreDataStore`，这移动应用程序 SDK 中提供。 您可以改为提供不同的本地存储区通过实施`MSSyncContextDataSource`协议。 
    
    此外，第一个参数的`MSSyncContext`用于指定的冲突处理。 由于我们已通过`nil`，我们将会默认冲突处理程序，在任何冲突无法正常工作。
    
3. 现在，我们来执行实际的同步操作，并从远程后端获取数据。

    **目标 C**:
    
    `syncData`第一次将新的更改，然后调用`pullData`从远程后端获取数据。 在打开该方法`pullData`获取与查询匹配的新数据︰
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    在 OBJECTIVE-C 版本中，在`syncData`，我们首先调用`pushWithCompletion`上的同步上下文。 此方法是的成员`MSSyncContext`（而不是在同步表本身） 因为它将会使跨所有表的更改。 将会以某种方式 （通过 CUD 操作） 本地已修改的记录发送到服务器。 然后帮助器`pullData`被称为哪些调用`MSSyncTable.pullWithQuery`检索远程数据并存储到本地数据库中。
    
    在 Swift 的版本中，则不需要调用`pushWithCompletion`。 这是因为推送操作不是必需的。 如果有任何挂起的更改同步上下文推送操作正在执行的操作的表中，拉总是问题推第一次。 但是，如果您有多个同步表，则最好显式调用以确保一切是一致在相关表之间强制。
    
    在 OBJECTIVE-C 和 Swift 版本中，此方法`pullWithQuery`允许您指定一个查询来筛选记录您要检索。 在此示例中，查询只检索所有记录在远程`TodoItem`表。
    
    第二个参数`pullWithQuery`用于*增量同步*的查询 id。 增量同步检索仅修改自从上次同步，使用记录的那些记录`UpdatedAt`时间戳 (名为`updatedAt`的本地存储区中。)查询 ID 应该是唯一的应用程序中每个逻辑查询的描述性字符串。 若要退出的增量同步，传递`nil`作为查询 id。 请注意，这可能会有可能效率低下，因为它将检索所有记录在每个请求操作。

5. 当我们修改或添加数据目标 C 应用程序同步，用户执行刷新势，并在启动。 反应迅速的应用程序同步用户执行的刷新笔势和启动。 

因为应用程序同步数据时修改 (目标 C) 或应用程序启动 (目标 C & Swift) 时，应用程序会假定用户处于联机状态。 在另一节中，我们将更新应用程序，以便用户可以编辑甚至在脱机时。

## <a name="review-core-data"></a>检查的核心数据模型

当使用的核心数据脱机存储区，您需要在数据模型中定义特定的表和字段。 示例应用程序中已包含具有正确格式的数据模型。 在本节中我们将遍历这些表以及如何使用它们。

- 打开**QSDataModel.xcdatamodeld**。 有四个表定义-使用 sdk，三个，一个表对应 todo 项本身︰     *MS_TableOperations︰ 用于跟踪需要与服务器进行同步的项目    *MS_TableOperationErrors︰ 用于跟踪脱机同步过程中所发生的任何错误    *MS_TableConfig︰ 用于跟踪最后一次更新所有抽取操作的最后一次同步操作的时间    *TodoItem︰ 用于存储 todo 项。 系统列**createdAt**、 **updatedAt**和**版本**是可选的系统属性。

>[AZURE.NOTE] Azure 移动应用程序 SDK 保留列名称与该所"**``**"。 您不应使用此前缀系统列之外的任何字段，否则将使用远程端时修改列名称。

- 使用脱机同步功能时，您必须定义系统表，如下所示。

    ### <a name="system-tables"></a>系统表

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| 属性  |    类型     |
  	|----------- |   ------    |
  	| 标识         | 整数 64  |
  	| itemId     | 字符串      |
  	| 属性 | 二进制数据 |
  	| 表      | 字符串      |
  	| tableKind  | 16 整数  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| 属性  |    类型     |
  	|----------- |   ------    |
  	| 标识         | 字符串      |
  	| operationId | 整数 64 |
  	| 属性 | 二进制数据 |
  	| tableKind  | 16 整数  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| 属性  |    类型     |
  	|----------- |   ------    |
  	| 标识         | 字符串      |
  	| 密钥        | 字符串      |
  	| 关键字类型    | 整数 64  |
  	| 表      | 字符串      |
  	| 值      | 字符串      |

    ### <a name="data-table"></a>模拟运算表

    **TodoItem**

  	| 属性    |  类型   | 请注意                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| 标识           | 标记为必需的字符串  | 在远程存储中的主关键字                            |
  	| 完成     | 布尔值 | 托多项字段                                        |
  	| 文本         | 字符串  | 托多项字段                                        |
  	| createdAt | 日期    | （可选） 映射到 createdAt 系统属性         |
  	| updatedAt | 日期    | （可选） 映射到 updatedAt 系统属性         |
  	| 版本   | 字符串  | （可选） 用于检测冲突，映射到的版本 |


## <a name="setup-sync"></a>更改应用程序的同步行为

在本节中，您将修改该应用程序以便它不会不同步应用程序启动或在插入和更新的项目，但仅当执行笔势的刷新按钮时。

**目标 C**:

1. 在**QSTodoListViewController.m**，更改**viewDidLoad**方法调用中移除`[self refresh]`结尾的方法。 现在，数据不会与应用程序启动的服务器同步，但是将改为本地存储的内容。

2. 在**QSTodoService.m**中，修改该定义的`addItem`，以便它不会同步后插入项。 删除`self syncData`阻止并替换为以下︰

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. 修改定义的`completeItem`与上面;删除的块`self syncData`并替换为以下︰

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. 在`viewDidLoad` **ToDoTableViewController.swift**，在注释掉这两行，以停止在应用程序启动同步。 在这篇文章的写作时间，Swift Todo 应用程序不更新服务当某人添加或完成某项，仅在应用程序启动。

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>测试应用程序

在本节中，您将连接到无效的 URL 来模拟脱机的情况。 添加数据项时，它们会保存在本地的核心数据存储，但不是会同步到移动的后端。

1. 将**QSTodoService.m**中的移动应用程序 URL 更改为无效的 URL，并再次运行该应用程序︰

    在 QSTodoService.m 中**目标 C** :
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    在 ToDoTableViewController.swift 的**Swift** :

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. 添加一些 todo 项。 退出该模拟器 （或强制关闭应用程序），然后重新启动。 请保存您的更改。

3. 查看远程 TodoItem 表的内容︰

    + Node.js 后端，转到[Azure 的门户网站](https://portal.azure.com/)和移动应用程序中后端请单击**简单表** > **TodoItem**来查看内容的`TodoItem`表。
    + 查看.NET 后端，使用 SQL Server 管理 Studio 中，例如一个 SQL 工具或其他客户端如 Fiddler 或把邮递员弄的表内容。

    验证新项*尚未与服务器同步*︰

4. 将 URL 更改为正确上**QSTodoService.m**中并重新运行该应用程序。 通过拖动的项的列表中向下执行刷新笔势。 您将看到进行微调。

5. 再次查看 TodoItem 的数据。 现在应该显示新的和更改 TodoItems。

## <a name="summary"></a>摘要

为支持脱机同步功能，我们采用`MSSyncTable`接口，并初始化`MSClient.syncContext`与本地存储。 在这种情况下本地存储是基于核心数据的数据库。

当使用核心数据的本地存储区，您必须定义具有[正确的系统属性](#review-core-data)的多个表。

普通的 CRUD 操作 Azure 移动应用程序的工作应用程序仍处于连接状态，但对本地存储区中发生的所有操作。

当我们想要与服务器同步本地存储时，我们使用`MSSyncTable.pullWithQuery`方法。


## <a name="additional-resources"></a>其他资源

* [在 Azure 的移动应用程序的脱机数据同步]

* [云盖︰ Azure 的移动服务在脱机同步]\(注︰ 视频移动服务但脱机同步以类似的方式在 Azure 移动应用程序中的工作\)

<!-- URLs. -->


[创建 iOS 应用程序]: app-service-mobile-ios-get-started.md
[在 Azure 的移动应用程序的脱机数据同步]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[在移动服务 Azure 云盖︰ 脱机同步]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
