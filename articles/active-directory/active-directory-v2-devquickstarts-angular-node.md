<properties
    pageTitle="入门的 azure AD v2.0 AngularJS |Microsoft Azure"
    description="如何生成签名中有两个人的 Microsoft 帐户的用户和工作或学校科目的角度 JS 单页应用程序。"
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a>添加登录到 AngularJS 的单页应用程序-NodeJS

在这篇文章中我们将为 AngularJS 应用程序使用 Azure Active Directory v2.0 端点添加使用 Microsoft 打开帐户登录。 v2.0 终结点使您能够在您的应用程序中执行单个集成和对个人和工作/学校帐户的用户进行身份验证。

此示例是将任务存储在后端 REST API，NodeJS 编写并使用 OAuth 载体令牌从 Azure 广告保护一个简单的待办事项列表单页应用程序。  AngularJS 应用程序将使用 JavaScript 验证库[adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js)我们开放源代码来处理整个注册过程，并获得针对 REST API 调用标记。  可以应用相同的模式对其他 REST Api，如[Microsoft Graph](https://graph.microsoft.com)或 Azure 资源管理器 Api 进行身份验证。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

## <a name="download"></a>下载

若要开始，您将需要下载和安装[node.js](https://nodejs.org)。  然后您可以复制或[下载](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip)主干应用程序︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

主干应用程序包括用于简单的 AngularJS 应用程序中，所有的样板代码，但是缺少的所有标识相关部分。  如果您不希望要继续下去，而是可以克隆或[下载](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip)完整的示例。

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>注册应用程序

首先，在[注册门户应用程序](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)中，创建一个应用程序或执行以下[详细的步骤](active-directory-v2-app-registration.md)。  请确保︰

- 添加您的应用程序的**Web**平台。
- 请输入正确的**重定向 URI**。 此示例的默认值是`http://localhost:8080`。
- 将启用**允许隐式流**复选框。 

复制下分配给您的应用程序的**应用程序 ID** ，您需要很快。 

## <a name="install-adaljs"></a>安装 adal.js
要开始，请导航到项目您下载和安装 adal.js。  如果您有安装[bower](http://bower.io/) ，您只需运行此命令。  对于任何依赖项的版本不匹配，只需选择更高的版本。
```
bower install adal-angular#experimental
```

或者，您可以手动下载[adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js)和[adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js)。  添加到这两个文件`app/lib/adal-angular-experimental/dist`目录。

现在在您喜爱的文本编辑器中，打开该项目并加载 adal.js 页正文末尾︰

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>设置 REST API，

而我们要建立的事情，可以获取后端 REST API，工作。  在命令提示符处，通过运行安装所有必需的软件包 （确保你在项目的顶级目录中）︰

```
npm install
```

现在打开`config.js`和替换`audience`值︰

```js
exports.creds = {
     
     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',
     
     ...
}
```

REST API 将使用此值来验证的令牌从 AJAX 请求的角度应用程序接收。  请注意这简单的 REST API 存储数据的内存中-因此每次停止服务器，您将丢失以前创建的所有任务。

这是我们花在讨论 REST API 是如何工作的所有时间。  随意四处在代码中，但如果您想要了解更多关于保护 web Api 加装 Azure 广告，签出[这篇文章](active-directory-v2-devquickstarts-node-api.md)。 

## <a name="sign-users-in"></a>登录的用户
若要编写一些标识代码的时间。  您可能已经注意到，adal.js 包含 AngularJS 提供程序，后者在很好地扮演角度路由机制。  通过将 adal 的模块添加到应用程序启动︰

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

现在，您可以初始化`adalProvider`应用程序 id:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

好，现在 adal.js 具有保护您的应用程序和用户登录所需的所有信息。  若要强制登录应用程序中的特定路由，所有方法都是一行代码︰

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

现在当用户单击`TodoList`链接，adal.js 将自动重定向到 Azure 广告的符号中如有必要。  此外明确可以通过调用 adal.js 在您的控制器发送登录和注销请求︰

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>显示用户信息
现在，用户已登录，您可能需要访问您的应用程序中的已登录的用户身份验证数据。  Adal.js 公开此信息，以便您在`userInfo`对象。  若要访问此对象在视图中的，首先将 adal.js 添加到相应的控制器的根范围︰

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

然后您可以直接解决`userInfo`视图中的对象︰ 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

您还可以使用`userInfo`对象以确定是否用户已登录或不。

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>REST API 调用
最后，就应该获得某些标记并调用 REST API 来创建、 读取、 更新和删除任务。  好您猜怎么样？  您不必做*的事*。  Adal.js 将自动负责获取、 缓存和刷新令牌。  它将还负责将这些标记附加到传出的 AJAX 请求发送到 REST API。  

究竟如何实现这一点呢？ 它是借助[AngularJS 拦截器](https://docs.angularjs.org/api/ng/service/$http)，它允许 adal.js 转换传出和传入的 http 消息的魔力。  此外，adal.js 假定任何请求发送到同一个域中，如窗口应该用适用于相同的应用程序 ID 为 AngularJS 的应用程序的标记。  这就是为什么我们在这两个角度的应用程序和 NodeJS REST API，使用相同的应用程序 ID。  当然，您可以重写此行为并告诉 adal.js 令牌获取其他 REST Api，如有必要-但这种简单的情况下默认值即可。

以下是显示从 Azure 广告发送请求，以不记名的标记是多么容易的代码段︰

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

祝贺您 ！  Azure AD 集成的单页应用程序现已完成。  来吧，来谢幕。  可对用户进行身份验证、 安全地调用其后端 REST API，使用 OpenID 连接，并获取有关用户的基本信息。  开箱即用，它支持与 Microsoft 个人帐户或从 Azure 广告工作/学校帐户的任何用户。  通过运行为应用程序提供一试︰

```
node server.js
```

在浏览器中定位到`http://localhost:8080`。  使用 Microsoft 个人帐户或工作/学校帐户登录。  将任务添加到用户的待办事项列表，并注销。  请尝试其他类型的帐户进行登录。 如果您需要创建与工作/学校用户 Azure AD 租户[了解如何获取此处](active-directory-howto-tenant.md)（它是免费的）。

要继续学习 v2.0 终结点，返回我们的[2.0 版开发指南 》](active-directory-appmodel-v2-overview.md)。  更多的资源，请查阅︰

- [在 GitHub 的 azure 样本 >>](https://github.com/Azure-Samples)
- [在堆栈溢出的 azure AD >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- 在 azure AD 文档[Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>我们的产品以获得安全更新

我们鼓励您能够访问[此页](https://technet.microsoft.com/security/dd252948)并订阅安全通报警告出现安全事件时的通知。
