<properties
    pageTitle="Azure AD B2C︰ 安全 web API 通过 Node.js |Microsoft Azure"
    description="如何构建 Node.js web API 接受令牌从一个 B2C 租户"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C︰ 通过 Node.js 安全 web API

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

使用 Azure 活动目录 (AD Azure) B2C 可以使用 OAuth 2.0 的访问令牌来保护 web API。 这些标记允许您使用 Azure AD B2C 对 API 进行身份验证的客户端应用程序。 这篇文章演示了如何创建"待办事项列表"API，使用户能够添加和列出的任务。 Web API 使用 Azure AD B2C 保护，而只允许经过身份验证的用户可以管理他们的待办事项列表。

> [AZURE.NOTE]  此示例是使用我们的[iOS B2C 示例应用程序](active-directory-b2c-devquickstarts-ios.md)连接到而编写的。 首先，执行当前浏览，然后按照该示例。

**护照**是 Node.js 对身份验证中间件。 灵活的和模块化，护照可以后台安装在任何基于表达式的或 Restify 的 web 应用程序。 一套全面的策略支持使用身份验证的用户名和密码、 Facebook、 Twitter，等等。 我们对 Azure 活动目录 (AD Azure) 开发了一种战略。 安装本模块并将 Azure AD`passport-azure-ad`插件。

若要执行本示例，您需要︰

1. 向应用程序注册 Azure 的广告。
2. 设置应用程序以使用 Passport 的`azure-ad-passport`插件。
3. 配置客户端应用程序调用"待办事项列表"web API。


## <a name="get-an-azure-ad-b2c-directory"></a>获取 Azure AD B2C 目录

您可以使用 Azure AD B2C 之前，必须创建一个目录，或租户。  目录是所有用户、 应用程序、 组和更多的容器。  如果您没有，然后再[创建一个 B2C 的目录](active-directory-b2c-get-started.md)继续。

## <a name="create-an-application"></a>创建应用程序

接下来，您需要在您提供与您的应用程序安全地进行通信所需的一些信息的 Azure 的广告的 B2C 目录中创建应用程序。 在这种情况下，客户端应用程序和 web API 由表示单个**应用程序 ID**，因为它们构成了一个逻辑的应用程序。 若要创建一个应用程序，请按照[以下说明操作](active-directory-b2c-app-registration.md)。 请务必︰

- 包括**的 web 应用程序/web api**应用程序中
- 输入`http://localhost/TodoListService`作为**回复 URL**。 它是该代码示例的默认 URL。
- 创建应用程序的**应用程序密码**并将其复制。 稍后需要此数据。 请注意，此值必须是[转义 XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape)之前使用它。
- 复制**应用程序 ID**分配给您的应用程序。 稍后需要此数据。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>创建您的策略

在 Azure AD B2C，[策略](active-directory-b2c-reference-policies.md)定义了每个用户体验。 此应用程序包含两个标识体验︰ 注册并登录。 您需要创建的每个类型，一种策略[策略参考文章](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)中所述。  创建您的三个策略时，请务必︰

- 在您注册策略选择的**显示名称**和注册的其他属性。
- 在每个策略选择的**显示名称**和**对象 ID**的应用程序声明。  您可以选择以及其他索赔。
- 您在创建后，将复制下的每个策略的**名称**。 它应具有前缀`b2c_1_`。  稍后需要这些策略名称。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

您已经创建了三种策略后，就可以生成您的应用程序。

若要了解有关在 Azure AD B2C 策略如何工作，开始与[.NET web 应用程序快速入门教程](active-directory-b2c-devquickstarts-web-dotnet.md)。

## <a name="download-the-code"></a>下载的代码

[在 GitHub 上维护](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS)此教程的代码。 当您生成示例，您可以[下载的主干项目为.zip 文件](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip)。 您还可以复制骨架︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

已完成应用程序同时也是[一个.zip 文件](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip)或在`complete`的同一存储库中的分支。

## <a name="download-nodejs-for-your-platform"></a>下载用于您的平台的 Node.js

要成功地使用此示例，您需要正常运行的 Node.js。 

从[nodejs.org](http://nodejs.org)安装 Node.js。

## <a name="install-mongodb-for-your-platform"></a>安装用于您的平台的 MongoDB

要成功地使用此示例，您需要 MongoDB 的正常运行。 我们使用 MongoDB 让 REST API 在服务器实例之间保持不变。

从[mongodb.org](http://www.mongodb.org)安装 MongoDB。

> [AZURE.NOTE] 此预排假定默认安装与服务器终结点使用 MongoDB，在撰写本文时它是`mongodb://localhost`。

## <a name="install-the-restify-modules-in-your-web-api"></a>Restify 模块安装在您的 web API

我们使用 Restify 生成 REST API。 Restify 最小且灵活的 Node.js 应用程序框架从速成版。 它有一套可靠的构建连接在 REST Api 的功能。

### <a name="install-restify"></a>安装 Restify

从命令行，您目录更改为`azuread`。 如果`azuread`目录并不存在，则创建它。

`cd azuread`或`mkdir azuread;`

输入以下命令︰

`npm install restify`

此命令会安装 Restify。

#### <a name="did-you-get-an-error"></a>您获得一个错误吗？

在某些操作系统中，当您使用`npm`，您可能会收到错误消息`Error: EPERM, chmod '/usr/local/bin/..'`和请求您以管理员身份运行的帐户。 如果发生此问题，请使用`sudo`命令来运行`npm`在更高的特权级别。

#### <a name="did-you-get-a-dtrace-error"></a>您未获得 DTrace 错误？

安装 Restify 时，您可能会看到此文本类似︰

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify 提供了强大的机制，用于跟踪其余部分使用 DTrace 调用。 但是，许多操作系统不具有可用 DTrace。 您可以安全地忽略这些错误。

命令的输出与以下文本类似︰

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>在您的 web API 中安装护照

从命令行，您目录更改为`azuread`，如果尚不存在。

安装护照使用下面的命令︰

`npm install passport`

命令的输出应该与以下文本类似︰

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>将护照 azuread 添加到您的 web API

接下来，通过使用添加 OAuth 战略`passport-azuread`，一套与 Passport Azure AD 的战略。 针对 REST API 示例中的持有者标记使用此策略。

> [AZURE.NOTE] 尽管 OAuth2 提供了一个框架，可发出任何已知的标记类型，只有某些标记类型得到广泛使用。 为保护终结点的标记是持有者的标记。 将 OAuth2 中最广泛颁发这些类型的标记。 许多实现假定载荷令牌颁发令牌的唯一类型。

从命令行，您目录更改为`azuread`，如果尚不存在。

安装护照`passport-azure-ad`模块使用下面的命令︰

`npm install passport-azure-ad`

命令的输出应该与以下文本类似︰

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>将 MongoDB 模块添加到您的 web API

此示例使用 MongoDB 作为数据存储。 对于安装 Mongoose，广泛使用插件来管理模型和架构。

* `npm install mongoose`

## <a name="install-additional-modules"></a>安装附加模块

接下来，安装其余所需的模块。

从命令行，您目录更改为`azuread`，如果尚不存在︰

`cd azuread`

安装的模块中您`node_modules`目录︰

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>创建一个 server.js 文件，您的依赖性

`server.js`文件的 Web API 服务器提供的大部分功能。 

从命令行，您目录更改为`azuread`，如果尚不存在︰

`cd azuread`

创建`server.js`在编辑器中的文件。 添加以下信息︰

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

保存该文件。 您稍后返回到它。

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>创建一个用于存储 Azure 广告设置的 config.js 文件

此代码文件传递到 Azure 广告门户网站中的配置参数`Passport.js`文件。 当您将 web API 添加到门户中演练的第一部分，您可以创建这些配置值。 我们解释后复制代码，将这些参数的值。

从命令行，您目录更改为`azuread`，如果尚不存在︰

`cd azuread`

创建`config.js`在编辑器中的文件。 添加以下信息︰

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>所需的值

`clientID`︰ Web API 应用程序客户端 ID。

`IdentityMetadata`︰ 这是在哪里`passport-azure-ad`查找标识提供程序的配置数据。 它还查找要验证的 JSON web 标记的键。 

`audience`︰ 在门户中标识您调用的应用程序统一资源标识符 (URI)。 

`tenantName`︰ 您组织的名称 (例如， **contoso.onmicrosoft.com**)。

`policyName`︰ 要验证进入到您的服务器的令牌策略。 此策略应该在客户端应用程序使用用于登录到相同的策略。

> [AZURE.NOTE] 我们 B2C 预览，跨客户端和服务器的安装中使用相同的策略。 如果您已经完成考察并创建这些策略，您不需要再次这样做。 因为完成预排，不应需要在网站上设置的客户端预排的新策略。

## <a name="add-configuration-to-your-serverjs-file"></a>添加到 server.js 文件的配置

若要读取的值`config.js`文件创建、 添加`.config`为您的应用程序中所需的资源文件，然后将全局变量设置为那些在`config.js`文档。

从命令行，您目录更改为`azuread`，如果尚不存在︰

`cd azuread`

打开`server.js`在编辑器中的文件。 添加以下信息︰

```Javascript
var config = require('./config');
```
添加新节到`server.js`，其中包括下列代码︰

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

接下来，让我们添加一些占位符我们收到来自我们调用应用程序的用户。

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

接下来，创建我们记录器太。

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>通过使用 Mongoose 添加 MongoDB 模型和架构信息

因为将一起在 REST API 服务这三个文件的较早的准备工作事半功倍。

此预排，使用 MongoDB 来存储您的任务，如前文所述。

在`config.js`文件，您可以呼叫您的数据库**任务列表**。 该名称也是在结尾处放置`mongoose_auth_local`连接 URL。 您不需要在 MongoDB 中预先创建此数据库。 创建数据库的服务器应用程序首次运行。

告诉服务器要使用的 MongoDB 数据库后，您需要编写一些额外的代码来创建模型和架构的服务器任务。

### <a name="expand-the-model"></a>扩展模型

此架构模型非常简单。 可以根据需要将其展开。

`owner`︰ 谁被分配给该任务。 此对象是一个**字符串**。  

`Text`︰ 自身任务。 此对象是一个**字符串**。

`date`︰ 任务的截止日期日期。 此对象是**日期时间**。

`completed`︰ 如果该任务已完成。 此对象是一个**boolean 类型的值**。

### <a name="create-the-schema-in-the-code"></a>在代码中创建架构

从命令行，您目录更改为`azuread`，如果尚不存在︰

`cd azuread`

打开`server.js`在编辑器中的文件。 添加配置项的下列信息︰

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
首先创建架构，然后创建用于定义您**的路由**存储您的数据中的代码模型对象。

## <a name="add-routes-for-your-rest-api-task-server"></a>添加针对 REST API 任务服务器的路由

现在，您已经使用的数据库模型，添加用于 REST API 服务器的路由。

### <a name="about-routes-in-restify"></a>关于在 Restify 中的路由

工艺路线的作用及其工作的使用速成堆栈时相同的方式在 Restify 中。 通过使用您希望调用的客户端应用程序的 URI 定义路由。 

Restify 工艺的典型模式是︰

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify 和速成版可以提供更深入的功能，如定义应用程序类型和执行复杂路由跨不同的终结点。 出于本教程中，我们使这些工艺路线简单。

#### <a name="add-default-routes-to-your-server"></a>将默认路由添加到您的服务器

现在，您针对 REST API，我们添加基本的 CRUD 路由**创建**和**列表**。 可以在中找到其他路由`complete`样本的分支。

从命令行，您目录更改为`azuread`，如果尚不存在︰

`cd azuread`

打开`server.js`在编辑器中的文件。 下面做上面的数据库条目中添加以下信息︰

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>添加错误处理的路由

添加某些错误处理，以便您可以返回到客户端以其可理解的方式传达您遇到的任何问题。

添加以下代码︰

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>创建您的服务器

您现在已定义了数据库并就地放置您的路由。 为您要做最后一件事情是添加服务器实例来管理您的呼叫。

Restify 和快速提供深层次自定义针对 REST API 服务器，但在此处，我们使用最基本的安装程序。 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>将路由添加到服务器 （无身份验证）

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>添加到您的 REST API 服务器身份验证

现在，您已经运行的 REST API 服务器，您可以进行对 Azure 的广告很有用。

从命令行，您目录更改为`azuread`，如果尚不存在︰

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>使用 passport azure 广告中包含 OIDCBearerStrategy


> [AZURE.TIP]
在编写 Api 时，您应始终的数据链接到一个独特无法欺骗用户的令牌中。 当服务器上存储 ToDo 项时，它是用户的根据**oid** （通过 token.oid 调用） 的标记，在其"所有者"字段中。 此值确保只有该用户可以访问他们自己的 ToDo 项目。 没有暴露在 API 中的"所有者"，使外部用户可以请求其他用户的 ToDo 项，即使它们进行身份验证。

接下来，使用附带的持有者战略`passport-azure-ad`。

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

护照的所有策略使用相同的模式。 向其传递`function()`，`token`和`done`作为参数。 战略转回来，您之后它已完成其工作的所有。 然后应该存储用户并保存标记，以便您不需要再次对其进行询问。

> [AZURE.IMPORTANT]
上面的代码将碰巧到您的服务器进行身份验证的任何用户。 这一过程被称为 autoregistration。 在生产服务器中，不要让任何用户访问 API 而无需事先他们经历注册过程。 这一过程通常是在使用者应用程序允许您使用 Facebook 注册但然后要求您填写其他信息中看到的图案。 如果该程序不是一个命令行程序，我们可以提取电子邮件的标记的对象的返回，然后要求用户填写的其他信息。 因为这是一个示例，我们可以将它们添加到内存中的数据库。



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>运行服务器应用程序以验证它拒绝您

您可以使用`curl`以查看是否已将 OAuth2 保护现在拥有针对您的终结点。 返回的标头就会告诉您您是在正确的路径上。

请确保您的 MongoDB 实例正在运行︰

    $sudo mongodb

切换到该目录并运行服务器︰

    $ cd azuread
    $ node server.js

在新的终端窗口中，运行`curl`

请尝试基本开机自检︰

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 错误时所需的响应。 它表示护照层正试图重定向到授权的终结点。


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>您现在已经使用 OAuth2 的 REST API 服务

通过使用 Restify 和 OAuth 实现 REST API ！ 您现在有足够的代码，以便您可以继续开发服务和生成此示例。 您已为不使用的 OAuth2 兼容的客户端可以使用此服务器。 这下一步使用与我们[连接到 web API 通过 B2C 的 Io](active-directory-b2c-devquickstarts-ios.md)演练类似其他预排。


## <a name="next-steps"></a>下一步行动

您现在可以移动到更高级的主题，如︰

[连接到 web API 的 B2C 中使用 iOS](active-directory-b2c-devquickstarts-ios.md)