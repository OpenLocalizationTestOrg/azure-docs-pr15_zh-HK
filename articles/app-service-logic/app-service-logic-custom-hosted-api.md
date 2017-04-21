<properties
    pageTitle="逻辑应用程序中调用一个自定义的 API"
    description="使用您自定义的 API 位于与应用程序逻辑的应用程序服务"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>使用您自定义的 API 位于与应用程序逻辑的应用程序服务

尽管逻辑应用程序具有一套丰富的 40 多个连接器的各种服务，但也可以调入自己自定义的 API，可以运行您自己的代码。 承载您自己的自定义 web API 的最简单和最具可扩展性的方法之一是使用应用程序服务。 本文介绍了如何连接到任何 web 应用程序服务 API 的应用程序、 web 应用程序或移动应用程序中承载的 API。

作为触发器或逻辑的应用程序中的操作生成的 Api 的信息，请检查出[这篇文章](app-service-logic-create-api-app.md)。

## <a name="deploy-your-web-app"></a>部署 Web 应用程序

首先，您需要部署在应用程序服务 Web 应用程序作为您的 API。 此处的说明介绍基本的部署︰[创建 ASP.NET web 应用程序](../app-service-web/web-sites-dotnet-get-started.md)。  虽然可以从逻辑应用程序调入任何 API，我们建议您最好添加 Swagger 元数据，以便能够方便地集成与逻辑应用程序操作。  您可以在[添加 swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)中找到详细信息。

### <a name="api-settings"></a>API 设置

为了使逻辑应用程序设计器来分析您的 Swagger，值得您启用 CORS 和设置您的 web 应用程序的 APIDefinition 属性。  这是很容易在 Azure 门户内设置。  只需打开 Web 应用程序，设置刀片式服务器和部分下面的 API 集 API 定义到 URL 的 swagger.json 文件 （通常为 https://{name}.azurewebsites.net/swagger/docs/v1），以及添加 CORS 策略为 * 以允许从逻辑应用程序设计器的请求。

## <a name="calling-into-the-api"></a>API 调用

逻辑应用程序门户中时，如果您已经设置了 CORS 和 API 定义属性应该能够轻松地添加自定义 API 操作您的流中。  在设计器中您可以选择浏览您订阅的网站来定义一个 swagger url 列表的网站。  您还可以使用 HTTP + Swagger 点至 swagger，并列出可用的操作和输入操作。  最后，您始终可以使用 HTTP 操作调用任何 API，甚至包括那些没有或公开的 swagger 文档的请求。

如果您想要确保您的 API 的安全，有以下几种不同的方法来做到这一点︰

1. 可以不使用所需的任何代码更改 Azure Active Directory 来保护您的 API，而不需要任何代码更改或重新部署。
1. 在您的 API 的代码强制执行基本身份验证、 AAD 认证或证书身份验证。

## <a name="securing-calls-to-your-api-without-a-code-change"></a>保护对您无需代码更改的 API 的调用

在本节中，您将创建两个 Azure Active Directory 应用程序 – 一个用于您的逻辑的应用程序，另一个用于您的 Web 应用程序。  将对您的 Web 应用程序使用与您的应用程序逻辑的 AAD 应用程序关联的服务主体 （客户机 id 和密码） 进行身份验证。 最后，您将包括应用程序逻辑的应用程序定义中的 ID。

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>第 1 部分︰ 逻辑应用程序的应用程序标识的设置

这是逻辑应用程序用来对 active directory 进行身份验证。 您只*需要*做这一次为目录。 例如，您可以选择对所有您的逻辑的应用程序，使用相同的标识尽管如果您愿意，您还可以创建唯一的标识，每个逻辑应用程序。 您可以在 UI 中执行此操作，也可以使用 PowerShell。

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>创建使用 Azure 的传统门户的应用程序标识

1. 到[Active directory 中的 Azure 的传统门户](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory)导航并选择要用于您的 Web 应用程序的目录
2. 单击**应用程序**选项卡
3. 在页面底部的命令栏中单击**添加**
4. 命名您的身份信息，请单击下箭头
5. 放入一个唯一的字符串的格式设置为两个文本框中的域，然后单击复选标记
6. 单击该应用程序的**配置**选项卡
7. 复制**客户机 ID**，将逻辑应用程序中使用此
8. 在**键**部分中单击**选择持续时间**并选择 1 年或 2 年
9. 单击**保存**按钮底部的屏幕 （您可能需要等待几秒钟）
10. 现在一定要在框中复制的项。 这还将逻辑应用程序中使用

#### <a name="create-the-application-identity-using-powershell"></a>创建使用 PowerShell 的应用程序标识
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. 一定要复制**租户 ID**、**应用程序 ID**和您所使用的密码

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>第 2 部分︰ 保护 Web 应用程序使用的 AAD 的应用程序标识

如果已部署 Web 应用程序就可以在门户中启用它。 否则，请启用授权 Azure 资源管理器部署的一部分。

#### <a name="enable-authorization-in-the-azure-portal"></a>启用在 Azure 门户中的授权

1. 导航到 Web 应用程序，然后单击命令栏中的**设置**。
2. 单击**授权/身份验证**。
3. 将其**打开**。

在这种情况下，应用程序会自动为您创建。 需要此应用程序的客户机 ID 为 3 部分，所以您需要︰

1. 转到[Active directory 中 Azure 的传统门户网站](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory)并选择您的目录。
2. 应用程序在搜索框中的搜索
3. 单击列表中的上它
4. 在**配置**选项卡上单击
5. 您应该会看到**客户机 ID**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>部署 Web 应用程序使用 ARM 模板

首先，您需要创建用于您的 Web 应用程序的应用程序。 这应该与为您的逻辑应用程序使用的应用程序不同。 首先按照上面的步骤第 1 部分中，但现在**主页**和**IdentifierUris**用于实际 https://**URL**的 Web 应用程序。

>[AZURE.NOTE]为您的 Web 应用程序创建应用程序时，必须使用[Azure 门户的传统方法](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory)，如 PowerShell commandlet 未设置所需的权限登录到网站的用户。

一旦您有 ID 和租户 ID，部署模板中包括 Web 应用程序的子资源为以下客户端︰

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

若要运行的部署自动部署的空 Web 应用程序和逻辑应用程序一起使用 AAD 的请单击下面的按钮︰

[![将部署到 Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

有关完整的模板，请参阅[逻辑应用程序调入自定义 API 上的应用程序服务，并受 AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json)。


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>第 3 部分︰ 填充应用程序逻辑中的授权部分

在**HTTP**操作的**授权**部分中︰`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| 元素 | 说明 |
|---------|-------------|
| 类型 | 身份验证类型。 对于 ActiveDirectoryOAuth 身份验证，则值为 ActiveDirectoryOAuth。 |
| 租户 | 用来标识广告租户的租户标识符。 |
| 访问群体 | 必填。 要连接到的资源。 |
| 客户机 Id | Azure AD 应用程序的客户端标识符。 |
| 秘密 | 必填。 客户端请求令牌的机密。 |

上面的模板已经有此设置，但如果直接创作逻辑应用程序时，您需要包括的完全授权部分。

## <a name="securing-your-api-in-code"></a>保护您的代码中的 API

### <a name="certificate-auth"></a>证书身份验证

您可以使用客户证书来为您的 Web 应用程序验证传入的请求。 请参阅有关如何设置您的代码[如何对配置 TLS 相互身份验证的 Web 应用程序](../app-service-web/app-service-web-configure-tls-mutual-auth.md)。

在*授权*部分应提供︰ `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`。

| 元素 | 说明 |
|---------|-------------|
| 类型 | 必填。 身份验证类型。对于 SSL 客户端证书，则必须提出。 |
| pfx | 必填。 PFX 文件的 Base64 编码的内容。 |
| 密码 | 必填。 要访问该 PFX 文件密码。 |

### <a name="basic-auth"></a>基本身份验证

可以使用基本身份验证 （例如用户名和密码） 来验证传入的请求。 基本身份验证是一种常见模式，可以做到在任何语言中构建您的应用程序。

在*授权*部分中，您应提供︰ `{"type": "basic","username": "test","password": "test"}`。

| 元素 | 说明 |
|---------|-------------|
| 类型 | 必填。 身份验证类型。 对于基本的身份验证的值必须是基本。 |
| 用户名 | 必填。 进行身份验证的用户名。 |
| 密码 | 必填。 进行身份验证的密码。 |

### <a name="handle-aad-auth-in-code"></a>在代码中的句柄 AAD 身份验证

默认情况下，您在门户网站中启用的 Azure Active Directory 身份验证不进行细粒度的授权。 例如，它不会锁定您为特定用户或应用程序，而不仅仅是对特定租户的 API。

如果您想要限制 API 只是逻辑的应用程序，例如，在代码中，您可以提取包含 JWT 和检查的调用方，拒绝不匹配任何请求标头。

继续，如果您想要完全在您自己的代码中实现它，并不利用门户网站功能，您可以阅读这篇文章︰[使用 Active Directory 在 Azure 应用程序服务的身份验证](../app-service-web/web-sites-authentication-authorization.md)。

您仍然需要按照上面的步骤创建一个应用程序标识为您的逻辑应用程序并使用它来调用 API。
