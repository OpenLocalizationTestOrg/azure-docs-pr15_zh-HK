<properties 
    pageTitle="生成和部署对使用 WebMatrix Azure 的 Node.js web 应用程序" 
    description="教您如何使用 WebMatrix 开发 Node.js 应用程序并将其部署到 Azure 应用程序服务 Web 应用程序的教程。" 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>生成和部署对使用 WebMatrix Azure 的 Node.js web 应用程序

本教程展示如何使用 WebMatrix 开发 Node.js 应用程序并将它部署到[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序。 WebMatrix 是包括一切所需的网站或 web 应用程序开发的 microsoft 的免费的 web 开发工具。 WebMatrix 包含几项功能，使其易于使用的 Node.js 更少，包括代码完成、 预构建的模板和玉，编辑器支持和 CoffeeScript。 了解有关[WebMatrix](https://www.microsoft.com/web/webmatrix/)。

在完成本指南，您将必须 Node.js web 应用程序运行在 Azure 应用程序服务。
 
下面是完整的应用程序的屏幕快照︰

![Azure 节点 Web 站点][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="sign-into-azure"></a>登录到 Azure

请按照下列步骤在 Azure 应用程序服务中创建 web 应用程序。

1. WebMatrix 启动
2. 如果是第一次已经用 WebMatrix，系统将提示您登录到 Azure。  否则为您可以单击**登录**按钮，并选择**添加帐户**。  选择使用您的 Microsoft 帐户**登录**到。

    ![添加帐户][addaccount]

3. 如果您已经注册了 Azure 帐户，您可以使用 Microsoft 客户登录︰

    ![登录到 Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>创建网站的 Azure 使用内置的模板

1. 在启动屏幕上，单击**新建**按钮，然后从模板库中创建新的网站**模板库**中选择︰

    ![新网站模板库][sitefromtemplate]

2. 在**从模板创建网站**对话框中，选择**节点**，然后选择**速成网站**。 最后，请单击**下一步**。 如果缺少任何系统必备项**速成网站**模板，将提示您进行安装。

    ![选择快递的模板][webmatrix-templates]

3. 如果您已登录至 Azure，您现在可以选择创建应用程序服务 web 应用程序为您的本地站点。  请选择一个唯一的名称，并选择您想要创建您的应用程序服务 web 应用程序的数据中心︰ 

    ![在 Azure 上创建站点][nodesitefromtemplateazure]
    
4. WebMatrix 完成构建本地站点和创建应用程序服务 web 应用程序后，将显示 WebMatrix IDE。

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>发布到 Azure 应用程序

1. 在 WebMatrix，从**主**功能区来显示该网站**发布预览**对话框中单击**发布**。

    ![发布预览][webmatrix-node-publishpreview]

2. 单击**继续**。 当发布完成时，将应用程序服务 web 应用程序的 URL 显示 WebMatrix IDE 底部

    ![发布完整][webmatrix-publish-complete]

3. 单击链接以在浏览器中打开的应用程序服务 web 应用程序。

    ![表示 web 应用程序][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>修改和重新发布您的应用程序

可以很容易地修改和重新发布您的应用程序。 在这里，您将要进行简单更改为在标题中的**index.jade**文件，并重新发布应用程序。

1. WebMatrix，在选择**文件**，然后展开 [**视图**] 文件夹。 通过双击它打开**index.jade**文件。

    ![webmatrix 查看 index.jade][webmatrix-modify-index]

2. 将段落的行更改为以下︰

        p Welcome to #{title} with WebMatrix on Azure!

3. 保存您的更改，然后单击发布图标。 最后，在**发布预览**对话框中单击**继续**并等待要发布的更新。

    ![发布预览][webmatrix-republish]

4. 当发布完成时，使用返回发布过程完毕要查看更新后的应用程序服务 web 应用程序的链接。

    ![Azure 节点 web 应用程序][webmatrix-node-completed]

##<a name="next-steps"></a>下一步行动

Node.js 附带 Azure 以及如何指定要与应用程序一起使用的版本的版本有关详细信息，请参阅[指定 Node.js 版本 Azure 应用程序中](../nodejs-specify-node-version-azure-apps.md)。

如果您遇到问题的应用程序部署到 Azure 后，，了解[如何调试 Node.js web 应用程序在 Azure 应用程序服务](web-sites-nodejs-debug.md)有关诊断问题的信息。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 