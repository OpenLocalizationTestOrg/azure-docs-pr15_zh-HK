<properties
    pageTitle="在 Azure 应用程序服务的 Node.js API 应用程序 |Microsoft Azure"
    description="了解如何创建 Node.js RESTful API 并将其部署到 Azure 应用程序服务 API 的应用程序。"
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>构建了 Node.js rest 风格的 API 并将其部署到 Azure 中 API 的应用程序

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

本教程展示如何创建简单[Node.js](http://nodejs.org) API 和使用[Git](http://git-scm.com)将其部署到[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md) [API 的应用程序](app-service-api-apps-why-best-platform.md)。 您可以使用任何操作系统可以运行 Node.js，并将您的所有工作，使用命令行工具，例如 cmd.exe 或指责。

## <a name="prerequisites"></a>系统必备组件

1. （[打开一个免费的帐户](https://azure.microsoft.com/pricing/free-trial/)） 的 Microsoft Azure 帐户
1. [Node.js](http://nodejs.org)安装 （此示例假定您已经 Node.js 版本 4.2.2）
2. [Git](https://git-scm.com/)安装
1. [GitHub](https://github.com/)帐户

尽管应用程序服务支持多种方法将代码部署到 API 的应用程序，本教程演示 Git 的方法和假定您已经了解如何使用 Git 的基础知识。 有关其他部署方法的详细说明，请参见[部署您的应用程序到 Azure 应用程序服务](../app-service-web/web-sites-deploy.md)。

## <a name="get-the-sample-code"></a>获取示例代码

1. 打开一个命令行界面，可运行 Node.js，Git 的命令。

1. 导航到本地 Git 存储库中，并克隆[GitHub 的存储库中包含的示例代码](https://github.com/Azure-Samples/app-service-api-node-contact-list)可以使用的文件夹。

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    示例 API 提供了两个终结点︰ 对 Get 请求`/contacts`以 JSON 格式返回列表中的姓名和电子邮件地址时`/contacts/{id}`返回选择的联系人。

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>构架 （自动生成） Node.js 代码根据 Swagger 元数据

[Swagger](http://swagger.io/)是用于描述 RESTful API 的元数据的文件格式。 Azure 应用程序服务有[Swagger 元的内置支持](app-service-api-metadata.md)。 本教程的这一部分模型 API 开发工作流首先创建 Swagger 元数据并使用它来构架 （自动生成） API 的服务器代码。 

>[AZURE.NOTE] 如果您不想要学习如何构架 Node.js Swagger 元数据文件中的代码，您可以跳过本节。 如果您希望只将代码示例部署到新的 API 应用程序，直接进入[创建 Azure 中的 API 应用程序](#createapiapp)部分。

### <a name="install-and-execute-swaggerize"></a>安装和执行 Swaggerize

1. 执行以下命令以安装**yo**和**生成器 swaggerize** NPM 模块全局。

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize 是一个由 Swagger 元数据文件中介绍的 API 生成服务器代码的工具。 您将使用的 Swagger 文件命名为*api.json*和位于您克隆存储库的*启动*文件夹中。

2. 导航到*启动*文件夹，然后执行`yo swaggerize`命令。 Swaggerize 将会提出一系列问题。  **要调用此项目的内容**， **swagger 文档的路径**中输入"ContactList"、 输入"api.json"和"快速"**明确的、 快乐的或 Restify**，输入。

        yo swaggerize

    ![Swaggerize 命令行](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **注意**︰ 如果您在此步骤中出错下, 一步解释如何修复它。

    Swaggerize 创建应用程序文件夹、 支架处理程序和配置文件，并生成**package.json**文件。 快速视图引擎用于生成 Swagger 的帮助页。  

3. 如果`swaggerize`命令失败，出现"意外的标记"或"无效的转义序列"错误，通过编辑生成的*package.json*文件中更正错误的原因。 在`regenerate`下线`scripts`，更改之前对正斜杠， *api.json*反斜杠，使线条看起来像下面的示例︰

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. 导航到包含基架的代码 （在本例中为*/start/ContactList*子文件夹） 的文件夹。

1. 运行`npm install`。
    
        npm install
        
2. 安装**jsonpath** NPM 模块。 

        npm install --save jsonpath
        
    ![Jsonpath 安装](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. **Swaggerize-ui** NPM 模块安装。 

        npm install --save swaggerize-ui
        
    ![Swaggerize 用户界面安装](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>自定义基架的代码

1. 将**lib**文件夹从**启动**文件夹复制到**ContactList**文件夹中创建的 scaffolder。 

1. **Handlers/contacts.js**文件中的代码替换为以下代码。 

    此代码使用 JSON 数据存储在由**lib/contactRepository.js**提供的**lib/contacts.json**文件。 新的 contacts.js 代码响应 HTTP 请求以获取所有联系人，并将它们返回作为 JSON 负载。 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. **Handlers/contacts/{id}.js**文件中的代码替换的 fofllowing 代码。 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. **Server.js**中的代码替换为以下代码。 

    对 server.js 文件所做的更改称为使用批注，以便您可以看到所做的更改。 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>使用本地运行的 API 进行测试

1. 激活服务器使用 Node.js 命令行可执行文件。 

        node server.js

1. 当您浏览到**http://localhost:8000/联系人**时，请参阅联系人列表的 JSON 输出 （或提示您下载，具体取决于您的浏览器）。 

    ![所有联系人 Api 调用](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. 当您浏览到**http://localhost:8000/联系人/2**时，您将看到该 id 值所表示的联系人。

    ![特定联系人 Api 调用](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Swagger 的 JSON 数据通过**的/swagger**终结点提供服务︰

    ![联系 Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Swagger 用户界面通过**/docs**终结点提供服务。 在 Swagger 用户界面中，可以使用丰富的 HTML 客户端功能来测试您的 API。

    ![Swagger 用户界面](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>创建新的 API 应用程序

在这一节中您使用 Azure 门户在 Azure 中创建一个新的 API 应用程序。 此 API 的应用程序表示 Azure 将提供运行代码的计算资源。 在后面几节中将为新的 API 应用程序部署代码。

1. 浏览到[Azure 的门户](https://portal.azure.com/)。 

1. 单击**新 > Web + 移动 > API 应用程序**。 

    ![在门户网站中的新 API 应用程序](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. 输入**应用程序名称**是唯一在*azurewebsites.net*域，如 NodejsAPIApp 和一个数字，以确保其唯一性。 

    例如，如果名为`NodejsAPIApp`，URL 将被`nodejsapiapp.azurewebsites.net`。

    如果您输入一个其他人已经使用的名称，您将看到右边一个红色感叹号。

6. 在**资源组**下拉列表，单击**新建**，然后在**新资源组的名称**输入"NodejsAPIAppGroup"或其他名称如果您喜欢。 

    [资源组](../azure-resource-manager/resource-group-overview.md)是 Azure 资源如 API 的应用程序、 数据库和虚拟机的集合。 对于本教程，则最好创建一个新的资源组，因为这可以使它轻松地在一个步骤中删除所有为教程创建的 Azure 资源。

4. **应用程序服务的规划位置**，请单击，然后单击**新建**。

    ![创建应用程序服务计划](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    在以下步骤中，您将创建新的资源组的应用程序服务计划。 应用程序服务计划指定 API 应用程序运行的计算资源。 例如，如果您选择的免费层，API 应用程序上运行共享虚拟机，而为某些支付层运行在专用的虚拟机上。 有关应用程序服务计划的信息，请参阅[应用程序服务计划概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。

5. 在**应用程序服务计划**刀片式服务器，输入"NodejsAPIAppPlan"或其他名称如果您喜欢。

5. 在**位置**下拉列表中选择与您最近的位置。

    此设置指定您的应用程序将运行在哪个 Azure 数据中心。 在本教程中，您可以选择任何地区，它不会带来明显差异。 但是，对于生产应用程序中，您希望服务器要尽可能接近到它尽量[延迟](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)访问的客户端。

5. 单击**定价层 > 全部查看 > F1 免费**。

    对于本教程，自由定价层将提供足够的性能。

    ![选择自由定价层](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. 在**应用程序服务计划**刀片式服务器，请单击**确定**。

7. 在**API 的应用程序**刀片式服务器，请单击**创建**。

## <a name="set-up-your-new-api-app-for-git-deployment"></a>设置 Git 部署新 API 应用程序

您将推到 Azure 应用程序服务中的 Git 存储库提交到 API 的应用程序部署代码。 本教程的这一部分，在您创建凭据与您将用于部署的 Azure 中的 Git 存储库。  

1. 在创建您的 API 应用程序之后，请单击**应用程序服务 > {API 应用程序}**从门户网站主页。 

    门户显示的**API 的应用程序**和**设置**的薄片。

    ![门户的 API 应用程序并设置刀片式服务器](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. 在**设置**刀片式服务器，向下滚动到**发布**部分，然后单击**部署凭据**。
 
3. 在**设置部署凭据**刀片式服务器，输入用户名和密码，然后单击**保存**。

    您将使用这些凭据将 Node.js 代码发布到您的 API 应用程序。 

    ![部署的凭据](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. 在**设置**刀片式服务器，请单击**部署源 > 选择源 > 本地 Git 存储库**，然后单击**确定**。

    ![创建 Git Repo](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. 一旦您的 Git 存储库创建刀片式服务器将更改为显示您当前部署。 存储库是新的因为列表中有任何活动的部署。 

    ![没有活动的部署](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. 复制的 Git 存储库 URL。 若要执行此操作，您新的 API 应用程序导航到刀片式服务器，并查看刀片式服务器的**概要**部分。 请注意**精要**部分中的**Git 克隆 URL** 。 当鼠标悬停在此 URL 时，您将看到一个图标，右侧会将 URL 复制到剪贴板。 单击此图标可复制的 URL。

    ![从门户网站获取 Git Url](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **注意**︰ 您将需要在下一节中的 URL 以确保保存在某处目前的 Git 克隆。

现在您有了 API 的应用程序使用 Git 存储库备份，您可以将代码推送到存储库中，将代码部署到 API 的应用程序。 

## <a name="deploy-your-api-code-to-azure"></a>部署到 Azure 的 API 代码

在本节中，您创建本地 Git 存储库的 API，包含服务器代码，然后您将推代码从该存储库中以前创建的 Azure 存储库。

1. 复制`ContactList`文件夹的位置，您可以使用新的本地 Git 存储库。 如果忘了本教程的第一部分，复制`ContactList`从`start`文件夹。否则，复制`ContactList`从`end`文件夹。

1. 在命令行工具中，导航到新文件夹，然后执行以下命令以创建一个新的本地 Git 存储库。 

        git init

     ![新本地 Git Repo](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. 执行以下命令以添加远程 API 应用程序的存储库中的 Git。 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **注意**︰ 替换为先前复制自己 Git 克隆 URL 字符串"YOUR_GIT_CLONE_URL_HERE"。 

1. 执行以下命令来创建提交包含所有您的代码。 

        git add .
        git commit -m "initial revision"

    ![Git 提交输出](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. 执行命令以将您的代码推送到 Azure。 系统会提示您输入密码，输入在 Azure 门户的前面部分中创建的一个。

        git push azure master

    这将触发于 API 应用程序的部署。  

1. 在浏览器中，向后定位到您 API 的应用程序，并且您**部署**刀片式服务器请参见部署正在发生。 

    ![部署情况](media/app-service-api-nodejs-api-app/deployment-happening.png)

    同时，命令行界面发生时将反映部署的状态。 

    ![节点 Js 部署情况](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    一旦完成部署，**部署**刀片将反映于 API 应用程序代码更改的成功部署。 

## <a name="test-with-the-api-running-in-azure"></a>与 API 在 Azure 中运行测试
 
3. API 的应用程序刀片的**精要**部分中复制**的 URL** 。 

    ![部署完成](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. 使用 REST API，客户端把邮递员弄 Fiddler （或 web 浏览器），如提供 API 调用，这是您的联系人的 URL `/contacts` API 的应用程序的终结点。 该 URL 将是`https://{your API app name}.azurewebsites.net/contacts`

    当发出一个 GET 请求对该终结点时，您得到您 API 的应用程序的 JSON 输出。

    ![按 Api 把邮递员弄](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. 在浏览器中，转到`/docs`试验 Swagger 用户界面在 Azure 上运行的终结点。

既然有连线的连续传递，可以进行代码更改并将它们部署到 Azure 中，只需通过将提交推送到 Azure Git 存储库。

## <a name="next-steps"></a>下一步行动

此时已成功创建 API 的应用程序，部署到它的 Node.js API 代码。 下一个教程演示如何[使用从使用 CORS 的 JavaScript 客户端 API 应用程序](app-service-api-cors-consume-javascript.md)。
