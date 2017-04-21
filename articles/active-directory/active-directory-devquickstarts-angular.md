<properties
    pageTitle="入门的 azure AD AngularJS |Microsoft Azure"
    description="如何构建的角度 JS 单页应用程序集成登录的 Azure AD 和调用 Azure 广告受保护的 Api 使用 OAuth。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>保护与 Azure AD AngularJS 单页面应用程序

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure 的广告使它简单明了地添加中，符号，标志和安全 OAuth API 调用的单页应用程序。  它使您的应用程序，其 Active Directory 帐户的用户进行身份验证和使用任何 web Azure 的广告，例如，Office 365 Api 或 Azure API 由受保护的 API。

对于运行在浏览器的 javascript 应用程序，Azure 广告提供了活动目录身份验证库或 adal.js。  在现实生活中的 Adal.js 的唯一目的是为了方便您的应用程序才能访问令牌。  以展示它是多么简单，此处我们将构建待办事项列表 AngularJS 应用程序的︰

- 签名使用 Azure 广告为身份标识提供程序应用程序的用户。
- 显示有关用户的某些信息。
- 安全地调用该应用程序为执行列表 API 使用载体令牌从 AAD。
- 用户在该应用程序进行签名。

若要生成完整的工作应用程序，您需要︰

2. 向应用程序注册 Azure 的广告。
3. 安装 ADAL & 配置 SPA。
5. 使用 ADAL SPA 中的页面。

若要开始，[下载应用程序骨架](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip)或[下载已完成的示例](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip)。  您还将需要 Azure AD 租户可以在其中创建用户和注册应用程序。  如果您尚没有一个租户，[了解如何获取一个](active-directory-howto-tenant.md)。

## <a name="1-register-the-directorysearcher-application"></a>*1.注册的 DirectorySearcher 应用程序*
若要使您的应用程序对用户进行身份验证并获取令牌，将首先需要将其注册在 Azure AD 租户︰

-   登录到[Azure 的管理门户](https://manage.windowsazure.com)
-   在左侧导航中，单击**活动目录**
-   选择要在其中注册该应用程序一个租户。
-   单击**应用程序**选项卡，并单击**添加**在底部抽屉里。
-   按照提示进行操作并创建新**的 Web 应用程序和/或 WebAPI**。
    -   应用程序**名称**将介绍您给最终用户的应用程序。
    -   **重定向 Uri**是指 AAD 将返回标记的位置。  有关此示例的默认位置`https://localhost:44326/`
-   完成注册后，AAD 会将您的应用程序分配一个唯一的**客户机 ID**。  您将需要此值在下一节中，所以将其复制从**配置**选项卡。
- Adal.js 使用 OAuth 隐式流与 Azure 广告进行通信。  您必须通过您的应用程序启用隐式流︰
    - 单击**管理清单**的下载的应用程序清单。
    - 打开清单，然后找到`oauth2AllowImplicitFlow`属性。 将其值设为`true`。
    - 保存并上载通过再次单击**管理清单**的应用程序清单。

## <a name="2-install-adal--configure-the-spa"></a>*2.安装 ADAL & 配置 SPA*
现在，您已经在 Azure 广告应用程序，可以安装 adal.js 和编写标识相关代码。

-   首先将 adal.js 添加到 TodoSPA 项目中使用程序包管理器控制台︰
  - 下载[adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js)并将其添加到`App/Scripts/`项目目录。
  - 下载[adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) ，并将其添加到`App/Scripts/`项目目录。
  - 加载结束之前的每个脚本`</body>`在`index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   SPA 的后端到执行列表 API 接受从浏览器的标记，进行后端需要有关应用程序注册的配置信息。 在 TodoSPA 项目中，打开`web.config`。  中的元素的值替换`<appSettings>`节，以反映到 Azure 门户输入的值。  它使用 ADAL 时，您的代码将引用这些值。
    -   `ida:Tenant`是您 Azure AD 租户，例如 contoso.onmicrosoft.com 的域
    -   `ida:Audience`必须是来自门户应用程序的**客户端 ID** 。

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3.使用 ADAL 来保护 SPA 中的页面*
Adal.js 已内置集成使用 AngularJS 路由和 http 提供程序，它使您能够保护您 SPA 中的单个视图。

- 在`App/Scripts/app.js`，adal.js 模块中将︰

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- 现在，您可以初始化`adalProvider`您的应用程序注册的配置值也在`App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- 现在力保`TodoList`查看在应用程序中，只有一行代码是必需的- `requireADLogin`。

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

您现在可以安全的单页面应用程序登录用户并向其后端 API 颁发令牌持有者的保护的请求的能力。  当用户单击`TodoList`链接，adal.js 将自动重定向到登录的 Azure AD 如有必要。  此外，adal.js 将被发送到应用程序的后端的任何 ajax 请求将自动连接 access_token。  以上是裸机最低必要构建 SPA 与 adal.js-但有多种其他 SPAs 中非常有用的功能︰

- 明确问题的登录和注销请求可以定义函数，在调用 adal.js 在控制器中。  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- 您可能希望应用程序的用户界面中显示的用户信息。  Adal 服务已添加到`userDataCtrl`控制器，以便可以访问`userInfo`对象关联的视图中，在`App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- 也有很多情况下需要知道用户已登录。  您还可以使用`userInfo`对象来收集此信息。  例如，在`index.html`可以显示基于验证状态的"登录"或"注销"按钮︰

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

祝贺您 ！ Azure 广告集成的单页面应用程序现已完成。  可对用户进行身份验证、 安全地调用及其后端使用 OAuth 2.0，并获取有关用户的基本信息。  如果还没有的话，现在是时间来填充某些用户与您租户。  运行您到执行列表 SPA，并使用一个这些用户登录。  将任务添加到其执行列表，接着又退出，并重新登录的用户。

Adal.js 可以轻松地将所有这些公共标识功能合并到您的应用程序。  它会为您的高速缓存管理，OAuth 协议支持，为各用户提供登录用户界面，刷新过期的标记，以及其他负责所有差事。

为了便于参考，提供完整的示例 （无需您的配置值）[此处](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip)。  您可以立即将移动到其他方案。  您可能想要尝试︰

[从 SPA 调用 API 的 CORS 网站 >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
