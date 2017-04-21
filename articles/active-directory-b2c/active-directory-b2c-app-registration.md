<properties
    pageTitle="Azure 的活动目录 B2C: 应用程序注册 |Microsoft Azure"
    description="如何向应用程序注册 Azure 活动目录 B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure 的活动目录 B2C︰ 注册应用程序

## <a name="prerequisite"></a>系统必备组件

若要生成接受使用者注册和登录的应用程序，首先需要向应用程序注册 Azure 活动目录 B2C 租户。 通过使用[创建 Azure AD B2C 租户](active-directory-b2c-get-started.md)中列出的步骤获取您自己的组织。 在这篇文章中的所有步骤后，您必须固定到您 Startboard B2C 功能刀片式服务器。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>导航到 B2C 功能刀片式服务器

如果您有固定到您 Startboard B2C 功能刀片式服务器，您将看到刀片式服务器，一旦您登录到[Azure 门户](https://portal.azure.com/)为 B2C 租户的全局管理员。

您还可以通过[Azure 的门户网站](https://portal.azure.com/)上的左的导航窗格中单击**浏览**，然后再**Azure AD B2C**访问刀片式服务器。

> [AZURE.IMPORTANT] 必须是全局管理员的 B2C 租户，若要能够访问 B2C 功能刀片式服务器。 从任何其他租户全局管理员或用户从任何客户端不能访问它。  您可以使用切换到 B2C 租户租户切换器在 Azure 门户右上角。

## <a name="register-an-application"></a>注册应用程序

1. 在 B2C 功能的刀片在 Azure 的门户网站上，单击**应用程序**。
2. 单击**+ 添加**刀片式服务器顶部。
3. 将描述为使用者应用程序的应用程序输入一个**名称**。 例如，您可以输入"Contoso B2C app"。
4. 如果您正在编写一个基于 web 的应用程序，切换为**是****包括 web 应用程序 / web API**开关。 **回复 Url**是终结点 Azure AD B2C 返回应用程序请求任何标记的位置。 例如，输入`https://localhost:44321/`。 如果您的 web 应用程序还将调用某些 web API 受 Azure AD B2C，您需要通过单击**生成密钥**按钮以及创建**应用程序密码**。

    > [AZURE.NOTE] **应用程序密码**是重要的安全凭据，并应适当地保护。

5. 如果您正在编写一个移动应用程序，则切换**包括本机客户端**切换到**是**。 复制下来的默认值自动为您创建的**重定向 URI** 。
6. 单击**创建**来注册您的应用程序。
7. 单击刚创建的应用程序和复制下来以后在代码中您将使用全局唯一**的应用程序客户端 ID** 。

> [AZURE.IMPORTANT] 在 B2C 功能刀片式服务器中创建的应用程序必须在相同的位置中管理。 如果编辑 B2C 应用程序使用 PowerShell 或另一个门户网站，它们变得不受支持，可能不会使用 Azure AD B2C。

## <a name="build-a-quick-start-application"></a>构建一个快速启动应用程序

现在，您已经使用 Azure AD B2C 注册的应用程序，您可以完成即可启动并运行我们的快速启动教程之一。 下面是一些建议︰

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
