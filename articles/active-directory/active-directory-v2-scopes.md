<properties
    pageTitle="Azure AD v2.0 范围、 权限和同意的情况下 |Microsoft Azure"
    description="在 Azure AD v2.0 终结点，包括范围、 权限和许可授权的说明。"
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>范围、 权限和 v2.0 终结点中的同意

将与 Azure AD 集成的应用程序按照特定的授权模型，使用户能够控制应用程序如何访问它们的数据。  此授权模型的 2.0 版实现已经更新，更改应用程序必须使用 Azure AD 的交互方式。  本主题涵盖包括范围、 权限和同意此授权模型的基本概念。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

## <a name="scopes--permissions"></a>范围和权限

Azure 广告实现[OAuth 2.0](active-directory-v2-protocols.md)授权协议，这是一种允许第三方应用程序代表用户访问 web 承载资源的方法。  资源标识符或**应用程序 ID URI**必须与 Azure AD 集成的任何 web 承载的资源。  例如，一些 Microsoft 的 web 托管资源包括︰

- Office 365 统一邮件 API:`https://outlook.office.com`
- Azure 广告图形 API:`https://graph.windows.net`
- Microsoft Graph 中︰`https://graph.microsoft.com`

同样适用于使用 Azure AD 集成了任何第三方资源。  这些资源还可以定义一组可用于划分成更小块的功能的资源的权限。  例如，Microsoft Graph 定义了一些权限︰

- 读取用户的日历
- 写入用户的日历
- 发送邮件的用户
- [+ 更多](https://graph.microsoft.io)

通过定义这些权限，则资源可以有细粒度地控制其数据和如何向外界公开。  一个第三方应用程序然后可以从最终用户的请求这些权限，最终用户必须批准权限之前应用程序可对代表他们。  通过分块资源的功能集成到更小的权限集，可以生成第三方应用程序请求只为执行其职责所需的特定权限。  它还使最终用户能够知道完全的应用程序将如何使用他们的数据，以使它们更加确信，该应用程序行为不心怀恶意企图。

在 Azure 广告和 OAuth，这些权限被称为**作用域**。  您可能会看到它们被称为**oAuth2Permissions**。  作用域 Azure AD 中表示为一个字符串值。  在 Microsoft Graph 的示例，每个权限的范围值是︰

- 读取用户的日历︰`Calendar.Read`
- 写入用户的日历︰`Mail.ReadWrite`
- 作为用户发送邮件︰`Mail.Send`

应用程序可以通过指定作用域到 2.0 版的端点，请求在请求这些权限，如下所述。

## <a name="openid-connect-scopes"></a>OpenId 连接的作用域

OpenID 连接 v2.0 实施了几个明确定义的作用域，不应用于任何特定的资源- `openid`， `email`， `profile`，和`offline_access`。

#### <a name="openid"></a>OpenId

如果应用程序执行登录使用[OpenID 连接](active-directory-v2-protocols.md#openid-connect-sign-in-flow)，则必须请求`openid`范围。  `openid`范围将显示在工作帐户同意屏幕为"登录"的权限，并在个人 Microsoft 客户同意的情况下屏幕与"查看您的配置文件和连接到应用程序和服务使用 Microsoft 帐户"权限。  此权限可以使应用程序的窗体中接收用户的唯一标识符`sub`说。  它还提供了对用户信息终结点的应用程序访问。  `openid`范围也可 v2.0 令牌端点处获得 id_tokens，可用于保护应用程序的不同组件之间的 HTTP 调用。

#### <a name="email"></a>电子邮件

`email`作用域可以包含沿`openid`范围和任何其他人。  它可以提供对用户的主要电子邮件地址的形式的应用程序访问`email`说。  `email`索赔将只包含在标记的电子邮件地址是否与用户帐户，它并不总是这样。  如果使用的`email`的范围内，您的应用程序应准备好处理这种情况在其`email`令牌中不存在声明。

#### <a name="profile"></a>配置文件

`profile`作用域可以包含沿`openid`范围和任何其他人。  它提供了大量有关用户的信息的应用程序访问。  这包括但不限于用户的名字、 姓氏、 首选的用户名、 对象 ID 等。  有关给定用户的配置文件声明 id_tokens 中提供的完整列表，请参阅[v2.0 令牌引用](active-directory-v2-tokens.md)。

#### <a name="offlineaccess"></a>Offline_access

[`offline_access`范围](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess)允许您访问资源的应用程序代表用户时间较长。  在工作帐户的同意屏幕中，该范围将显示与"随时访问您的数据"权限。  在个人 Microsoft 客户同意的情况下屏幕，它将显示为"随时访问您的信息"权限。  当用户批准`offline_access`的范围内，您的应用程序将能够从 v2.0 令牌终结点接收刷新令牌。  刷新令牌将长期存在和允许您的应用程序，以获取新的访问令牌，较旧的或使其过期。

如果您的应用程序不会请求`offline_access`的范围内，它将不会收到 refresh_tokens。  这意味着，当您兑换[OAuth 2.0 授权代码流](active-directory-v2-protocols.md#oauth2-authorization-code-flow)中的授权，您将只能收到回从 access_token`/token`终结点。  该 access_token 的时间 （通常是一个小时），短时间内将保持有效，但最终会到期。  此时，时间点，您的应用程序需要将用户重定向回`/authorize`检索新授权的终结点。  在此重定向用户可能会或可能不需要重新输入其凭据或重新同意访问权限，具体情况取决于应用程序的类型。

有关如何获取和使用刷新令牌， [2.0 版协议参考](active-directory-v2-protocols.md)。


## <a name="requesting-individual-user-consent"></a>单个用户同意的情况下请求

在[OpenID 连接或 OAuth 2.0](active-directory-v2-protocols.md)的授权请求，应用程序可以请求所需使用的权限`scope`查询参数。  例如，当用户应用程序进行签名，该应用程序将发送的请求如下所示 （使用的换行以提高可读性）︰

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

`scope`参数是以空格分隔列表的应用程序正在请求的范围。  每个单个作用域将范围值附加到资源的标识符 (应用程序 ID URI) 来表示。  上述请求指示该应用程序需要读取用户的日历并发送邮件的用户的权限。

用户输入其凭据后，将检查 v2.0 终结点匹配记录的**用户同意**。  如果用户已不同意所请求的任何的权限在过去，2.0 版终结点将询问用户授予所请求的权限。  

![工作帐户同意屏幕抓图](../media/active-directory-v2-flows/work_account_consent.png)

如果用户审批权限，同意将记录，以便用户不必重新同意在后续的登录。

## <a name="requesting-consent-for-an-entire-tenant"></a>整个组织的请求同意

通常，当组织购买许可证或订阅的应用程序，他们希望完全为其员工提供它。  作为此过程的一部分，公司管理员可以授予该应用程序，以便代表所有员工的同意。  通过授予整个组织的同意，该组织的员工将不会遇到该应用程序的许可屏幕。

要申请一个租户中所有用户同意的情况下，您的应用程序可以使用**管理员同意终结点**，如下所述。

## <a name="admin-restricted-scopes"></a>管理员限制范围

某些 Microsoft 生态系统中的高特权权限可以标记为**管理员限制**。  这种作用域的示例包括︰

- 阅读 organizaion 的目录数据︰`Directory.Read`
- 将数据写入到一个组织的目录︰`Directory.ReadWrite`
- 组织目录中读取安全组︰`Groups.Read.All`

而消费者用户可能授予应用程序对此类数据的访问，被限制组织用户授予访问权限的相同的一套敏感的公司数据。  如果您的应用程序请求访问下列权限之一从组织的用户，则用户将收到错误消息，指明它们未经授权，以表示同意在您的应用程序的权限。

如果您的应用程序需要访问组织这些管理员限制作用域，您应请求它们直接从公司管理员还使用**管理员同意终结点**，如下所述。

当管理员授予这些权限通过管理员同意的情况下终结点时，将将租户中的所有用户授予许可，如上面所述。

## <a name="using-the-admin-consent-endpoint"></a>使用管理员同意的情况下终结点

通过执行以下步骤，您的应用程序将能够收集在一个给定的租户，包括管理员限制作用域中的所有用户权限。  要查看实现步骤 desribed 下面的代码示例，请参考[管理限制的范围的示例](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2)。

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>请求的应用程序注册门户的权限

- 如果还没有的话，请导航到您应用程序[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)，或[创建一个应用程序](active-directory-v2-app-registration.md)中。
- 找到**Microsoft Graph 权限**部分，并添加您的应用程序要求的权限。
- 确保**保存**应用程序注册

#### <a name="recommended-sign-the-user-into-your-app"></a>建议︰ 用户登录您的应用程序

通常时生成的应用程序使用该管理员同意终结点，应用程序将需要有一个页/视图，允许管理员批准的应用程序的权限。  该网页可以是应用程序的注册流程，应用程序的设置的一部分或专用的"连接"流的一部分。  在许多情况下，其意义的应用程序将此页显示"连接"视图仅在用户已登录的工作或学校 Microsoft 帐户后。

用户登录该应用程序使您可以确定该管理员然后再要求他们批准所需的权限对所属组织。  虽然并非绝对必需的它可以帮助您创建组织用户更直观的体验。  签名中的用户，请按照我们[2.0 版协议教程](active-directory-v2-protocols.md)。

#### <a name="request-the-permissions-from-a-directory-admin"></a>从目录管理员请求权限

当您准备请求权限从该公司的管理时，可以将用户重定向到 v2.0**同意终结点的管理**。

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| 参数 | | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 必填 | 要请求的权限目录租户。  可以提供 guid 或好记的名称格式。 |
| client_id | 必填 | 应用程序 Id 注册门户网站 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList))，分配给您的应用程序。 |
| redirect_uri | 必填 | 要为您的应用程序处理的发送的响应的 redirect_uri。  它必须完全匹配您在门户中注册 redirect_uris 之一。 |
| 状态 | 建议 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  使用的状态进行编码之前发生身份验证请求，例如在页面或视图上的应用程序中用户的状态信息。 |

在这种情况下，只有一名租户管理员可以登录以完成该请求 Azure 广告将强制执行。  管理员可能需要批准所有您需要获得为您的应用程序中注册门户的权限。 

##### <a name="successful-response"></a>成功的响应
如果管理员批准的应用程序的权限，将为成功的响应︰

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 租户 | 它请求中的 guid 格式的权限授予您的应用程序目录租户。 |
| 状态 | 此外将令牌响应中返回的请求中包含一个值。  它可以是您希望的任何内容的字符串。  使用的状态进行编码之前发生身份验证请求，例如在页面或视图上的应用程序中用户的状态信息。 |
| admin_consent | 将设置为`True`。 |



##### <a name="error-response"></a>错误响应
如果管理员不赞成您应用程序的权限，失败的响应将是︰

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| 参数 | 说明 |
| ----------------------- | ------------------------------- | --------------- |
| 错误 | 错误代码字符串，可用于划分类型的错误的发生，并可用于对错误作出反应。 |
| error_description | 特定错误消息可帮助开发人员识别错误的根本原因。  |

一旦您已收到到成功的响应来自管理员同意的情况下终结点，您的应用程序获得了它所请求的权限。  您现在可以移动到请求的所需资源的标记，如下所述。

## <a name="using-permissions"></a>使用权限

为您的应用程序权限的用户同意后，您的应用程序可以获取表示您的应用程序中某些容量的资源的访问权限的访问令牌。  一个给定的访问令牌只能用于单个 resorce，但其内部编码将是您的应用程序已被授予对该资源的所有权限。  获取一个访问令牌，您的应用程序可以到 v2.0 令牌终结点发出一个请求︰

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

然后可以到资源的 HTTP 请求中使用生成的访问令牌-它将可靠地指示资源，您的应用程序具有适当的权限来执行给定的任务。  

OAuth 2.0 协议以及如何获取访问令牌的详细信息，请参阅[v2.0 终结点协议引用](active-directory-v2-protocols.md)。
