<properties 
    pageTitle="如何授权开发人员帐户在 Azure API 管理使用 OAuth 2.0" 
    description="了解如何在 API 管理使用 OAuth 2.0 的用户授权。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>如何授权开发人员帐户在 Azure API 管理使用 OAuth 2.0

许多 Api 支持[OAuth 2.0](http://oauth.net/2/)安全 API，并确保只有有效用户才能访问，并且它们只能访问到他们所享有的资源。 为了使用这种 Api 使用 Azure API 管理交互式开发人员控制台，该服务允许您将使用您 OAuth 2.0 启用的 API 服务实例配置。

## <a name="prerequisites"></a>系统必备组件

本指南说明如何将使用 OAuth 2.0 授权开发人员帐户管理 API 服务实例配置为但不显示如何配置 OAuth 2.0 提供程序。 不同，每个 OAuth 2.0 提供程序的配置有虽然步骤都是相似的而且所必需的部件在 API 管理服务实例配置 OAuth 2.0 中使用的信息相同。 本主题介绍使用 Azure Active Directory 作为 OAuth 2.0 提供程序的示例。

>[AZURE.NOTE] OAuth 2.0 使用 Azure Active Directory 配置的详细信息，请参阅[WebApp-GraphAPI-最低][]示例。

## <a name="step1"></a>在 API 管理配置 OAuth 2.0 授权服务器

若要开始，请单击 Azure 经典门户 API 管理服务中的**管理**。 这将您带到管理 API 发布门户。

![出版商门户][api-management-management-console]

>[AZURE.NOTE] 如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

从左侧的**API 管理**菜单中单击**安全**， **OAuth 2.0**中，请单击，然后单击**添加授权服务器**。

![OAuth 2.0][api-management-oauth2]

单击后**添加授权服务器**，将显示新的授权服务器窗体。

![新的服务器][api-management-oauth2-server-1]

在**名称**和**说明**字段中输入一个名称和可选说明。 

>[AZURE.NOTE] 这些字段用于标识当前 API 管理服务实例中的 OAuth 2.0 授权服务器和它们的值不是来自 OAuth 2.0 服务器。

输入**客户端注册页的 URL**。 此页是在其中用户可以创建和管理他们的帐户，并使用 OAuth 2.0 提供程序而异。 **客户端注册页 URL**指向的页面的用户可以使用它来创建和配置 OAuth 2.0 支持的提供程序的帐户的用户管理他们自己的帐户。 有些组织没有配置或使用此功能，即使 OAuth 2.0 提供程序都支持它。 如果 OAuth 2.0 提供程序没有配置帐户的用户管理，如输入占位符 URL 这里如您的公司的 URL 或 URL `https://placeholder.contoso.com`。

窗体的下一节包含**授权的代码授予类型**、**授权端点 URL**和**授权请求方法**设置。

![新的服务器][api-management-oauth2-server-2]

通过检查所具有的类型来指定**授权的代码授予类型**。 默认情况下指定**授权代码**。

输入**授权端点 URL**。 对于 Azure Active Directory，此 URL 将类似于下面的 URL，其中`<client_id>`将被替换为标识到 OAuth 2.0 服务器应用程序的客户端 id。

    https://login.windows.net/<client_id>/oauth2/authorize

**授权请求方法**指定授权请求发送到 OAuth 2.0 服务器的方式。 默认情况下选中**获得**。

下一节是指定的**令牌端点 URL**，**客户端身份验证方法**、**访问令牌发送方法**，和**默认作用域**。

![新的服务器][api-management-oauth2-server-3]

对于 Azure 活动目录 OAuth 2.0 服务器，**标记端点 URL**将具有以下格式，其中`<APPID>`格式的`yourapp.onmicrosoft.com`。

    https://login.windows.net/<APPID>/oauth2/token

**客户端身份验证方法**的默认设置是**基本**，和**发送方法的访问令牌**是**授权标头**。 本节的窗体，以及**默认作用域**上配置这些值。

**客户端凭据**部分包含**客户机 ID**和**客户端密钥**，OAuth 2.0 服务器的创建和配置过程中获取。 一旦指定了**客户机 ID**和**客户端密钥**，会生成**redirect_uri**的**授权码**。 此 URI 使用 OAuth 2.0 服务器配置中配置的回复 URL。

![新的服务器][api-management-oauth2-server-4]

如果**授权代码授予类型**设置为**资源所有者密码**，**资源所有者密码凭据**部分用于指定这些凭据;否则您可以将其保留为空。

![新的服务器][api-management-oauth2-server-5]

一旦完成该窗体，请单击**保存**以保存 API 管理 OAuth 2.0 授权服务器配置。 保存服务器配置后，您可以配置 Api 使用此配置中，如在下一节中所示。

## <a name="step2"> </a>API 配置为使用 OAuth 2.0 用户授权

单击左侧**API 管理**菜单上的**Api**所需的 API 的名称单击**安全**和，然后选中复选框， **OAuth**2.0。

![用户授权][api-management-user-authorization]

从下拉列表中，选择所需的**授权服务器**并单击**保存**。

![用户授权][api-management-user-authorization-save]

## <a name="step3"></a>在开发人员门户中测试 OAuth 2.0 用户授权

OAuth 2.0 授权服务器配置并配置您的 API 使用该服务器后，您可以通过转到开发人员门户并调用 API 来测试它。  顶部右菜单中单击**开发人员门户**。

![开发人员门户][api-management-developer-portal-menu]

顶部的菜单中单击**Api**并选择**回响 API**。

![回音式 API][api-management-apis-echo-api]

>[AZURE.NOTE] 如果您有一个 API 配置或可见到您的帐户，然后单击 Api 使您直接进入该 API 的操作。

选择**获取资源**操作，单击**打开控制台**，然后从下拉列表中选择**授权码**。

![打开的控制台][api-management-open-console]

选择**授权码**时，弹出式窗口将显示 OAuth 2.0 提供程序的登录窗体。 在此示例中通过 Azure Active Directory 提供了登录窗体。

>[AZURE.NOTE] 如果您必须禁用弹出窗口将提示您通过浏览器以启用它们。 使他们后，将显示再次选择**授权码**和登录窗体。

![登录][api-management-oauth2-signin]

一旦您已登录，**请求标头**中填充`Authorization : Bearer`授权请求的标头。

![请求标头的标记][api-management-request-header-token]

此时您可以配置所需的值，对于剩余的参数，并提交申请。 

## <a name="next-steps"></a>下一步行动

有关使用 OAuth 2.0 和 API 管理的详细信息，请参阅下面的视频和相关[文章](api-management-howto-protect-backend-with-aad.md)。

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API 管理入门]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-最低]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

