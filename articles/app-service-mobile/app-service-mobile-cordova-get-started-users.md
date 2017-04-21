<properties
    pageTitle="在 Apache Cordova 与移动应用程序上添加身份验证 |Azure 应用程序服务"
    description="了解如何使用在 Azure 应用程序服务移动应用 Apache Cordova 通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft 应用程序的用户进行身份验证。"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>添加于 Apache Cordova 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>摘要

在本教程中，您添加到任务列表快速入门项目中使用支持的标识提供程序的 Apache Cordova 在身份验证。 本教程基于[移动应用程序入门]教程，您必须首先完成。

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[观看视频显示相似的步骤](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

现在，您可以验证已禁用匿名访问您的后端。 在 Visual Studio，打开创建的项目，当您完成本教程[开始使用移动应用程序]，然后在**Google Android 模拟器**上运行应用程序并确认该应用程序启动后显示意外的连接失败。

接下来，您将更新应用程序请求的移动应用程序的后端资源之前验证用户的身份。

##<a name="add-authentication"></a>向应用程序添加验证

1. 在**Visual Studio**中，打开您的项目，然后打开`www/index.html`文件进行编辑。

2. 查找`Content-Security-Policy`文件头部分中的 meta 标记。  您将需要 OAuth 将主机添加到允许的源的列表。

  	| 提供程序               | SDK 提供程序名称 | OAuth 主机                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure 的活动目录 | aad               | https://login.windows.net   |
  	| Facebook               | facebook          | https://www.facebook.com    |
  	| Google                 | google            | https://accounts.google.com |
  	| Microsoft              | microsoftaccount  | https://login.live.com      |
  	| Twitter                | twitter           | https://api.twitter.com     |

    内容的安全策略 （为 Azure Active Directory 实现） 的示例如下所示︰

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    您应该替换`https://login.windows.net`使用 OAuth 主机从上面的表。  有关此 meta 标记的详细信息，请查阅[内容安全策略文档]。

    请注意，某些身份验证提供程序不需要内容安全策略改变时相应移动设备上使用。  例如，没有内容安全策略的更改是必需的在 Android 设备上使用 Google 身份验证时。

3. 打开`www/js/index.js`文件以进行编辑，找到`onDeviceReady()`方法，并在客户端创建代码中添加以下︰

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    请注意此代码替换现有代码创建表引用，然后刷新 UI。

    Login() 方法使用提供程序启动身份验证。 Login() 方法是异步函数返回一个 JavaScript 承诺。  初始化的其余部分位于内承诺响应，以便前在 login() 方法完成它不被执行。

4. 在刚添加的代码，将`SDK_Provider_Name`您的登录提供程序的名称。 例如，对于 Azure Active Directory，使用`client.login('aad')`。

4. 运行您的项目。  当项目已完成初始化时，您的应用程序将选定的身份验证提供程序显示 OAuth 登录页。

##<a name="next-steps"></a>下一步行动

* 了解更多[有关身份验证]与 Azure 应用程序服务。
* 通过向 Apache Cordova 应用程序[推送通知]继续本教程。

了解如何使用 Sdk。

* [Apache Cordova SDK]
* [ASP.NET 服务器 SDK]
* [Node.js SDK 服务器]

<!-- URLs. -->
[开始使用移动应用程序]: app-service-mobile-cordova-get-started.md
[内容安全策略文档]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[推式通知]: app-service-mobile-cordova-get-started-push.md
[有关身份验证]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.NET 服务器 SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js SDK 服务器]: app-service-mobile-node-backend-how-to-use-server-sdk.md
