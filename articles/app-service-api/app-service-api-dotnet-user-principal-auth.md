<properties
    pageTitle="API 在 Azure 应用程序服务的应用程序的用户身份验证 |Microsoft Azure"
    description="了解如何允许仅为已经过身份验证的用户访问，从而保护在 Azure 应用程序服务 API 的应用程序。"
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

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>API 在 Azure 应用程序服务的应用程序的用户身份验证

## <a name="overview"></a>概述

本文介绍如何保护 Azure API 的应用程序，以便只有通过身份验证的用户可以调用它。 本文假定您已经阅读了[Azure 应用程序服务的身份验证的概述](../app-service/app-service-authentication-overview.md)。

您将学习︰

* 如何配置身份验证提供程序，使用 Azure 活动目录 (AD Azure) 的详细信息。
* 如何使用受保护的 API 应用程序使用[活动目录身份验证库 (ADAL) 的 JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js)。

本文包含两个部分︰

* [如何配置在 Azure 应用程序服务中的用户身份验证](#authconfig)部分一般解释如何配置任何 API 的应用程序的用户身份验证，并同样适用于所有的应用程序服务，包括.NET，Node.js 和 Java 支持的框架。

* 您通过使用.NET 配置示例应用程序[继续.NET API 应用程序教程](#tutorialstart)部分，文章参考线启动后端和 AngularJS 前结束，以便它使用 Azure Active Directory 进行用户身份验证。 

## <a id="authconfig"></a>如何在 Azure 应用程序服务配置用户身份验证

此部分提供了适用于任何 API 的应用程序的一般说明。 对于执行列表.NET 示例应用程序特定的步骤，请转到[继续.NET 快速入门教程](#tutorialstart)。

1. 在[Azure 的门户网站](https://portal.azure.com/)中，导航到 API 应用程序想要保护，查找**功能**部分，然后单击**设置**刀片式服务器**身份验证 / 授权**。

    ![Azure 门户网站身份验证/授权](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. 在**身份验证 / 授权**刀片式服务器，请**在上**单击。

4. 从**请求没有通过身份验证时要采取的操作**下拉列表中选择一个选项。

    * 如果希望只有已通过身份验证的呼叫到达您 API 的应用程序，选择**...日志中的**选项之一。 此选项使您能够无需编写任何代码在其中运行保护 API 的应用程序。

    * 如果您希望所有呼叫到达您 API 的应用程序，选择**允许请求 （无操作）**。 可以使用此选项以将未经身份验证的调用方身份验证提供程序的选择。 使用此选项，您需要编写代码来处理授权。

    有关详细信息，请参阅[身份验证和授权的 API 在 Azure 应用程序服务的应用程序](app-service-api-authentication.md#multiple-protection-options)。

5. 选择一个或多个**身份验证提供程序**。

    图像显示要求所有调用方进行身份验证的 Azure 广告的选项。
 
    ![Azure 门户的身份验证/授权刀片式服务器](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    当您选择一个身份验证提供程序时，门户将显示为该提供程序配置刀片式服务器。 

    介绍了如何配置身份验证提供程序配置刀片的详细说明，请参阅[如何配置应用程序服务应用程序使用 Azure Active Directory 登录](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)。 （该链接会转到 Azure 广告，有关的文章，但文章中包含链接到文章的其他身份验证提供程序的选项卡）。

7. 完成身份验证提供程序配置刀片式服务器后，单击**确定**。

7. 在**身份验证 / 授权**刀片式服务器，单击**保存**。

完成后，应用程序服务在到达该 API 应用程序之前验证所有 API 调用。 身份验证服务使用相同的应用程序服务支持，包括.NET，Node.js 和 Java 中的所有语言。 

若要验证的 API 调用，调用方授权标头的 HTTP 请求中包括身份验证提供程序的 OAuth 2.0 载体令牌。 使用的身份验证提供的 SDK，可以获得令牌。

## <a id="tutorialstart"></a>继续.NET API 应用程序教程

如果按照 Node.js 或 Java API 的应用程序的教程，请跳到下一篇文章中，[服务 API 应用程序主体的身份验证](app-service-api-dotnet-service-principal-auth.md)。 

如果采用.NET API 的应用程序的教程系列，已经部署示例应用程序按照[第一](app-service-api-dotnet-get-started.md)和[第二个](app-service-api-cors-consume-javascript.md)教程中的说明，跳到[设置身份验证应用程序服务和 Azure 的广告](#azureauth)节。

如果您想要按照本教程中，而无需经过第一和第二个教程，执行下列步骤介绍如何通过使用一个自动化的过程来部署示例应用程序开始。

>[AZURE.NOTE] 以下步骤使您进入相同的起始点就像就像前两个教程，有一个例外︰ Visual Studio 不会知道哪个 web 应用程序或 API 应用程序部署到每个项目。 这意味着您不必在本教程中的精确说明，解释如何将部署到适当的目标。 如果你不愿意搞清楚如何做自己的部署步骤，则最好从比要开始与此自动化的部署过程的[第一个教程](app-service-api-dotnet-get-started.md)按照教程系列。

1. 请确保您具有所有系统必备组件在[第一个教程](app-service-api-dotnet-get-started.md)中列出。 除了列出的先决条件，这些身份验证教程假设您已经使用应用程序服务 web 应用程序和 API 在 Visual Studio 和 Azure 门户的应用程序。

2. 单击[待办事项列表的示例存储库自述文件](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md)来部署 API 的应用程序和 web 应用程序中的**部署到 Azure**按钮。 记下获取创建，因为您可以使用此以后查找 web 应用程序和 API 的应用程序名称的 Azure 的资源组。
 
3. 下载或克隆[到待办事项列表的示例存储库](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)以获取您将使用本地在 Visual Studio 中的代码。

## <a id="azureauth"></a>设置应用程序服务和 Azure AD 中的身份验证

现在，您可以在 Azure 应用程序服务中运行，而无需对用户进行身份验证的应用程序。 在这一节中添加身份验证通过执行以下任务︰

* 配置应用程序服务所需 Azure 活动目录 (AD Azure) 身份验证的调用中间层 API 的应用程序。
* 创建 AD Azure 应用程序。
* 将持有者标记登录后发送到 AngularJS 前端 Azure AD 应用程序配置。 

如果按照教程说明时遇到问题，请参见[疑难解答](#troubleshooting)部分教程的结尾 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>中间层 API 应用程序的配置身份验证

1. 在[Azure 的门户网站](https://portal.azure.com/)中，导航到**设置**刀片式服务器 API 的应用程序创建的 ToDoListAPI 项目，查找**功能**部分，然后单击**身份验证 / 授权**。

    ![Azure 门户网站身份验证/授权](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. 在**身份验证 / 授权**刀片式服务器，请**在上**单击。

4. 在**请求没有通过身份验证时要采取的操作**下拉列表中，选择**使用 Azure Active Directory 登录**。

    此选项确保没有 unauathenticated 请求的访问 API 的应用程序。 

5. 在**身份验证提供程序**，请单击**Azure Active Directory**。

    ![Azure 门户的身份验证/授权刀片式服务器](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. 在**Azure 活动目录设置**刀片式服务器，请单击**表达**

    ![Azure 门户直通身份验证/授权选项](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    **Express**选项后，应用程序服务可以自动创建一个 Azure 的广告应用在您 Azure AD[租户](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)。 

    您不必创建承租人，因为每个 Azure 帐户自动有一个。

7. 在**管理模式下**，如果尚未选中，请单击**新广告应用程序**并注意在**创建应用程序**文本框中; 的值您将查找此 AAD 应用程序在 Azure 经典门户后面。

    ![Azure 门户 Azure 广告设置](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure 将自动创建具有指示的名称在 Azure AD 租户的 Azure 广告应用程序。 默认情况下，AD Azure 应用程序称为 API 的应用程序相同。 如果您愿意，您可以输入一个不同的名称。
 
7. 单击**确定**。

7. 在**身份验证 / 授权**刀片式服务器，单击**保存**。

    ![单击保存](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

现在只有 Azure AD 租户的用户可以调用 API 的应用程序。

### <a name="optional-test-the-api-app"></a>可选︰ 测试 API 的应用程序

1. 在浏览器转到 API 的应用程序的 URL︰ 在**API 的应用程序**在 Azure 门户刀片式服务器，在**URL**下单击该链接。  

    您将被重定向到一个登录屏幕，因为不允许未经身份验证的请求访问 API 的应用程序。

    如果您的浏览器将转到"已成功创建"网页中，浏览器可能已记录上 — 在这种情况下，打开 InPrivate 或 Incognito 窗口，然后转到 API 的应用程序的 URL。

2. 使用 Azure AD 租户的用户凭据登录。

    在您登录后，在浏览器中将显示"已成功创建"页。

9. 关闭浏览器。

### <a name="configure-the-azure-ad-application"></a>配置 AD Azure 应用程序

当配置 Azure AD 身份验证时，应用程序服务将为您创建 Azure 广告应用程序。 默认情况下，新的 Azure AD 应用程序配置来提供载体令牌提供给 API 的应用程序的 URL。 在本节中提供对 AngularJS 直接到中间层 API 的应用程序而不是前端的持有者标记 Azure AD 应用程序配置。 AngularJS 前端将标记发送到 API 的应用程序时，它调用 API 的应用程序。

>[AZURE.NOTE] 为尽可能的简单，本教程使用一个单一的 Azure 广告使进程应用程序前端和中间层 API 的应用程序。 另一种方法是使用两个 Azure AD 应用程序。 在这种情况下必须为调用中间层的 Azure 广告应用程序前端的 Azure AD 应用程序权限。 多个应用程序的这种方法将为您提供更精细的控制到每一层。

11. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)，请转到**Azure Active Directory**。

    您必须使用经典的门户，因为您需要访问某些 Azure Active Directory 设置目前尚不在当前的 Azure 门户。

12. 在**目录**选项卡上，单击 AAD 租户。

    ![在门户中经典的 azure 广告](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. 单击**应用程序 > 我的公司拥有的应用程序**，然后单击复选标记。

    您可能需要刷新页面以查看新的应用程序。

15. 在应用程序的列表中，单击 Azure 时启用了身份验证，请为您的 API 应用程序为您创建一个的名称。

    ![Azure AD 应用程序选项卡](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. 单击**配置**。

    ![Azure AD 配置选项卡](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. 将**登录 URL**设置为您 AngularJS 的 web 应用程序，没有尾部反斜杠的 URL。

    例如︰ https://todolistangular.azurewebsites.net

    ![登录 URL](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. 将**回复 URL**设置为您的 web 应用程序，没有尾部反斜杠的 URL。

    例如︰ https://todolistsangular.azurewebsites.net

16. 单击**保存**。

    ![回复 URL](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. 在页面的底部，单击**管理清单 > 下载清单**。

    ![下载清单](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. 文件下载到您可以编辑的位置。

16. 在下载的清单文件，搜索`oauth2AllowImplicitFlow`属性。 更改此属性的值`false`到`true`，然后保存该文件。

    此设置是从 JavaScript 单页面应用程序进行访问所必需的。 它使 Oauth 2.0 载体令牌中返回 URL 片段。

16. 单击**管理清单 > 上载清单**，并上传您在前面的步骤中更新该文件。

    ![将上载清单](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. 复制**客户机 ID**值并将其保存在以后可以将其从地方。

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>ToDoListAngular 将项目配置为使用身份验证

在这一节中您更改 AngularJS 前端，以便它使用活动目录身份验证库 (ADAL) 的 JS 来获取登录的用户从 Azure 广告载体令牌。 以下图中所示的代码将在 HTTP 请求中发送到中间层，包括标记。 

![用户身份验证关系图](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

对 ToDoListAngular 项目中的文件进行以下更改。

1. 打开*index.html*文件。

2. 取消注释引用的 JS 脚本活动目录身份验证库 (ADAL) 的行。

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. 打开*app/scripts/app.js*文件。

2. 注释掉的标记为无需身份验证的代码块并取消注释标记为"的身份验证"的代码块。

    此更改引用 JS ADAL 身份验证提供程序，并提供给它的配置值。 在以下步骤中您可以设置您的 API 的应用程序和 Azure 广告应用程序的配置值。

8. 在代码中设置`endpoints`变量，对 API 的应用程序的 URL 设置 API URL，您创建了 ToDoListAPI 项目，并到 Azure 的传统门户网站从复制的客户端 ID 设置 Azure AD 应用程序 ID。

    现类似于下面的示例代码。

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. 对的调用中`adalProvider.init`、`tenant`为您的组织名称和`clientId`到前一步骤中所用的相同值。

    现类似于下面的示例代码。

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    这些更改为`app.js`指定的调用代码和调用的 API 相同 AD Azure 应用程序中。

1. 打开*app/scripts/homeCtrl.js*文件。

2. 注释掉的标记为无需身份验证的代码块并取消注释标记为"的身份验证"的代码块。

1. 打开*app/scripts/indexCtrl.js*文件。

2. 注释掉的标记为无需身份验证的代码块并取消注释标记为"的身份验证"的代码块。

### <a name="deploy-the-todolistangular-project-to-azure"></a>ToDoListAngular 项目部署到 Azure

8. 在**解决方案资源管理器**中用鼠标右键单击 ToDoListAngular 项目，然后单击**发布**。

9. 单击**发布**。

    Visual Studio 部署项目并打开浏览器访问 web 应用程序的基 URL。 这将显示 403 错误页面，它通常会尝试从浏览器转到 Web API 的基 URL。

    您仍需要对中间层 API 应用程序中进行更改，可以测试应用程序之前。

10. 关闭浏览器。

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>ToDoListAPI 将项目配置为使用身份验证

当前的 ToDoListAPI 项目将发送"*"作为`owner`到 ToDoListDataAPI 的值。 本节中，您将更改发送登录的用户的 ID 代码。

在 ToDoListAPI 项目中进行以下更改。

1. 打开*Controllers/ToDoListController.cs*文件，并取消注释的行中设置每个操作方法`owner`对 Azure AD`NameIdentifier`声明值。 例如︰

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **重要提示**︰ 不要取消注释代码在`ToDoListDataAPI`方法;以后服务主体身份验证本教程，您将执行的操作。

### <a name="deploy-the-todolistapi-project-to-azure"></a>ToDoListAPI 项目部署到 Azure

8. 在**解决方案资源管理器**中用鼠标右键单击 ToDoListAPI 项目，然后单击**发布**。

9. 单击**发布**。

    Visual Studio 部署项目并打开浏览器访问 API 的应用程序的基 URL。

10. 关闭浏览器。

### <a name="test-the-application"></a>测试应用程序

9. 转至 web 应用程序中，**使用 HTTPS，不是 HTTP**URL。

8. 单击**任务列表**选项卡。

    提示您以用户身份登录。

9. 在 AAD 租户的用户凭据登录。

10. 显示**待办事项列表**页面。

    ![待办事项列表页](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    因为到目前为止他们都遇到过这样的所有者显示没有待办事项"*"。 中间层现在要求登录的用户，项，但尚未创建。

11. 添加新的待办事项项目，以验证应用程序正常工作。

12. 在另一个浏览器窗口中，转到 ToDoListDataAPI API 应用程序 Swagger 用户界面的 URL，然后单击**任务列表 > 获取**。 输入一个星号的`owner`参数，然后单击**重试出来**。

    响应显示新的待办事项有实际 Azure AD 用户 ID 中的所有者属性。

    ![在 JSON 响应的所有者 ID](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>生成项目从零开始

通过使用**Azure API 的应用程序**项目模板和任务列表控制器替换默认值控制器创建两个 Web API 项目。 

有关如何创建具有 Web API 2 后端的 AngularJS 单页面应用程序的信息，请参阅[手上实验室︰ 生成 ASP.NET Web API 和 Angular.js 单页面应用程序 (SPA)](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs)。 有关如何添加 Azure AD 身份验证代码的信息，请参阅以下资源︰

* [保护与 Azure AD AngularJS 单页面应用程序](../active-directory/active-directory-devquickstarts-angular.md)。
* [介绍 ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>故障排除

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* 请确保您不要混淆 ToDoListAPI （中间层） 和 ToDoListDataAPI （数据层）。 例如，验证您将身份验证添加到中间层 API 应用程序中，不在数据层。 
* 请确保 AngularJS 源代码引用的中间层 API 应用程序 URL （ToDoListAPI，不 ToDoListDataAPI） 和正确的 Azure AD 客户端 id。 

## <a name="next-steps"></a>下一步行动

在本教程中，您学习了如何使用 API 使应用程序的应用程序服务的身份验证以及如何使用 ADAL JS 库调用 API 的应用程序。 下一步的教程中，您将学习如何[安全地访问您的 API 应用程序服务的服务方案](app-service-api-dotnet-service-principal-auth.md)。

