<properties
    pageTitle="Azure 的 Active Directory B2C︰ 使用 Graph API |Microsoft Azure"
    description="B2C 租户使用应用程序标识来自动执行过程调用图形 API 的方式。"
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C︰ 使用 Graph API

Azure 的活动目录 (AD Azure) B2C 承租人往往会变得非常大。 这意味着，需要以编程方式执行许多常见的租户管理任务。 主要的例子就是用户管理。 您可能需要将现有的用户存储迁移到 B2C 租户。 您可以拥有您自己的网页上的用户注册，幕后的 Azure AD 中创建用户帐户。 这些类型的任务需要能够创建、 读取、 更新和删除用户帐户。 您可以通过使用 Azure 广告图形 API 来执行这些任务。

对于 B2C 承租人有与图形 API 进行通信的两种主要模式。

- 对于交互式、 运行一次任务，您应该作为管理员帐户在 B2C 租户执行任务时。 这种模式要求管理员之前，管理员可以执行任何调用图形 API 使用的凭据登录。
- 自动化、 连续的任务，应使用某种类型的服务帐户提供必要的权限来执行管理任务。 在 Azure 的广告，你可以通过注册应用程序并到 Azure AD 身份验证。 这是通过使用[OAuth 2.0 客户端凭据授予](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)**应用程序 ID** 。 在这种情况下，应用程序充当本身，不是作为一个用户，以调用图形 API。

在本文中，我们将讨论如何执行自动化的用例。 为了演示，我们将构建.NET 4.5`B2CGraphClient`用户执行创建、 读取、 更新和删除 (CRUD) 操作。 客户端将有一个 Windows 命令行界面 (CLI) 允许您调用的各种方法。 但是，编写的代码要以非交互式的、 自动化的方式工作。

## <a name="get-an-azure-ad-b2c-tenant"></a>获取 Azure AD B2C 租户

可以创建应用程序或用户，或在所有交互 Azure 的广告之前，您需要 Azure AD B2C 租户和租户中的全局管理员帐户。 如果您没有租户已经，[开始使用 Azure AD B2C](active-directory-b2c-get-started.md)。

## <a name="register-a-service-application-in-your-tenant"></a>在您的组织注册一个服务应用程序

B2C 租户之后，您需要通过使用 Azure AD PowerShell cmdlet 创建服务应用程序。
首先，下载并安装[Microsoft 联机服务登录的助手](http://go.microsoft.com/fwlink/?LinkID=286152)。 然后下载并安装[64 位 Windows PowerShell Azure Active Directory 模块](http://go.microsoft.com/fwlink/p/?linkid=236297)。

> [AZURE.IMPORTANT]
B2C 租户使用图形 API，您需要使用 PowerShell 注册专用应用程序。 请按照本文中的指令来做到这一点。 不能重用现有的 B2C 应用程序在 Azure 门户中注册。

PowerShell 模块安装后，打开 PowerShell 并连接到 B2C 租户。 在您运行`Get-Credential`，您将被提示用户名和密码，输入的用户名称和您 B2C 租户管理员帐户的密码。

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

在创建应用程序之前，您需要生成新的**客户端密码**。  您的应用程序将使用客户端密码到 Azure AD 身份验证并获取访问令牌。 您可以在 PowerShell 生成有效的密码︰

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

最后一个命令应打印新客户机密。 复制它安全的地方。 您将稍后需要它。 现在您可以创建您的应用程序通过应用程序作为凭据提供新的客户端密码︰

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

如果成功创建了该应用程序，它应打印出的应用程序，如上文所示的属性。 您将需要两个`ObjectId`和`AppPrincipalId`，所以复制这些值，太。

在 B2C 租户中创建应用程序之后，您需要为其分配执行用户 CRUD 操作所需的权限。 分配的应用程序的三个角色︰ 目录读取器 （用于读取用户）、 目录的编写器 （用于创建和更新用户），和用户帐户管理员 （要删除的用户）。 这些角色具有已知的标识符，因此可以替换`-RoleMemberObjectId`参数与`ObjectId`从上面，然后运行以下命令。 若要查看列表中的所有目录角色，尝试运行的`Get-MsolRole`。

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

您现在有一个应用程序有权创建、 读取、 更新和删除 B2C 租户的用户。

## <a name="download-configure-and-build-the-sample-code"></a>下载、 配置和生成的示例代码

首先，下载示例代码，并使其运行。 然后，我们将采取更仔细地看。  您可以[下载示例代码为.zip 文件](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip)。 您可以克隆它到您选择的目录︰

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

打开`B2CGraphClient\B2CGraphClient.sln`在 Visual Studio 中的 Visual Studio 解决方案。 在`B2CGraphClient`项目，请打开该文件`App.config`。 三个应用程序设置替换为您自己的值︰

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

接下来，右键单击`B2CGraphClient`解决方案并重新生成此示例。 如果您是成功的现在应该有`B2C.exe`可执行文件位于`B2CGraphClient\bin\Debug`。

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>通过使用图形 API 生成用户 CRUD 操作

若要使用 B2CGraphClient，打开`cmd`Windows 命令提示并更改到目录`Debug`目录。 然后运行`B2C Help`命令。

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

这将显示每个命令的简短说明。 每次您调用这些命令之一， `B2CGraphClient` Azure 广告图形 api 发出请求。

### <a name="get-an-access-token"></a>获取一个访问令牌

任何图形 api 请求需要一个访问令牌进行身份验证。 `B2CGraphClient`使用开放源代码活动目录身份验证库 (ADAL) 来帮助获取访问令牌。 ADAL 提供一个简单的 API 和负责处理的一些重要的细节，例如高速缓存的访问令牌通过使标记购置更容易。 您不需要使用 ADAL 标记，但是。 此外可以通过精心编制的 HTTP 请求来获取令牌。

> [AZURE.NOTE]
    此代码示例使用 ADAL v2 是为了与图形 API 进行通信。  为了获取访问令牌可以使用 Azure 广告图形 API 使用，必须使用 ADAL v2 或 v3。

当`B2CGraphClient`运行时，它会创建一个`B2CGraphClient`类。 此类的构造函数设置 ADAL 身份验证基架︰

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

我们将使用`B2C Get-User`命令为例。 当`B2C Get-User`调用不带任何附加的输入 CLI 调用`B2CGraphClient.GetAllUsers(...)`方法。 此方法调用`B2CGraphClient.SendGraphGetRequest(...)`，它提交给图形 API HTTP GET 请求。 前`B2CGraphClient.SendGraphGetRequest(...)`发送 GET 请求，它首先获取访问令牌通过 ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

您可以获取访问令牌的图形 API 通过调用 ADAL`AuthenticationContext.AcquireToken(...)`方法。 然后返回 ADAL `access_token` ，表示应用程序的标识。

### <a name="read-users"></a>读取用户

当您想要获取列表的用户或特定用户获得图形 API 时，您可以发送 HTTP`GET`请求的`/users`终结点。 在一个租户的用户的所有请求如下都所示︰

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

若要查看该请求，请运行︰

 ```
 > B2C Get-User
 ```

有两个需要注意的重要事项︰

- 通过 ADAL 获得的访问令牌添加到`Authorization`头通过`Bearer`方案。
- 对于 B2C 承租人必须使用查询参数`api-version=1.6`。

这些详细信息都以处理`B2CGraphClient.SendGraphGetRequest(...)`方法︰

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>创建客户的用户帐户

在 B2C 租户中创建用户帐户时，您可以发送 HTTP`POST`请求的`/users`终结点︰

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

在此请求中的这些属性中的大多数需要创建者的用户。 若要了解详细信息，请单击[此处](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser)。 请注意，`//`注释已包括有关说明。 不包含它们在实际的请求。

若要查看该请求，请运行下列命令之一︰

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

`Create-User`命令将.json 文件作为输入参数。 此文件中包含用户对象的 JSON 的表示。 在代码示例中有两个示例.json 文件︰ `usertemplate-email.json` ， `usertemplate-username.json`。 您可以修改这些文件以适合您的需要。 除了上面的必填字段，在这些文件中包含多个可选字段，您可以使用。 可选字段的详细信息可以在[Azure 广告图形 API 实体引用](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity)。

您可以看到如何将 POST 请求构造中`B2CGraphClient.SendGraphPostRequest(...)`。

- 它连接到一个访问令牌`Authorization`在请求标头。
- 它将设置`api-version=1.6`。
- 它包括 JSON 用户对象请求的正文中。

> [AZURE.NOTE]
如果您要从现有的用户存储迁移的帐户具有比[Azure AD B2C 强制执行强密码强度](https://msdn.microsoft.com/library/azure/jj943764.aspx)的密码强度较低，则可以禁用强密码要求使用`DisableStrongPassword`中的值`passwordPolicies`属性。 例如，您可以修改创建用户请求提供以上如下︰ `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`。

### <a name="update-consumer-user-accounts"></a>更新使用者用户帐户

更新用户对象时，该进程是类似于用于创建用户对象。 但这种过程使用 HTTP`PATCH`方法︰

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

尝试更新用户使用新数据更新 JSON 文件。 然后，您可以使用`B2CGraphClient`运行以下命令之一︰

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

检查`B2CGraphClient.SendGraphPatchRequest(...)`详细介绍了如何将此请求发送的方法。

### <a name="search-users"></a>搜索用户

您可以采用两种方法您 B2C 租户中搜索用户。 其中一个，使用该用户的对象 ID 或两个，使用用户的登录标识符 (即`signInNames`属性)。

运行下列命令来搜索特定用户之一︰

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

下面是几个例子︰

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>删除用户

删除用户的过程非常简单。 使用 HTTP`DELETE`方法，并构造使用正确的 URL 对象 ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

若要查看示例，请输入以下命令，查看打印到控制台删除请求︰

```
> B2C Delete-User <object-id-of-user>
```

检查`B2CGraphClient.SendGraphDeleteRequest(...)`详细介绍了如何将此请求发送的方法。

您可以执行与 Azure 广告图形 API 用户管理以及许多其他的动作。 [Azure 广告图形 API 参考](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)提供的每个操作的示例请求的详细信息。

## <a name="use-custom-attributes"></a>使用自定义属性

大多数使用者应用程序需要存储某种类型的自定义用户配置文件信息。 您可以执行此操作的一种方法是在 B2C 租户中定义的自定义特性。 然后可以将该属性相同的方式对待用户对象上的任何其他属性。 可以更新属性、 删除该属性、 属性查询、 发送作为登录令牌等中声明的属性。

B2C 租户中定义的自定义属性，请参阅[B2C 的自定义属性引用](active-directory-b2c-reference-custom-attr.md)。

您可以查看在 B2C 租户中使用定义的自定义特性`B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

这些函数的输出如显示每个自定义属性的详细信息︰

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

您可以使用完整的名称，如`extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`，为您的用户对象的属性。  使用新的属性和属性的值，更新您的.json 文件，然后运行︰

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

通过使用`B2CGraphClient`，有一个服务应用程序，可以以编程方式管理您的 B2C 租户用户。 `B2CGraphClient`使用其自己的应用程序标识来验证到 Azure 广告图形 API。 它还通过使用客户端密钥获取令牌。 您将该功能合并到您的应用程序，请记住对于 B2C 的应用的几个关键点︰

- 您需要在组织中适当的权限授予该应用程序。
- 现在，您需要使用 ADAL v2 获取访问令牌。 （您还可以发送协议消息直接，而无需使用库。）
- 当调用图形 API 时，使用`api-version=1.6`。
- 当创建和更新用户的使用者时，几个属性是必需的如上面所述。

如果您有任何疑问或想在 B2C 租户使用 Graph API 来执行的操作的请求，在这篇文章发表评论或在 GitHub 代码示例库中文件的问题。
