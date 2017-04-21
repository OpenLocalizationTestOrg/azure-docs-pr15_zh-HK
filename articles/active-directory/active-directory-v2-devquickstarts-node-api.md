<properties
    pageTitle="Azure AD v2.0 NodeJS Web API |Microsoft Azure"
    description="如何构建一个 NodeJS Web API 接受令牌从两个人的 Microsoft 帐户和工作或学校的科目。"
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>安全使用 node.js Web API

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

Azure Active Directory v2.0 终结点，可以防止使用[OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)的访问令牌，Web API 允许用户使用个人 Microsoft 帐户和工作或者学校帐户添加到安全地访问您的 Web API。

**护照**是 Node.js 对身份验证中间件。 非常灵活，模块化的护照可以后台删除为任何基于表达式的或 Resitify 的 web 应用程序。 一套全面的策略支持身份验证使用用户名和密码、 Facebook、 Twitter，等等。 对于 Microsoft Azure Active Directory，我们开发了一种战略。 我们将安装该模块，然后添加 Microsoft Azure Active Directory`passport-azure-ad`插件。

## <a name="download"></a>下载
本教程中的代码[在 GitHub 上](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs)维护。  要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip)或克隆主干︰

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

同时本教程末尾提供了完整的应用程序。


## <a name="1-register-an-app"></a>1.注册应用程序
在[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)，创建新的应用程序，或按照以下[详细的步骤](active-directory-v2-app-registration.md)。  请确保︰

- 复制下分配给您的应用程序的**应用程序 Id** ，您需要很快。
- 添加您的应用程序的**移动**平台。
- 从门户中复制下**重定向 URI** 。 您必须使用默认值为`urn:ietf:wg:oauth:2.0:oob`。


## <a name="2-download-nodejs-for-your-platform"></a>2︰ 下载用于您的平台的 node.js
若要成功使用此示例，您必须正常运行的 Node.js。

从[http://nodejs.org](http://nodejs.org)安装 Node.js。

## <a name="3-install-mongodb-on-to-your-platform"></a>3︰ 安装到您的平台的 MongoDB

若要成功使用此示例，您必须 MongoDB 的正常运行。 我们将使用 MongoDB 来使我们的 REST API，persistant 在整个服务器实例。

从[http://mongodb.org](http://www.mongodb.org)安装 MongoDB。

> [AZURE.NOTE] 本演练假定 MongoDB，在撰写本文时它是使用默认安装与服务器终结点︰ mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4︰ 安装到您的 Web API 中的 Restify 模块

我们将使用 Resitfy 来构建我们的 REST API。 Restify 最小且灵活的 Node.js 应用程序框架从速成版有一套可靠的构建连接在 REST Api 的功能。

### <a name="install-restify"></a>安装 Restify

从命令行中，将目录更改到 azuread 目录。 如果**azuread**目录不存在，则创建它。

`cd azuread`-或者-`mkdir azuread;`

键入以下命令︰

`npm install restify`

此命令会安装 Restify。

#### <a name="did-you-get-an-error"></a>您获得一个错误吗？

在某些操作系统上使用 npm，您可能会收到错误的错误︰ EPERM，chmod / usr/本地/bin /.. 并尝试以管理员身份运行的帐户的请求。 如果发生这种情况，使用 sudo 命令以更高的特权级别运行 npm。

#### <a name="did-you-get-an-error-regarding-dtrace"></a>您没有获得有关 DTrace 的错误？

安装 Restify 时，您可能会看到类似下面这样︰

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


Restify 提供了强大的机制来跟踪使用 DTrace 的其余部分调用。 但是，许多操作系统不具有可用 DTrace。 您可以安全地忽略这些错误。


此命令的输出应类似于以下内容︰


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5︰ 将 Passport.js 安装到您的 Web API

[护照](http://passportjs.org/)是 Node.js 对身份验证中间件。 非常灵活，模块化的护照可以后台删除为任何基于表达式的或 Resitify 的 web 应用程序。 一套全面的策略支持身份验证使用用户名和密码、 Facebook、 Twitter，等等。 对于 Azure Active Directory，我们开发了一种战略。 我们将安装该模块，然后再添加插件的 Azure Active Directory 策略。

从命令行中，将目录更改到 azuread 目录。

输入以下命令以安装 passport.js

`npm install passport`

命令的输出应类似于以下内容︰

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6︰ 添加到您的 Web API 护照 Azure 广告

接下来，我们将添加了 OAuth 战略，使用 passport azuread，一套与 Passport Azure Active Directory 的战略。 在 Rest API，在本例中，我们将为载体标记使用此策略。

> [AZURE.NOTE] 尽管 OAuth2 提供了一个框架，可发出任何已知的标记类型，只有某些标记类型已获得广泛的使用。 为保护已被证明是持有者的标记的端点。 持有者标记是标记的最广泛颁发 OAuth2 中的类型和许多实现假定载荷标记是标记的唯一的颁发令牌类型。

从命令行中，将目录更改到 azuread 目录

键入以下命令以安装 Passport.js 护照 azure 广告模块︰

`npm install passport-azure-ad`

命令的输出应类似于以下内容︰

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7︰ 将 MongoDB 模块添加到您的 Web API

我们将使用 MongoDB 作为我们的数据存储，因此，我们需要安装两个广泛使用插件来管理模型和架构称为的 Mongoose，以及 MongoDB，数据库驱动程序也称为 MongoDB。


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8︰ 安装附加模块

接下来，我们将安装其余所需的模块。


从命令行中，将目录更改到**azuread**文件夹如果不是已经存在︰

`cd azuread`


输入以下命令以安装在 node_modules 目录中的下列模块︰

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9︰ 创建您的依赖性与 server.js

Server.js 文件将为我们 Web API 服务器提供的大部分我们的功能。 我们将添加大部分我们的代码到该文件。 用于生产目的将重构中为更小的文件，如单独的工艺路线和控制器的功能。 此演示目的，我们将使用 server.js 实现此功能。

从命令行中，将目录更改到**azuread**文件夹如果不是已经存在︰

`cd azuread`

创建`server.js`文件在我们最喜爱的编辑器中，添加以下信息︰

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

保存该文件。 我们将很快返回到它。

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10︰ 创建配置文件 Azure 广告设置的存储

此代码文件将从 Azure 活动目录门户网站的配置参数传递给 Passport.js。 当您添加到在本演练的第一部分门户 Web API，您可以创建这些配置值。 我们将介绍什么复制完代码之后，将这些参数的值。


从命令行中，将目录更改到**azuread**文件夹如果不是已经存在︰

`cd azuread`

创建`config.js`文件在我们最喜爱的编辑器中，添加以下信息︰

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>所需的值

*IdentityMetadata*︰ 这是护照 azure 广告将寻找 IdP 为密钥来验证 JWT 令牌配置数据。 您可能不希望使用 Azure Active Directory 如果更改。

*观众*︰ 您从该门户的重定向 URI。

> [AZURE.NOTE]
频繁地进行我们的键。 请确保要始终将"openid_keys"URL 中，应用程序可以访问互联网。


## <a name="11-add-configuration-to-your-serverjs-file"></a>11︰ 添加到 server.js 文件的配置

我们需要您刚创建跨我们的应用程序配置文件中读取这些值。 要做到这一点，我们只是在我们的应用程序所需的资源添加.config 文件，然后将全局变量设置为那些在 config.js 文档中

从命令行中，将目录更改到**azuread**文件夹如果不是已经存在︰

`cd azuread`

打开您`server.js`文件在我们最喜爱的编辑器中，添加以下信息︰

```Javascript
var config = require('./config');
```
然后，添加到新的一节`server.js`用下面的代码︰

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>第 12 步︰ 添加 MongoDB 模型和使用 Moongoose 架构信息

现在所有这些准备工作即将开始为我们风力这三个文件放在一起在 REST API 服务取得成效。

在本演练中我们将使用 MongoDB 来存储我们的任务，如***步骤 4***所述。

如果您还记得我们在步骤 11 中创建的 config.js 文件中，我们称我们数据库的*任务列表*，因为这是我们在我们的 mogoose_auth_local 连接 URL 的末尾放置。 您不必在 MongoDB 中预先创建此数据库，它将创建此我们上第一次运行服务器应用程序 （假设不存在）。

现在，我们已告诉我们想要使用哪种 MongoDB 数据库服务器，我们需要编写一些额外的代码来为我们的服务器任务创建的模型和架构。

#### <a name="discussion-of-the-model"></a>该模型的讨论

我们的架构模式是非常简单，并根据需要对其进行展开。

名称-分配给此任务的名称。 一个***字符串***

任务-任务本身。 一个***字符串***

日期-任务的截止日期。 ***日期时间***

-如果完成或不完成任务。 ***布尔***

#### <a name="creating-the-schema-in-the-code"></a>在代码中创建架构


从命令行中，将目录更改到**azuread**文件夹如果不是已经存在︰

`cd azuread`

打开您`server.js`我们喜爱的编辑器中的文件和添加的配置项的下列信息︰

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
将连接到 MongoDB 服务器并退回给我们的架构对象。

#### <a name="using-the-schema-create-our-model-in-the-code"></a>在代码中使用该架构，创建我们的模型

上面您编写的代码，下面添加以下代码︰

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
您可以看出从代码，我们创建我们的架构，然后创建一个模型对象，我们用来存储我们的数据中的代码时我们定义我们的***工艺路线***。

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>步骤 13︰ 添加我们我们 REST API，任务的服务器的路由

现在，我们有数据库模型来处理，让我们添加我们将使用 REST API 服务器的路由。

### <a name="about-routes-in-restify"></a>关于路由在 Restify

工艺路线的作用完全相同的方式一样使用 Express 堆栈在 Restify 中。 定义路由使用您希望调用的客户端应用程序的 URI。 通常，在一个单独的文件中定义您的路由。 对于我们来说，我们将为我们的路由中的 server.js 文件。 我们建议到各自的文件用于生产环境考虑这些。

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


这是最基本的级别模式。 Resitfy （和 Express） 提供如定义应用程序类型和执行跨不同的终结点的复杂路由更深入了解 functionaltiy。 对于我们来说，我们会保留这些路由非常简单。

#### <a name="add-default-routes-to-our-server"></a>将默认路由添加到我们的服务器

我们现在将添加基本 CRUD 路由创建、 检索，更新和删除。

从命令行中，将目录更改到**azuread**文件夹如果不是已经存在︰

`cd azuread`

打开您`server.js`我们喜爱的编辑器中的文件并添加下面做上面的数据库项的以下信息︰

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>添加一些错误处理的路由

就应该添加某些错误处理，以便我们可以向客户端通信的方式我们遇到的问题可以理解它。

添加以下代码，下面上面已经编写的代码︰

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


## <a name="step-14-create-your-server"></a>第 14 步︰ 创建您的服务器 ！

我们有我们的数据库中定义，我们在的地方，有我们的路由以及最后一个办法就是添加我们将管理我们的呼叫的服务器实例。

Restify （和 Express） 有很大的深层次自定义可以执行针对 REST API 服务器，但是再次我们将使用我们的目的而言最基本的安装程序。

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15︰ 添加的路由 （而不是现在的身份验证）

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16︰ 我们添加 OAuth 的支持之前，让我们来运行服务器。

测试您的服务器之前我们添加身份验证

若要执行此操作的最简单方法是在命令行中使用卷曲。 我们这样做之前，我们需要使我们能够分析作为 JSON 输出的简单实用工具。 要做到这一点，安装 json 工具，如下面的所有示例都使用的。

`$npm install -g jsontool`

此全局安装的 JSON 工具。 现在，我们已经实现的--让我们播放服务器︰

首先，请确保 monogoDB 实例正在运行。

`$sudo mongod`

然后，更改到目录开始出现卷曲.

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

然后，我们可以通过这种方式添加一项任务︰

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

应该响应︰

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
然后，我们可以通过这种方式为 Brandon 中列出的任务︰

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

如果所有这些都没什么问题，我们就可以到 REST API 服务器添加 OAuth。

**您必须使用 MongoDB 的 REST API 服务器 ！**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17︰ 添加到我们的 REST API 服务器身份验证

现在，我们已经正在运行的 REST API (恭喜，顺便说一下 ！) 让我们开始进行对 Azure 的广告很有用。

从命令行中，将目录更改到**azuread**文件夹如果不是已经存在︰

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1︰ 使用 passport azure 广告中包含 oidcbearerstrategy

到目前为止我们已经构建了典型的其余部分托多服务器没有任何类型的授权。 这是我们开始将它放在一起。

首先，我们需要表明我们想要使用护照。 在您的服务器配置后将此权限︰

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
在编写 Api 时应始终数据链接到一个独特无法欺骗用户的令牌中。 在此服务器上存储 TODO 项，它将存储它们基于我们放在"所有者"字段中的标记 （通过 token.sub 调用） 中用户的订阅 ID。 这将确保只有该用户才可以访问他 TODOs 并没有其他人可以访问输入 TODOs。 没有暴露在 API 中的"所有者"使外部用户可以请求其他人 TODOs 即使它们进行身份验证。

接下来，我们将使用带有护照 azure 广告载体打开连接 ID 策略。 现在只要看一下代码，我将稍后解释。 将放置这之后您 pated 上面︰

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

护照是所有战略编写者都遵循的战略 （Twitter、 Facebook 等） 全部使用类似的模式。 您看一下战略看到我们将它传递具有一个标记和作为参数，完成 function()。 战略将尽职回来给我们一旦那样所有的工作。 一旦它我们需要存储用户和隐藏标记，因此我们无需再问它。

> [AZURE.IMPORTANT]
上面的代码将碰巧到我们的服务器进行身份验证的任何用户。 这称为自动登记。 在生产服务器中不希望让任何人而无需事先经过注册过程决定它们。 这通常是在使用者应用程序都允许您使用 Facebook 注册但然后要求您填写其他信息中看到的图案。 如果这不是一个命令行程序，我们可以只提取电子邮件的标记的对象的返回，然后要求他们填写的其他信息。 因为这是我们只需将它们添加到内存中数据库的测试服务器。

### <a name="2-finally-protect-some-endpoints"></a>2.最后，保护某些终结点

您可以通过 passport.authenticate() 调用指定您想要使用的协议保护的终结点。

让我们来编辑服务器代码要做一些更有趣的事情中我们路由︰

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18︰ 您服务器再次运行应用程序，并确保它将拒绝您

我们将使用`curl`再次来看我们现在对我们终结点有过 OAuth2 保护。 我们将这样做之前在运行中任何该终结点对我们客户端 Sdk。 返回的标头应足以告诉我们我们是正确的道路。

首先，请确保 monogoDB 实例正在运行。

    $sudo mongod

然后，更改到目录开始出现卷曲.

    $ cd azuread
    $ node server.js

请尝试基本开机自检︰

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 是您正在寻找在这里，响应指示护照层正试图重定向到授权的终结点，这恰好是您所需。


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>祝贺您 ！ 必须使用 OAuth2 REST API 服务 ！

过得既不使用 OAuth2 兼容的客户端可以使用此服务器。 您将需要经历更多的演练。

如果您只要寻找如何实现 REST API，使用 Restify 和 OAuth2 的信息，则必须多足够保持开发服务并学习如何构建此示例的代码。

## <a name="next-steps"></a>下一步行动

为了便于参考，[作为.zip 此处提供](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip)（无需您配置的值） 的完整的示例，或者您可以克隆它从 GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

您现在可以移动到更高级的主题。  您可能想要尝试︰

[安全使用 v2.0 端点 Node.js web 应用程序 >>](active-directory-v2-devquickstarts-node-web.md)

更多的资源，请查阅︰
- [2.0 版的开发人员指南 》 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"azure 活动目录"标签 >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>我们的产品以获得安全更新

我们鼓励您能够访问[此页](https://technet.microsoft.com/security/dd252948)并订阅安全通报警告出现安全事件时的通知。
