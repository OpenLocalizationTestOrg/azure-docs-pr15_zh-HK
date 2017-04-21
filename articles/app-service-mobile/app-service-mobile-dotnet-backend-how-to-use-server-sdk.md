<properties
    pageTitle="如何使用.NET 后端服务器 SDK 的移动应用程序 |Azure 应用程序服务"
    description="了解如何使用.NET 后端服务器 SDK 的 Azure 应用程序服务移动应用程序。"
    keywords="应用程序服务、 azure 应用程序服务、 移动应用程序、 比例，可伸缩的应用程序部署，azure 应用程序部署的移动服务"
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

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>对 Azure 移动应用程序使用.NET 后端服务器 SDK

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

本主题演示如何使用.NET 后端服务器 SDK 在 Azure 应用程序服务移动应用程序的关键方案。 Azure 移动应用程序 SDK 可帮助您使用从 ASP.NET 应用程序的移动客户端。

>[AZURE.TIP] [.NET server SDK Azure 移动应用程序的][2]是开源在 GitHub 上的。 知识库中含有包括整个服务器 SDK 单元测试套件和一些样例项目的所有源代码。

## <a name="reference-documentation"></a>参考文档

服务器 SDK 的参考文档的位置如下︰ [Azure 移动应用程序的.NET 引用][1]。

## <a name="create-app"></a>如何︰ 创建.NET 移动应用程序的后端

如果您要启动一个新项目，您可以创建使用[Azure 的门户网站]或 Visual Studio 应用程序服务应用程序。 您可以在本地运行的应用程序服务应用程序或将项目发布到您的基于云的应用程序服务移动应用程序。  

如果要移动功能添加到现有项目，请参见[下载并初始化 SDK](#install-sdk) 。

### <a name="create-a-net-backend-using-the-azure-portal"></a>创建.NET 后端使用 Azure 门户

若要创建应用程序服务移动后端，要么按照[快速入门教程][3]或者执行下列步骤︰

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

在_入门_刀片式服务器，**创建表 API**下, 步作为**后端语言**中选择**C#** 。 单击**下载**、 提取压缩的项目文件复制到本地计算机，并在 Visual Studio 中打开该解决方案。

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>创建使用 Visual Studio 2013年和 2015 Visual Studio.NET 后端

安装[.NET 的 Azure SDK] [ 4] (版本 2.9.0 或更高版本) 在 Visual Studio 中创建一个 Azure 移动应用程序项目。 一旦您已安装的 SDK，创建 ASP.NET 应用程序可以通过以下步骤︰

1. 打开**新建项目**对话框 (从*文件* > **New** > **项目...**)。
2. 展开**模板** > **C#**，然后选择**Web**。
3. 选择**ASP.NET Web 应用程序**。
4. 在项目名称来填充。 然后单击**确定**。
5. 在_4.5.2 ASP.NET 模板_，选择**Azure 的移动应用程序**。 检查在云发布此项目中创建移动后端的**云环境中的主机**。
6. 单击**确定**。

## <a name="install-sdk"></a>如何︰ 下载并初始化 SDK

[NuGet.org]提供了 SDK。 该软件包包括入门使用 SDK 所需的基本功能。 要初始化的 SDK，您需要在**HttpConfiguration**对象上执行操作。

### <a name="install-the-sdk"></a>安装 SDK

若要安装 SDK，用鼠标右键单击服务器项目中，Visual Studio、 选择**管理 NuGet 程序包**，搜索[Microsoft.Azure.Mobile.Server]程序包，然后单击**安装**。

###<a name="server-project-setup"></a>初始化服务器项目

类似于其他 ASP.NET 项目，.NET 后端服务器项目初始化通过包括 OWIN 启动类。 确保引用了 NuGet 程序包`Microsoft.Owin.Host.SystemWeb`。 若要在 Visual Studio 中添加此类，用鼠标右键单击您的服务器项目并选择**添加** > 
**新项**，然后**Web** > **常规** > **OWIN 启动类**。  生成具有以下特性︰

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

在`Configuration()`OWIN 启动类，使用**HttpConfiguration**对象来配置 Azure 移动应用程序环境。
下面的示例初始化服务器项目没有添加功能︰

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

若要启用各个功能，必须调用扩展方法**MobileAppConfiguration**对象上调用**ApplyTo**之前。 例如，下面的代码添加默认路由到拥有该属性的所有 API 控制器`[MobileAppController]`在初始化过程中︰

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

从 Azure 门户服务器快速入门将调用**UseDefaultConfiguration()**。 这等效于以下设置︰

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

使用扩展方法是︰

* `AddMobileAppHomeController()`提供默认 Azure 移动应用程序的主页。
* `MapApiControllers()`提供自定义的 API 功能使用修饰的 WebAPI 控制器`[MobileAppController]`属性。
* `AddTables()`提供的映射`/tables`到表控制器的终结点。
* `AddTablesWithEntityFramework()`是用于映射短手`/tables`终结点使用实体框架基于控制器。
* `AddPushNotifications()`提供注册通知集线器设备的简单方法。
* `MapLegacyCrossDomainController()`为本地的开发提供了标准的 CORS 头。

### <a name="sdk-extensions"></a>SDK 扩展

下面的 NuGet 基于扩展软件包提供了各种可供您的应用程序的移动功能。 通过使用**MobileAppConfiguration**对象来初始化期间启用扩展。

- [Microsoft.Azure.Mobile.Server.Quickstart]支持基本的移动应用程序设置。 通过在初始化过程中调用**UseDefaultConfiguration**扩展方法添加到配置。 此扩展包括以下扩展︰ 通知、 身份验证、 实体、 表格跨域和家庭包。 此软件包使用 Azure 门户上提供移动应用程序快速入门。

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
  实现默认值*此移动应用程序已启动并正在运行页*为 web 站点的根目录。 通过调用  **AddMobileAppHomeController**扩展方法添加到配置。

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
  包括用于处理数据类和集上的数据管道。 通过调用**AddTables**扩展方法添加到配置。

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
  使实体框架访问 SQL 数据库中的数据。 通过调用**AddTablesWithEntityFramework**扩展方法添加到配置。

- [Microsoft.Azure.Mobile.Server.Authentication]支持认证并设置成用于验证标记 OWIN 中间件。 向配置添加通过调用**AddAppServiceAuthentication**  
  和**IAppBuilder**。**UseAppServiceAuthentication**的扩展方法。

- [Microsoft.Azure.Mobile.Server.Notifications]启用推式通知和定义强制注册终结点。 通过调用**AddPushNotifications**扩展方法添加到配置。

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
  创建提供给旧的 web 浏览器的数据从您的移动应用程序的控制器。 通过调用  **MapLegacyCrossDomainController**扩展方法添加到配置。

- [Microsoft.Azure.Mobile.Server.Login]提供了 AppServiceLoginHandler.CreateToken() 方法，它是自定义的身份验证方案过程中使用的静态方法。   

## <a name="publish-server-project"></a>如何︰ 发布服务器项目

本节说明了如何将发布从 Visual Studio.NET 后端项目。 您还可以部署后端项目使用 Git，或任何在[Azure 应用程序服务部署文档](../app-service-web/web-sites-deploy.md)中介绍的其他方法。

1. 在 Visual Studio 中，重新生成项目以恢复 NuGet 程序包。

2. 在解决方案资源管理器中右击该项目，单击**发布**。 第一次发布时，您需要定义的发布配置文件。 当您已定义的配置文件时，您可以选择它并单击**发布**。

2. 如果询问您是否选择了发布目标，则单击**Microsoft Azure 应用程序服务** > **下一步**，（如果需要），然后登录使用 Azure 的凭据。 
   Visual Studio 下载并安全地存储您发布直接从 Azure 的设置。

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. 选择您的**订阅**，从**视图**中选择**资源类型**、 展开**移动应用程序**，并单击您的移动应用程序端，然后单击**确定**。

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. 验证的发布配置文件信息，然后单击**发布**。

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    当您移动应用程序的后端已成功发布时，您将看到指示成功登陆页。

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>如何︰ 定义一个表格控制器

定义要公开给移动式客户端 SQL 表的表控制器。  配置表控制器需要三个步骤︰

1. 创建一个数据传输对象 (DTO) 类。
2. 配置移动 DbContext 类中的表引用。
3. 创建一个表控制器。

数据传输对象 (DTO) 是纯 C# 对象，该对象继承自`EntityData`。  例如︰

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

DTO 用来定义 SQL 数据库中的表。  若要创建数据库项目时，添加`DbSet<>`为您正在使用 DbContext 属性。  在 Azure 移动应用程序的默认的项目模板，称为 DbContext `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

如果您有安装 Azure SDK，现在可以创建一个模板表控制器，如下所示︰

1. 用鼠标右键单击控制器文件夹并选择**添加** > **控制器...**。
2. 选择**Azure 移动应用程序表控制器**选项，然后单击**添加**。
3. 在**添加控制器**对话框中︰
    * 在**模型类**下拉列表中，选择您新的 DTO。
    * 在**DbContext**下拉列表中，选择移动服务 DbContext 类。
    * 为您创建该控制器的名称。
4. 单击**添加**。

快速入门服务器项目的简单**TodoItemController**中包含的示例。

### <a name="how-to-adjust-the-table-paging-size"></a>如何︰ 调整表的页面大小

默认情况下，Azure 移动应用程序返回的每个请求的 50 条记录。  确保了页面，客户端不会不会占用其 UI 线程，也不太长的时间，服务器确保良好的用户体验。 若要更改表的页面大小，增加"允许查询大小"服务器端和客户端页面大小的服务器端使用"允许查询大小"调整`EnableQuery`属性︰

    [EnableQuery(PageSize = 500)]

确保 PageSize 等于或大于客户端请求的大小。  请参阅特定的客户端如何更改客户端页面尺寸的详细信息的文档。

## <a name="how-to-define-a-custom-api-controller"></a>如何︰ 定义一个自定义的 API 控制器

自定义的 API 控制器提供最基本的功能，为您的移动应用程序后端通过公开的终结点。 您可以注册一个移动特定 API 控制器使用 [MobileAppController] 属性。 `MobileAppController`属性注册路由、 设置移动应用程序 JSON 序列化程序和打开的[客户端版本检查](app-service-mobile-client-and-server-versioning.md)。

1. 在 Visual Studio 中，用鼠标右键单击控制器文件夹，然后单击**添加** > **控制器**，选择**Web API 2 控制器&mdash;空**并单击**添加**。

2. 如提供的**控制器名称**， `CustomController`，然后单击**添加**。

3. 在新的控制器类文件中，添加以下 using 语句︰

        using Microsoft.Azure.Mobile.Server.Config;

4. 将**[MobileAppController]**属性应用到 API 控制器的类定义，如以下示例所示︰

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. 在 App_Start/Startup.MobileApp.cs 文件中，添加对**MapApiControllers**扩展方法，如以下示例所示︰

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

您还可以使用`UseDefaultConfiguration()`扩展方法而不是`MapApiControllers()`。 没有**MobileAppControllerAttribute**应用任何控制器仍然可以通过客户端，但可能会不正确地使用它由客户端使用任何移动应用程序客户端 SDK。

## <a name="how-to-work-with-authentication"></a>如何︰ 使用身份验证

Azure 的移动应用程序使用应用程序服务的身份验证 / 授权来保护您移动的后端。  本节说明如何在您的.NET 后端服务器项目执行身份验证相关的以下任务︰

+ [如何︰ 添加身份验证到服务器项目](#add-auth)
+ [如何︰ 使用应用程序的自定义身份验证](#custom-auth)
+ [如何︰ 检索通过身份验证的用户信息](#user-info)
+ [如何︰ 限制授权用户的数据访问](#authorize)

### <a name="add-auth"></a>如何︰ 添加身份验证到服务器项目

通过扩展的**MobileAppConfiguration**对象并配置 OWIN 中间件，可以对服务器项目中添加身份验证。 当您安装[Microsoft.Azure.Mobile.Server.Quickstart]软件包，并调用**UseDefaultConfiguration**扩展方法时，您可以跳到步骤 3。

1. 在 Visual Studio，安装[Microsoft.Azure.Mobile.Server.Authentication]软件包。

2. 在 Startup.cs 项目文件中，**配置**方法的开头添加下面这行代码︰

        app.UseAppServiceAuthentication(config);

    该 OWIN 中间件组件验证相关联的应用程序的服务网关所颁发的令牌。

3. 添加`[Authorize]`属性到任何控制器或要求进行身份验证的方法。 

若要了解有关如何为移动应用程序端的客户端身份验证，请参阅[添加到您的应用程序的身份验证](app-service-mobile-ios-get-started-users.md)。

### <a name="custom-auth"></a>如何︰ 使用应用程序的自定义身份验证

如果您不希望使用应用程序服务的身份验证/授权供应商之一，您可以实现您自己的登录系统。 安装[Microsoft.Azure.Mobile.Server.Login]软件包能够通过身份验证令牌生成。  提供您自己的代码来验证用户凭据。 例如，您可能会对数据库中的 salted 和哈希密码进行检查。 在以下示例中，`isValidAssertion()`方法 （在其他地方定义） 负责进行这些检查。

自定义身份验证由创建 ApiController 并公开公开`register`和`login`操作。 客户端应使用自定义用户界面从用户处收集信息。  使用标准的 HTTP POST 调用 api 然后提交信息。 服务器验证断言之后, 使用颁发令牌`AppServiceLoginHandler.CreateToken()`方法。  ApiController**不应**使用`[MobileAppController]`属性。 

例如`login`操作︰

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

在前面的示例中，LoginResult 和 LoginResultUser 是可序列化对象所需的属性公开。 客户端需要登录响应以 JSON 对象的形式返回︰

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

`AppServiceLoginHandler.CreateToken()`方法包括_观众_和_颁发者_参数。 这两个参数都设置为您的应用程序根的 URL 使用 HTTPS 方案。 同样应设置_secretKey_为您的应用程序的值签名密钥。 不分发客户端中的签名密钥，因为它可以用薄荷键和模拟用户。 您可以获取签名密钥时承载的应用程序服务中通过引用_网站\_身份验证\_签名\_键_环境变量。 如果需要在本地调试上下文，按照检索密钥并将其存储为应用程序设置[本地身份验证使用调试](#local-debug)部分中的说明。

颁发的令牌可能还包括其他索赔和到期日期。  至少，颁发的令牌必须包括主题 (**子**) 索赔。

可以支持标准客户端`loginAsync()`通过重载身份验证路由的方法。  如果客户端调用`client.loginAsync('custom');`若要登录，您必须已设置路由`/.auth/login/custom`。  您可以设置自定义身份验证控制器使用的工艺路线`MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] 使用`loginAsync()`方法可确保为每个后续调用服务所附加的身份验证令牌。

###<a name="user-info"></a>如何︰ 检索通过身份验证的用户信息

当用户进行身份验证的应用程序服务时，您可以在.NET 后端代码中访问分配的用户 ID 和其他信息。 用户信息可以用于在后端进行授权决策。 下面的代码将获取与请求关联的用户 ID:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

SID 从特定于提供程序的用户 ID，对于给定的用户和登录提供程序是静态的。  SID 不适用于无效的身份验证令牌。

应用程序服务还允许您从您的登录提供程序请求特定索赔。 每个标识提供程序可以提供使用 SDK 身份标识提供程序的详细信息。  例如，您可以使用 Facebook 图形 API 朋友的信息。  您可以在 Azure 门户提供刀片式服务器中指定请求的索赔。 一些索赔要求与标识提供程序的其他配置。

下面的代码将调用**GetAppServiceIdentityAsync**扩展方法来获取登录凭据，包括使向 Facebook 图形 API 发出的请求所需的访问令牌︰

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

添加一条 using 语句`System.Security.Principal`可以提供**GetAppServiceIdentityAsync**扩展方法。

### <a name="authorize"></a>如何︰ 限制授权用户的数据访问

在前面的部分中，我们介绍了如何检索通过身份验证的用户的用户 ID。 您可以对数据和其他资源，根据此值来限制访问。 例如，向表中添加一个用户 Id 列和筛选的用户 ID 的查询结果是限制仅对授权用户返回的数据的简单方法。 仅当 SID 匹配 TodoItem 表上的用户 Id 列中的值时，下面的代码返回数据行︰

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

`Query()`方法返回`IQueryable`，可以通过 LINQ 来处理筛选。

## <a name="how-to-add-push-notifications-to-a-server-project"></a>如何︰ 添加推送到服务器项目的通知

通过扩展的**MobileAppConfiguration**对象并创建通知集线器客户机向服务器项目中添加推式通知。

1. 在 Visual Studio 中，用鼠标右键单击该服务器项目并单击**管理 NuGet 程序包**，搜索`Microsoft.Azure.Mobile.Server.Notifications`，然后单击**安装**。 

2. 重复此步骤以安装`Microsoft.Azure.NotificationHubs`包，其中包括通知集线器客户端库。

3. 在 App_Start/Startup.MobileApp.cs，并在初始化期间添加的**AddPushNotifications()**扩展方法的调用︰

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. 添加下面的代码创建一个通知集线器客户端︰

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

您现在可以使用集线器通知客户端发送推式通知已注册的设备。 有关详细信息，请参阅[您的应用程序添加推送通知](app-service-mobile-ios-get-started-push.md)。 若要了解有关通知集线器的详细信息，请参阅[通知集线器概述](../notification-hubs/notification-hubs-push-notification-overview.md)。

##<a name="tags"></a>如何︰ 启用目标使用标签推送

集线器可以使用标记将目标的通知发送到特定登记的通知。 会自动创建多个标记︰

* 安装 ID 标识的特定设备。
* 经过身份验证的 SID 的用户 Id 标识的特定用户。

从**MobileServiceClient**的**installationId**属性可以访问安装 ID。  下面的示例演示如何使用安装 ID 将标记添加到通知集线器中特定的安装︰

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

后端创建安装时忽略客户端推送通知注册过程中提供的任何标记。 若要使客户端可以将标记添加到安装，必须创建一个自定义的 API，添加标记使用前面的模式。 

请参阅[客户端添加推送通知标记][5]示例应用程序服务移动应用程序已完成快速入门示例中。

##<a name="push-user"></a>如何︰ 为已经过身份验证的用户发送推式通知

当身份验证的用户注册的推式通知时，用户 ID 标记会自动添加到注册中。 通过使用此标记，您可以注册该用户的所有设备发送推式通知。 下面的代码获取发出请求的用户的 SID 并将模板推式通知发送到每个设备注册，此人︰

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

当注册来自已通过身份验证的客户端推送通知，确保身份验证之前尝试注册已完成。 有关详细信息，请参阅[将推送给用户][6]在.NET 后端应用程序服务移动应用程序已完成快速入门示例。

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>如何︰ 调试和故障排除.NET 服务器 SDK

Azure 应用程序服务提供几个调试和故障排除技巧为 ASP.NET 应用程序︰

- [监视 Azure 应用程序服务](../app-service-web/web-sites-monitor.md)
- [启用诊断日志记录在 Azure 应用程序服务](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Visual Studio 中的 Azure 应用程序服务的疑难解答](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>日志记录

使用标准的 ASP.NET 跟踪写作可以编写应用程序服务的诊断日志。 您可以写入日志之前，必须在您移动应用程序的后端中启用诊断。

要启用诊断并写入日志︰

1. 按照[如何启用诊断程序](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag)中的步骤。

2. 添加以下代码文件中使用语句︰

        using System.Web.Http.Tracing;

3. 创建跟踪编写器写入从.NET 后端的诊断日志，如下所示︰

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. 重新发布服务器项目，并访问移动应用程序后端执行日志记录的代码路径。

5. 下载并评估这些日志，如中所述[如何︰ 下载日志](../app-service-web/web-sites-enable-diagnostic-log.md#download)。

### <a name="local-debug"></a>本地调试与验证

您可以运行您的应用程序本地更改之前将其发布到云测试。 大多数的 Azure 移动应用程序 backends，请按*f5 键*在 Visual Studio 中。 但是，还有一些其他注意事项使用身份验证时。

您必须具有一个基于云的移动应用程序与应用程序服务的身份验证/授权配置，并且您的客户端必须具有云终结点指定为替代登录主机。 请参阅客户端平台所需的特定步骤的文档。

确保您的移动后端具有[Microsoft.Azure.Mobile.Server.Authentication]安装。 然后，在应用程序的 OWIN 启动类后, 添加以下，`MobileAppConfiguration`已经应用到您`HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

在前面的示例中，应配置的_authAudience_和_authIssuer_的应用程序设置在 Web.config 文件中为每个被您的应用程序根的 URL 使用 HTTPS 方案。 同样应设置_authSigningKey_为您的应用程序的值签名密钥。 获取签名的密钥︰

1. 导航到您在[Azure 门户]的应用程序 
2. 单击**工具**， **Kudu**，**转**。
3. 在 Kudu 管理网站上，单击**环境**。
4. 查找的值为_的网站\_身份验证\_签名\_键_。 

本地应用程序配置中的_authSigningKey_参数中使用的签名密钥。  现在配您移动的后端验证令牌时，在本地运行该客户端获取令牌从基于云的终结点。

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure 门户]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

