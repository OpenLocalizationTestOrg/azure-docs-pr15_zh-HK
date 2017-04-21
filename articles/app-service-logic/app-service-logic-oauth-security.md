<properties
    pageTitle="OAUTH 安全的 SaaS 连接器和 API 的应用程序 |Azure"
    description="阅读有关 OAUTH 的连接器和 Azure 应用程序服务; 在 API 应用程序中的安全信息microservices 的体系结构;saas"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>在 SaaS 连接器了解 OAUTH 安全

>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2014年-12-01-预览架构版本。

许多软件即服务 (SaaS) 连接器，像 Facebook、 Twitter、 收存箱，等要求使用 OAUTH 协议进行身份验证的用户。  当您使用应用程序逻辑从这些 SaaS 连接器时，我们提供逻辑应用程序设计器中单击"授权"的其中一种简化的用户体验。 当您**授权**要求您签署的 （如果不是已经） 并提供同意的情况下连接到以您的名义上的 SaaS 服务。 不要提供许可和授权之后，逻辑应用程序可以访问这些 SaaS 服务。

## <a name="create-your-own-saas-app"></a>创建您自己的 SaaS 应用程序
此简化的体验是可能的因为我们以前创建和注册我们的应用程序中这些 SaaS 服务。  在某些情况下，您可能想要注册并使用您自己的应用程序。  这是必需的例如，如果要在自定义应用程序中使用这些 SaaS 连接器。 本示例使用收存箱接头，但过程依靠 OAUTH 的所有连接器的相同。

即使在逻辑的应用程序的上下文中，可以使用您自己的应用程序，而不是使用我们提供的默认应用程序。 如果无法连接的"授权"按钮，您可以尝试创建您自己的应用程序。 下面列出了 Twitter 连接器的下列步骤︰

1. 在 Azure 预览门户打开 Twitter 连接器。 转到**浏览** > **API 的应用程序**。 选择您的 Twitter 连接器︰  
    ![][1]

2. 选择**设置** > **身份验证**︰  
    ![][2]

3. 将复制的**重定向 URI**值︰  
    ![][3]

4. 转到[使用 Twitter](http://apps.twitter.com)和**创建新的应用程序**。 在**回调 URL**属性中，将粘贴复制从 Twitter 接头的**重定向 URI**值︰ ![][4]  
5. 当创建您的 Twitter 应用程序时，选择**键和访问令牌**。 将这些值复制。
6. 在 Twitter 连接器的身份验证设置，请在**客户机 ID**和**客户端密钥**属性中粘贴这些值︰   
    ![][5]  
7. 保存您的连接器设置。  

现在，您应该能够使用您的应用程序逻辑从连接器。 当您使用此连接器从应用程序逻辑时，而不是默认的应用程序使用您的应用程序。  

> [AZURE.NOTE] 如果以前已获得授权的应用程序，您可能需要台上的应用程序。


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
