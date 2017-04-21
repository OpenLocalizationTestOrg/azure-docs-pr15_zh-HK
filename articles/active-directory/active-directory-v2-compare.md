<properties
    pageTitle="Azure AD v2.0 终结点 |Microsoft Azure"
    description="比较原始的 Azure AD 和 v2.0 终结点。"
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

# <a name="whats-different-about-the-v20-endpoint"></a>V2.0 终结点有关的区别是什么？

如果您熟悉 Azure Active Directory 或具有与过去的 Azure AD 集成应用程序时，可能有一些差异 v2.0 端点，您不会希望中。  本文档调用您的理解这些不同之处。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft 帐户和 Azure 的广告帐户
v2.0 终结点使开发人员能够编写接受登录 Microsoft 帐户和 Azure 广告客户、 使用单个身份验证终结点的应用程序。  这使您能够编写您的应用程序完全帐户无关;它可以是帐户的未知的用户签名中的类型。  当然，您*可以*使您的应用程序特定的会话中正在使用的帐户类型的意识到，但您不必。

例如，如果您的应用程序调用[Microsoft Graph](https://graph.microsoft.io)，一些额外的功能和数据将提供给企业用户，如 SharePoint 网站或目录数据。  但对于很多操作，例如[读取用户的邮件](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message)，可以编写代码对 Microsoft 帐户和 Azure 的广告帐户完全相同。  

将您的应用程序集成与 Microsoft 帐户和 Azure 的广告帐户现在是一个非常简单的过程。  可以使用一组终结点、 单个库文件和单个应用程序注册以获得访问权限的使用者和企业领域。  要了解有关 v2.0 终结点的详细信息，请签出[概述](active-directory-appmodel-v2-overview.md)。


## <a name="new-app-registration-portal"></a>新的应用程序注册门户
v2.0 终结点只能注册中的新位置︰ [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)。  这是门户网站，您可以获取与应用程序 Id，自定义您的应用程序登录页面，等等。  您只需要访问该门户是供电的 Microsoft 帐户-个人或工作中的学校。  

我们将继续随着时间的推移将越来越多的功能添加到此应用程序注册门户。  其目的是此门户将为您可以管理任何和一切与您的 Microsoft 应用程序的新位置。


## <a name="one-app-id-for-all-platforms"></a>所有平台的一个应用程序 Id
在原始的 Azure Active Directory 服务中，您可能注册为单个项目若干个不同的应用程序。  他们不得不使用单独的应用程序注册为您的本机客户端和 web 应用程序︰

![旧应用程序注册用户界面](../media/active-directory-v2-flows/old_app_registration.PNG)

例如，如果您创建了一个网站和 iOS 应用程序，您必须将它们分开，进行注册使用两个不同的应用程序 Id。  如果您有网站后, 端 web api，您可能为一个单独的应用程序的每个在注册 Azure 的广告。  如果有 iOS 应用程序，Android 应用程序，您也可以注册两个不同的应用程序。  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

现在，您只需要为单个应用程序注册和单一应用程序 Id 为每个项目。  可以添加到每个项目中，几个"平台"，并为您添加的每个平台提供相应数据。  当然，您可以创建多个应用程序要根据您的要求，但多数情况下只有一个应用程序 Id 应该是有必要。

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

我们的目标是这将导致更简化了应用程序管理和开发经验，并创建一个项目，可能在工作的更统一的视图。


## <a name="scopes-not-resources"></a>不是资源的范围
在原始的 Azure 的广告服务，应用程序可以作为**资源**或收件人的标记。  一个资源可以定义的**作用域**或**oAuth2Permissions** ，它可以理解，许多允许客户端应用程序请求对一组特定的作用域为该资源的标记。  作为一种资源的 Azure 广告图形 API，请考虑︰

- 资源标识符，或`AppID URI`:`https://graph.windows.net/`
- 作用域，或`OAuth2Permissions`: `Directory.Read`， `Directory.Write`，等等。  

所有这些对于如此 v2.0 终结点。  应用程序仍可以表现为资源，定义作用域，并由 URI 标识。  客户端应用程序仍然可以请求对这些范围的访问。  但是，已更改的客户端请求这些权限的方法。  在过去，2.0 的 OAuth 授权请求到 Azure 的广告所示类似︰

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

**资源**参数所指示的资源的客户端应用程序正在请求的授权。  Azure AD 计算基于静态配置在 Azure 门户中，并据此发放令牌的应用程序所需的权限。  现在，同样的 OAuth 2.0 授权请求如下所示︰

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

在**scope**参数表示的资源和权限的应用程序正在请求的授权。 所需的资源仍然是非常在请求中存在-它只是包含在每个作用域参数的值。  以这种方式使用 scope 参数允许 v2.0 终结点以更符合 OAuth 2.0 规范，并与公共行业实践更好地对齐。  它还使应用程序要执行[增量的同意](#incremental-and-dynamic-consent)，在下一节中描述。

## <a name="incremental-and-dynamic-consent"></a>增量和动态的同意
应用程序注册上市的 Azure AD 中为其指定所需的 OAuth 2.0 权限在 Azure 门户中，在应用程序创建时所需的服务︰

![权限注册用户界面](../media/active-directory-v2-flows/app_reg_permissions.PNG)

**静态**配置的应用程序所需的权限。  这允许应用程序存在于 Azure 门户的配置并保持漂亮而简单的代码，而它为开发人员提供几个问题︰

- 应用程序必须知道所有的权限创建应用程序时需要不断。  随着时间的推移添加权限是一个艰难的过程。
- 应用程序必须知道所有像以往任何时候都提前访问的资源。  很难创建的应用程序可以访问任意数量的资源。
- 应用程序必须请求它以往任何时候都需要对用户的第一个登录的所有权限。  在某些情况下这导致提倡从批准初始登录的应用程序的访问权限的最终用户的权限的列表很长。

使用 v2.0 的终结点，您可以指定您的应用程序需要**动态**，在运行时，在您的应用程序的常规使用的权限。  若要执行此操作，您可以指定您的应用程序需要在任何指定时刻的时间它们中包括的范围`scope`授权请求的参数︰

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

应用程序还可以将数据写入到其目录读取 Azure AD 用户的目录数据，上述请求权限。  如果用户已同意在该特定应用程序的过去的那些权限，他们将只需输入他们的凭据并登录到该应用程序。  如果用户不具有这些权限的任何许可的 v2.0 终结点将询问用户同意这些权限。  若要了解详细信息，可以在[权限、 同意的情况下和范围](active-directory-v2-scopes.md)上查阅。

允许应用程序请求权限动态通过`scope`参数使您可以完全控制您的用户体验。  如果您愿意，您可以选择的 frontload 您同意的情况下体验并寻求一个初始的授权请求中的所有权限。  或者，如果您的应用程序需要大量的权限，您可以选择这些权限从用户收集以增量方式，因为他们都要随着时间的推移使用您的应用程序的某些功能。

## <a name="well-known-scopes"></a>已知的作用域

#### <a name="offline-access"></a>脱机访问
v2.0 终结点可能需要使用一个新的已知权限的应用程序-`offline_access`范围。  所有应用程序将需要请求该权限，如果他们需要长时间的时间，即使该用户可能没有积极地使用应用程序访问资源的用户的名义运行。  `offline_access`范围将向用户显示为同意对话中"访问您的数据脱机"，该用户必须同意。  请求`offline_access`权限将使您的 web 应用程序从 v2.0 终结点接收 OAuth 2.0 refresh_tokens。  Refresh_tokens 寿命较长，并可进行交换新 OAuth 2.0 access_tokens 为长时间的访问。  

如果您的应用程序不会请求`offline_access`的范围内，它将不会收到 refresh_tokens。  这意味着，当您兑换[OAuth 2.0 授权代码流](active-directory-v2-protocols.md#oauth2-authorization-code-flow)中的授权，您将只能收到回从 access_token`/token`终结点。  该 access_token 的时间 （通常是一个小时），短时间内将保持有效，但最终会到期。  此时，时间点，您的应用程序需要将用户重定向回`/authorize`检索新授权的终结点。  在此重定向用户可能会或可能不需要重新输入其凭据或重新同意访问权限，具体情况取决于应用程序的类型。

若要了解更多有关 OAuth 2.0、 refresh_tokens 和 access_tokens，检查出[2.0 版协议参考](active-directory-v2-protocols.md)。

#### <a name="openid-profile--email"></a>OpenID、 配置文件和电子邮件

在原始的 Azure Active Directory 服务中，最基本连接 OpenID 登录流程可提供大量的信息中得到的 id_token 的用户。  在 id_token 的声明可以包含用户的名称、 首选的用户名、 电子邮件地址、 对象 ID 和更多。

我们现在限制信息的`openid`范围提供了应用程序的访问。  Openid' 范围将仅允许您的应用程序进行签名，用户和接收用户的应用程序特定的标识符。  如果您想要获取关于您的应用程序中的用户个人身份信息 (PII)，您的应用程序将需要从用户请求额外的权限。  我们正在推出两种新的作用域-- `email` ，`profile`范围 – 允许您这样做。

`email`范围是非常简单 — 它允许应用程序访问该用户的主电子邮件地址通过`email`id_token 中声称。  `profile`范围提供了所有其他基本用户信息 – 其名称、 首选的用户名，您的应用程序访问对象 ID，等等。

这使您可以以最少披露的方式您的应用程序的代码--只可以要求用户输入相关的一套应用程序执行其任务所需的信息。  有关这些作用域的详细信息，请参阅[v2.0 范围引用](active-directory-v2-scopes.md)。 

## <a name="token-claims"></a>令牌的索赔

V2.0 终结点所颁发的令牌中的声明将不能与通过上市所颁发的令牌 Azure 广告的终结点的应用程序迁移到新的服务不应假设特定索赔将存在于 id_tokens 或 access_tokens。   V2.0 终结点所颁发的令牌符合 OAuth 2.0 和 OpenID 连接的技术指标，但可能遵循不同的语义比上市 Azure 的广告服务。

要了解有关特定发出在 v2.0 令牌中的声明，请参见[v2.0 令牌引用](active-directory-v2-tokens.md)。

## <a name="limitations"></a>限制
有几个需要注意的使用 v2.0 点时的限制。  请参阅[v2.0 限制文档](active-directory-v2-limitations.md)以查看是否任何这些限制适用于您的特定情况。
