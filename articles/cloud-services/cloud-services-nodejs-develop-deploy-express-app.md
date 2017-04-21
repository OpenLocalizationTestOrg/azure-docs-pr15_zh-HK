<properties 
    pageTitle="速成版 (Node.js) 与应用程序的 web |Microsoft Azure" 
    description="基于云服务的教程，并演示如何使用速成版模块的教程。" 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>建立 Azure 云服务的使用速成版的 Node.js web 应用

Node.js 包括核心运行时中的最小功能集。
开发人员通常使用第三方模块开发 Node.js 应用程序时提供其他功能。 在本教程中，您将创建新的应用程序使用[直通][]模块，提供了一个 MVC 框架创建 Node.js web 应用程序。

下面是完整的应用程序的屏幕快照︰

![Web 浏览器到 Azure 中快速显示欢迎使用](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>创建一个云服务项目

执行以下步骤来创建一个新的云服务项目名为 expressapp:

1. 从**「 开始 」 菜单**或**启动屏幕**，请搜索**Windows PowerShell**。 最后，用鼠标右键单击**Windows PowerShell**并选择**以管理员身份运行**。

    ![Azure PowerShell 图标](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. 将目录改为**c:\\节点**目录，然后输入以下命令以创建一个名为**expressapp**的新的解决方案和一个名为**WebRole1**的 web 角色︰

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] 默认情况下，**添加 AzureNodeWebRole**使用较旧版本的 Node.js。 以上**一组 AzureServiceProjectRole**语句指示 Azure 使用 v0.10.21 的节点。  注意参数都区分大小写。  您可以验证正确的 Node.js 版本已选择通过检查**WebRole1\package.json**中的**引擎**属性。

##<a name="install-express"></a>安装速成版

1. 通过发出以下命令安装速成版生成器︰

        PS C:\node\expressapp> npm install express-generator -g

    Npm 命令的输出结果应类似于下面的结果。 

    ![显示的输出 npm 的 Windows PowerShell 安装明示命令。](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. 将目录更改到**WebRole1**目录，并明确命令用于生成新的应用程序︰

        PS C:\node\expressapp\WebRole1> express

    系统将提示您覆盖早期的应用程序。 输入**y**或**是**以继续。 速成版将生成 app.js 文件和生成应用程序的文件夹结构。

    ![明确的命令的输出](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  要安装的 package.json 文件中定义的附加依赖项，请输入以下命令︰

        PS C:\node\expressapp\WebRole1> npm install

    ![Npm 的输出安装命令](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  使用下面的命令将**bin/www**文件复制到**server.js**。 这是云服务可以查找此应用程序的入口点。

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    此命令完成后，应该在 WebRole1 目录中的**server.js**文件。

7.  **Server.js**以删除其中的一个修改 '。 下面的行中的字符。

        var app = require('../app');

    进行相应的修改后, 行应如下显示。

        var app = require('./app');

    此更改是必需的因为我们移动文件 (以前称为**bin/www**，) 为进行所需的应用程序文件所在的目录。 之后进行此更改，请将**server.js**文件保存。

8.  使用以下命令在 Azure 的模拟器上运行应用程序︰

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![包含表示欢迎的 web 页。](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>修改视图

现在修改视图以显示"欢迎到表达在 Azure"的消息。

1.  输入以下命令以打开 index.jade 文件︰

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Index.jade 文件的内容。](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    玉是速成版应用程序使用的默认视图引擎。 Jade 视图引擎的详细信息，请参阅[http://jade-lang.com][]。

2.  通过附加**在 Azure 中**修改文本的最后一行。

    ![Index.jade 文件中，最后一个行的内容为︰ 欢迎使用 p \#Azure 中的 {标题}](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  保存文件并退出记事本。

4.  请刷新浏览器，您将看到您的更改。

    ![浏览器窗口中，该页面包含欢迎使用 Azure 中的速成版](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

在测试应用程序，使用**Stop AzureEmulator** cmdlet 停止仿真程序。

##<a name="publishing-the-application-to-azure"></a>发布到 Azure 应用程序

在 Azure PowerShell 窗口中，使用**发布 AzureServiceProject** cmdlet 将应用部署到云服务

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

在部署操作完成后，您的浏览器将打开，并显示 web 页。

![Web 浏览器显示速成版页。 该 URL 指示它现在位于 Azure 上。](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅[Node.js 开发中心](/develop/nodejs/)。

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [速成版]: http://expressjs.com/
  [http://jade-lang.com]: http://jade-lang.com

 
