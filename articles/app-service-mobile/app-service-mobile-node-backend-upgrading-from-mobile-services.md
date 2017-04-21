<properties
    pageTitle="从移动服务升级到 Azure 应用程序服务的 Node.js"
    description="了解如何轻松地升级移动服务应用程序与应用程序服务移动应用程序"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>将您现有的 Node.js Azure 移动服务升级为应用程序服务

应用程序服务移动是一种生成使用 Microsoft Azure 的移动应用程序的新方法。 若要了解详细信息，请参阅[手机应用程序是什么？]。

本主题介绍如何将现有的 Node.js 后端应用程序从 Azure 移动服务升级到新的应用程序服务移动应用程序。 虽然执行此升级时，现有的移动服务应用程序可以继续运行。  如果您需要升级 Node.js 后端应用程序，请参阅[升级您的.NET 移动服务](./app-service-mobile-net-upgrading-from-mobile-services.md)。

当移动端升级到 Azure 应用程序服务时，它有权访问所有应用程序服务功能和根据[应用程序服务的定价]，不移动服务定价计费方式。

## <a name="migrate-vs-upgrade"></a>迁移和升级

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] 我们建议，在[执行迁移](app-service-mobile-migrating-from-mobile-services.md)之前经过升级。 这种方式，可以将两个版本的应用程序放在同一个应用程序服务计划并会导致没有额外的费用。

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>移动应用程序 Node.js 服务器 SDK 中的改进

升级到新的[移动应用程序 SDK](https://www.npmjs.com/package/azure-mobile-apps)提供了大量的改进，其中包括︰

- 根据[表达框架](http://expressjs.com/en/index.html)，新节点 SDK 是细线，用来跟上新节点版本就。 您可以自定义与速成中间件应用程序的行为。

- 与移动服务 SDK 比较显著的性能改进。

- 您现在可以承载网站以及您移动的后端;与此类似，很容易地添加到任何现有的 express.v4 应用程序的 Azure 移动 SDK。

- 生成用于跨平台和本地开发，移动应用程序 SDK 可以开发，并在本地运行在 Windows 和 Linux，OSX 的平台上。 现在是简单易用的公共节点开发技术像[Mocha](https://mochajs.org/)测试在部署之前运行。

## <a name="overview"></a>升级概述

以帮助升级 Node.js 后端，Azure 应用程序服务提供了兼容性软件包。  升级后，您必须将部署到新的应用程序服务网站 niew 网站。

移动服务客户端 Sdk 不**是**兼容的新的移动应用程序服务器 SDK。 为了使您的应用程序的服务的连续性，不应将更改发布到当前为已发布的客户端提供服务的网站。 相反，您应该创建新的移动应用程序作为一个重复。 可以将此应用程序放置在相同的应用程序服务计划，以避免招致更多的财务成本。

然后将有两个版本的应用程序︰ 一种保持一致并提供发布应用程序在疯狂状态，及其它然后可以升级和新的客户端版本的目标。 您可以移动并测试您的代码在您的节奏，但应确保所做的任何 bug 修复会应用到两个。 一旦您觉得需的大量处于放任状态的应用程序已更新为最新版本的客户端，可以删除原始迁移应用程序根据需要。 它不会带来任何额外的货币成本，如果承载在同一应用程序服务计划为您的移动应用程序。

升级过程的完整轮廓如下所示︰

1. 下载您现有的 （迁移的） Azure 移动服务。
2. 将项目转换为使用兼容性软件包 Azure 移动应用程序。
3. 更正 （例如，身份验证设置） 的任何差异。
4. 将转换后的 Azure 的移动应用程序项目部署到新的应用程序服务。
4. 发布新版本的客户端应用程序使用新的移动应用程序。
5. （可选）删除原始迁移的移动服务应用程序。

原迁移移动服务上看不到任何通信时，会发生删除操作。

## <a name="install-npm-package"></a>安装系统必备

您应该在您的本地计算机上安装 [节点]。  此外应安装兼容包。  节点安装后，您可以从新命令或 PowerShell 提示符下运行以下命令︰

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>获取移动的 Azure 服务脚本

- 登录到[Azure 的门户]。
- 使用**的所有资源**或**应用程序服务**，查找您的移动服务站点。
- 在站点内，单击**工具** -> **Kudu** -> **转**到打开的 Kudu 网站。
- **调试控制台**上单击 -> **PowerShell**打开调试控制台。
- 定位到`site/wwwroot/App_Data/config`通过依次单击每个目录
- 单击下载图标旁边`scripts`目录。

这将下载 ZIP 格式中的脚本。  在您的本地计算机上创建新的目录并解压缩`scripts.ZIP`目录中的文件。  这将创建`scripts`目录。

## <a name="scaffold-app"></a>构架新 Azure 移动应用程序的后端

从包含脚本目录的目录运行以下命令︰

```scaffold-mobile-app scripts out```

这将创建在基架的 Azure 移动应用程序端`out`目录。  尽管没有要求，它是一个好主意，检查`out`到您选择的源代码储存库的目录。

## <a name="deploy-ama-app"></a>部署您 Azure 移动应用程序的后端

在部署期间，您需要执行下列操作︰

1. 在[Azure 门户网站]中创建新的移动应用程序。
2. 运行`createViews.sql`您连接的数据库上的脚本。
3. 链接链接到您的移动服务为您的新应用程序服务数据库。
4. 链接到新的应用程序服务的任何其他资源 （例如通知集线器）。
5. 将生成的代码部署到您的新站点。

### <a name="create-a-new-mobile-app"></a>创建新的移动应用程序

1. 在[Azure 门户]登录。

2. 单击**新建 +** > **Web + 移动** > **移动应用程序**，然后提供移动应用程序端的名称。

3. **资源组**中，选择一个现有的资源组，或创建一个新 （为您的应用程序使用相同的名称）。 
 
    您可以选择另一个应用程序服务计划，或新建一个。 应用程序服务的详细信息计划，以及如何在不同的定价中创建一个新的计划层并在所需的位置，请参阅[Azure 应用程序服务计划深度探讨](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。

4. **应用程序服务计划**中，默认的计划 （在[标准层](https://azure.microsoft.com/pricing/details/app-service/)） 处于选中状态。 您还可以选择不同的计划，或[新建一个](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)。 应用程序服务计划的设置确定的[位置的功能，成本和计算资源](https://azure.microsoft.com/pricing/details/app-service/)与应用程序相关联。 

    该计划确定后，请单击**创建**。 这将创建移动应用程序的后端。 


### <a name="run-createviewssql"></a>运行 CreateViews.SQL

基架的应用程序包含一个名为文件`createViews.sql`。  必须对目标数据库执行该脚本。  目标数据库的连接字符串可以从您的**连接字符串****设置**刀片式服务器从迁移的移动服务。  它被命名为`MS_TableConnectionString`。

您可以运行 SQL Server 管理 Studio 或 Visual Studio 内的从该脚本。

### <a name="link-the-database-to-your-app-service"></a>链接到您的应用程序服务数据库

将现有数据库链接到您的应用程序服务︰

- 在[Azure 门户]中，打开您的应用程序服务。
- 选择**的所有设置** -> **的数据连接**。
- 单击**添加**。
- 在下拉列表，选择**SQL 数据库**
- 在**SQL 数据库**中，选择您现有的数据库，然后单击**选择**。
- 在**连接字符串**中，对于数据库，输入用户名和密码，然后单击**确定**。
- 在**添加数据连接**刀片式服务器，请单击**确定**。

通过查看迁移的移动服务中的目标数据库的连接字符串，可以找到的用户名和密码。


### <a name="set-up-authentication"></a>设置身份验证

Azure 的移动应用程序允许您配置 Azure 活动目录、 Facebook、 Google、 Microsoft 和 Twitter 身份验证服务中。  自定义身份验证需要单独开发。  请参阅[身份验证概念]文档和[验证快速入门]文档以了解更多信息。  

## <a name="updating-clients"></a>更新移动客户端

一旦操作移动应用程序的后端，您可以处理使用该客户端应用程序的新版本。 移动应用程序还包括新版客户端 Sdk，以及类似于上面的服务器升级，您将需要安装的移动应用程序版本之前删除所有引用到移动服务 Sdk。

在版本之间的主要变化之一是构造函数不再需要的应用程序键。 您现在只需通过移动应用程序的 URL 中。 例如，在.NET 客户端，`MobileServiceClient`构造函数现︰

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

您可以阅读有关安装新的 Sdk 和使用新结构通过下面的链接︰

- [Android 版本 2.2 或更高版本](app-service-mobile-android-how-to-use-client-library.md)
- [iOS 3.0.0 版或更高版本](app-service-mobile-ios-how-to-use-client-library.md)
- [(Windows/Xamarin) 版本 2.0.0.NET 或更高版本](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova 2.0 或更高版本](app-service-mobile-cordova-how-to-use-client-library.md)

如果您的应用程序可使发生了一些变化存在以及使用推式通知，请记下的每个平台中，特定的注册指令。

当准备好新的客户端版本时，试一下针对您已升级的服务器项目。 验证之后，它才有效，可以发布到客户应用程序的新版本。 最终，一旦客户有机会接收这些更新，您只能删除您的应用程序的移动服务版本。 此时，您已完全升级到使用最新的移动应用程序服务器 SDK 应用程序服务移动应用程序。

<!-- URLs. -->

[Azure 门户]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[移动应用程序有哪些？]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[应用程序服务的定价]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[身份验证概念]: ../app-service/app-service-authentication-overview.md
[身份验证快速入门]: app-service-mobile-auth.md

[Azure 门户]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
