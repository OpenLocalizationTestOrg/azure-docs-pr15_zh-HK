###<a name="server-auth"></a>如何︰ 使用提供商 （服务器流） 进行身份验证

要管理您的应用程序在身份验证过程的移动应用程序，您必须注册您的应用程序标识提供商。 然后在 Azure 应用程序服务，您需要配置的应用程序 ID 和您的供应商所提供的机密。
有关详细信息，请参阅本教程 [添加到您的应用程序身份验证]。

一旦您注册您的身份标识提供程序，只需调用.login() 方法与您的提供程序的名称。 例如，登录 Facebook 使用下面的代码。

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

如果您正在使用 Facebook 不标识提供程序，更改传递给 login 方法上面到下面的一个值︰ `microsoftaccount`， `facebook`， `twitter`， `google`，或`aad`。

在这种情况下，Azure 应用程序服务管理 OAuth 2.0 身份验证流通过显示所选提供程序的登录页，然后与标识提供程序的成功登录后生成的应用程序服务的身份验证令牌。 登录功能，完成后，返回 JSON 对象 （用户） 提供的用户 ID 和应用程序服务的身份验证令牌中用户 Id 和 authenticationToken 字段中，分别。 此标记可以缓存和重新使用，直到它过期。

###<a name="client-auth"></a>如何︰ 使用一个提供程序 （客户端流） 进行身份验证

您的应用程序可以还分别与标识提供商联系，然后提供返回的标记到您的应用程序服务进行身份验证。 这个客户端流量，可以为用户提供单一登录体验或从标识提供程序检索其他用户数据。

#### <a name="social-authentication-basic-example"></a>基本的社会身份验证示例

本示例使用 Facebook 客户端 SDK 进行身份验证︰

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
本示例假定由各自提供 SDK 提供的标记标记变量中存储。

#### <a name="microsoft-account-example"></a>Microsoft 客户示例

下面的示例使用单点登录的 Windows 应用商店应用程序通过使用 Microsoft 客户支持实时 SDK:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

本示例获取从实时连接，后者通过调用登录函数提供对您的应用程序服务的令牌。

###<a name="auth-getinfo"></a>如何︰ 获取有关身份验证的用户的信息

当前用户的身份验证信息，可从`/.auth/me`使用 AJAX 方式的终结点。  请确保您设置`X-ZUMO-AUTH`身份验证令牌的标头。  身份验证令牌存储在`client.currentUser.mobileServiceAuthenticationToken`。  例如，若要使用 fetch API:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

提取是以 npm 包还是从 CDNJS 的浏览器下载。 您还可以使用 jQuery 或另一个 AJAX API 来获取信息。  作为一个 JSON 对象，将接收的数据。
