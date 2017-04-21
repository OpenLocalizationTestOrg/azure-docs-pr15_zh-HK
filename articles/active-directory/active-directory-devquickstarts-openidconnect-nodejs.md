<properties
    pageTitle="开始使用 Azure 广告登录和注销使用 node.js"
    description="如何构建 node.js 表达 MVC Web 应用程序集成了 Azure 广告中的符号。"
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
    ms.date="08/15/2016"
    ms.author="brandwe"/>

# <a name="nodejs-web-app-sign-in--sign-out-with-azure-ad"></a>NodeJS 的 Web 应用程序的登录和注册 Azure 的广告，


这里我们将使用到的护照︰

- 用户登录应用程序使用 Azure 的广告。
- 显示有关该用户的某些信息。
- 对用户在该应用程序进行签名。

**护照**是 Node.js 对身份验证中间件。 非常灵活，模块化的护照可以后台删除为任何基于表达式的或 Resitify 的 web 应用程序。 一套全面的策略支持身份验证使用用户名和密码、 Facebook、 Twitter，等等。 对于 Microsoft Azure Active Directory，我们开发了一种战略。 我们将安装该模块，然后添加 Microsoft Azure Active Directory`passport-azure-ad`插件。

为此，您需要︰

1. 注册应用程序。
2. 将您的应用程序设置为使用 Passport azure 广告战略。
3. 使用护照到 Azure AD 颁发登录和注销请求。
4. 打印出与用户有关的数据。

本教程中的代码[在 GitHub 上](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS)维护。  要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip)或克隆主干︰

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

同时本教程末尾提供了完整的应用程序。

## <a name="1-register-an-app"></a>1.注册应用程序
- 登录到 Azure 的管理门户。
- 在左侧导航中，单击**活动目录**。
- 选择租户想要注册的应用程序。
- 单击**应用程序**选项卡，然后单击添加在底部抽屉里。
- 按照提示进行操作并创建新**的 Web 应用程序和/或 WebAPI**。
    - 应用程序**名称**将介绍您给最终用户的应用程序
    -   **登录 URL**为您的应用程序的基 URL。  框架的默认值是 http://localhost:3000/身份验证/openid/返回。
    - **应用程序 ID URI**是您的应用程序的唯一标识符。  约定是使用`https://<tenant-domain>/<app-name>`，例如`https://contoso.onmicrosoft.com/my-first-aad-app`
- 完成注册后，AAD 会将您的应用程序分配一个唯一的客户机标识符。  您将需要此值在下一节中，所以将其复制从配置选项卡。

## <a name="2-add-pre-requisities-to-your-directory"></a>2.前 requisities 添加到目录

从命令行更改目录到根文件夹如果不是已经存在并运行下面的命令︰

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`

- 此外，还需要我们`passport-azure-ad`以及︰

- `npm install passport-azure-ad`

这将安装库依赖于该护照 azure 广告。

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3.设置您的应用程序以使用 passport-节点的 js 战略
在这里，我们将配置速成中间件使用 OpenID 连接的身份验证协议。  护照将用于颁发登录和注销请求，管理用户的会话并获得用户有关的信息，在其他事情。

-   若要开始，请打开`config.js`项目的根目录中的文件，然后输入您的应用程序中的配置值`exports.creds`部分。
    -   `clientID:`是分配给您的应用程序中注册门户的**应用程序 Id** 。
    -   `returnURL`是在门户中输入的**重定向 Uri** 。
    - `clientSecret`在门户中生成的秘密

- 下一次打开`app.js`选的根目录中的文件，并添加以下调用，调用`OIDCStrategy`附带的战略`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// add a logger

var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- 之后，请使用我们刚才引用来处理我们登录请求的策略

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2) 
// 
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    if (!profile.email) {
      return done(new Error("No email found"), null);
    }
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
护照是所有战略编写者都遵循的战略 （Twitter、 Facebook 等） 全部使用类似的模式。 您看一下战略看到我们将它传递具有一个标记和作为参数，完成 function()。 战略将尽职回来给我们一旦那样所有的工作。 一旦它我们需要存储用户和隐藏标记，因此我们无需再问它。


> [AZURE.IMPORTANT] 
上面的代码将碰巧到我们的服务器进行身份验证的任何用户。 这称为自动登记。 在生产服务器中不希望让任何人而无需事先经过注册过程决定它们。 这通常是在使用者应用程序都允许您使用 Facebook 注册但然后要求您填写其他信息中看到的图案。 如果这不是一个示例应用程序，我们可以只提取电子邮件的标记的对象的返回，然后要求他们填写的其他信息。 因为这是我们只需将它们添加到内存中数据库的测试服务器。

- 接下来，让我们添加将使我们能够跟踪的已登录用户所需的护照中的方法。 这包括序列化和反序列化的用户信息︰

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
   log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};
```

- 接下来，让我们添加代码来加载明示的引擎。 这里您可以看到我们使用默认的 /views 和 /routes 模式的表达提供了。

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- 最后，让我们添加的路由将移交到实际登录请求的`passport-azure-ad`引擎︰

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return
app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4.使用护照到 Azure AD 颁发登录和注销请求

您的应用程序现在正确配置与使用 OpenID 连接的身份验证协议的 2.0 版终结点进行通信。  `passport-azure-ad`进行处理的所有丑陋精心编制的消息身份验证，验证令牌从 Azure 的广告，和维护用户会话的详细信息。  剩下的是可以为用户提供一种方法来登录、 注销，并收集关于登录的用户的其他信息。

- 首先，我们来添加到默认、 登录、 帐户和注销方法我们`app.js`文件︰

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   让我们回顾一下这些详细信息︰
    -   `/`路由将重定向到请求传入用户 （如果有的话） 的 index.ejs 视图
    - `/account`路由将***确保我们都经过身份验证***的第一个 (我们实现的下方)，然后将用户请求中传递，以便我们能够获取有关用户的其他信息。
    - `/login`路由将调用从我们 azuread openidconnect 身份验证器`passport-azuread`和，如果不能成功将用户重定向回 /login
    - `/logout`将只需调用 logout.ejs （并路由） 的清除 cookie，然后返回到 index.ejs 的用户


- 最后一部分为`app.js`，让我们添加的 EnsureAuthenticated 方法中使用的`/account`以上。

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}
```

- 最后，让我们来实际创建服务器自身中`app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5.在速成版，显示我们的用户在网站中创建视图和工艺路线

我们有我们`app.js`完成。 现在我们只需要添加路由和视图将显示用户以及句柄，我们得到的信息， `/logout` ，`/login`我们创建的工艺路线。

- 创建`/routes/index.js`在根目录下的工艺路线。

```JavaScript
/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- 创建`/routes/user.js`在根目录下的路由

```JavaScript
/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

这些简单的路由将只是将请求传递到我们的视图，包括用户，如果存在。

- 创建`/views/index.ejs`在根目录下的视图。 这是一个简单的页将调用我们的登录和注销方法，并使我们能够获取帐户信息。 请注意，我们可以使用条件`if (!user)`作为用户请求中传递是我们在用户中有记录的证据。

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- 创建`/views/account.ejs`查看在根目录下，以便我们可以查看其他信息的`passport-azuread`还将用户请求中。

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- 最后，让我们使它看起来相当通过添加布局。 创建 / 根目录下的 views/layout.ejs 的视图

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/account">Account</a> | 
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

最后，生成并运行您的应用程序 ！ 

运行`node app.js`，然后定位到`http://localhost:3000`


使用 Microsoft 个人帐户或一个工作或学校的帐户，登录，并注意如何在 /account 列表中反映用户的身份。  现在，您可以使用行业标准协议，可以对其个人和工作/学校这两个帐户的用户进行身份验证保护 web 应用程序。

为了便于参考，[作为.zip 此处提供](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip)（无需您配置的值） 的完整的示例，或者您可以克隆它从 GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```


您现在可以移动到更高级的主题。  您可能想要尝试︰

[安全的 Web API 加装了 Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
