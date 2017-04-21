<properties
    pageTitle="从移动服务升级到 Azure 应用程序服务"
    description="了解如何轻松地升级移动服务应用程序与应用程序服务移动应用程序"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>将您现有的.NET Azure 移动服务升级为应用程序服务

应用程序服务移动是一种生成使用 Microsoft Azure 的移动应用程序的新方法。 若要了解详细信息，请参阅[手机应用程序是什么？]。

本主题介绍如何将现有的.NET 后端应用程序从 Azure 移动服务升级到新的应用程序服务移动应用程序。 虽然执行此升级时，现有的移动服务应用程序可以继续运行。   如果您需要升级 Node.js 后端应用程序，请参阅[升级您 Node.js 移动服务](./app-service-mobile-node-backend-upgrading-from-mobile-services.md)。

当移动端升级到 Azure 应用程序服务时，它有权访问所有应用程序服务功能和根据[应用程序服务的定价]，不移动服务定价计费方式。

##<a name="migrate-vs-upgrade"></a>迁移和升级

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] 我们建议，在[执行迁移](app-service-mobile-migrating-from-mobile-services.md)之前经过升级。 这种方式，可以将两个版本的应用程序放在同一个应用程序服务计划并会导致没有额外的费用。

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>移动应用程序的.NET server SDK 中的改进

升级到新的[移动应用程序 SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/)提供了以下好处︰

- NuGet 依赖更多的灵活性。 宿主环境不再提供 NuGet 程序包的版本，因此您可以使用替代的兼容版本。 但是，如果有新的关键 bugfixes 或移动服务器 SDK 或依赖项的安全更新，您必须手动更新您的服务。

- 移动 SDK 中的更多灵活性。 您可以显式地控制哪些功能和路由设置，如身份验证、 表 Api，以及推入注册终结点。 若要了解详细信息，请参阅[如何使用.NET server SDK 的 Azure 移动应用程序](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup)。

- 支持其他 ASP.NET 项目类型和路由。 您现在可以承载移动后端项目的同一项目中的 MVC 和 Web API 控制器。

- 对新应用程序服务的身份验证功能，使您可以跨 web 和移动应用程序中使用通用的身份验证配置的支持。

##<a name="overview"></a>升级概述

在许多情况下，升级将像切换到新的移动应用程序服务器 SDK 并重新发布到新的移动应用程序实例代码简单。 有，但是某些情况下，这将需要一些额外的配置，如高级身份验证方案和处理计划的作业。 每个在后面几节中介绍。

>[AZURE.TIP] 建议您阅读并了解本主题的其余部分完全开始升级前。 请记下任何功能使用的下面加注。

移动服务客户端 Sdk 不**是**兼容的新的移动应用程序服务器 SDK。 为了使您的应用程序的服务的连续性，不应将更改发布到当前为已发布的客户端提供服务的网站。 相反，您应该创建新的移动应用程序作为一个重复。 可以将此应用程序放置在相同的应用程序服务计划，以避免招致更多的财务成本。

然后将有两个版本的应用程序︰ 一个保持一致并提供疯狂状态，和其他然后可以升级和目标提供一个新的客户端版本中发布应用程序。 您可以移动并测试您的代码在您的节奏，但应确保所做的任何 bug 修复会应用到两个。 一旦您觉得需的大量处于放任状态的应用程序已更新为最新版本的客户端，可以删除原始迁移应用程序根据需要。

升级过程的完整轮廓如下所示︰

1. 创建新的移动应用程序
2. 更新项目以使用新服务器 Sdk
3. 发布新版本的客户端应用程序
4. （可选）删除原始迁移的实例

##<a name="mobile-app-version"></a>创建第二个应用程序实例
升级的第一步是创建的移动应用程序资源，它将承载的应用程序的新版本。 已迁移现有的移动服务，如果要创建此版本的相同的托管计划。 打开[Azure 门户]并定位到已迁移应用程序。 请记下其上运行的应用程序服务计划。

接下来，创建第二个应用程序实例的[.NET 后端创建说明](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app)。 当系统提示您选择您的应用程序服务计划或驻留计划"选择迁移应用程序的计划。

您可能需要使用相同的数据库和通知中心，就像在移动服务。 您可以将这些值复制通过打开[Azure 门户]并导航到该原始的应用程序，然后单击**设置** > **应用程序设置**。 在**连接字符串**中，复制`MS_NotificationHubConnectionString`， `MS_TableConnectionString`。 导航到新升级的网站并将其粘贴在覆盖任何现有值。 重复此过程的任何其他应用程序设置为您的应用程序的需要。 如果没有使用迁移的服务，可以从[Azure 的传统门户网站]的移动服务部分的**配置**选项卡中读取连接字符串，应用程序设置。

使您的应用程序的 ASP.NET 项目的副本并将其发布到您的新站点。 使用客户端应用程序使用新的 URL 更新的副本，验证一切按预期的方式工作。

## <a name="updating-the-server-project"></a>更新服务器项目

移动应用程序提供了新的[移动应用程序服务器 SDK]提供相同的功能的移动服务运行时的很多。 首先，应删除对移动服务包的所有引用。 NuGet 程序包管理器，在搜索`WindowsAzure.MobileServices.Backend`。 大多数应用程序将看到几个包在这里，包括`WindowsAzure.MobileServices.Backend.Tables`， `WindowsAzure.MobileServices.Backend.Entity`。 在这种情况下，启动与最低包中的依赖项树，如`Entity`，并将其删除。 出现提示时，不要删除所有相关的包。 重复此过程，直到您删除`WindowsAzure.MobileServices.Backend`本身。

此时，您可以不再引用移动服务 Sdk 的项目。

接下来您将添加引用移动应用程序 SDK。 此次升级，大多数开发人员将需要下载并安装`Microsoft.Azure.Mobile.Server.Quickstart`包，因为这将纳入整个需要集中。

会有很多编译器错误造成的差异的 Sdk，但这些都是轻松应对，在本节的其余部分中介绍。

### <a name="base-configuration"></a>基本配置

然后，在 WebApiConfig.cs，您可以替换︰

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

使用

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] 如果您想了解有关新的.NET server SDK 以及如何从您的应用程序的添加/删除功能的详细信息，请参阅[如何使用.NET server SDK]主题。

如果您的应用程序可以使用身份验证功能，您需要注册 OWIN 中间件。 在这种情况下，应将上述配置代码移到一个新的 OWIN 启动类。

1. NuGet 程序包中添加`Microsoft.Owin.Host.SystemWeb`如果未包括在您的项目。
2. 在 Visual Studio 中，右键单击项目并选择**添加** -> **新项**。 选择**Web** -> **常规** -> **OWIN 启动类**。
3. 将上面的代码移动的 MobileAppConfiguration 从`WebApiConfig.Register()`到`Configuration()`新启动类的方法。

请确保`Configuration()`方法结尾︰

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

有与身份验证相关的在下面的完整的身份验证一节中介绍的其他更改。

### <a name="working-with-data"></a>处理数据

中移动服务的移动应用程序名担任实体框架设置中的默认架构名称。

若要确保您已经被引用前，使用以下设置对您应用程序 DbContext 中的架构相同的架构︰

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

请确保已设置如果您执行上述操作外的 MS_MobileServiceName。 如果您的应用程序自定义这以前，还可以提供另一种架构名称。

### <a name="system-properties"></a>系统属性

#### <a name="naming"></a>命名

在 Azure 移动服务服务器 SDK 中，系统属性始终包含双下划线 (`__`) 的属性的前缀︰

- __createdAt
- __updatedAt
- __deleted
- __version

移动服务客户端 Sdk 具有分析在此格式中的系统属性的特殊逻辑。

在 Azure 的移动应用程序，系统属性不再具有特殊格式，并具有以下名称︰

- createdAt
- updatedAt
- 删除
- 版本

移动应用程序客户端 Sdk 使用新的系统属性名称，因此需要对客户端代码没有更改。 但是，如果您要直接调用其余部分与您的服务然后您应查询相应地更改。

#### <a name="local-store"></a>本地存储

系统属性的名称的更改意味着移动服务脱机同步本地数据库不兼容的移动应用程序。 如果可能，应避免升级客户端应用程序从手机服务直到之后的移动应用程序挂起的更改已发送到服务器。 然后，升级后的应用程序应使用新的数据库文件名。

如果有挂起的脱机更改操作队列中时，客户端应用程序到移动应用程序升级从移动服务中，然后必须更新系统数据库以使用新的列名称。 在 iOS 中，这可以实现使用轻量迁移更改列的名称。 在 Android 和.NET 托管客户端，应编写自定义 SQL 重命名数据对象表的列。

在 iOS 中，您应该更改为您的数据实体，以匹配以下核心数据架构。 请注意，属性`createdAt`， `updatedAt` ，`version`不再有`ms_`前缀︰

| 属性 |  类型   | 请注意                                                 |
|---------- |  ------ | -----------------------------------------------------|
| 标识        | 标记为必需的字符串  | 在远程存储中的主关键字         |
| createdAt | 日期    | （可选） 映射到 createdAt 系统属性         |
| updatedAt | 日期    | （可选） 映射到 updatedAt 系统属性         |
| 版本   | 字符串  | （可选） 用于检测冲突，映射到的版本 |

#### <a name="querying-system-properties"></a>查询系统属性

在 Azure 移动服务，系统属性将不发送默认情况下，但仅当被请求使用查询字符串`__systemProperties`。 与此相反，在 Azure 移动应用程序系统中属性是**始终处于选中状态**因为它们是服务器 SDK 对象模型的一部分。

此更改主要影响的域管理器，如扩展的自定义实现`MappedEntityDomainManager`。 在移动服务，客户端永远不会要求任何系统属性中，它是否可以使用`MappedEntityDomainManager`，实际上未映射的所有属性。 但是，在 Azure 的移动应用程序，这些未映射的属性将导致 GET 查询时出错。

若要解决此问题的最简单方法是修改您的 Dto，以便他们继承`ITableData`而不是`EntityData`。 然后，添加`[NotMapped]`应忽略的字段特性。

例如，下列定义`TodoItem`没有系统属性︰

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

注意︰ 如果您收到错误消息`NotMapped`，添加对该程序集的引用`System.ComponentModel.DataAnnotations`。

### <a name="cors"></a>CORS

移动服务由环绕 ASP.NET CORS 解决方案包括 CORS 一些支持。 这环绕一层已被删除，使开发人员更多的控制，以便您可以直接利用[ASP.NET CORS 支持](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)。

如果使用 CORS 考虑的主要方面是︰ `eTag` ，`Location`标头必须允许客户端 Sdk 才能正常工作。

### <a name="push-notifications"></a>推式通知
对于推送，您可能会发现缺少服务器 SDK 中的主要项目是 PushRegistrationHandler 类。 默认情况下启用了对于无标记的登记和登记在移动应用程序，处理稍有不同。 管理标签可能会使用自定义的 Api 来实现。 请[注册标记](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags)有关的详细信息，参阅。

### <a name="scheduled-jobs"></a>计划的作业
计划的作业不内置了移动应用程序，因此您有在您的.NET 后端的任何现有作业需要分别升级。 一种选择是移动应用程序代码网站上创建[Web 作业]计划的。 此外可以设置控制器包含作业代码并配置[Azure 计划程序]按预期的计划上该终结点。

### <a name="miscellaneous-changes"></a>其他更改
现在必须具有其消耗的移动客户端的所有 ApiControllers`[MobileAppController]`属性。 这不再是默认情况下包含转受移动格式化程序，以便其他 ApiControllers。

`ApiServices`对象不再是 SDK 的一部分。 若要访问移动的应用程序设置，您可以使用︰

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

同样，日志记录是现在通过标准的 ASP.NET 跟踪书写︰

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>身份验证的注意事项

移动服务的身份验证组件现在已移到应用程序服务的身份验证/授权功能。 您可以了解有关启用此选项，为您的网站[添加到您的移动应用程序的身份验证](app-service-mobile-ios-get-started-users.md)主题阅读。

对于某些提供程序，例如 AAD、 Facebook 中和 Google，应该是能够利用现有注册复制应用程序。 您只需导航到标识提供商的门户，并将新的重定向 URL 添加到注册。 然后配置应用程序服务的身份验证/授权的客户端 ID 和密码。

### <a name="controller-action-authorization"></a>控制器的操作的授权
任何实例的`[AuthorizeLevel(AuthorizationLevel.User)]`现在必须将属性更改为使用标准 ASP.NET`[Authorize]`属性。 此外，控制器现在是匿名，默认情况下，如在其他 ASP.NET 应用程序。
如果您正在使用的另一个 AuthorizeLevel 选项，如管理或应用程序中，请注意，这些都是不见了。 而是可以将 AuthorizationFilters 设置为共享的机密或配置以便安全地到服务调用 AAD 服务主体。

### <a name="getting-additional-user-information"></a>获取附加的用户信息

您可以获得附加的用户信息，包括访问令牌通过`GetAppServiceIdentityAsync()`方法︰

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

此外，如果您的应用程序依赖项用户 Id，如将它们存储在数据库中，则一定要注意之间移动服务和应用程序服务移动应用程序的用户 Id 的不同。 您仍然可以移动服务用户 ID，但。 所有 ProviderCredentials 子类都具有用户 Id 属性。 因此在继续之前的示例︰

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

如果您的应用程序会在用户 Id 的任何依赖项，很重要，您要利用同一注册与标识提供程序如果可能的话。 用户 Id 通常限于所使用的应用程序注册，因此引入新的注册无法创建匹配的用户对其数据的问题。

### <a name="custom-authentication"></a>自定义身份验证

如果您的应用程序使用的自定义身份验证解决方案，要确保已升级的网站具有对系统的访问。 按照新的[.NET 服务器 SDK 概述]中的自定义身份验证集成解决方案。 请注意，自定义身份验证组件仍在预览中。

##<a name="updating-clients"></a>更新客户机
一旦操作移动应用程序的后端，您可以处理使用该客户端应用程序的新版本。 移动应用程序还包括新版客户端 Sdk，以及类似于上面的服务器升级，您将需要安装的移动应用程序版本之前删除所有引用到移动服务 Sdk。

在版本之间的主要变化之一是构造函数不再需要的应用程序键。 您现在只需通过移动应用程序的 URL 中。 例如，在.NET 客户端，`MobileServiceClient`构造函数现︰

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

您可以阅读有关安装新的 Sdk 和使用新结构通过下面的链接︰

- [iOS 3.0.0 版或更高版本](app-service-mobile-ios-how-to-use-client-library.md)
- [(Windows/Xamarin) 版本 2.0.0.NET 或更高版本](app-service-mobile-dotnet-how-to-use-client-library.md)

如果您的应用程序可使发生了一些变化存在以及使用推式通知，请记下的每个平台中，特定的注册指令。

当准备好新的客户端版本时，试一下针对您已升级的服务器项目。 验证之后，它才有效，可以发布到客户应用程序的新版本。 最终，一旦客户有机会接收这些更新，您只能删除您的应用程序的移动服务版本。 此时，您已完全升级到使用最新的移动应用程序服务器 SDK 应用程序服务移动应用程序。

<!-- URLs. -->

[Azure 门户]: https://portal.azure.com/
[Azure 的传统门户网站]: https://manage.windowsazure.com/
[移动应用程序有哪些？]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[移动应用服务器 SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure 计划程序]: /en-us/documentation/services/scheduler/
[Web 作业]: ../app-service-web/websites-webjobs-resources.md
[如何使用.NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[应用程序服务的定价]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET 服务器 SDK 概述]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
