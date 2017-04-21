<properties
    pageTitle="指定 Node.js 版本"
    description="了解如何指定 Node.js Azure 网站和云服务使用的版本"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>在 Azure 应用程序指定 Node.js 版本

当承载 Node.js 应用程序时，您可能希望确保应用程序使用特定版本的 Node.js。 有几种方法来实现此目的在 Azure 上承载的应用程序。

##<a name="default-versions"></a>默认版本

由 Azure 的 Node.js 版本会不断更新。 除非另外指定，否则在中指定的默认版本`WEBSITE_NODE_DEFAULT_VERSION`将使用环境变量。 若要覆盖此默认值，请按照本文的以下部分中的步骤

> [AZURE.NOTE] 如果承载 Azure 云服务 （web 或辅助角色，） 您的应用程序，它是第一次部署了该应用程序将尝试 Azure 使用相同版本的 Node.js，如安装在您的开发环境与 Azure 上可用的默认版本之一相匹配。

##<a name="versioning-with-packagejson"></a>与 package.json 版本控制

您可以指定 Node.js 可被添加到**package.json**文件的以下的版本︰

    "engines":{"node":version}

其中*版本*是要使用的特定版本号码。 可以可以指定更复杂条件的版本，如︰

    "engines":{"node": "0.6.22 || 0.8.x"}

0.6.22 不是一个在宿主环境中可用的版本，因为可用的系列将为 0.8 的最高版本使用-0.8.4。

##<a name="versioning-websites-with-app-settings"></a>版本控制网站的应用程序设置
如果您承载网站的应用程序，则可以到所需版本设置环境变量**WEBSITE_NODE_DEFAULT_VERSION** 。 

##<a name="versioning-cloud-services-with-powershell"></a>版本控制与 PowerShell 的云服务

如果承载在云服务中，应用程序和部署使用 Azure PowerShell 的应用程序，您可以通过**设置 AzureServiceProjectRole** PowerShell cmdlet 覆盖默认 Node.js 版本。 例如︰

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

请注意上面的语句中的参数都区分大小写。  您可以通过检查在您的角色**package.json****引擎**属性选择了正确的 Node.js 版本进行验证。

也可以使用**Get AzureServiceProjectRoleRuntime**检索 Node.js 版本可用作为一种云服务承载的应用程序的列表。  始终验证的 Node.js 取决于您的项目版本在此列表中。

##<a name="using-a-custom-version-with-azure-websites"></a>Azure 网站中使用的自定义版本

虽然 Azure 提供了多个默认 Node.js 版本，可以使用默认情况下未提供版本。 作为 Azure 网站承载您的应用程序，如果可以使用**iisnode.yml**文件来实现此目的。 下列步骤将与 Azure 网站使用自定义版本的 Node.Js 一步步指导︰

1. 创建一个新目录，然后创建该目录中的**server.js**文件。 **Server.js**文件应包含以下信息︰

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    这将显示在浏览该网站时使用的 Node.js 版本。

2. 创建新网站，请注意网站的名称。 例如，下面的示例使用[Azure 的命令行工具]来创建名为**mywebsite**，Azure 网站，然后启用该网站的 Git 存储库。

        azure site create mywebsite --git

3. 新建一个名为童年包含**server.js**文件的目录的**bin**目录。

4. 下载**node.exe** （Windows 版本），您希望您的应用程序中使用的特定版本。 例如，下面的示例使用**卷曲**下载版本 0.8.1:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    将**node.exe**文件保存到先前创建的**bin**文件夹中。

5. 在**server.js**文件所在的目录中创建的**iisnode.yml**文件，然后将以下内容添加到**iisnode.yml**文件︰

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    此路径是项目中的**node.exe**文件将位于其中一旦发布到 Azure 网站应用程序。

6. 发布您的应用程序。 例如，由于我前面用 — git 参数中创建一个新网站，下面的命令将添加到我本地 Git 存储库，这些应用程序文件并随后将它们推送到网站存储库︰

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    已发布的应用程序后，请在浏览器中打开该网站。 您应该看到一条消息指出"Hello 从 Azure 运行节点版本︰ v0.8.1"。

##<a name="next-steps"></a>下一步行动

现在，您已了解如何指定应用程序所使用的 Node.js 版本，了解如何[处理模块]、[生成和部署 Node.js 网站]和[如何使用 Mac 和 Linux 的 Azure 命令行工具]。

有关详细信息，请参阅[Node.js 开发中心](/develop/nodejs/)。

[如何使用 Mac 和 Linux 的 Azure 命令行工具]: xplat-cli-install.md
[Azure 的命令行工具]: xplat-cli-install.md
[使用模块]: nodejs-use-node-modules-azure-apps.md
[生成和部署 Node.js Web 站点]: web-sites-nodejs-develop-deploy-mac.md
