<properties
    pageTitle="使用应用程序服务移动应用程序托管的客户端库 (Windows |Xamarin) |Microsoft Azure"
    description="了解如何使用 Azure 应用程序服务移动应用程序窗口和 Xamarin 的应用程序使用.NET 客户端。"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>如何使用 Azure 移动应用程序的托管客户端

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>概述

本指南介绍了如何执行对 Azure 应用程序服务移动应用程序的窗口和 Xamarin 应用程序中使用托管的客户端库的常见方案。 如果您是移动应用程序，则应考虑首先完成[Azure 移动应用程序快速入门][1]教程。 在本指南中，我们着重讨论托管客户端 SDK。 要了解有关服务器端 Sdk 的移动应用程序，请参阅[.NET 服务器 SDK]文档[2]或[Node.js 服务器 SDK][3]。

## <a name="reference-documentation"></a>参考文档

客户端 SDK 的参考文档的位置如下︰ [Azure 移动应用程序的.NET 客户端引用][4]。
您还可以找到一些客户端示例[Azure 样本 GitHub 资料库]中[5]。

## <a name="supported-platforms"></a>支持的平台

.NET 平台支持以下平台︰

* Xamarin Android 版本的 API 19 至 24 (KitKat 通过 Nougat)
* Xamarin iOS 版本的 iOS 版本 8.0 版或更高版本
* 通用的 Windows 平台
* Windows Phone 8.1
* 除了 Silverlight 应用程序的 Windows Phone 8.0

"服务器流程"身份验证提供的用户界面使用 web 视图。  如果该设备不能提供一个 web 视图用户界面，则需要其他身份验证方法。  此 SDK 不因此适合监视类型或同样受限制的设备。

##<a name="setup"></a>安装和系统必备组件

我们假定您已经创建并发布您移动应用程序的后端项目，其中包括至少一个表。  在使用本主题中的代码，表被命名为`TodoItem`，它具有以下列︰ `Id`， `Text`，和`Complete`。 此表是同一个表创建时完成 [Azure 移动应用程序快速入门]。

在 C# 中相应类型的客户端类型是下面的类︰

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

[JsonPropertyAttribute] [6]用来定义表与表之间的客户端类型的*属性名称*映射。

若要了解如何在您的移动应用程序后端中创建表，请参见[.NET 服务器 SDK 主题]中的信息[7]或[Node.js 服务器 SDK 主题][8]。 如果您使用快速入门 Azure 门户中创建您移动应用程序的后端，还可以在[Azure 门户]使用**简单表**设置。

###<a name="how-to-install-the-managed-client-sdk-package"></a>如何︰ 将托管客户端 SDK 程序包安装

使用以下方法之一为[NuGet]从移动应用程序安装的托管客户端 SDK 包[9]:

+ **Visual Studio**用鼠标右键单击您的项目，请单击**管理 NuGet 程序包**搜索`Microsoft.Azure.Mobile.Client`包，然后单击**安装**。

+ **Xamarin Studio**用鼠标右键单击项目，单击**添加** > **添加 NuGet 程序包**，搜索`Microsoft.Azure.Mobile.Client `打包，然后单击**添加程序包**。

在主活动文件中，请记住要将以下**using**语句添加︰

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>如何︰ 使用 Visual Studio 中调试符号

Microsoft.Azure.Mobile 命名空间的符号位于[SymbolSource][10]。  请参阅[SymbolSource 说明][11]与 Visual Studio 集成 SymbolSource。

##<a name="create-client"></a>创建移动应用程序客户端

下面的代码创建[MobileServiceClient] [12]用来访问您的移动应用程序端的对象。

    var client = new MobileServiceClient("MOBILE_APP_URL");

在上面的代码中，将`MOBILE_APP_URL`的移动应用程序的后端 url，其中发现您在[Azure 门户]的移动应用程序后端的刀片式服务器中。 MobileServiceClient 对象应该是一个单一实例。

## <a name="work-with-tables"></a>使用表格

以下部分详细介绍如何搜索和检索的记录并修改表中的数据。  包括以下主题︰

* [创建一个表引用](#instantiating)
* [查询数据](#querying)
* [筛选返回的数据](#filtering)
* [返回的数据的排序](#sorting)
* [返回在页中的数据](#paging)
* [选择特定列](#selecting)
* [查找记录的 Id](#lookingup)
* [处理非类型化查询](#untypedqueries)
* [插入数据](#inserting)
* [更新数据](#updating)
* [删除数据](#deleting)
* [冲突解决和开放式并发](#optimisticconcurrency)
* [绑定到 Windows 用户界面](#binding)
* [更改页面尺寸](#pagesize)

###<a name="instantiating"></a>如何︰ 创建表引用

访问或修改数据后端表中的所有代码都调用函数`MobileServiceTable`对象。 通过按以下方式调用[GetTable]方法，获得对表的引用︰

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

返回的对象使用的类型化序列化模型。 非类型化序列化模型还支持。 下面的示例[创建一个非类型化的表的引用]︰

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

在非类型化的查询，必须指定基础 OData 查询字符串。

###<a name="querying"></a>如何︰ 查询从您的移动应用程序的数据

本节介绍如何将查询发送到移动应用程序的后端，其中包括以下功能︰

- [筛选返回的数据](#filtering)
- [返回的数据的排序](#sorting)
- [返回在页中的数据](#paging)
- [选择特定列](#selecting)
- [查找数据的 ID](#lookingup)

>[AZURE.NOTE]服务器驱动的纸型实施以防止返回的所有行。  分页可以防止大型数据集的默认请求对服务造成负面影响。  若要返回 50 多个行，使用`Skip`和`Take`方法，如 [返回数据页中] 中所述。

###<a name="filtering"></a>如何︰ 筛选器未返回数据

下面的代码演示如何筛选数据，包括由`Where`查询中的子句。 它返回所有项从`todoTable`的`Complete`属性等于`false`。 [其中]函数应用筛选谓词的查询对表的行。

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

您可以查看通过使用消息检查软件，如浏览器开发人员工具或[Fiddler]发送到后端的请求的 URI。 如果您看一下请求 URI，请注意，查询字符串被修改︰

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

此 OData 请求服务器 sdk 转换为 SQL 查询︰

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

传递给该函数`Where`方法可以有任意数目的条件。

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

本示例将由服务器 SDK 转换为 SQL 查询︰

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

此外可将该查询拆分为多个子句︰

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

这两种方法是等效的可以互换使用。  前一个选项&mdash;的连接在一个查询中的多个谓语&mdash;是更紧凑和推荐。

`Where`子句支持操作会转换成 OData 子集。 操作包括︰

* 关系运算符 (= = 和 ！ =，<、 < =、 >，> =)，
* 算术运算符 (+，-，/，*，%)，
* 数字精度 （Math.Floor，Math.Ceiling）
* 字符串函数 （长度、 子字符串、 替换、 IndexOf、 StartsWith、 EndsWith），
* 日期属性 （年、 月、 日、 小时、 分钟、 秒），
* 访问对象的属性，以及
* 将这些操作组合的表达式。

在考虑服务器 SDK 的支持时，您可以考虑[OData v3 文档]。

###<a name="sorting"></a>如何︰ 排序返回数据

下面的代码演示如何通过在查询中包括[排序依据]或[OrderByDescending]函数对数据进行排序。 它返回的项目`todoTable`按升序排序的`Text`字段。

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>如何︰ 返回在页中的数据

默认情况下后, 端返回仅前 50 的行。 可以通过调用[花费]增加返回的行数。 使用`Take`以及请求"页"总数据集的查询所返回的特定的[跳过]方法。 下面的查询中，在执行时，将返回表中的前三项。

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

以下修改后的查询跳过前三个结果，并返回接下来三个结果。 此查询生成数据，其中的页面大小是三个项目的第二个"的页"。

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

[IncludeTotalCount]方法请求的总数为_所有_已返回，忽略任何页面/限制子句指定的记录︰

    query = query.IncludeTotalCount();

在实际应用程序中，可用于查询类似于前面的示例中使用的页导航控件或可比较的用户界面页之间导航。

>[AZURE.NOTE]若要覆盖 50 行限制移动应用程序的后端，您必须将[EnableQueryAttribute]应用到公共的 GET 方法并指定分页行为。 应用于该方法时，以下设置返回为 1000年行的最大值︰
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>如何︰ 选择特定列

您可以指定要通过将[Select]子句添加到查询结果中包括的属性集。 例如，下面的代码显示了如何选择一个字段以及如何选择和设置多个域的格式︰

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

到目前为止所描述的所有函数都都累加式的因此我们可以保持链接它们。 每个链接的调用会影响查询的详细信息。 一个详细的示例︰

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>如何︰ 查找数据的 ID

可以使用[LookupAsync]函数来查找对象从数据库与一个特定的 id。

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>如何︰ 执行非类型化的查询

在执行时使用非类型化的表对象的查询，则必须显式指定 OData 查询字符串，通过调用[ReadAsync]，如以下示例所示︰

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

您获得 JSON 值，您可以使用属性包等。 JToken 和 Newtonsoft Json.NET 的详细信息，请参阅[Json.NET]站点。

### <a name="inserting"></a>如何︰ 将数据插入到移动应用程序的后端

所有的客户端类型必须包含名为**Id**，它是默认情况下，字符串的成员。 执行 CRUD 操作所需的此**Id**和脱机同步。 下面的代码阐释了如何使用[InsertAsync]方法将新行插入表中。 参数包含要插入作为.NET 对象的数据。

    await todoTable.InsertAsync(todoItem);

如果自定义一个唯一 ID 值不包括在`todoItem`期间插入、 GUID 由服务器生成的。
您可以通过检查该对象在调用返回后检索生成的 Id。

插入非类型化的数据，您可以利用 Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

下面是使用电子邮件地址作为唯一的字符串 id 的示例︰

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>使用 ID 值

移动应用程序支持的表的**id**列的唯一的自定义字符串值。 一个字符串值，允许应用程序使用自定义值，例如电子邮件地址或用户名的 id。  字符串 Id 为您提供了以下好处︰

* 不到数据库做一次往返行程生成 Id。
* 记录是容易合并不同的表或数据库中。
* Id 值可以更好地集成应用程序的逻辑。

移动应用程序的后端上插入记录未设置字符串 ID 值，生成一个唯一的值 id。 您可以使用[Guid.NewGuid]方法来生成自己的 ID 值，请在客户端或后端。

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>如何︰ 修改移动应用程序的后端中的数据

下面的代码阐释了如何使用[UpdateAsync]方法具有相同的 ID，使用新信息更新现有记录。 参数包含要更新作为.NET 对象的数据。

    await todoTable.UpdateAsync(todoItem);

要更新非类型化的数据，您可能会利用[Json.NET] ，如下所示︰

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

`id`进行更新时，必须指定字段。 后端使用`id`字段来标识要更新的行。 `id`字段可以从结果`InsertAsync`调用。 `ArgumentException`如果您尝试更新某个项目而无需提供`id`值。

###<a name="deleting"></a>如何︰ 删除移动应用程序的后端中的数据

下面的代码阐释了如何使用[DeleteAsync]方法来删除现有的实例。 该实例由`id`字段上设置`todoItem`。

    await todoTable.DeleteAsync(todoItem);

若要删除非类型化的数据，您可能利用 Json.NET，如下所示︰

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

当您删除请求时，必须指定 ID。 其他属性不会传递到该服务或服务将被忽略。 结果`DeleteAsync`调用通常是`null`。 结果可以从传入的 ID`InsertAsync`调用。 A`MobileServiceInvalidOperationException`当您尝试删除的项目，而无需指定引发`id`字段。

###<a name="optimisticconcurrency"></a>如何︰ 使用开放式并发冲突解决方法

两个或多个客户端可能在同一时间将更改写入到同一个项。 无冲突检测，最后一次写入将覆盖任何以前的更新。 **乐观并发控制**假定每个事务可以提交，因此没有使用任何资源锁定。  提交事务之前, 乐观并发控制验证，任何其他事务都已修改数据。 如果数据已被修改，则回滚提交的事务。

移动应用程序支持通过修订为每个物料使用乐观并发控制`version`为您的移动应用程序后端中的每个表定义系统属性列。 每次更新一条记录时，移动应用程序设置`version`属性的新值对该记录。 在每个更新请求，`version`请求中包含该记录的属性进行比较的服务器上记录的相同属性。 如果版本传递与请求不匹配的后端，然后客户端库将引发`MobileServicePreconditionFailedException<T>`异常。 异常中包含的类型是从包含该记录的服务器版本的后端的记录。 应用程序然后可以使用此信息来决定是否执行更新请求再次使用正确的`version`后端提交更改的值。

定义的列的表类上`version`系统属性来启用开放式并发。 例如︰

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


使用非类型化的表的应用程序将通过设置启用开放式并发`Version`标志在`SystemProperties`的表，如下所示。

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

不仅乐观并发性，您必须捕获`MobileServicePreconditionFailedException<T>`在您的代码中调用[UpdateAsync]时的异常。  通过应用正确解决冲突`version`更新的记录和与解析记录的调用[UpdateAsync] 。 下面的代码演示如何解决一次检测到写入冲突︰

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

有关详细信息，请参阅[脱机数据同步在 Azure 移动应用程序]的主题。

###<a name="binding"></a>如何︰ 绑定移动应用程序数据复制到 Windows 用户界面

这一节演示如何显示返回的数据对象在 Windows 应用程序中使用的 UI 元素。  下面的代码示例将绑定到的查询未完成的项目的列表的源。 [MobileServiceCollection]创建一个移动应用程序意识的绑定集合。

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

托管运行时中的某些控件支持的接口调用[ISupportIncrementalLoading]。 此接口允许控件用户滚动时请求额外的数据。 没有通用 Windows 应用程序，通过[MobileServiceIncrementalLoadingCollection]，它会自动处理控件中的调用该接口的内置支持。 使用`MobileServiceIncrementalLoadingCollection`在 Windows 应用程序，如下所示︰

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

要在 Windows Phone 8 和"Silverlight"应用程序使用新的集合，使用`ToCollection`的扩展方法`IMobileServiceTableQuery<T>`， `IMobileServiceTable<T>`。 若要加载数据，请调用`LoadMoreItemsAsync()`。

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

当您使用通过调用创建的集合`ToCollectionAsync`或`ToCollection`，就可以绑定到用户界面控件的集合。  此集合是分页注意。  因为集合从网络加载数据，有时加载将失败。 若要处理此类失败，请重写`OnException`方法在`MobileServiceIncrementalLoadingCollection`处理异常产生的对的调用`LoadMoreItemsAsync`。

如果您的表包含很多字段，但您只想在您的控件中显示它们中的一些考虑。 您可以使用在"[选择特定列](#selecting)"一节所述指南来选择要显示在 UI 中的特定列。

###<a name="pagesize"></a>更改页面大小

Azure 的移动应用程序默认情况下返回 50 项的每个请求的最大值。  您可以更改页面的大小，通过增加客户端和服务器上的最大页面大小。  若要增加请求的页大小，指定`PullOptions`时使用`PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

假设您已经`PageSize`等于或大于 100 的服务器内，请求返回多达 100 个项目。

##<a name="#offlinesync"></a>使用脱机的表

离线表用于脱机工作时使用本地的 SQLite 存储区来存储数据。  操作完成对本地的所有表 SQLite 都存储而不是远程服务器存储都库。  要创建脱机表，首先准备好您的项目︰

1. 在 Visual Studio 中，右击解决方案 >**解决方案...管理 NuGet 程序包**，然后搜索并安装**Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet 程序包的解决方案中的所有项目。

2. （可选）为了支持 Windows 设备，安装下面的 SQLite 运行时包之一︰

    * **Windows 8.1 运行时︰**安装[Windows 8.1 的 SQLite][3]。
    * **Windows Phone 8.1:**安装[Windows Phone 8.1 的 SQLite][4]。
    * **通用的 Windows 平台**安装[通用的 Windows 通用的 SQLite][5]。

3. （可选）。 对于 Windows 的设备，单击**引用** > **添加引用...**，展开**Windows**文件夹 >**扩展**，然后启用相应**SQLite 的 Windows** SDK 以及**Visual C++ 2013年运行 Windows** SDK。
    每一种 Windows 平台，SQLite SDK 名称略有不同。

可以创建表引用之前，必须准备好本地存储区︰

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

存储在初始化通常客户端创建之后立即完成。  **OfflineDbPath**应该是适合您支持的所有平台上使用的文件名。  如果路径是完全限定的路径 （即开始以斜杠），则将使用该路径。  如果不完全限定的路径，该文件位于特定于平台的位置。

* 对于 iOS 和 Android 设备，默认路径是"个人文件"文件夹。
* 对于 Windows 设备，默认路径是特定于应用程序的"应用程序数据"文件夹。

可以使用获取表引用`GetSyncTable<>`方法︰

    var table = client.GetSyncTable<TodoItem>();

您不需要进行身份验证要使用脱机的表。  您只需时进行通信的后端服务进行身份验证。

###<a name="syncoffline"></a>同步脱机的表

与后端，离线表不默认同步。  同步被拆分成两个部分。  您可以将更改推送分别从下载新邮件。  下面是一个典型的同步方法︰

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
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

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
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

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

如果第一个参数为`PullAsync`为空，则不使用增量同步。  每个同步操作中检索所有记录。

SDK 执行隐式`PushAsync()`前拉出记录。

冲突处理发生在`PullAsync()`方法。  您可以联机表相同的方式处理冲突。  在产生冲突时`PullAsync()`在插入、 更新或删除过程中而不是调用。 如果多个冲突发生，它们将被绑定到单个 MobileServicePushFailedException。  分别处理每个故障。

##<a name="#customapi"></a>使用自定义的 API

自定义的 API 使您能够定义自定义终结点公开服务器功能的不映射到插入、 更新、 删除或读取操作。 通过使用自定义的 API，您可以更好地控制消息，包括读取和设置 HTTP 消息头定义非 JSON 消息正文格式。

通过在客户端调用[InvokeApiAsync]方法之一调用一个自定义的 API。 例如，下面这行代码为**completeAll** API 后端发送 POST 请求︰

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

此窗体的类型化的方法调用，需要**MarkAllResult**返回类型定义。 支持类型化和非类型化的方法。

##<a name="authentication"></a>对用户进行身份验证

移动应用程序支持身份验证和授权应用程序用户使用各种外部身份提供程序︰ Facebook、 Google、 Microsoft 帐户、 Twitter 和 Azure 活动目录。 可以对表来限制对特定操作只有已通过身份验证的用户设置权限。 此外可以使用已经过身份验证的用户的身份在服务器脚本中实现授权规则。 有关详细信息，请参阅本教程[添加到您的应用程序的身份验证]。

支持两种身份验证流程︰_客户端管理_和_服务器托管_流。 服务器管理流程提供最简单的身份验证体验，因为它依赖于提供商的 web 身份验证接口。 因为它依赖于特定于提供程序的特定于设备的 Sdk 的客户端管理流程允许使用特定于设备的功能更深度的集成。

>[AZURE.NOTE] 我们建议在您的生产应用程序中使用客户端管理流程。

若要设置身份验证，您必须注册您的应用程序与一个或多个身份提供程序。  标识提供程序生成的客户端 ID 和客户端密码为您的应用程序。  这些值然后设置您的后端中启用 Azure 应用程序服务的身份验证/授权。  有关详细信息，请详细的说明按照教程[向您的应用程序添加身份验证]中。

本部分包含以下主题︰

+ [管理客户端的身份验证](#clientflow)
+ [管理服务器的身份验证](#serverflow)
+ [缓存的身份验证令牌](#caching)

###<a name="clientflow"></a>管理客户端的身份验证

您的应用程序可以独立联系身份标识提供程序，并与您的后端在登录期间提供返回的标记。 这个客户端流量，可以为用户提供单一登录体验或从标识提供程序检索其他用户数据。 客户端流量身份验证是首选为身份标识提供程序 SDK 提供了更多本机用户体验感觉和允许进行更多自定义使用服务器流。

所提供的下列客户端流的身份验证模式的示例︰

+ [Active Directory 验证库](#adal)
+ [Facebook 或 Google](#client-facebook)
+ [实时的 SDK](#client-livesdk)

#### <a name="adal"></a>使用活动目录身份验证库的用户进行身份验证

您可以使用活动目录身份验证库 (ADAL) 从使用 Azure Active Directory 身份验证的客户端启动的用户身份验证。

1. [如何配置应用程序服务 Active Directory 登录]教程配置 AAD 登录您移动应用程序后的端。 请务必完成注册本机客户端应用程序的可选步骤。
2. 在 Visual Studio 或 Xamarin Studio 中，打开您的项目并添加到引用`Microsoft.IdentityModel.CLients.ActiveDirectory`NuGet 程序包。 在搜索时，包括预发行版本。
3. 将以下代码添加到应用程序中，根据您使用的平台。 在中，进行以下替换︰

    * 组织资源调配您的应用程序的名称替换**插入的主管机构-这里**。 格式应为 https://login.windows.net/contoso.onmicrosoft.com。 此值可以在[Azure 的传统门户]将 Azure Active Directory 中的域选项卡中复制。
    * **插入资源 ID 这里**替换为您的移动应用程序后端的客户端 ID。 从门户下**Azure 活动目录设置**的**高级**选项卡，您可以获得的客户端 ID。
    * 从本机客户端应用程序中复制的客户机 ID 替换**插入客户端 ID 这里**。
    * 替换为您的网站_/.auth/login/done_的终结点，使用 HTTPS 方案**插入重定向 URI 这里**。 此值应与_https://contoso.azurewebsites.net/.auth/login/done_类似。

    为每个平台所需的代码如下所示︰

    **窗口︰**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>单一登录使用 Facebook 或 Google 中的某个标记

对于 Facebook 或 Google 此代码段所示，您可以使用客户端流。

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>一个实时 SDK 中使用 Microsoft 帐户登录

若要对用户进行身份验证，您必须注册您的应用程序开发人员中心中的 Microsoft 科目。 在您移动应用程序的后端配置注册详细信息。 若要创建 Microsoft 帐户注册并将其连接到您的移动应用程序端，完成[注册使用 Microsoft 帐户登录您的应用程序]中的步骤操作。 如果您有您的应用程序的 Windows 应用商店和 Windows Phone 8/Silverlight 版本，请首先注册 Windows 存储版本。

下面的代码使用实时 SDK 进行身份验证并使用返回的标记登录到您的移动应用程序后端。

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

有关详细信息，请参阅[Windows Live SDK]文档。

###<a name="serverflow"></a>管理服务器的身份验证

一旦您注册您的身份标识提供程序，您的提供程序的[MobileServiceAuthenticationProvider]值调用 [MobileServiceClient] 的[LoginAsync]方法。 例如，下面的代码启动服务器流登录使用 Facebook。

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

如果您使用的 Facebook 不标识提供程序，您的提供程序的值更改[MobileServiceAuthenticationProvider]的值。

服务器流，在 Azure 应用程序服务管理 OAuth 身份验证流通过显示所选提供程序的登录页。  一旦标识提供程序返回，Azure 应用程序服务生成的应用程序服务的身份验证令牌。 [LoginAsync]方法返回[MobileServiceUser]，它提供经过身份验证的用户的[用户 Id]和[MobileServiceAuthenticationToken]中，都作为一个 JSON web 标记 (JWT)。 此标记可以缓存，并可以重复使用，直到它过期。 有关详细信息，请参阅[缓存身份验证令牌](#caching)。

###<a name="caching"></a>缓存的身份验证令牌

在某些情况下，调用登录方法可以避免第一的身份验证成功后通过存储身份验证令牌提供程序。  Windows 应用商店和 UWP 应用程序可以使用[PasswordVault] ，如下所示成功注册后，缓存当前身份验证令牌︰

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

用户 Id 值存储为凭据的用户名，密码存储标记。 在后续刚起步的公司，您可以检查**PasswordVault**的缓存凭据。 他们发现，和否则尝试与后端重新进行身份验证时，下面的示例将使用缓存的凭据︰

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

当注销用户时，还必须删除所存储的凭据，如下所示︰

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Xamarin 应用程序使用[Xamarin.Auth] Api 可以安全地将凭据存储在一个**帐户**对象。 使用这些 Api 的示例，请参见[ContosoMoments 照片共享示例]中的[AuthStore.cs]文件。

使用客户端管理身份验证时，您也可以缓存来自 Facebook 和 Twitter 提供程序的访问令牌。 可以提供此标记，如下所示从后端，请求一个新的身份验证令牌︰

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>推式通知

下列主题介绍了推送通知︰

* [推式通知的登记](#register-for-push)
* [获取 Windows 应用商店包 SID](#package-sid)
* [用跨平台模板注册](#register-xplat)

###<a name="register-for-push"></a>如何︰ 注册推式通知

移动应用程序客户端可以使用 Azure 通知集线器的推式通知的注册。 注册时，您将获得获得从特定于平台的推送通知服务 （企业型） 的句柄。 当创建注册，然后提供此值及其任何标记。 下面的代码注册推送通知 Windows 应用的程序与 Windows 通知服务 (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

如果您推向 WNS，您必须[获取 Windows 应用商店包 SID](#package-sid)。  Windows 应用程序，包括如何为模板登记注册的详细信息请参阅[您的应用程序添加推送通知]。

不支持请求来自客户端的标记。  从注册以静默方式除去标记的请求。
如果您想要使用标记注册您的设备，创建一个自定义的 API，使用通知集线器 API 以您的名义执行注册。  而不是[调用自定义 API](#customapi) `RegisterNativeAsync()`方法。

###<a name="package-sid"></a>如何︰ 获取 SID 的 Windows 应用商店软件包

启用推式通知 Windows 应用商店应用程序中的所需包的 SID。  若要接收包的 SID，注册 Windows 应用商店应用程序。

若要获取此值︰

1. 在 Visual Studio 解决方案资源管理器中，右击 Windows 应用商店应用程序项目，单击**存储** > **相关联的应用程序与存储...**。
2. 在向导中，单击**下一步**，使用您的 Microsoft 帐户登录、**保留新的应用程序名称**，键入您的应用程序的名称，然后单击**保留**。
3. 成功创建的应用程序注册后，选择应用程序的名称，请单击**下一步**，，然后单击**关联**。
4. 登录到使用 Microsoft 帐户的[Windows 开发人员中心]。 在**我的应用程序**，请单击您创建的应用程序注册。
5. 单击**应用程序管理** > **应用程序标识**，然后进行滚动以查找您**包 SID**。

许多软件包 SID 的使用将其视为一个 URI，在这种情况下，您需要使用_ms app: / /_为方案。 记下您的软件包通过此值作为前缀的 SID 的版本。

Xamarin 应用程序需要更多的代码才可以注册在 iOS 或 Android 平台上运行的应用程序。 有关详细信息，请参阅您的平台的主题︰

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>如何︰ 注册推模板发送跨平台通知

若要注册模板，使用`RegisterAsync()`的模板，如下所示的方法︰

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

您的模板应该是`JObject`类型，并可以包含多个模板中 JSON 格式如下︰

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

方法**RegisterAsync()**还接受辅助图标︰

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

所有标记将消失都去除安全注册过程。 若要将标记添加到安装或安装中的模板，请参阅 [与.NET 后端服务器 SDK Azure 移动应用程序的工作]。

要发送通知利用这些已注册的模板，请[通知集线器 Api]参考。

##<a name="misc"></a>其他主题

###<a name="errors"></a>如何︰ 处理错误

当在后端发生错误时，客户端 SDK 将引发`MobileServiceInvalidOperationException`。  下面的示例演示如何处理异常，则返回由后端︰

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

中[移动应用程序文件的示例]，可以找到处理错误条件的另一个例子。 [LoggingHandler]示例提供日志记录代理处理 （以下） 记录对后端进行请求。

###<a name="headers"></a>如何︰ 自定义请求标头

为了支持您的特定应用程序方案，可能需要自定义通信与移动应用程序的后端。 例如，可能需要将自定义标头添加到每个传出请求或甚至更改响应状态代码。 您可以使用自定义[DelegatingHandler]，如下面的示例所示︰

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[添加到您的应用程序的身份验证]: app-service-mobile-windows-store-dotnet-get-started-users.md
[在 Azure 的移动应用程序的脱机数据同步]: app-service-mobile-offline-data-sync.md
[将推式通知添加到您的应用程序]: app-service-mobile-windows-store-dotnet-get-started-push.md
[注册您的应用程序使用 Microsoft 帐户登录]: app-service-mobile-how-to-configure-microsoft-authentication.md
[如何为 Active Directory 登录配置应用程序服务]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[可获取]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[创建非类型化的表的引用]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[排序依据]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[执行]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[选择]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[跳过]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[用户 Id]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[在哪里]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure 门户]: https://portal.azure.com/
[Azure 的传统门户网站]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows 开发人员中心]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[通知集线器 Api]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[移动应用程序文件的示例]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 文档]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[ContosoMoments 照片共享示例]: https://github.com/azure-appservice-samples/ContosoMoments