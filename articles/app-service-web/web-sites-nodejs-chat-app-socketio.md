<properties
    pageTitle="使用 Socket.IO 在 Azure 应用程序服务创建 Node.js 聊天应用程序"
    description="演示如何在 Azure 上承载 node.js web 应用程序中使用 socket.io 教程。"
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

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>使用 Socket.IO 在 Azure 应用程序服务创建 Node.js 聊天应用程序

Socket.IO 提供了 node.js 服务器和客户端使用 Websocket 之间的实时通信。 它还支持回退到使用旧版本浏览器的其他传输 （例如长轮询）。 本教程将指导您完成承载 Azure 的 web 应用程序中，作为一个基于 Socket.IO 聊天应用程序，向您展示如何扩展使用[Azure Redis 缓存]的应用程序。 Socket.IO 的详细信息，请参阅<http://socket.io/>。

> [AZURE.NOTE] 在此任务中的过程适用于[应用程序服务 Web 应用程序];对于云服务，请参阅[生成 Node.js 聊天应用程序与 Socket.IO Azure 云服务上]。

## <a name="download-the-chat-example"></a>下载聊天示例

对于此项目，我们将使用[Socket.IO GitHub 存储库]中的聊天示例。 执行以下步骤来下载该示例并将其添加到以前创建的项目。

1.  下载[ZIP 或 GZ 存档版本]的 Socket.IO 项目 （版本 1.3.5 已用于本文档）

1.  提取存档和复制**示例\\聊天**目录到新的位置。 例如，**\\节点\\聊天**。

## <a name="modify-appjs-and-install-modules"></a>修改 app.js，并安装模块

1.  将**index.js**文件重命名为**app.js**。 这使得 Azure 来检测这是 Node.js 应用程序。

1.  在文本编辑器中打开**app.js**文件。 更改行包含`var io = require('../..')(server);`如下所示︰

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. 打开**package.json**文件并添加参考下的 socket.io `dependencies`，如下所示︰

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. 从命令行中，将更改为**\\节点\\聊天**目录和使用 npm 安装此应用程序所需的模块︰

        npm install

    这会将模块安装到一个子文件夹命名为**node_modules**。

## <a name="create-an-azure-web-app"></a>创建一个 Azure 的 Web 应用程序

按照以下步骤创建 Azure 的 web 应用程序启用 Git 发布，然后允许 WebSocket 使 web 应用程序。

> [AZURE.NOTE] 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure 免费试用版</a>。

1. 安装 Azure 命令行界面 (Azure CLI) 和连接到 Azure 订购。 请参见[安装和配置的 Azure CLI](../xplat-cli-install.md)。

1. 如果这是您第一次设置 Azure 中的存储库，您需要创建登录凭据。 从 Azure CLI 中，输入以下命令︰

        azure site deployment user set [username] [password]

1. 更改为**\\node\chat**目录，并使用下面的命令来创建新的 Azure web 应用程序和本地 Git 存储库。 此命令还创建了 Git 远程命名 azure。

        azure site create mysitename --git

    您必须替换您的 web 应用程序的唯一名称 mysitename。

1. 通过使用下列命令提交到本地存储库中的现有文件︰

        git add .
        git commit -m "Initial commit"

1. 将文件推送到 Azure Web 应用程序存储库使用以下命令︰

        git push azure master

    出现提示时，请在步骤 2 中输入您的凭据。 因为在服务器上导入模块，您将收到的状态消息。 完成此过程后，将在 Azure 的 web 应用程序上承载的应用程序。

    > [AZURE.NOTE] 在模块安装时，您可能会注意到的错误，导入的项目找不到。 可以安全地忽略这些。

1. Socket.IO 使用 Websocket，默认情况下，在 Azure 上未启用。 若要启用 web 套接字，请使用下面的命令︰

        azure site set -w

    如果出现提示，请输入该 web 应用程序的名称。

    >[AZURE.NOTE]
    >'设置-w azure 站点命令将仅使用版本 0.7.4 或更高版本的 Azure 命令行界面。 您还可以启用 WebSocket 支持使用[Azure 门户](https://portal.azure.com)。
    >
    >若要启用使用 Azure 门户的 Websocket，单击刀片式服务器 Web 应用程序从 web 应用程序中，单击**所有设置** > **应用程序设置**。 **在**Web 套接字**，请单击。** 然后单击**保存**。

1. 若要查看在 Azure 上使 web 应用程序，请使用以下命令以启动 web 浏览器并定位到承载的 web 应用程序︰

        azure site browse

您的应用程序是现在对 Azure 上运行，并可以将中继使用 Socket.IO 的不同客户端之间的聊天消息。

## <a name="scale-out"></a>向外扩展

Socket.IO 应用程序可以使用__适配器__分发消息和应用程序的多个实例之间的事件进行扩容。 虽然有多个适配器， [socket.io redis]适配器可以轻松地使用 Azure Redis 缓存功能。

> [AZURE.NOTE] 横向扩展 Socket.IO 解决方案的其他要求是对粘滞会话的支持。 通过请求路由选择 Azure 的 Azure Web 应用程序默认情况下启用粘滞会话。 有关详细信息，请参见[实例相似性 Azure 网站中]。

### <a name="create-a-redis-cache"></a>创建一个 Redis 高速缓存

[创建 Redis Azure 的高速缓存中的缓存]创建一个新的高速缓存中执行的步骤。

> [AZURE.NOTE] 保存的__主机名__和__主键__为缓存，这些将在接下来的步骤中所需。

### <a name="add-the-redis-and-socketio-redis-modules"></a>添加了 redis 和 socket.io redis 模块

1. 从命令行更改为__\\节点\\聊天__目录，并使用下面的命令。

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] 在此命令中指定的版本是在测试本文所使用的版本。

1. 修改__app.js__文件，以添加以下行紧随其后`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    替换为__redishostname__和__rediskey__的主机名和 Redis 缓存键。

    这将创建发布和订阅以前创建的 Redis 缓存到客户端。 客户端然后用于与适配器配置 Socket.IO 使用 Redis 缓存实例的应用程序之间传递消息和事件

    > [AZURE.NOTE] 直接向 Redis， __socket.io redis__适配器可以进行通信，而当前版本不支持 Azure Redis 缓存所需的身份验证。 因此在建立初始连接使用__redis__模块中，然后客户端将被传递给__socket.io redis__适配器。
    >
    > Azure Redis 缓存支持安全连接使用端口 6380，虽然在本示例中使用的模块不支持安全连接 7/14/2014 年。 上面的代码使用默认的 6379 不安全端口。

1. 保存修改后的__app.js__

### <a name="commit-changes-and-redeploy"></a>提交更改并重新部署

从命令行中__\\节点\\聊天__目录中，使用下列命令以提交更改并重新部署应用程序。

    git add .
    git commit -m "implementing scale out"
    git push azure master

后所做的更改已被推送到该服务器，可以使用下面的命令多个实例缩放您的站点。

    azure site scale instances --instances #

其中__#__是要创建的实例数。

可以从多个浏览器或计算机以验证，正确地将消息发送到所有客户端连接到您的 web 应用程序。

## <a name="troubleshooting"></a>故障排除

### <a name="connection-limits"></a>连接限制

Azure 的 Web 应用程序可用于多个 Sku，确定您的站点的可用资源。 这包括允许 WebSocket 连接数。 有关详细信息，请参阅[Web 应用程序定价页]。

### <a name="messages-arent-being-sent-using-websockets"></a>不使用 Websocket 正在发送邮件

如果客户端浏览器将回退到而不是使用 Websocket 长轮询，可能是由于下列情况之一。

* **尝试限制到只 Websocket 传输**

    为了使 Socket.IO Websocket 用作消息传输，在服务器和客户端必须支持 Websocket。 如果一个或另一个却没有，Socket.IO 将协商另一种传输，如长轮询。 运输由 Socket.IO 的默认列表是` websocket, htmlfile, xhr-polling, jsonp-polling`。 您可以强制它只通过后行包含到**app.js**文件中，添加下面的代码使用 Websocket `, nicknames = {};`。

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] 请注意，旧版本的浏览器不支持 Websocket 将不能连接到该网站，在上面的代码处于活动状态，因为它限制只有到 Websocket 通信。

* **使用 SSL**

    Websocket 依赖于一些较少使用 HTTP 标头，如**升级**的标题。 一些中间网络设备，例如，web 代理服务器，可以删除这些标头。 若要避免此问题，您可以通过 SSL 建立 WebSocket 连接。

    完成此操作的简便方法是配置为 Socket.IO `match origin protocol`。 这会指示 Socket.IO 力保 Websocket 通信与 web 页的原始 HTTP/HTTPS 请求相同。 如果浏览器使用 HTTPS URL 来访问您的网站，通过 Socket.IO 的后续 WebSocket 通信将通过 SSL 保护。

    若要修改此示例以启用此配置，下面的代码向文件中添加**app.js**后行包含`, nicknames = {};`。

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **验证 web.config 设置**

    Azure 的 web 应用程序宿主 Node.js 应用程序使用**web.config**文件可将传入的请求路由到 Node.js 应用程序。 正常运行 Node.js 应用程序到 Websocket， **web.config**必须包含以下项。

        <webSocket enabled="false"/>

    这将禁用 IIS Websocket 模块，其中包括 Websocket 和 Node.js 特定 WebSocket 模块，如 Socket.IO 与冲突的它自己的实现。 如果该行不存在，或者设置为`true`，这可能是 WebSocket 传输您的应用程序无法正常工作的原因。

    通常情况下，Node.js 应用程序中不包括一个**web.config**文件，因此在部署时，Azure 网站会自动生成一个用于 Node.js 应用程序。 由于在服务器上自动生成此文件，必须使用 FTP 或提取您的网站的 URL 来查看此文件。 可以通过选择 web 应用程序，然后单击**仪表板**链接在传统门户网站找到 FTP 和提取的 Url。 Url 显示在**快速**部分。

    > [AZURE.NOTE] 如果您的应用程序不提供，Azure 网站才会生成**web.config**文件。 如果您提供应用程序项目的根目录中的一个**web.config**文件，Azure Web 应用程序会使用它。

    如果该条目不存在，或被设置为一个值的`true`，则应在 Node.js 应用程序的根目录中创建**web.config**并将值指定为`false`。  为了便于参考，下面是使用**app.js**作为入口点的应用程序默认**web.config** 。

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    如果您的应用程序使用**app.js**以外的入口点，必须将**app.js**的所有匹配项替换为正确的入口点。 例如，将**app.js**替换**server.js**。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务]，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="next-steps"></a>下一步行动

在本教程中，您学习了如何创建 Azure 的 web 应用程序中托管的聊天应用程序。 您还可以存放 Azure 云服务为此应用程序。 有关如何完成此操作的步骤，请参阅[生成 Node.js 聊天应用程序与 Socket.IO Azure 云服务上]。

有关详细信息，请参见[Node.js 开发中心]。

## <a name="whats-changed"></a>会发生什么变化

* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响]。

<!-- URL List -->

[Azure 的 Redis 高速缓存]: /documentation/services/redis-cache/
[应用程序服务 Web 应用程序]: http://go.microsoft.com/fwlink/?LinkId=529714
[网页应用程序定价]: http://go.microsoft.com/fwlink/?LinkId=511643
[在 Azure 的云服务上生成 Socket.IO 的 Node.js 聊天应用程序]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure 应用程序服务，并对现有的 Azure 服务的影响]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js 开发人员中心]: /develop/nodejs/
[试用应用程序服务]: http://go.microsoft.com/fwlink/?LinkId=523751
[在 Azure 网站实例相似性]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[在 Azure Redis 缓存中创建缓存]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub 存储库]: https://github.com/socketio/socket.io
[ZIP 或 GZ 存档版本]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
