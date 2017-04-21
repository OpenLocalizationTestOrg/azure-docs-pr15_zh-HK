<properties
    pageTitle="如何使用 SDK Node.js 后端服务器的移动应用程序 |Azure 应用程序服务"
    description="了解如何使用 SDK Node.js 后端服务器的 Azure 应用程序服务移动应用程序。"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>如何使用 Azure 移动应用程序 Node.js SDK

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

本文提供了详细的信息和示例来演示如何使用 Node.js 端在 Azure 应用程序服务移动应用程序。

## <a name="Introduction"></a>介绍

Azure 应用程序服务移动应用程序提供了将移动优化数据访问 Web API 添加到 web 应用程序的功能。  为 ASP.NET 和 Node.js web 应用程序提供了 Azure 应用程序服务移动应用程序 SDK。  该 SDK 提供了以下操作︰

- 用于数据访问的表操作 （读取、 插入、 更新、 删除）
- 自定义 API 操作

这两种操作提供跨所有标识提供程序 Azure 应用程序服务，包括例如 Facebook、 Twitter、 Google 和 Microsoft 以及 Azure Active Directory 为企业标识的社会身份提供程序所允许的身份验证。

您可以查找[在 GitHub 上的示例目录]中每个用例样本。

## <a name="supported-platforms"></a>支持的平台

Azure 移动应用程序节点 SDK 支持当前 LTS 版本中的节点和更高版本。  截稿时止，最新的 LTS 版本的 v4.5.0 节点。  其他版本的节点可能会使用，但不是支持。

Azure 移动应用程序节点 SDK 支持两个数据库驱动程序-节点 mssql 驱动程序支持 SQL Azure 和本地 SQL Server 实例。  Sqlite3 驱动程序支持 SQLite 数据库上的单个实例。

### <a name="howto-cmdline-basicapp"></a>如何︰ 创建基本 Node.js 后端使用命令行

每个 Azure 应用程序服务移动应用程序 Node.js 后端为 ExpressJS 应用程序启动。  ExpressJS 是用于 Node.js 最流行 web 服务框架。  您可以创建一个简单[快速]的应用程序，如下所示︰

1. 在命令行或 PowerShell 窗口，创建一个目录，用于您的项目。

        mkdir basicapp

2. 运行 npm 初始化来初始化的包结构。

        cd basicapp
        npm init

    Npm init 命令提出一组问题来初始化项目。  请参阅示例输出︰

    ![Npm 初始化输出][0]

3. 从 npm 资料库中安装快递和 azure 的移动应用库。

        npm install --save express azure-mobile-apps

4. 创建要实现基本的移动服务器的 app.js 文件。

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

此应用程序创建具有一个终结点的移动优化的 WebAPI (`/tables/TodoItem`) 提供对基础 SQL 的数据存储区使用动态架构未经身份验证的访问。  它是适用于以下客户端库快速启动︰

- [Android 的客户端快速入门]
- [Apache Cordova 客户端快速入门]
- [iOS 客户端快速入门]
- [Windows 应用商店的客户端快速入门]
- [Xamarin.iOS 客户端快速入门]
- [Xamarin.Android 客户端快速入门]
- [Xamarin.Forms 客户端快速入门]

您可以[在 GitHub 上的 basicapp 示例]中此基本应用程序中找到的代码。

### <a name="howto-vs2015-basicapp"></a>如何︰ 使用 Visual Studio 2015年创建节点后端

Visual Studio 2015年需要扩展开发 Node.js IDE 内的应用程序。  若要开始，安装[的 Visual Studio 的 Node.js 工具 1.1]。  一旦安装了 Visual Studio Node.js 工具，创建快速 4.x 版应用程序︰

1. 打开**新建项目**对话框 (从**文件** > **New** > **项目...**)。

2. 展开**模板** > **JavaScript** > **Node.js**。

3. 选择**基本的 Azure Node.js 快车 4 应用程序**。

4. 在项目名称来填充。  单击*确定*。

    ![Visual Studio 2015年新项目][1]

5. 用鼠标右键单击**npm**节点并选择**安装新 npm 包...**。

6. 您可能需要刷新 npm 目录上创建首个 Node.js 应用程序。  如有必要，请单击**刷新**。

7. 在搜索框中输入_应用程序移动 azure 的_。  单击**azure 移动应用 2.0.0**包，然后单击**安装程序包**。

    ![安装新 npm 软件包][2]

8. 单击**关闭**。

9. 打开要添加支持的 Azure 移动应用程序 sdk 的_app.js_文件。  在行 6 在磁带库底部需要语句，添加以下代码︰

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    在大约行 27 其他 app.use 语句之后，添加以下代码︰

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    保存该文件。

10. 运行该应用程序本地 （在 http://localhost:3000 提供的 API），或者发布到 Azure。

### <a name="create-node-backend-portal"></a>如何︰ 创建使用 Azure 门户 Node.js 后端

可以在[Azure 的门户网站]中创建移动应用程序的后端权限。 可以按照以下步骤操作，也可以按照[创建移动应用程序](app-service-mobile-ios-get-started.md)教程一起创建客户端和服务器。 本教程包含这些指令的简化的版本，最适合用来证明概念的项目。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

在_开始_刀片式服务器，**创建表 API**下, 作为**后端语言**选择**Node.js** 。 复选框"**我确认，这将覆盖所有站点内容。**"，然后单击**创建的 TodoItem 表**。

### <a name="download-quickstart"></a>如何︰ 使用 Git Node.js 后端快速入门代码项目下载

可以使用门户**快速入门**刀片 Node.js 移动应用程序的后端，Node.js 项目是为您创建并部署到您的网站。 您可以添加表和 Api 和编辑门户中 Node.js 后端的代码文件。 您可以使用各种部署工具下载后端项目，以便您可以添加或修改表和 Api，然后重新发布该项目。 有关详细信息，请参阅[Azure 应用程序服务部署指南 》]。 下面的过程使用 Git 存储库下载快速入门项目代码。

1. 安装 Git，如果还没有这么做。 Git 安装所需的步骤因操作系统而异。 有关特定操作系统版本和安装指南，请参阅[安装 Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) 。

2. 按照[启用应用程序服务的应用程序存储库](../app-service-web/app-service-deploy-local-git.md#Step3)启用后端站点，部署用户名和密码的︰ 记下的 Git 存储库中的步骤。

3. 在您移动应用程序的后端的刀片，记下的**Git 克隆 URL**设置。

4. 执行`git clone`使用 Git 的命令复制 URL，并输入您的密码，当需要时，如以下示例所示︰

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. 浏览到本地目录中，这在前面的示例中是 /todolist，并注意到已下载的项目文件。 查找`todoitem.json`文件以`/tables`目录。  此文件在表上定义的权限。  此外可以找到`todoitem.js`文件所在的目录，定义该 CRUD 操作中的表脚本。

6. 对项目文件进行更改后，执行以下命令以添加，提交，然后将所做的更改上载到站点︰

        $ git commit -m "updated the table script"
        $ git push origin master

    当向项目添加新文件时，您首先需要执行`git add .`命令。

每次一组新的提交被推送到该站点，该站点会重新发布。

### <a name="howto-publish-to-azure"></a>如何︰ 发布到 Azure 的 Node.js 端

Microsoft Azure 提供发布 Azure 应用程序服务移动应用程序 Node.js 端到 Azure 服务的许多机制。  这些功能包括使用集成到 Visual Studio 的部署工具、 命令行工具和基于源代码的连续部署选项。  有关此主题的详细信息，请参阅[Azure 应用程序服务部署指南 》]。

Azure 应用程序服务具有 Node.js 应用程序在部署之前应检查的具体建议︰

- 如何[指定节点版本]
- 如何[使用节点模块]

### <a name="howto-enable-homepage"></a>如何︰ 启用应用程序的主页

许多应用程序是 web 和移动应用程序组合和 ExpressJS 框架允许您合并两个方面。  有时，但是，您可能希望仅实现移动界面。  它可用于提供登陆页以确保应用程序服务已启动并正在运行。  可以提供您自己的主页，也可以启用临时的主页。  要启用临时的主页上，请使用以下方法来实例化 Azure 移动应用程序︰

    var mobile = azureMobileApps({ homePage: true });

如果开发本地时，只想用此选项，您可以添加到此设置您`azureMobile.js`文件。

## <a name="TableOperations"></a>表操作 

应用程序移动 azure 的 Node.js 服务器 SDK 提供了机制，以公开在 Azure WebAPI 的 SQL 数据库中存储的数据表格。  提供了五个操作。

| 操作 | 说明 |
| --------- | ----------- |
| 获得 /tables/_表名_ | 获取表中的所有记录 |
| 获得 /tables/_表名_/:id | 获得表中的特定记录 |
| 开机自检 /tables/_表名_ | 在表中创建记录 |
| 修补程序 /tables/_表名_/:id | 更新表中的记录 |
| 删除 /tables/_表名_/:id | 删除表中的记录 |

此 WebAPI 支持[OData]并扩展支持[脱机数据同步]的表架构。

### <a name="howto-dynamicschema"></a>如何︰ 定义使用动态架构表

可以使用一个表之前，必须定义它。  可以定义表，使用静态架构 （在其中开发人员定义架构中的列） 或动态 （其中 SDK 控制基于传入请求的架构）。 此外，开发人员可以添加的 Javascript 代码来定义控制 WebAPI 的特定的方面。

作为最佳实践，应该表目录中的 Javascript 文件中定义的每个表，然后使用 tables.import() 方法来导入的表。  扩展基本应用程序，则会调整 app.js 文件︰

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

定义在表。 / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

默认情况下，表使用动态架构。  要全局关闭动态架构，设置为 false Azure 门户中的应用程序设置**MS_DynamicSchema** 。

[托多在 GitHub 上的示例]中，您可以找到一个完整的示例。

### <a name="howto-staticschema"></a>如何︰ 定义使用静态架构表

您可以显式定义要公开通过 WebAPI 的列。  应用程序移动 azure 的 Node.js SDK 将自动添加到您提供的列表的脱机数据同步所需的任何其他列。  例如，快速启动客户端应用程序需要具有两列的表︰ 文本 （字符串），并完成 （布尔值）。  
可以在表定义 JavaScript 文件 （位于表目录中），如下所示定义的表︰

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

如果静态定义的表，然后还必须调用 tables.initialize() 方法，以在启动时创建数据库架构。  Tables.initialize() 方法返回一个[承诺]，以便 web 服务不会不为请求提供服务之前要初始化的数据库。

### <a name="howto-sqlexpress-setup"></a>如何︰ 使用 SQL Express 作为开发数据存储在本地计算机上

Azure 移动应用程序 AzureMobile 应用程序节点 SDK 提供了现成的服务数据的三个选项︰ SDK 提供了现成的服务数据的三个选项︰

- 使用**内存**的驱动程序来提供这种非永久性的示例存储
- 使用**mssql**驱动程序的开发提供直通 SQL 的数据存储区
- 使用**mssql**驱动程序为生产提供的 SQL Azure 数据库数据存储库

Azure 移动应用程序 Node.js SDK 时使用[mssql Node.js 包]来建立和使用与 SQL Express 和 SQL 数据库的连接。  此软件包要求您在您的 SQL Express 实例上启用 TCP 连接。

> [AZURE.TIP]内存的驱动程序不提供一套完整的用于测试的设施。  如果您想要测试您的后端本地，我们建议使用的 SQL Express 数据存储区和 mssql 驱动程序。

1. 下载并安装[Microsoft SQL Server 2014年表达]。  确保您安装 SQL Server 2014年表达工具版本。  除非明确需要 64 位支持，当运行 32 位版本会消耗更少的内存。

2. 运行 SQL Server 2014年配置管理器。

  1. 展开左边树状结构菜单中的**SQL Server 网络配置**节点。
  2. 单击**为 SQLEXPRESS 的协议**。
  3. **TCP/IP**用鼠标右键单击并选择**启用**。  在弹出对话框中单击**确定**。
  4. **TCP/IP**用鼠标右键单击并选择**属性**。
  5. 请单击**IP 地址**选项卡。
  6. **IPAll**节点下找到。  在**TCP 端口**字段中，输入**1433年**。

         ![Configure SQL Express for TCP/IP][3]

  7. 单击**确定**。  在弹出对话框中单击**确定**。
  8. 单击左边树状结构菜单中的**SQL Server 服务**。
  9. 用鼠标右键单击**SQL Server (SQLEXPRESS)**并选择**重新启动**
  10. 关闭 SQL Server 2014年配置管理器。

3. 运行 SQL Server 2014年管理工作室并连接到您的本地 SQL Express 实例

  1. 用鼠标右键单击在对象资源管理器实例，然后选择**属性**
  2. 选择**安全**页。
  3. 确保选择了**SQL Server 和 Windows 身份验证模式**
  4. 单击**确定**

        ![配置 SQL 直通身份验证][4]

  5. 展开**安全性** > 对象资源管理器中的**登录名**
  6. 右键单击**登录**并选择**新建登录...**
  7. 输入登录名。  选择**SQL Server 身份验证**。  输入密码，然后在**确认密码**中输入相同的密码。  密码必须符合复杂性要求 Windows。
  8. 单击**确定**

        ![将新用户添加到 SQL 直通][5]

  9. 用鼠标右键单击您的新登录名，然后选择**属性**
  10. 选择**服务器角色**页
  11. **Dbcreator**服务器角色旁边的复选框
  12. 单击**确定**
  13. 关闭 SQL Server 2015年管理工作室

确保记录的用户名和密码，选择。  您可能需要指定其他服务器角色或权限，具体取决于特定的数据库要求。

Node.js 应用程序读取此数据库的连接字符串**SQLCONNSTR_MS_TableConnectionString**环境变量。  在您的环境中，您可以设置此变量。  例如，您可以使用 PowerShell 若要设置此环境变量︰

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

通过 TCP/IP 连接访问数据库，并提供用于连接的用户名和密码。

### <a name="howto-config-localdev"></a>如何︰ 配置为本地开发项目

Azure 的移动应用程序读取 JavaScript 文件从本地文件系统调用_azureMobile.js_ 。  不使用此文件来配置在生产中的 Azure 移动应用程序 SDK-改为使用[Azure 门户]中的应用程序设置。  _AzureMobile.js_文件应导出的配置对象。  最常用的设置是︰

- 数据库设置
- 诊断日志记录设置
- 备用 CORS 设置

实施上述数据库设置_azureMobile.js_文件示例如下所示︰

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

我们建议您将_azureMobile.js_添加到_.gitignore_文件 （或其他源代码管理忽略文件） 来防止密码存储在云中。  总是在[Azure 门户]中的应用程序设置中配置生产设置。

### <a name="howto-appsettings"></a>如何︰ 配置应用程序设置为您的移动应用程序

在_azureMobile.js_文件中的大多数设置[Azure 门户]中具有相当的应用程序设置。  若要在应用程序设置中配置您的应用程序，请使用以下列表︰

| 应用程序设置                 | _azureMobile.js_设置  | 说明                               | 有效的值                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | 名称                      | 该应用程序的名称                       | 字符串                                      |
| **MS_MobileLoggingLevel**   | logging.level             | 可记录的消息的最小日志级别      | 错误、 警告、 信息、 详细、 调试、 很愚蠢 |
| **MS_DebugMode**            | 调试                     | 启用或禁用调试模式              | 为 true，假                                 |
| **MS_TableSchema**          | data.schema               | 默认的 SQL 表的架构名        | 字符串 (默认︰ dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | 启用或禁用调试模式              | 为 true，假                                 |
| **MS_DisableVersionHeader** | 版本 （设置为未定义）| 禁用 X ZUMO 服务器版本标头 | 为 true，假                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | 禁用客户端 API 版本检查     | 为 true，假                                 |

若要设置应用程序设置︰

1. 登录到[Azure 的门户]。
2. 选择**所有资源**或**应用程序服务**，然后单击您的移动应用程序的名称。
3. 默认情况下打开刀片式服务器的设置。 如果没有，请单击**设置**。
4. 在常规菜单中单击**应用程序设置**。
5. 滚动到应用程序设置部分。
6. 如果您的应用程序设置已经存在，请单击要编辑的值的应用程序设置的值。
7. 如果您的应用程序设置不存在，请在关键字框和值框中的值输入应用程序设置。
8. 完成后，单击**保存**。

更改大部分应用程序设置需要重新启动服务后。

### <a name="howto-use-sqlazure"></a>如何︰ 使用 SQL 数据库作为生产数据存储

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

在所有的 Azure 应用程序服务应用程序类型，SQL Azure 数据库用作数据存储区是相同的。 如果您有没有完成此操作，请按照下列步骤创建移动应用程序的后端。

1. 登录到[Azure 的门户]。

2. 在顶部窗口的左侧，单击**+ 新建**按钮 > **Web + 移动** > **移动应用程序**，然后提供移动应用程序端的名称。

3. 在**资源组**框中，输入您的应用程序相同的名称。

4. 选择默认的应用程序服务计划。  如果您想更改您的应用程序服务的计划，您可以做到通过单击应用程序服务计划 > **+ 创建新**。  提供新的应用程序服务计划的名称并选择一个合适的位置。  单击定价层然后选择适当的定价层服务。 选择**所有的视图**以查看更多的定价选项，如**免费**和**共享**。  一旦选择了定价层，请单击**选择**按钮。  返回在**应用程序服务计划**刀片式服务器，请单击**确定**。

5. 单击**创建**。 移动应用程序的后端资源调配可以花几分钟的时间。  一旦移动应用程序的后端配置，门户打开**设置**刀片式服务器移动应用程序的后端。

移动应用程序的后端创建后，您可以选择连接到您的移动应用程序端的现有 SQL 数据库或创建新的 SQL 数据库。  在本节中，我们将创建一个 SQL 数据库。

> [AZURE.NOTE]如果中移动应用程序的后端的同一个位置已经有一个数据库，可以选择**使用现有的数据库**，然后选择该数据库。 由于较长的延迟不建议使用一个不同的位置中的数据库。

6. 单击**设置**中的新的移动应用程序后端， > **移动应用程序** > **数据** > **+ 添加**。

7. 在**添加数据连接**刀片式服务器，请单击**SQL 数据库的配置所需的设置** > **创建一个新数据库**。  在**名称**字段中输入新数据库的名称。

8. 单击**服务器**。  在**新的服务器**刀片式服务器，在**服务器名称**字段中输入一个唯一的服务器名称并提供合适的**服务器管理登录名**和**密码**。  确保已选中**允许 azure 服务来访问服务器**。  单击**确定**。

    ![创建 SQL Azure 数据库][6]

9. 在**新数据库**刀片式服务器，请单击**确定**。

10. 在**添加数据连接**刀片式服务器，选择**连接字符串**，输入登录名和密码，则在创建数据库时。  如果使用现有的数据库，该数据库提供的登录凭据。  输入后，单击**确定**。

11. 再次**添加数据连接**刀片式服务器上，单击**确定**以创建数据库。

<!--- END OF ALTERNATE INCLUDE -->

创建数据库可能需要几分钟。  使用**通知**区域来监视部署的进度。  已成功部署数据库之前没有进行。  成功部署后，将为您移动的后端应用程序设置中的 SQL 数据库实例创建连接字符串。  您可以看到此应用程序设置中**设置** > **应用程序设置** > **的连接字符串**。

### <a name="howto-tables-auth"></a>如何︰ 对表进行访问要求身份验证

如果您想要使用与表终结点的应用程序服务的身份验证，必须在第一次在[Azure 门户]配置应用程序服务的身份验证。  在 Azure 应用程序服务中配置身份验证的详细信息，您打算使用的标识提供程序查看配置指南︰

- [如何配置 Azure 活动目录身份验证]
- [如何配置 Facebook 的身份验证]
- [如何配置 Google 身份验证]
- [如何配置 Microsoft 身份验证]
- [如何配置使用 Twitter，身份验证]

每个表都有一个访问属性，可用于控制对表的访问。  下面的示例演示使用身份验证所需的静态定义的表。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

在访问属性可以接受三个值之一

  - 客户端应用程序可以读取数据而无需身份验证*匿名*指示
  - *经过身份验证的*客户端应用程序必须发送请求的有效的身份验证令牌指示
  - *禁用*指示此表当前已被禁用

如果 access 属性未定义，则允许未经身份验证的访问。

### <a name="howto-tables-getidentity"></a>如何︰ 使用身份验证声明表

您可以设置被请求的身份验证设置时的各种索赔。  这些说法不是正常情况下可通过`context.user`对象。  但是，它们可以使用检索`context.user.getIdentity()`方法。  `getIdentity()`方法返回解析为对象的一个承诺。  对象键由 facebook、 google、 twitter、 microsoftaccount，（aad） 的身份验证方法。

例如，如果您设置了 Microsoft 的帐户验证并且请求的电子邮件地址声称，您可以添加的电子邮件地址记录与以下表控制器︰

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

若要查看可用何种索赔，使用 web 浏览器来查看`/.auth/me`的网站终结点。

### <a name="howto-tables-disabled"></a>如何︰ 禁用对特定表操作的访问

除了在表上显示，access 属性可用于控制单个操作。  有四个操作︰

  - *阅读*是 rest 风格获得表上操作
  - *插入*为 rest 风格的 POST 操作表
  - *更新*是 rest 风格修补程序已发布表上操作
  - *删除*了表的 rest 风格的删除操作

例如，您可能希望提供只读的未经身份验证的表︰

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>如何︰ 调整表操作一起使用的查询

表操作的一个常见要求是提供数据的受限的视图。  例如，您可以提供标有经过身份验证的用户 ID，您只能读取或更新自己的记录的表。  下面的表定义中提供了此功能︰

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

通常执行的查询的操作具有查询属性，您可以调整与 where 子句。 查询属性是一个[QueryJS]对象，该对象用于将 OData 查询转换成可以处理数据的后端。  对于简单相等 （如上面的代码） 的情况下，可以使用地图。 您还可以添加特定的 SQL 子句︰

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>如何︰ 配置软删除的表

软删除不会实际删除记录。  而是它将其标记为在数据库中删除的已删除的列设置为 true。  Azure 移动应用程序 SDK 自动从中移除软删除记录结果除非移动客户端 SDK 中使用 IncludeDeleted()。  若要配置的软删除表，请设置`softDelete`表定义文件中的属性︰

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

应建立一种机制来清除记录-从客户端应用程序，通过 WebJob，Azure 函数或自定义的 API。

### <a name="howto-tables-seeding"></a>方法︰ 种子数据库的数据

在创建新的应用程序时，您可能希望设置为种子包含数据的表。  这可以在表定义 JavaScript 文件中，如下所示︰

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Azure 移动应用程序 SDK 创建表时，只进行播种的数据。  如果表在数据库中已存在，没有数据插入到表。  如果启用了动态架构，从植入的数据推断架构。

我们建议您显式调用`tables.initialize()`方法来创建表，当服务开始运行。

### <a name="Swagger"></a>如何︰ 启用 Swagger 支持

Azure 应用程序服务移动应用程序附带了内置[Swagger]支持。  若要启用 Swagger 支持，首先为依赖项安装 swagger 用户界面︰

    npm install --save swagger-ui

安装完成后，您可以启用 Swagger 支持在 Azure 移动应用程序构造函数︰

    var mobile = azureMobileApps({ swagger: true });

您可能只想开发版本中支持 Swagger。  你可以利用`NODE_ENV`应用程序设置︰

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Swagger 端点位于 http://_yoursite_.azurewebsites.net/swagger。  您可以访问 Swagger 用户界面通过`/swagger/ui`终结点。  如果您选择需要在整个应用程序进行身份验证，则 Swagger 将产生错误。  为了获得最佳效果，选择允许未经身份验证的请求，通过在 Azure 应用程序服务的身份验证 / 授权设置，然后控制身份验证使用`table.access`属性。

您还可以添加 Swagger 选项对您`azureMobile.js`文件如果您仅希望 Swagger 支持本地开发。

## <a name="a-namepushpush-notifications"></a><a name="push">推式通知

移动应用程序集成了 Azure，使您能够跨所有主要平台向数百万台设备发送目标推式通知的通知集线器中。 通过使用通知集线器，可以发送推式通知，到 iOS，Android 和 Windows 设备。 若要了解详细信息，可以处理通知集线器，请参见[通知集线器概述](../notification-hubs/notification-hubs-push-notification-overview.md)。

### </a><a name="send-push"></a>如何︰ 发送推式通知

下面的代码演示如何使用强制对象向已注册的 iOS 设备发送广播推式通知︰

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

通过从客户端创建模板强制注册，可以改为所有支持的平台上的设备发送模板推消息。 下面的代码演示如何发送通知模板︰

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>如何︰ 为已经过身份验证的用户使用标记发送推式通知

当身份验证的用户注册的推式通知时，用户 ID 标记会自动添加到注册中。 通过使用此标记，您可以注册的特定用户的所有设备发送推式通知。 下面的代码获取发出请求的用户的 SID，并将该用户模板推式通知发送到每个设备注册︰

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

当注册来自已通过身份验证的客户端推送通知，确保身份验证之前尝试注册已完成。

## <a name="CustomAPI"></a>自定义的 Api

###  <a name="howto-customapi-basic"></a>如何︰ 定义一个自定义的 API


除了通过 /tables 端点数据访问 API，Azure 移动应用程序可以提供自定义的 API 覆盖范围。  自定义 Api 定义以类似的方式对表定义，并且可以访问所有功能，包括身份验证。

如果您想要使用一个自定义的 API 的应用程序服务的身份验证，您必须首先在[Azure 门户]配置应用程序服务的身份验证。  在 Azure 应用程序服务中配置身份验证的详细信息，您打算使用的标识提供程序查看配置指南︰

- [如何配置 Azure 活动目录身份验证]
- [如何配置 Facebook 的身份验证]
- [如何配置 Google 身份验证]
- [如何配置 Microsoft 身份验证]
- [如何配置使用 Twitter，身份验证]

自定义 Api 定义表 API 的方式大致相同。

1. 创建**api**目录
2. 在**api**目录下创建 API 定义 JavaScript 文件。
3. 使用导入方法的**api**目录导入。

这是基于我们在先前使用的基本应用程序示例的原型 api 定义。

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

让我们看一个示例返回服务器日期使用_Date.now()_方法的 API。  下面是 api/date.js 文件︰

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

每个参数是一个标准的 rest 风格谓词的获取、 发布、 修补程序、 或删除。  该方法是一个标准的[ExpressJS 中间件]函数发送所需的输出。

### <a name="howto-customapi-auth"></a>如何︰ 自定义 API 访问要求身份验证

Azure 的移动应用程序 SDK 的表终结点和自定义的 Api 相同的方式实现身份验证。  若要添加身份验证 api 开发上一节中，添加**访问**属性︰

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

在具体操作上，还可以指定身份验证︰

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

同一个标记用于表格终结点必须使用自定义 Api 要求进行身份验证。

### <a name="howto-customapi-auth"></a>如何︰ 处理大的文件上载

Azure 的移动应用程序 SDK 使用[正文分析器中间件](https://github.com/expressjs/body-parser)来接受和解码在您提交的词典中的正文内容。  可以预先配置正文解析器接受更大的文件将上载︰

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

该文件是 base 64 编码传输前。  这增加了实际上载的大小 （并因此大小必须考虑）。

### <a name="howto-customapi-sql"></a>如何︰ 执行自定义的 SQL 语句

Azure 移动应用程序 SDK 允许访问整个上下文通过请求对象，使您可以轻松地执行已定义的数据提供程序的参数化的 SQL 语句︰

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>调试、 简单的表，以及方便的 Api

### <a name="howto-diagnostic-logs"></a>如何︰ 调试、 诊断和解决 Azure 移动应用程序

Azure 应用程序服务提供几个调试和疑难解答 Node.js 应用程序技术。
请参阅以下文章着手解决 Node.js 移动端︰

- [监视 Azure 应用程序服务]
- [启用诊断日志记录在 Azure 应用程序服务]
- [Visual Studio 中的 Azure 应用程序服务的疑难解答]

Node.js 应用程序具有访问权限范围广泛的诊断日志工具。  在内部，Azure 移动应用程序 Node.js SDK 使用[Winston]的诊断日志记录。  启用调试模式或通过设置**MS_DebugMode**的应用程序设置为 true 的[Azure 的门户]，将自动启用日志记录。 在[Azure 门户]上诊断日志中显示生成的日志。

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>如何︰ 简单的表使用 Azure 门户中

在门户中的简单表允许您创建和使用表权限的门户。 您甚至可以编辑表操作使用编辑器应用程序服务。

在后端站点设置单击**简单表**时，可以添加、 修改或删除表。 您还可以查看表中的数据。

![使用简单的表](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

可用在命令栏中的表中使用以下命令︰

+ **更改权限**的修改的权限读取、 插入、 更新和删除操作的表。 
  选项将允许匿名访问，要求身份验证，或禁用对该操作的所有访问。 
+ **编辑脚本**的脚本文件的表是在应用程序服务编辑器中打开。
+ **管理架构**的添加或删除列或更改表索引。
+ **清除表格**-截断现有的表将删除所有数据行，但保留架构不变。
+ **删除行**删除每一行数据。
+ **流式传输日志视图**-连接到为您的站点的流式处理日志服务。

###<a name="work-easy-apis"></a>如何︰ 使用简单的 Api 在 Azure 门户

在门户中的简单 Api 使您可以创建和使用自定义的 Api 权限的门户。 您可以编辑 API 使用编辑器应用程序服务的脚本。

当您单击**方便的 Api**后端站点设置中时，可以添加、 修改或删除自定义的 API 端点。

![使用简单的 Api](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

在门户中，可以更改给定 HTTP 操作的访问权限、 API 脚本文件在应用程序服务编辑器中，编辑或查看流式处理日志。

###<a name="online-editor"></a>如何︰ 在应用程序服务编辑器中编辑代码

Azure 门户允许您编辑 Node.js 后端脚本文件在应用程序服务编辑器中而无需将项目下载到本地计算机。 若要编辑在线编辑器中的脚本文件︰

1. 在您移动应用程序的后端刀片式服务器，请单击**所有设置**>**简单表**或**简单的 Api**，都单击表或 API，然后都单击**编辑脚本**。 在应用程序服务编辑器中打开脚本文件。

    ![应用程序服务编辑器](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. 对在线编辑器中的代码文件进行更改。 键入时，将自动保存更改。


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android 的客户端快速入门]: app-service-mobile-android-get-started.md
[Apache Cordova 客户端快速入门]: app-service-mobile-cordova-get-started.md
[iOS 客户端快速入门]: app-service-mobile-ios-get-started.md
[Xamarin.iOS 客户端快速入门]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android 客户端快速入门]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms 客户端快速入门]: app-service-mobile-xamarin-forms-get-started.md
[Windows 应用商店的客户端快速入门]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[脱机数据同步]: app-service-mobile-offline-data-sync.md
[如何配置 Azure 活动目录身份验证]: app-service-mobile-how-to-configure-active-directory-authentication.md
[如何配置 Facebook 的身份验证]: app-service-mobile-how-to-configure-facebook-authentication.md
[如何配置 Google 身份验证]: app-service-mobile-how-to-configure-google-authentication.md
[如何配置 Microsoft 身份验证]: app-service-mobile-how-to-configure-microsoft-authentication.md
[如何配置使用 Twitter，身份验证]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure 应用程序服务部署指南]: ../app-service-web/web-sites-deploy.md
[监视 Azure 应用程序服务]: ../app-service-web/web-sites-monitor.md
[启用诊断日志记录在 Azure 应用程序服务]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Visual Studio 中的 Azure 应用程序服务的疑难解答]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[指定的节点版本]: ../nodejs-specify-node-version-azure-apps.md
[使用节点模块]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[速成版]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure 门户]: https://portal.azure.com/
[OData]: http://www.odata.org
[承诺]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[在 GitHub 上的 basicapp 示例]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[在 GitHub 上托多示例]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[在 GitHub 上的示例目录]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Visual Studio 的 Node.js 工具 1.1]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js 包]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014年速成版]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS 中间件]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
