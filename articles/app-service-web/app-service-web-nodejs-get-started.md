<properties
    pageTitle="在 Azure 应用程序服务的 Node.js web 应用程序入门"
    description="了解如何部署 Node.js 到 Azure 应用程序服务 web 应用程序的应用程序。"
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
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>在 Azure 应用程序服务的 Node.js web 应用程序入门

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

本教程展示如何创建一个简单的[Node.js]应用程序以及从命令行环境，例如 cmd.exe 将其部署到[Azure 应用程序服务]或指责。 在本教程中的说明进行操作的后面可以 Node.js 运行任何操作系统上。

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>系统必备组件

- [Node.js]
- [Bower]
- [Yeoman]
- [Git]
- [Azure CLI]
- 一种 Microsoft Azure 帐户。 如果您没有帐户，您可以[注册免费试用版]或[激活您的 Visual Studio 的订阅者权益]。

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>创建和部署一个简单的 Node.js web 应用程序

1. 打开您选择的命令行终端并安装[速成版的 Yeoman 生成器]。

        npm install -g generator-express

2. `CD`到工作目录和生成快速的应用程序，使用以下语法︰

        yo express
        
    出现提示时，下列选项中选择︰  

    `? Would you like to create a new directory for your project?`**是**  
    `? Enter directory name`**{应用程序名。**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**无**  
    `? Select a database to use:`**无**  
    `? Select a build tool to use:`**Grunt**

3. `CD`为您的新应用程序的根目录下，启动它以确保它在您的开发环境中运行︰

        npm start

    在浏览器中，导航到<http://localhost:3000> ，以确保您可以看到快递主页。 一旦您已经正确验证在应用程序运行过程中，使用`Ctrl-C`以将其停止。
    
1. 更改到 ASM 模式和登录到 Azure （需要[Azure CLI](#prereq)）︰

        azure config mode asm
        azure login

    请按照提示在浏览器中具有 Azure 订阅 Microsoft 帐户继续登录。

2. 确保在您的应用程序的根目录下然后在 Azure 使用唯一的应用程序名称与下一条命令中创建应用程序服务的应用程序资源。 例如︰ http://{appname}.azurewebsites.net

        azure site create --git {appname}

    请按照提示您选择要部署到 Azure 的地区。 如果您已经永远不会将 Git/FTP 部署凭据设置 Azure 订阅，您将还会提示您创建它们。

3. 从您的应用程序的根目录中打开./config/config.js 文件，并更改生产端口以连接到`process.env.port`;您`production`在`config`对象应类似于以下示例︰

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    这样，Node.js 应用程序响应以该 iisnode 的 web 请求的默认端口上侦听。
    
4. 打开./package.json 并添加`engines`属性[指定所需的 Node.js 版本](#version)。

        "engines": {
            "node": "6.6.0"
        }, 

4. 保存您的更改，然后使用 git 到 Azure 部署您的应用程序︰

        git add .
        git commit -m "{your commit message}"
        git push azure master

    速成版生成器已经提供了一个.gitignore 文件，因此您`git push`不消耗带宽尝试上载 node_modules / 目录。

5. 最后，启动实时 Azure 应用程序在浏览器中︰

        azure site browse

    您现在应该看到 Node.js web 应用程序实时 Azure 应用程序服务中运行。
    
    ![浏览到已部署的应用程序的示例。][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>更新您的 Node.js web 应用程序

若要对您运行的应用程序服务中的 Node.js web 应用程序进行更新，只需运行`git add`， `git commit`，和`git push`您一样首次部署 web 应用程序时。
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>应用程序服务将 Node.js 应用程序的部署

Azure 应用程序服务使用[iisnode]运行 Node.js 应用程序。 Azure CLI 和 Kudu 引擎 （Git 部署） 协同工作以开发和部署 Node.js 应用程序从命令行时为您提供的简化的体验。 

- `azure site create --git`可以识别常见的 server.js app.js 模式，Node.js 并在根目录下创建的 iisnode.yml。 此文件可用于自定义 iisnode。
- 在`git push azure master`，Kudu 自动执行下列部署任务︰

    - 如果存储库根 package.json，运行`npm install --production`。
    - 生成指向启动脚本中 （如 server.js 或 app.js） 的 package.json 的 iisnode 的 Web.config。
    - 自定义 Web.config 准备用节点检查器调试您的应用程序。
    
## <a name="use-a-nodejs-framework"></a>使用 Node.js 框架

如果您使用流行的 Node.js 框架，例如[Sails.js] [SAILSJS]或[MEAN.js] [MEANJS]若要开发的应用程序，可以将它们与应用程序服务部署。 常用的 Node.js 框架有其具体的特点，并保持获取更新的包的依赖项。 但是，应用程序服务使标准输出和 stderr 日志可以给您，以便您可以知道确切发生了什么情况与您的应用程序，并相应地进行更改。 有关详细信息，请参阅[获取标准输出和 stderr 从 iisnode 的日志](#iisnodelog)。

下面的教程将向您展示如何使用应用程序服务中特定框架︰

- [将 Sails.js web 应用程序部署到 Azure 应用程序服务]
- [使用 Socket.IO 在 Azure 应用程序服务创建 Node.js 聊天应用程序]
- [如何使用 io.js Azure 应用程序服务 Web 应用程序]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>使用特定的 Node.js 引擎

在典型的工作流程，您告诉像平常一样在 package.json 中使用特定的 Node.js 引擎的应用程序服务。
例如︰

    "engines": {
        "node": "6.6.0"
    }, 

Kudu 部署引擎确定的 Node.js 引擎按以下顺序使用︰

- 首先，看一下是否 iisnode.yml`nodeProcessCommandLine`指定。 如果是的话，然后用它。
- 下一步，看一看是否 package.json`"node": "..."`中指定`engines`对象。 如果是的话，然后用它。
- 默认情况下选择默认 Node.js 版本。

>[AZURE.NOTE] 建议您显式定义所需的 Node.js 引擎。 可以更改默认的 Node.js 版本，并可能在 Azure 的 web 应用程序中出现错误，因为默认 Node.js 版本不适合于您的应用程序。

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>从 iisnode 获取标准输出和 stderr 日志

若要读取 iisnode 日志，请按照下列步骤。

> [AZURE.NOTE] 完成这些步骤之后，日志文件可能不存在出错为止。

1. 打开 Azure CLI 提供的 iisnode.yml 文件。

2. 设置以下两个参数︰ 

        loggingEnabled: true
        logDirectory: iisnode
    
    他们在一起，告诉应用程序服务将其标准输出和 stderror 的输出放在 D:\home\site\wwwroot 中 iisnode\**iisnode** 目录。

3. 保存您的更改，然后将所做的更改推送到 Azure，使用下列的 Git 命令︰

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    现在配置 iisnode。 接下来的步骤向您展示了如何访问这些日志。
     
4. 在浏览器中，访问您的应用程序，它是在 Kudu 调试控制台︰

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    此 URL 与 web 应用程序 URL 添加"*.scm。*" 到的 DNS 名称。 如果您省略了这种添加到 URL，将得到一个 404 错误。

5. 定位到 D:\home\site\wwwroot\iisnode

    ![导航到 iisnode 日志文件的位置。][iislog-kudu-console-find]

6. 单击您想要阅读的日志的**编辑**图标。 如果需要，还可以单击**下载**或**删除**。

    ![打开 iisnode 日志文件。][iislog-kudu-console-open]

    现在您可以看到该日志可帮助您调试您的应用程序服务的部署。
    
    ![检查 iisnode 日志文件。][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>调试您的应用程序使用节点检查器

如果您使用节点检查调试 Node.js 应用程序，可以使用它为实时应用程序服务应用程序。 Iisnode 安装的应用程序服务中预安装节点检查器。 并从 Kudu 自动生成 Web.config Git 中部署时，如果已包含您需要启用节点检查的所有配置。

若要使节点检查器，请执行以下步骤︰

1. 打开存储库根 iisnode.yml，并指定以下参数︰ 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. 保存您的更改，然后将所做的更改推送到 Azure，使用下列的 Git 命令︰

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. 现在，只需导航到由开始脚本中您的 package.json，您的应用程序启动文件使用 /debug 添加到 URL 中。 例如，

        http://{appname}.azurewebsites.net/server.js/debug
    
    或者，
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>更多的资源

- [在 Azure 应用程序指定 Node.js 版本](../nodejs-specify-node-version-azure-apps.md)
- [最佳做法和在 Azure 上 Node.js 应用程序的故障排除指南](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [如何调试 Node.js web 应用程序在 Azure 应用程序服务](web-sites-nodejs-debug.md)
- [Node.js 模块使用 Azure 应用程序](../nodejs-use-node-modules-azure-apps.md)
- [Azure 应用程序服务 Web 应用程序︰ Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js 开发人员中心](/develop/nodejs/)
- [在 Azure 应用程序服务 web 应用程序入门](app-service-web-get-started.md)
- [了解超级机密 Kudu 调试控制台]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure 应用程序服务]: ../app-service/app-service-value-prop-what-is.md
[激活您的 Visual Studio 订户权益]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[使用 Socket.IO 在 Azure 应用程序服务创建 Node.js 聊天应用程序]: ./web-sites-nodejs-chat-app-socketio.md
[将 Sails.js web 应用程序部署到 Azure 应用程序服务]: ./app-service-web-nodejs-sails.md
[了解超级机密 Kudu 调试控制台]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Yeoman 直通生成器]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[如何使用 io.js Azure 应用程序服务 Web 应用程序]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[注册免费试用版]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
