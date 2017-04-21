<properties
    pageTitle="API 在 Azure 应用程序服务的应用程序的服务主体身份验证 |Microsoft Azure"
    description="了解如何保护在 Azure 应用程序服务 API 应用程序服务的服务方案。"
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016" 
    ms.author="rachelap"/>

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>API 在 Azure 应用程序服务的应用程序的服务主体身份验证

## <a name="overview"></a>概述

本文介绍如何使用 API 的应用程序*内部*访问应用程序服务的身份验证。 内部情况是您有一个 API 应用程序，您希望仅供您自己的应用程序代码。 应用程序服务中实现该方案的推荐的方式是使用 Azure 的广告来保护该调用的 API 的应用程序。 通过提供应用程序标识 （服务主体） 凭据从 Azure 的广告获得的载荷标记的受保护的 API 应用程序，则调用。 使用 Azure AD 的替代方法，请参阅[Azure 应用程序服务的身份验证概述](../app-service/app-service-authentication-overview.md#service-to-service-authentication)**服务到服务身份验证**部分。

在本文中，您将了解︰

* 如何使用 Azure 活动目录 (AD Azure) 来防止未经身份验证访问的 API 的应用程序。
* 如何使用 API 的应用程序、 web 应用程序中，或移动应用程序的受保护的 API 应用程序使用 Azure AD 服务主体 （应用程序标识） 凭据。 有关如何从逻辑应用程序使用的信息，请参阅[的使用您自定义的 API 位于与应用程序逻辑的应用程序服务](../app-service-logic/app-service-logic-custom-hosted-api.md)。
* 如何确保受保护的 API 应用程序不能在浏览器中调用登录的用户。
* 如何确保受保护的 API 应用程序只能通过特定的 Azure 调用 AD 服务主体。

本文包含两个部分︰

* [如何配置服务在 Azure 应用程序服务中的主体身份验证](#authconfig)部分一般解释如何配置身份验证的任何 API 的应用程序，以及如何使用受保护的 API 应用程序。 这部分同样适用于所有的应用程序服务，包括.NET，Node.js 和 Java 支持的框架。

* 开始[继续.NET 快速入门教程](#tutorialstart)部分，本教程将指导您配置应用程序服务中运行.NET 示例应用程序的"内部控制"方案。 

## <a id="authconfig"></a>如何在 Azure 应用程序服务配置服务主体身份验证

此部分提供了适用于任何 API 的应用程序的一般说明。 对于执行列表.NET 示例应用程序特定的步骤，请转到[继续.NET API 应用程序教程系列](#tutorialstart)。

1. 在[Azure 的门户网站](https://portal.azure.com/)中，导航到 API 应用程序想要保护，然后查找**功能**部分，并单击**设置**刀片式服务器**身份验证 / 授权**。

    ![身份验证/授权在 Azure 门户](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. 在**身份验证 / 授权**刀片式服务器，请**在上**单击。

4. 在**请求没有通过身份验证时要采取的操作**下拉列表中，选择**使用 Azure Active Directory 登录**。

5. 在**身份验证提供程序**下选择**Azure Active Directory**。

    ![在 Azure 门户中的身份验证/授权刀片式服务器](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. **Azure 活动目录设置**刀片式服务器将创建一个新配置 AD Azure 应用程序或使用现有的 AD Azure 应用程序如果您已经拥有一个您要使用。

    内部的方案通常涉及应用程序调用 API 的应用程序的 API。 您可以使用单独的 Azure AD 应用程序的每个 API 的应用程序或只是一个广告 Azure 应用程序。

    此刀片的详细说明，请参阅[如何配置应用程序服务应用程序使用 Azure Active Directory 登录](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)。

7. 完成身份验证提供程序配置刀片式服务器后，单击**确定**。

7. 在**身份验证 / 授权**刀片式服务器，单击**保存**。

    ![单击保存](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

完成后，应用程序服务只允许请求中已配置的调用方从 Azure AD 租户。 无身份验证或授权代码需要受保护的 API 应用程序中。 持有者标记传递给 API 的应用程序以及通常使用在 HTTP 头中的声明，您可以阅读代码来验证请求是从特定的调用方，例如服务主体中的信息。

此身份验证功能支持应用程序服务，包括.NET，Node.js 和 Java 中的所有语言的工作原理相同。 

#### <a name="how-to-consume-the-protected-api-app"></a>如何使用受保护的 API 应用程序

调用方必须将 Azure 广告载体令牌提供 API 调用。 若要获取使用服务主体凭据的持有者标记，调用方将使用活动目录身份验证库 (ADAL [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)， [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)，或[Java](https://github.com/AzureAD/azure-activedirectory-library-for-java))。 以获取令牌，调用 ADAL 的代码提供了 ADAL 到以下信息︰

* Azure AD 租户的名称。
* 客户机 ID 和 Azure AD 应用程序与呼叫相关联的客户端密码 （应用程序键）。
* 使用受保护的 API 应用程序关联的 Azure AD 应用程序客户端 ID。 （如果使用一个 Azure 广告应用程序，这是与调用方相同的客户端 ID。

这些值是在[Azure 的传统门户](https://manage.windowsazure.com/)的 Azure 的广告页面中可用。

获得令牌之后, 调用方包括其授权标头中的 HTTP 请求。  应用程序服务验证该令牌并允许访问受保护的 API 应用程序的请求。

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>如何防止同一组织中的用户访问的 API 的应用程序

持有者标记中同一个租户的用户被视为有效的受保护 API 应用程序。  如果您想要确保服务主体可以调用受保护的 API 应用程序，请验证令牌中的下列声明受保护的 API 应用程序中添加代码︰

* `appid`应该是 Azure 的广告应用程序，与呼叫相关联的客户端 ID。 
* `oid`(`objectidentifier`) 应为调用方的服务主体 ID。 

应用程序服务还提供了`objectidentifier`声称 X-MS-客户端-主体-ID 标头中。

### <a name="how-to-protect-the-api-app-from-browser-access"></a>如何防止浏览器访问的 API 的应用程序

如果不进行验证的受保护的 API 应用程序中的代码中的声明，并使用单独的 Azure AD 应用程序的受保护 API 应用程序中，请确保 Azure AD 应用程序的答复 URL 不是 API 的应用程序的基 URL 相同。 如果回复 URL 直接指向受保护的 API 应用程序中相同的 Azure AD 租户, 的用户可以浏览到 API 的应用程序，登录，并成功调用 API。

## <a id="tutorialstart"></a>继续.NET API 应用程序教程系列

如果按照 Node.js 或 Java 教程系列用于 API 的应用程序，请跳到[后续步骤](#next-steps)部分。 

本文的其余部分将继续.NET API 的应用程序的教程系列，并假定您已经完成[用户身份验证教程](app-service-api-dotnet-user-principal-auth.md)，并具有在 Azure 中运行与已启用的用户身份验证的示例应用程序。

## <a name="set-up-authentication-in-azure"></a>设置 Azure 中的身份验证

在这一节中您配置应用程序服务，以便仅允许访问数据层 API 应用程序的 HTTP 请求是已有效 Azure 广告载体标记。 

在下面的部分中，您可以配置应用程序凭据发送到 Azure AD、 获得持有者表示感激，和将持有者标记发送到数据层 API 应用程序中间层 API 应用程序。 这一过程如图所示。

![服务验证关系图](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

如果按照教程说明时遇到问题，请参见[疑难解答](#troubleshooting)部分教程的结尾 

1. 在[Azure 的门户网站](https://portal.azure.com/)中，导航到 ToDoListDataAPI （数据层） API 应用程序中，您创建的 API 应用程序**设置**刀片式服务器，然后单击**设置**。

2. 在**设置**刀片式服务器，查找**功能**部分，然后单击**身份验证 / 授权**。

    ![身份验证/授权在 Azure 门户](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. 在**身份验证 / 授权**刀片式服务器，请**在上**单击。

4. 在**请求没有通过身份验证时要采取的操作**下拉列表中，选择**使用 Azure Active Directory 登录**。

    这是使应用程序服务，以确保只有经过身份验证请求访问 API 的应用程序设置。 对于具有有效载荷的令牌的请求，应用程序服务将沿着标记传递给 API 的应用程序，并填充与常用的索赔，以使该信息到您的代码更易于使用的 HTTP 标头。

5. 在**身份验证提供程序**，请单击**Azure Active Directory**。

    ![在 Azure 门户中的身份验证/授权刀片式服务器](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. 在**Azure 活动目录设置**刀片式服务器，单击**速成**。

    **表达**与 Azure 选项可以自动创建 AAD 应用程序在您 Azure AD[租户](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)。 

    您不必创建承租人，因为每个 Azure 帐户自动有一个。

7. 在**管理模式下**，单击**新广告应用程序**如果它尚未选中。

    门户将用默认值**创建的应用程序**输入的框。 默认情况下，AD Azure 应用程序称为 API 的应用程序相同。 如果您愿意，您可以输入一个不同的名称。
    
    ![Azure 广告设置](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **注意**︰ 作为一种替代方法，您可以使用一个单一的 Azure 广告应用程序调用 API 应用程序和受保护的 API 应用程序。 如果您选择该替代方法，将不需要此处**创建新的 AD 应用程序**选项，因为已经在用户身份验证教程前面创建 AD Azure 应用程序。 对于本教程，您将使用分隔 Azure AD 应用程序的调用 API 的应用程序和受保护的 API 应用程序。

8. 记下的值是在**创建应用程序**的输入框中;您将查找此 AAD 应用程序在 Azure 经典门户后面。

7. 单击**确定**。

10. 在**身份验证 / 授权**刀片式服务器，单击**保存**。

    ![单击保存](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    应用程序服务使用**登录 URL**创建 Active Directory 的 Azure 应用程序，并**回复 URL**自动设置为您 API 的应用程序的 URL。 后者的值使您 AAD 租户以用户身份登录并访问 API 应用程序中的用户。

### <a name="verify-that-the-api-app-is-protected"></a>验证 API 的应用程序受保护

1. 在浏览器转到 API 的应用程序的 URL︰ 在**API 的应用程序**在 Azure 门户刀片式服务器，在**URL**下单击该链接。 

    您将被重定向到一个登录屏幕，因为不允许未经身份验证的请求访问 API 的应用程序。 

    如果您的浏览器到 Swagger 的用户界面，您的浏览器可能已记录上 — 在这种情况下，打开 InPrivate 或 Incognito 窗口，并转到 Swagger 的用户界面 URL。

18. 在 AAD 租户的用户的凭据登录。

    在您登录后，在浏览器中将显示"已成功创建"页。

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>配置 ToDoListAPI 项目，以获取和发送的 Azure AD 标记

在这一部分中，您执行以下任务︰

* 中间层 API 应用程序使用 Azure AD 应用程序凭据来获取令牌并将其与 HTTP 请求发送到数据层 API 应用程序中添加代码。
* 从 Azure 的广告获取所需的凭据。
* 到 Azure 应用程序服务中间层 API 应用程序中的运行时环境设置中输入的凭据。 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>配置 ToDoListAPI 项目，以获取和发送的 Azure AD 标记

在 ToDoListAPI 项目中，Visual Studio 中进行以下更改。

1. 取消注释所有的*ServicePrincipal.cs*文件中的代码。

    这是使用.net ADAL 获得的 Azure 广告载体标记的代码。  它使用您将稍后设置 Azure 的运行时环境中的几个配置值。 下面是该代码︰ 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **注意︰**这段代码要求 ADAL.NET NuGet 程序包 (Microsoft.IdentityModel.Clients.ActiveDirectory) 已经安装在项目中。 如果您从头开始创建此项目，您必须安装这个程序包。 此 API 的应用程序新项目模板没有自动安装此程序包。

2. 在*控制器/ToDoListController*，请取消注释中的代码`NewDataAPIClient`方法将标记添加到 HTTP 请求授权标头中。

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. 将 ToDoListAPI 项目部署。 (用鼠标右键单击该项目，然后单击**发布 > 发布**。)

    Visual Studio 部署项目并打开浏览器访问 web 应用程序的基 URL。 这将显示 403 错误页面，它通常会尝试从浏览器转到 Web API 的基 URL。

4. 关闭浏览器。

### <a name="get-azure-ad-configuration-values"></a>获取 Azure AD 配置值

11. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)，请转到**Azure Active Directory**。

12. 在**目录**选项卡上，单击 AAD 租户。

14. 单击**应用程序 > 我的公司拥有的应用程序**，然后单击复选标记。

15. 在应用程序的列表中，单击一个 Azure 启用 ToDoListDataAPI （数据层） API 的应用程序的身份验证时为您创建的名称。

16. 单击**配置**选项卡。

5. 复制**客户机 ID**值并将其保存以后可以将其从顶层。 

8. Azure 的经典门户中回到**我的公司拥有的应用程序**，程序的列表，单击 AAD 创建应用程序的中间层 ToDoListAPI API 应用程序 （在前面的教程中创建的一个，不是您在本教程中创建）。

16. 单击**配置**选项卡。

5. 复制**客户机 ID**值并将其保存以后可以将其从顶层。

6. **键**下, 从**选择持续时间**下拉列表中选择**1 年**。

6. 单击**保存**。

    ![生成的应用程序键](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. 复制的值并将其保存以后可以将其从顶层。

    ![复制新的应用程序键](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>在中间层 API 的应用程序的运行时环境配置 Azure 广告设置

1. 转到[Azure 的门户网站](https://portal.azure.com/)，并浏览到**应用程序 API**刀片式服务器 API 应用程序承载 TodoListAPI （中间层） 项目。

2. 单击**设置 > 应用程序设置**。

3. 在**应用程序设置**部分中，添加下列注册表项和值︰

  	| **密钥** | ida 机构︰ |
  	|---|---|
  	| **值** | {您 Azure AD 租户名称} https://login.microsoftonline.com/ |
  	| **示例** | https://login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **密钥** | ida︰ 客户机 Id |
  	|---|---|
  	| **值** | （中间层-ToDoListAPI） 的调用应用程序的客户端 ID |
  	| **示例** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **密钥** | ida: ClientSecret |
  	|---|---|
  	| **值** | 应用程序键的调用应用程序 （中间层-ToDoListAPI） |
  	| **示例** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **密钥** | ida︰ 资源 |
  	|---|---|
  	| **值** | 被调用的应用程序的客户端 ID （数据层-ToDoListDataAPI） |
  	| **示例** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **注意**︰ 为`ida:Resource`，请确保您使用调用应用程序的**客户机 ID**和不其**ID 的应用程序的 URI**。

    `ida:ClientId`和`ida:Resource`不同值对于本教程因为您使用单独的 Azure AD applicaations 的中间层和数据层。 如果您正在使用一个 Azure 广告应用程序调用 API 应用程序和受保护的 API 应用程序中，将使用相同的值，在这两`ida:ClientId`， `ida:Resource`。

    该代码使用 ConfigurationManager 来获得这些值，以便它们可以存储项目的 Web.config 文件中或在 Azure 的运行时环境中。 在 Azure 应用程序服务中运行 ASP.NET 应用程序时，环境设置自动重写 Web.config 中的设置。 环境设置通常是一种[较为安全的方法来存储相比到 Web.config 文件中的敏感信息](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)。

6. 单击**保存**。

    ![单击保存](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>测试应用程序

1. 在浏览器转到 HTTPS AngularJS 前端 web 应用程序的 URL。

2. 单击**任务列表**选项卡并使用 Azure 广告组织中的用户的凭据登录。 

4. 添加待办事项，以验证应用程序正常工作。

    ![待办事项列表页](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    如果应用程序没有按预期工作，仔细检查所有在 Azure 门户输入的设置。 如果所有的设置看起来是正确的在本教程后面看到的[疑难解答](#troubleshooting)一节。

## <a name="protect-the-api-app-from-browser-access"></a>从浏览器访问保护 API 的应用程序

在本教程中，您将创建单独的 Azure AD ToDoListDataAPI （数据层） API 应用程序的应用程序。 如您所见，当应用程序服务创建 AAD 应用程序时，它将以一种允许用户转到在浏览器中的 API 的应用程序的 URL 和登录配置 AAD 应用程序。 这意味着可能会在您 Azure AD 租户，不只是服务主体，访问 API 的最终用户。 

如果您想要阻止浏览器访问不受保护的 API 应用程序中编写任何代码的情况下，可以更改**回复 URL** AAD 应用程序中，以便它是不同于 API 的应用程序的基 URL。 

### <a name="disable-browser-access"></a>禁用浏览器访问

1. 在传统门户的**配置**选项卡中的 AAD 应用程序为 TodoListService 创建的以便它是有效的 URL，但不是 API 的应用程序的 URL 更改**回复 URL**字段中的值。
 
2. 单击**保存**。

### <a name="verify-browser-access-no-longer-works"></a>验证浏览器访问不再有效

前面您验证，您可以通过转到 API 应用程序 URL 在浏览器中使用单个用户的凭据登录。 在本节中，您将验证这已不再可能。 

1. 在新的浏览器窗口，再次到 API 的应用程序的 URL。

2. 当系统提示您这样做时，请登录。

3. 登录成功，但将导致一个错误页。

    已配置 AAD 的应用程序，以便 AAD 租户的用户不能登录，然后在浏览器中访问 API。 您仍然可以访问 API 应用程序使用服务主体令牌，您可以转至 web 应用程序的 URL 并添加更多待办事项通过验证。

## <a name="restrict-access-to-a-particular-service-principal"></a>限制对特定服务主体的访问  

现在，可以为用户获得令牌的任何调用方或在 Azure AD 租户的服务主体可以调用 TodoListDataAPI （数据层） API 的应用程序。 您可能想要确保数据层 API 应用程序仅接受调用从 TodoListAPI （中间层） API 应用程序中，并且只能从特定的服务主体。 

您可以通过添加代码以验证添加这些限制`appid`和`objectidentifier`对来电提出要求。

对于本教程，您将验证应用程序 ID 和服务主体 ID 直接在您的控制器操作的代码。  替代方法是使用自定义`Authorize`特性或为此验证在您的启动序列 （例如 OWIN 中间件）。 后者的示例，请参阅[本示例应用程序](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs)。 

对 TodoListDataAPI 项目中进行以下更改。

2. 打开*Controllers/TodoListController.cs*文件。

3. 取消注释行设置`trustedCallerClientId`， `trustedCallerServicePrincipalId`。

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. 取消注释 CheckCallerId 方法中的代码。 开始时每个控制器中的操作方法调用此方法。 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. 重新部署到 Azure 应用程序服务的 ToDoListDataAPI 项目。

6. 在浏览器中，转到 AngularJS 前端 web 应用程序的 HTTPS URL，并在主页中单击**待办事项列表**选项卡。

    因为到后端调用失败，应用程序不起作用。 新的代码检查实际的应用程序标识和 objectidentifier，但它还没有正确的值来检查它们对。 浏览器开发人员工具控制台报告服务器返回的 HTTP 401 错误。

    ![在开发人员的错误工具控制台](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    在下面的步骤中，您将配置所需要的值。

8. 使用 Azure AD PowerShell，Azure 的广告应用程序，您创建了 TodoListWebApp 项目获取服务主体的值。

    一。 有关如何安装 Azure PowerShell 和连接到您的订阅的说明，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。

    b。 若要获取服务主体的列表，请执行`Login-AzureRmAccount`命令，然后`Get-AzureRmADServicePrincipal`命令。

    c。 对象 id 查找服务主体的 TodoListAPI 应用程序，并将其保存在以后复制的位置。

7. 在 Azure 的门户中，导航至 API 应用程序 API 应用程序部署到的 ToDoListDataAPI 项目的刀片式服务器。

9. 单击**设置 > 应用程序设置**。

3. 在**应用程序设置**部分中，添加下列注册表项和值︰

  	| **密钥** | todo:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **值** | 调用应用程序的服务主体 id |
  	| **示例** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **密钥** | todo:TrustedCallerClientId |
  	|---|---|
  	| **值** | 客户端调用应用程序的复制从 TodoListAPI Azure AD 应用程序 ID |
  	| **示例** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. 单击**保存**。

    ![单击保存](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. 在您的浏览器，返回到 web 应用程序的 URL，并在主页中单击**待办事项列表**选项卡。

    这一次应用程序的工作，因为该受信任的调用方应用程序 ID 和服务主体 ID 是预期值。

    ![待办事项列表页](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>生成项目从零开始

通过使用**Azure API 的应用程序**项目模板和任务列表控制器替换默认值控制器创建两个 Web API 项目。 获取在 ToDoListAPI 项目中的 Azure AD 服务主体标记，[活动目录身份验证库 (ADAL) 为使.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet 程序包已安装。
 
有关如何创建具有 Web API 后端类似 ToDoListAngular 的 AngularJS 单页面应用程序的信息，请参阅[手上实验室︰ 生成 ASP.NET Web API 和 Angular.js 单页面应用程序 (SPA)](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs)。 有关如何添加 Azure AD 身份验证代码的信息，请参阅[保护 AngularJS 单页面应用程序使用 Azure 的广告](../active-directory/active-directory-devquickstarts-angular.md)。

## <a name="troubleshooting"></a>故障排除

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* 请确保您不要混淆 ToDoListAPI （中间层） 和 ToDoListDataAPI （数据层）。 例如，在本教程中添加身份验证到数据层 API 的应用程序，**但应用程序键必须来自 Azure 广告创建应用程序的中间层 API 的应用程序**。

## <a name="next-steps"></a>下一步行动

这是最后一本教程系列 API 应用程序中。 

关于 Azure 活动目录的详细信息，请参阅以下资源。

* [Azure 广告开发人员指南](http://aka.ms/aaddev)
* [Azure 的广告方案](http://aka.ms/aadscenarios)
* [Azure 的广告样本](http://aka.ms/aadsamples)

    [WebApp 的 WebAPI-OAuth2-AppIdentity-最低](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet)的示例是类似于所示在本教程中，而无需使用应用程序服务的身份验证。

通过使用 Visual Studio 或从[源代码控制系统](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)，[自动部署](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery)到 API 的应用程序，部署 Visual Studio 项目的其他方法的信息请参阅[如何部署 Azure 应用程序服务的应用程序](../app-service-web/web-sites-deploy.md)。
