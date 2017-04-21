<properties
    pageTitle="Azure AD v2.0 终结点将变为 |Microsoft Azure"
    description="对应用程序模型 v2.0 公共预览协议进行的更改的说明。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="important-updates-to-the-v20-authentication-protocols"></a>V2.0 身份验证协议的重要更新
注意开发人员 ！ 在下一次两个星期中，我们将做一些更新为重大更改为我们预览期间编写的任何应用程序的行为可能表示我们 v2.0 身份验证协议。  

## <a name="who-does-this-affect"></a>这将影响谁？
任何应用程序编写为使用 v2.0 集中身份验证终结点，

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

V2.0 终结点的详细信息可以找到[这里](active-directory-appmodel-v2-overview.md)。

如果生成通过编码直接到 v2.0 协议使用 v2.0 终结点的应用程序，使用任何我们 OpenID 连接或 OAuth 的网站 middlewares，或使用其他第三方函数库来执行身份验证，您应准备好测试您的项目并根据需要进行更改。

## <a name="who-doesnt-this-affect"></a>这并不影响谁？
任何应用程序已针对生产 Azure AD 身份验证终结点，

```
https://login.microsoftonline.com/common/oauth2/authorize
```

此协议在石头中设置并没有遇到任何更改。

此外，如果您的应用程序**仅**使用 ADAL 库来执行身份验证，则不必更改任何内容。  ADAL 具有屏蔽您的应用程序中所做的更改。  

## <a name="what-are-the-changes"></a>所做的更改是什么？
### <a name="removing-the-x5t-value-from-jwt-headers"></a>JWT 标题中删除 x5t 值
V2.0 终结点使用 JWT 令牌广泛，其中包含与标记有关的相关元数据标头参数部分。  如果解码我们当前的 JWTs 之一的标头，则将发现如下类似︰

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

其中的"x5t"和"孩子"属性标识应该使用 OpenID 连接的元数据终结点从检索验证令牌签名的公钥。

我们这里所做的更改将被删除"x5t"属性。  您可能会继续使用相同的机制来验证令牌，但应该只依赖要检索正确的公用密钥，OpenID 连接协议中指定的"孩子"属性。 

> [AZURE.IMPORTANT] **您的作业︰ 请确保您的应用程序不依赖于 x5t 值是否存在。**

### <a name="removing-profileinfo"></a>删除 profile_info
以前，2.0 版终结点具有已返回 base64 编码 JSON 对象标记的响应调用中`profile_info`。  当通过发送一个请求到从 v2.0 终结点请求访问令牌︰

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

响应将类似下面的 JSON 对象︰
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`profile_info`包含值的用户登录到应用程序 — 他们显示名称、 名字、 姓氏、 电子邮件地址、 标识符和等等的信息。  首先，`profile_info`用于令牌缓存并显示目的。

我们现在正在删除`profile_info`值 – 但不要担心，我们仍然为开发人员在稍有不同的地方提供此信息。  而不是`profile_info`，2.0 版终结点将立即返回`id_token`中每个标记的响应︰

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

您可能会解码和分析 id_token 检索您从 profile_info 接收相同的信息。  Id_token 是 JSON Web 标记 (JWT)，与所指定的 OpenID 连接的内容。  因此这样的代码应该是非常类似 — 只需提取的中间段 （正文） 的 id_token 和 base64 解码访问 JSON 对象中。

在下一次两个星期中，应将您的应用程序检索用户信息从`id_token`或`profile_info`;无论是存在的。  这样，所做的更改时，您的应用程序可以无缝地处理转换的`profile_info`到`id_token`而不会中断。

> [AZURE.IMPORTANT] **您的作业︰ 请确保您的应用程序不依赖于是否存在`profile_info`值。**

### <a name="removing-idtokenexpiresin"></a>删除 id_token_expires_in
类似于`profile_info`，我们还正在删除`id_token_expires_in`来自响应参数。  V2.0 终结点以前，将返回的值`id_token_expires_in`以及 id_token 的每个响应，例如在授权响应︰

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

或标记的响应︰

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`id_token_expires_in`值将指示 id_token 仍为有效的秒数。  现在，我们正在删除`id_token_expires_in`值完全。  相反，您可能会使用 OpenID 连接标准`nbf`和`exp`声称检查 id_token 的有效性。  在这些索赔中看到[v2.0 令牌引用](active-directory-v2-tokens.md)的详细信息。

> [AZURE.IMPORTANT] **您的作业︰ 请确保您的应用程序不依赖于是否存在`id_token_expires_in`值。**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>更改范围由返回的声明 = openid
此更改将是最重要的--事实上，它会影响几乎所有使用 v2.0 终结点的应用程序。  许多应用程序将请求发送到 v2.0 终结点使用`openid`范围，如︰

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

今天，用户授予同意时`openid`范围内，您的应用程序中得到 id_token 收到了大量关于用户的信息。  这些声明可以包括他们的姓名、 首选的用户名、 电子邮件地址、 对象 ID 和更多。

在此更新时，我们也变化信息的`openid`范围提供了更好使用 OpenID 连接规范 comform，您的应用程序访问。  `openid`范围将仅允许您的应用程序进行签名，用户和接收用户的应用程序特定的标识符`sub`id_token 的声明。  在仅用 id_token 索赔`openid`授予的范围将会没有任何个人识别信息。  示例 id_token 声明如下︰

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

如果您想要获取关于您的应用程序中的用户个人身份信息 (PII)，您的应用程序将需要从用户请求额外的权限。  我们要引入两个新的作用域支持 OpenID 连接规范 – 从`email`和`profile`范围 – 允许您这样做。

- `email`范围是非常简单 — 它允许应用程序访问该用户的主电子邮件地址通过`email`id_token 中声称。  请注意，`email`声明不会总是出现在 id_tokens – 它才包括在用户的配置文件中可用。
- `profile`范围提供了所有其他基本用户信息 – 其名称、 首选的用户名，您的应用程序访问对象 ID，等等。

这使您可以以最少披露的方式您的应用程序的代码，您可以要求用户为只是您的应用程序执行其任务所需的信息集。  如果您想要继续获得您的应用程序当前正在接收的用户信息的完整集，应该授权请求中包含所有三个范围︰

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

您的应用程序可以开始发送`email`和`profile`范围立即和 v2.0 终结点将接受这些两个范围并开始根据需要从用户请求权限。  但是，在解释的更改`openid`几周作用域都将不会生效。

> [AZURE.IMPORTANT] **您的作业︰ 添加`profile`和`email`范围如果您的应用程序要求用户有关的信息。**  请注意，ADAL 将包含这两种权限请求中，默认情况。 

### <a name="removing-the-issuer-trailing-slash"></a>正在删除尾随斜杠的颁发者。
以前，从 v2.0 终结点出现在令牌颁发者值需要窗体

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

其中 guid 已颁发令牌的 Azure AD 租户的 tenantId。  进行这些更改，颁发者值将变为

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

在这两个标记，OpenID 连接发现文档中。

> [AZURE.IMPORTANT] **您的任务︰ 确保您的应用程序颁发者在验证期间接受颁发者值同时出现带和不带一个尾随的斜杠。**

## <a name="why-change"></a>为什么更改？
引入这些变化的主要动机是符合连接 OpenID 标准规范。  OpenID 连接兼容，我们希望以最小化集成与 Microsoft 身份服务，并与业内其他标识服务之间的差异。  我们想要使开发人员可以使用他们最喜欢的开源的验证库而无需修改库以容纳 Microsoft 的差异。

## <a name="what-can-you-do"></a>您能做什么？
从现在起，可以开始进行所有上面所述的更改。  您应立即︰

1.  **在删除任何依赖项`x5t`标头参数。**
2.  **妥善处理转换的`profile_info`到`id_token`标记的响应中。**
3.  **在删除任何依赖项`id_token_expires_in`响应参数。**
3.  **添加`profile`和`email`如果您的应用程序需要基本用户信息到您的应用程序范围。**
4.  **接受中同时出现带和不带一个尾随的斜杠标记的颁发者值。**

我们的[2.0 版协议文档](active-directory-v2-protocols.md)已经已更新，以反映这些更改，因此您可以将其用作帮助更新您的代码中引用。

如果您有任何进一步的问题上所做的更改的范围，请随时给我们亲密在 Twitter 上@AzureAD。

## <a name="how-often-will-protocol-changes-occur"></a>协议的更改将出现频率？
我们没有预见任何重大更改为身份验证协议。  我们有意捆绑到一个发布这些更改，这样您就不必再次短时间仔细检查这种类型的更新过程。  当然，我们将继续将功能添加到聚合的 v2.0 身份验证服务，您可以利用的但这些更改应累加而不中断现有代码。

最后，我们想说感谢您在预览期间试验情况。  见解和经验我们早期采用者的估量到目前为止，并且我们希望将继续共享您的观点和想法。

祝您工作愉快 ！

Microsoft 身份部
