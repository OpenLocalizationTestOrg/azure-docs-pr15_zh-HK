<properties
    pageTitle="将 Sails.js web 应用程序部署到 Azure 应用程序服务"
    description="了解如何部署 Node.js Azure 应用程序服务的应用程序。 本教程展示了如何部署 Sails.js 的 web 应用程序。"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>将 Sails.js web 应用程序部署到 Azure 应用程序服务

本教程展示如何将 Sails.js 应用程序部署到 Azure 应用程序服务。 在此过程中，可以收集一些关于如何配置您的 Node.js 应用程序运行在应用程序服务方面的一般常识。 

您应该有 Sails.js 的工作知识。 本教程不是来帮助您解决与一般情况下运行 Sail.js 相关的问题。


## <a name="prerequisites"></a>系统必备组件

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Git](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- 一种 Microsoft Azure 帐户。 如果您没有帐户，您可以[注册免费试用版](/pricing/free-trial/?WT.mc_id=A261C142F)或[激活您的 Visual Studio 的订阅者权益](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)。

>[AZURE.NOTE] 要在操作之前注册 Azure 帐户查看 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)。 那里，您立即可以应用程序服务中创建短期初学者应用程序 — — 不需要信用卡，没有承诺。

## <a name="step-1-create-a-sailsjs-app-locally"></a>步骤 1︰ 创建一个 Sails.js 应用程序本地

首先，快速创建一个默认的 Sails.js 应用程序在开发环境中通过执行以下步骤︰

1. 打开您选择的命令行终端和`CD`到工作目录。

2. 创建一个 Sails.js 应用程序并运行它︰

        sails new <appname>
        cd <appname>
        sails lift

    请确保您可以导航到在 http://localhost:1377 的默认主页上。

## <a name="step-2-create-the-azure-app-resource"></a>步骤 2︰ 创建 Azure 应用程序资源

接下来，在 Azure 创建应用程序服务资源。 您将稍后将 Sails.js 应用程序部署到它。

1. 登录到 Azure 类似这样︰
1. 在同一终端中，更改到 ASM 模式和登录到 Azure 中︰

        azure config mode asm
        azure login

    请按照提示在浏览器中具有 Azure 订阅 Microsoft 帐户继续登录。

2. 确保您仍在根目录下的 Sails.js 项目。 在 Azure 使用唯一的应用程序名称与下一个命令创建的应用程序服务的应用程序资源。 您的 web 应用程序的 URL 是 http://&lt;应用程序名 >。 azurewebsites.net。

        azure site create --git <appname>

    请按照提示您选择要部署到 Azure 的地区。 如果您已经永远不会将 Git/FTP 部署凭据设置 Azure 订阅，您将还会提示您创建它们。

    一旦创建应用程序服务的应用程序资源︰

    - Sails.js 应用程序已初始化 Git，
    - 本地 Git 初始化资源库连接到新的应用程序服务应用程序与远程，Git 贴切的名称"azure"和
    - 并在根目录下创建 iisnode.yml 文件。 此文件可用于配置[iisnode](https://github.com/tjanczuk/iisnode)，其运行 Node.js 应用程序的应用程序服务使用。

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>步骤 3︰ 配置和部署您的 Sails.js 应用程序

 使用应用程序服务中的 Sails.js 应用程序由三个主要步骤组成︰

 - 配置您的应用程序为它运行在应用程序服务
 - 将其部署到应用程序服务
 - 读取的 stderr 和标准输出日志任何部署问题进行故障排除

请按照下列步骤操作︰

1. 在您的根路径中打开新的 iisnode.yml 文件，添加以下两行︰

        loggingEnabled: true
        logDirectory: iisnode

    对于 iisnode 现在启用日志记录。 这是如何工作的详细信息，请参阅 [获取标准输出和 stderr 从 iisnode 的日志](app-service-web-nodejs-get-started.md#iisnodelog)。

2. 打开配置您的生产环境，并设置 config/env/production.js `port` ， `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    在 [Sails.js 文档](http://sailsjs.org/documentation/reference/configuration/sails-config)中可以找到有关这些配置设置的文档。

    接下来，您需要确保[Grunt](https://www.npmjs.com/package/grunt) Azure 的网络驱动器兼容。 小于 1.0.0 grunt 版本使用过时的[glob](https://www.npmjs.com/package/glob)包 (小于 5.0.14)，它不支持网络驱动器。 

3. 打开 package.json，然后更改`grunt`版本设置为`1.0.0`，并删除所有`grunt-*`包。 您`dependencies`属性应如下所示︰

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. 在 package.json 中，添加以下`engines`属性设置为一个我们想要的 Node.js 版本。

        "engines": {
            "node": "6.6.0"
        },

6. 保存您的更改，测试更改以确保您的应用程序仍然运行本地。 若要执行此操作，删除`node_modules`文件夹，然后运行︰

        npm install
        sails lift

4. 现在，使用 git 到 Azure 部署您的应用程序︰

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. 最后，只需启动实时 Azure 应用程序在浏览器中︰

        azure site browse

    现在，您应该看到相同的 Sails.js 主页。
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>解决您的部署

如果 Sails.js 应用程序在应用程序服务由于某种原因失败，到 stderr 日志以帮助解决它。
有关详细信息，请参阅[获取标准输出和 stderr 从 iisnode 的日志](app-service-web-nodejs-sails.md#iisnodelog)。
如果它已成功启动，标准输出日志应该显示您熟悉邮件︰

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

您可以控制粒度的[config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging)文件中的标准输出日志。 

## <a name="connect-to-a-database-in-azure"></a>连接到 Azure 中的数据库

要连接到 Azure 中的数据库，在 Azure，如 SQL Azure 数据库和 MySQL，MongoDB，Azure (Redis) 高速缓存等，创建您所选的数据库，并使用相应的[数据存储适配器](https://github.com/balderdashy/sails#compatibility)连接到它。 本节中的步骤演示了如何连接到 MySQL 数据库在 Azure 中。

1. 按照教程[这里](../store-php-create-mysql-database.md)在 Azure 创建一个 MySQL 数据库。

2. 从命令行终端，MySQL 将适配器安装到︰

        npm install sails-mysql --save

3. 打开 config/connections.js，然后向列表中添加以下连接对象︰ 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. 为每个环境变量 (`process.env.*`)，您需要将其设置在应用程序服务。 若要执行此操作，从终端上运行以下命令。 在 Azure 门户是所有所需的连接信息 （请参见[连接到 MySQL 数据库](../store-php-create-mysql-database.md#connect)）。

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    将您的设置放在 Azure 应用程序设置中保留您的源控制 (Git) 的敏感数据。 接下来，您将配置开发环境以便使用相同的连接信息。

4. 打开 config/local.js 并添加下面的连接对象︰

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    此配置会覆盖您的本地环境的 config/connections.js 文件中的设置。 因此将不会存储在 Git 中通过在项目中，默认.gitignore 排除此文件。 现在，您将能够从 Azure 的 web 应用程序和本地开发环境连接到 MySQL 数据库。

4. 打开 config/env/production.js 来配置您的生产环境，并添加以下`models`对象︰

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. 打开 config/env/development.js 配置开发环境中，然后添加以下`models`对象︰

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`您可以使用数据库迁移功能创建并轻松地更新您的 MySQL 在数据库表。 但是，`migrate: 'safe'`因为 Sails.js 不允许您使用 Azure （生产） 环境使用`migrate: 'alter'`在生产环境中 （请参阅 [Sails.js 文档](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)）。

4. 从终端，[生成](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate)一个像您一样的 Sails.js[蓝图 API](http://sailsjs.org/documentation/concepts/blueprints)一般，然后运行`sails lift`与 Sails.js 数据库迁移创建数据库。 例如︰

         sails generate api mywidget
         sails lift

    `mywidget`为空，此命令生成的模型，但我们可以使用它来显示我们有数据库连接。
    当您运行`sails lift`，它将创建您的应用程序模型的缺少表使用。

6. 蓝图刚刚创建在浏览器中的 API 来访问。 例如︰

        http://localhost:1337/mywidget/create
    
    API 应该返回到您在浏览器窗口中，这意味着您的数据库创建成功创建的项。

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. 现在，将您的更改推送到 Azure，并浏览到您的应用程序，以确保它仍然有效。

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. 访问 Azure 的 web 应用程序的 API 的蓝图。 例如︰

        http://<appname>.azurewebsites.net/mywidget/create

    如果 API 返回另一个新的条目，Azure 的 web 应用程序正在对 MySQL 数据库。

## <a name="more-resources"></a>更多的资源

- [在 Azure 应用程序服务的 Node.js web 应用程序入门](app-service-web-nodejs-get-started.md)
- [Node.js 模块使用 Azure 应用程序](../nodejs-use-node-modules-azure-apps.md)
