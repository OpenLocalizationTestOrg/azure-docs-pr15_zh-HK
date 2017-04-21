<properties
    pageTitle="Node.js 快速入门指南 |Microsoft Azure"
    description="了解如何创建一个简单的 Node.js web 应用程序并将其部署到 Azure 的云服务。"
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
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>生成和部署 Node.js 对 Azure 云服务的应用程序

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

本教程展示如何创建一个简单的 Node.js 应用程序运行在 Azure 的云服务。 云服务是 Azure 中的可扩展的云应用程序的构建块。 它们允许的分离和独立的管理和向外扩展的应用程序的前端和后端组件。  云服务提供一个可靠的专用的虚拟机可靠地承载每个角色。

对于云服务的详细信息和如何到 Azure 网站和虚拟机进行比较，请参阅[Azure 网站、 云服务和虚拟机的比较]。

>[AZURE.TIP] 希望构建一个简单的网站吗？ 如果您的方案涉及的只是简单网站前端，请考虑[使用一个轻量级 web 应用程序]。 您可以轻松地升级到云服务，为您的 web 应用程序的增长和需求的变化。

通过遵循本教程中，您将生成简单的 web 应用程序承载于 web 角色内。 您将使用计算仿真程序测试您的应用程序本地，然后将其使用 PowerShell 命令行工具部署。

应用程序是一个简单的"你好世界"应用程序︰

![Web 浏览器显示 Hello World web 页][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>系统必备组件

> [AZURE.NOTE] 本教程使用 Azure PowerShell，要求 Windows。

- 安装和配置[Azure Powershell]。
- 下载并安装[.NET 2.7 的 Azure SDK]。 在安装设置，请选择︰
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>创建一个 Azure 云服务项目

执行以下任务来创建一个新的 Azure 云服务项目，以及基本的 Node.js 基架︰

1. 运行**Windows PowerShell**以管理员的身份;从**「 开始 」 菜单**或**启动屏幕**，请搜索**Windows PowerShell**。

2. 您的订阅[连接 PowerShell] 。

3. 输入下面的 PowerShell cmdlet，要创建以创建项目︰

        New-AzureServiceProject helloworld

    ![新 AzureService helloworld 命令的结果][The result of the New-AzureService helloworld command]

    **新 AzureServiceProject** cmdlet 生成 Node.js 应用程序发布到云服务的基本结构。 它包含配置文件所需的发布到 Azure。 该 cmdlet 也会更改工作目录服务的目录。

    该 cmdlet 将创建以下文件︰

    -   **ServiceConfiguration.Cloud.cscfg**、 **ServiceConfiguration.Local.cscfg**和**ServiceDefinition.csdef**: Azure 特定文件所需的发布应用程序。 有关详细信息，请参阅[创建承载的 Azure 服务的概述]。

    -   **deploymentSettings.json**︰ 存储所使用的 Azure PowerShell 部署 cmdlet 的本地设置。

4.  输入以下命令以添加新的 web 角色︰

        Add-AzureNodeWebRole

    ![添加 AzureNodeWebRole 命令的输出][The output of the Add-AzureNodeWebRole command]

    **添加 AzureNodeWebRole** cmdlet 将创建一个基本的 Node.js 应用程序。 它也修改了**.csfg**和**.csdef**文件来添加新角色的配置项。

    > [AZURE.NOTE] 如果不指定角色名，则使用默认名称。 作为第一个 cmdlet 参数，您可以提供一个名称︰`Add-AzureNodeWebRole MyRole`

Node.js 应用程序定义文件**server.js**，位于 web 角色 (默认情况下为**WebRole1** ) 的目录中。 下面是该代码︰

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

此代码在本质上相同的"Hello World"示例在[nodejs.org]网站上，不同之处在于它使用云环境由分配的端口号。

## <a name="deploy-the-application-to-azure"></a>部署到 Azure 应用程序

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>下载的 Azure 的发布设置

若要部署到 Azure 应用程序，您必须首先下载 Azure 订阅的发布设置。

1.  运行下面的 Azure PowerShell cmdlet:

        Get-AzurePublishSettingsFile

    这将使用您的浏览器导航到下载发布设置页。 可能会提示您使用 Microsoft 帐户登录。 如果是这样，使用 Azure 订阅关联的帐户。

    将下载的配置文件保存到的文件位置，您可以轻松访问。

2.  运行以下 cmdlet 导入发布配置文件下载︰

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] 后导入的发布设置，请考虑删除下载的.publishSettings 文件，因为它包含无法允许他人访问您的帐户的信息。

### <a name="publish-the-application"></a>发布应用程序

若要发布，请运行以下命令︰

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-服务**指定为部署的名称。 这必须是一个唯一的名称，否则发布过程将失败。 **获取 Date**命令采取在一个日期/时间字符串，应将该名称唯一。

- **-位置**指定将承载的应用程序中的数据中心。 若要查看可用的数据中心，使用**Get AzureLocation** cmdlet。

- **-启动**打开浏览器窗口，并导航到托管服务，在完成部署后。

发布成功后，您将看到类似下面的响应︰

![发布 AzureService 命令的输出][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> 可能需要为应用程序部署，并推出首次发布时的几分钟时间。

完成部署后，浏览器窗口将打开，并定位到云服务。

![一个浏览器窗口，显示你好世界页中;该 URL 指示页承载在 Azure 上。][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

现在在 Azure 上运行应用程序。

**发布 AzureServiceProject** cmdlet 执行以下步骤︰

1.  创建要部署的包。 该软件包包含您的应用程序文件夹中的所有文件。

2.  如果不存在，请创建新的**存储帐户**。 Azure 存储帐户用于在部署过程中将存储该应用程序包。 完成部署后，您可以安全删除存储帐户。

3.  如果尚不存在，请创建一个新的**云服务**。 **云服务**是部署到 Azure 时在其中承载应用程序的容器。 有关详细信息，请参阅[创建承载的 Azure 服务的概述]。

4.  将部署包发布到 Azure。


## <a name="stopping-and-deleting-your-application"></a>停止和删除应用程序

在部署您的应用程序之后, 可能要禁用它，这样可以避免额外的成本。 Azure 帐单的 web 角色实例的服务器消耗的时间每小时。 一旦部署您的应用程序，即使这些实例未运行且处于停止状态时才占用服务器的时间。

1.  在 Windows PowerShell 窗口停止以下 cmdlet 使用上一节中创建的部署服务︰

        Stop-AzureService

    停止该服务，则可能需要几分钟。 停止该服务后，您会收到一条消息，指示它已停止。

    ![Stop AzureService 命令的状态][The status of the Stop-AzureService command]

2.  若要删除该服务，请调用以下 cmdlet:

        Remove-AzureService

    出现提示时，输入**Y**以删除该服务。

    删除该服务可能需要几分钟的时间。 已删除该服务后您将收到一条消息，表明该服务已删除。

    ![删除 AzureService 命令的状态][The status of the Remove-AzureService command]

    > [AZURE.NOTE] 删除该服务不会删除存储帐户创建时最初发布服务，并将继续使用存储的记帐。 如果没有任何其他正在使用的存储，您可以将其删除。

## <a name="next-steps"></a>下一步行动

有关详细信息，请参阅[Node.js 开发中心]。

<!-- URL List -->

[Azure 网站、 云服务和虚拟机比较]: ../app-service-web/choose-web-site-cloud-service-vm.md
[使用轻量的 web 应用程序]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[使.NET 2.7 的 azure SDK]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[连接 PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[创建托管的 Azure 服务概述]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js 开发人员中心]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
