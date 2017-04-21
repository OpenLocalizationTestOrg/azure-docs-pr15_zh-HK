<properties
    pageTitle="如何使用 JavaScript SDK Azure 的移动应用程序"
    description="如何使用 v Azure 移动应用程序"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>如何使用 Azure 的移动应用程序客户端 JavaScript 库

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

本指南指导您执行使用最新的[JavaScript SDK Azure 移动应用程序的]常见方案。 如果您是新手 Azure 移动应用程序，首先完成[Azure 移动应用程序快速启动]创建后端，并创建表。 在本指南中，我们着重介绍如何使用 HTML/JavaScript Web 应用程序中移动的后端。

## <a name="supported-platforms"></a>支持的平台

我们限制了对主流浏览器的当前和最后版本的浏览器支持︰ Google Chrome、 Microsoft 边缘、 Microsoft Internet Explorer 和 Mozilla Firefox。  我们期望 SDK 进行任何相对较现代的浏览器的函数。

因此它支持全局、 AMD，和 CommonJS 格式，作为一个通用的 JavaScript 模块分发包。

##<a name="Setup"></a>安装和系统必备组件

本指南假定您已使用的表创建后端。 本指南假定表中这些教程中的表相同的架构。

可以通过完成安装的 Azure 移动应用程序是 JavaScript SDK`npm`命令︰

```
npm install azure-mobile-apps-client --save
```

安装完成后，库位于`node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`。  将此文件复制到您的站点区域。

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

库还可以用作一个 ES2015 模块，请在 CommonJS 环境如 Browserify 和 Webpack 以及 AMD 库内。  例如︰

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>如何︰ 验证用户的身份

Azure 应用程序服务支持身份验证和授权应用程序用户使用各种外部身份提供程序︰ Facebook、 Google、 Microsoft 帐户和 Twitter。 可以对表来限制对特定操作只有已通过身份验证的用户设置权限。 此外可以使用已经过身份验证的用户的身份在服务器脚本中实现授权规则。 有关详细信息，请参阅[身份验证入门]教程。

支持两种身份验证流程︰ 服务器和客户端流。  服务器流提供了最简单的身份验证体验，因为它依赖于提供商的 web 身份验证接口。 客户端流如允许使用特定于设备的功能更深度的集成的单点登录因为它依赖于提供程序特定的 Sdk。

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>如何︰ 配置应用程序服务移动的外部的重定向 Url。

几种类型的 JavaScript 应用程序使用环回功能处理 OAuth UI 流。  这些功能包括︰

* 在本地运行您的服务
* 使用 Ionic 框架的活动重新加载
* 将重定向到应用程序服务进行身份验证。 

在本地运行可能导致问题，因为默认情况下，应用程序服务的身份验证只被配置为允许从您的移动应用程序后端访问。 使用以下步骤更改以启用身份验证，服务器中本地运行的应用程序服务设置︰

1. 登录到[Azure 门户]
2. 导航到您的移动应用程序后端。
3. 在**开发工具**菜单中选择**资源管理器**。
4. 单击**转**到新的选项卡或窗口中打开您的移动应用程序后端资源管理器。
5. 展开**配置** > **authsettings**为您的应用程序的节点。
6. 单击**编辑**按钮以启用编辑的资源。
7. 发现**allowedExternalRedirectUrls**元素，应为空。 在数组中添加您的 Url:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    数组中的 Url 替换 Url 的服务，这在本例中是`http://localhost:3000`本地 Node.js 示例服务。 您也可以使用`http://localhost:4400`波纹服务或其他某个 URL，这取决于您的应用程序的配置方式。

8. 在页面的顶部，单击**读/写**的然后单击**将**保存您的更新。

您还需要将同一个环回 Url 添加到 CORS 白名单设置︰

1. 向后定位到[Azure 的门户]。
2. 导航到您的移动应用程序后端。
3. 单击**CORS** **API**菜单中。
4. 空的**允许起源**文本框中输入的每个 URL。  创建新的文本框。
5. 单击**保存**
    
后端更新之后，您将能够在您的应用程序中使用新的环回 Url。

<!-- URLs. -->
[Azure 的移动应用程序快速启动]: app-service-mobile-cordova-get-started.md
[开始使用身份验证]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure 门户]: https://portal.azure.com/
[JavaScript SDK 的 Azure 的移动应用程序]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

