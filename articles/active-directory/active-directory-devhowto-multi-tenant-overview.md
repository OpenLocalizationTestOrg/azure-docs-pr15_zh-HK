<properties
   pageTitle="如何构建的应用程序可以提供任何 Azure Active Directory 用户登录 |Microsoft Azure"
   description="逐步构建应用程序的说明可以在用户从任何 Azure Active Directory 租户，也称为多租户应用程序签名。"
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>如何登录任何 Azure 活动目录 (AD) 用户使用的多租户应用程序模式
如果用作许多组织的服务应用程序提供了一种软件，您可以配置应用程序接受号接从任何 Azure AD 租户。  在 Azure 的广告，这就称为使您应用程序的多租户。  在任何 Azure AD 租户的用户将能够登录到应用程序后同意您应用程序中使用他们的帐户。  

如果您有现有的应用程序都有其自己的帐户系统，或支持从其他云提供商登录其他种类，所以只要注册您的应用程序、 通过 OAuth2、 OpenID 连接，或者 SAML，代码中添加符号和登录与 Microsoft 按钮置于您的应用程序添加任何租户在 Azure 广告符号。 请单击下面的按钮以了解更多关于标记您的应用程序。

[![登录按钮][AAD — 登录]][AAD-App-Branding]


本文假定您已经熟悉构建 Azure 的广告单租户应用程序。  如果你不，头部备份到[开发人员指南主页][AAD-Dev-Guide]和试一下我们的快速启动 ！

有四个简单的步骤来将您的应用程序转换到 Azure 广告多租户应用程序︰

1.  更新您的应用程序注册为多租户
2.  更新代码以将请求发送到 /common 终结点 
3.  更新代码以处理多个颁发者值
4.  了解用户和管理员同意的情况下，并进行适当的代码更改

让我们看一下详细的每一步。 此外直接跳转到[此列表中的多租户样本][AAD-Samples-MT]。

## <a name="update-registration-to-be-multi-tenant"></a>更新注册为多租户
默认情况下，web 应用程序/API 登记在 Azure 的广告是一个租户。  您也可以在[Azure 的传统门户网站]应用程序注册的配置页上查找"应用程序是多租户"开关使您的注册多租户[AZURE-classic-portal]并将其设置为"是"。

注意︰ 应用程序可以进行多租户前，Azure 广告要求是全局唯一的应用程序的应用程序 ID URI。 应用程序 ID URI 是在协议消息中标识应用程序的方法之一。  单个客户端应用程序中，是在该组织中是唯一的应用程序 ID uri 足够。  对于多租户应用程序，它必须是全局唯一以便 Azure 广告可以跨所有承租人找到应用程序。  通过要求要有一个主机名相匹配的 Azure AD 租户已验证的域的应用程序 ID URI 强制具有全局唯一性。  例如，如果您组织的名称是 contoso.onmicrosoft.com，然后是有效应用程序 ID URI 将是`https://contoso.onmicrosoft.com/myapp`。  如果您的组织必须验证的域的`contoso.com`，也是有效的应用程序 ID URI `https://contoso.com/myapp`。  如果应用程序 ID URI 并不遵循此模式，将应用程序设置为多租户将失败。

本机客户端注册是默认情况下的多租户。  您不需要执行任何操作来使本机客户端应用程序注册多租户。

## <a name="update-your-code-to-send-requests-to-common"></a>更新代码以将请求发送到 /common
在单个租户应用程序，登录请求被发送到端点中的租户的符号。   例如，对于 contoso.onmicrosoft.com 终结点将是︰

    https://login.microsoftonline.com/contoso.onmicrosoft.com

对承租人的终结点发送的请求可以登录用户 （或客人） 在该组织与该组织中的应用。  多租户应用程序，该应用程序并不知道提前用户是从哪个租户因此不能将请求发送给租户的终结点。  相反，请求被发送到所有 Azure AD 承租人通过使用多路复用的终结点︰

    https://login.microsoftonline.com/common

当 Azure 广告接收 /common 上的请求终结点，则登录用户并因此发现用户是从哪个组织。  / 公共端点适用于所有由 Azure 的广告支持的身份验证协议︰ OpenID 连接、 OAuth 2.0、 SAML 2.0 和 WS 联合身份验证。

登录到该应用程序的响应包含一个标记，该标记表示的用户。  在令牌颁发者值告诉应用程序用户是从哪个组织。  当从 /common 返回响应端点，令牌中的颁发者值将对应于用户的租户。  值得注意的是 /common 终结点不是租户并不是某个颁发者，则只需多路复用器。  在使用 /common 时，您的应用程序来验证令牌中的逻辑需要更新，以考虑到这种。 

如前所述，多租户应用程序还应该为追随品牌准则 Azure 广告应用程序的用户提供一致的登录体验。 请单击下面的按钮以了解更多关于标记您的应用程序。

[![登录按钮][AAD — 登录]][AAD-App-Branding]

让我们看看 /common 的使用终结点和详细代码实现。

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>更新代码以处理多个颁发者值
Web 应用程序和 web Api 接收和验证令牌从 Azure 的广告。  

> [AZURE.NOTE] 虽然本机客户端应用程序请求并接收从 Azure AD 标记，它们这样做是为了将其发送到 Api，经过验证。  本机应用程序不验证令牌，并必须将其视为不透明。

让我们来看从 Azure AD 应用程序如何验证令牌在接收。  单个客户端应用程序通常需要与终结点值︰

    https://login.microsoftonline.com/contoso.onmicrosoft.com

并使用它来构建一个元数据 URL （在此例中，OpenID 连接） 类似︰

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

若要下载两个关键部分的信息用于验证令牌︰ 租户的签名密钥和颁发者值。  每个 Azure AD 租户的唯一颁发者值为窗体︰

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

其中的 GUID 值，是租户的租户 ID 的重命名安全版本。  如果单击的元数据链接上面的`contoso.onmicrosoft.com`，您可以查看文档中的该颁发者值。

当单个租户应用程序验证令牌时，它检查对照从元数据文档的签名密钥令牌签名并确保令牌中的颁发者值匹配元数据文档中找到的一个。

/Common 由于终结点并不对应于一个租户和时检查的元数据中的颁发者值/常见有模板的 URL，而不是实际值的不是某个颁发者︰

    https://sts.windows.net/{tenantid}/

因此，多租户应用程序无法验证标记，只是通过相匹配的元数据中的颁发者`issuer`在标记中的值。  多租户应用程序逻辑，以确定哪些颁发者值有效，哪些是不需要根据租户的颁发者值 ID 部分。  

例如，如果多租户应用程序只允许从特定租户注册使用其服务的登录，然后它必须检查颁发者值或`tid`声明中的标记，以确保该租户在其列表中的订阅服务器的值。  如果只是多租户应用程序处理个人而不作出任何访问基于承租人，则它可以忽略的颁发者值完全。

在本文末尾[相关内容](#related-content)部分中可以找到多租户样本，在禁用颁发者验证启用登录任何 Azure AD 租户。

现在让我们看看多租户应用程序登录的用户的用户体验。

## <a name="understanding-user-and-admin-consent"></a>了解用户和管理员同意的情况下
用户可以登录到 Azure 广告中的应用程序，应用程序必须在用户的租户表示。  这使组织能够执行某些操作，如将唯一策略应用时其租户的用户登录到该应用程序。  单个客户端应用程序注册方法十分简单;它是一个在[Azure 的传统门户网站]注册应用程序时将发生[AZURE-classic-portal]。

对于多租户应用程序，该应用程序的初始登记居住在由开发人员使用 Azure AD 租户。  当来自不同租户的用户登录到应用程序的第一次时，Azure 的广告要求他们同意由应用程序请求的权限。  如果他们同意，名为*主体的服务*应用程序的表示在用户的租户，则会创建和登录才能继续。 也记录到应用程序的用户同意的目录中创建一个委派。 请参见[应用程序对象和服务主体对象][AAD-App-SP-Objects]应用程序的应用程序和 ServicePrincipal 对象和它们之间的相互有关的详细信息。

![同意一层应用程序][Consent-Single-Tier] 

这种同意的情况下体验会影响应用程序请求的权限。  Azure 的广告支持两种类型的权限，只有应用程序和委派︰

- 委派的权限授予应用程序能够充当用户登录用户子集的事情可以做。  例如，您可以授予应用程序读取登录的用户的日历的委派的权限。
- 仅限应用程序的权限直接授予应用程序的标识。  例如，可以授予应用程序仅应用程序有权读取的组织中的用户列表并将能够执行此操作而不考虑登录到应用程序的人员。

而其他人则要求租户管理员同意的情况下，可以通过普通用户中，同意某些权限。 

### <a name="admin-consent"></a>管理员同意的情况下
只有应用程序权限总是要求租户管理员的许可。  如果您的应用程序请求仅限应用程序的权限，而普通用户尝试登录到应用程序，应用程序将得到错误消息，用户不能够同意。

某些委派的权限也要求租户管理员的许可。  例如，写回 Azure 广告作为登录的用户的能力要求租户管理员的许可。  仅限应用程序的权限，如普通用户尝试登录到应用程序请求委派的权限需要管理员同意的情况下，如果您的应用程序将收到错误信息。  需要权限管理员同意的情况下由发布资源，开发人员，并且可以为资源的文档中找到。  本文[相关内容](#related-content)一节中将描述 Azure 广告图形 API 和 Microsoft Graph API 的可用权限的主题的链接。

如果您的应用程序使用权限要求管理员同意的情况下，您需要在应用程序中的按钮或链接该管理员可以在其中启动操作如有势。  请求应用程序发送此操作是一个常规 OAuth2/OpenID 连接授权请求，但还包括`prompt=admin_consent`查询字符串参数。  一旦管理有异议，在客户的组织中创建服务主体，不需要后续登录请求`prompt=admin_consent`参数。   管理员已决定请求的权限是可以接受的因为组织中的任何其他用户将提示不您同意今后。

`prompt=admin_consent`参数还可由应用程序请求的权限，不需要管理员同意的情况下，但想要给在租户管理"注册"体验应用程序一次，并且没有其他用户提示同意从那个时间点上。

如果应用程序需要管理员同意的情况下，并且管理员登录到应用程序，但`prompt=admin_consent`未发送参数，管理员将能够成功地同意在该应用程序，但他们将只会同意为他们的用户帐户。  普通用户将仍然不能登录和应用程序表示同意。  这很有用，如果您想要授予组织管理员探索才允许其他用户访问您的应用程序的能力。

组织管理员可以禁用常规用户应用程序表示同意的能力。  如果禁用此功能，则管理员同意的情况下始终是必需的应用程序设置中的租户。  如果您想要使用常规用户同意禁用测试您的应用程序，您可以找到配置开关 Azure AD 租户在[Azure 的传统门户网站]的配置一节[AZURE-classic-portal]。

> [AZURE.NOTE] 某些应用程序想要体验其中普通用户能够同意最初，并且以后应用程序，则可能会要求管理员同意的管理员联系，请求权限。  有是没有办法在 Azure 广告今天完成此单个应用程序注册。  即将推出的 Azure AD v2 终结点可在运行时，请求权限的应用程序而不是在注册时，将使这种情况。  有关详细信息，请参阅[Azure 应用程序模型 AD v2 开发人员指南 》][AAD-V2-Dev-Guide]。

### <a name="consent-and-multi-tier-applications"></a>同意的情况下和多层应用程序
您的应用程序可能有多个层，每个表示自己注册 Azure AD 中。  例如，调用 web API 的本机应用程序或 web 应用程序调用 web API。  在这两种情况中，客户端 （本机应用程序或 web 应用程序） 请求调用资源 (web API) 的权限。  客户端到客户的租户会成功许可，它的请求的权限的所有资源必须已经都存在于客户的租户中。  如果不满足此条件，Azure 广告将返回错误，则必须首先添加资源。

如果逻辑应用程序包含两个或多个应用程序登记，例如一个单独的客户端和资源，可以出现问题。  如何获得资源为客户租户第一？  Azure 广告涵盖这种情况下启用客户端和资源被许可在单个步骤中，用户在其中看到请求的客户端和同意的情况下页上的资源的权限的总和。  若要启用此行为，资源的应用程序注册必须包含与客户端的应用程序 ID`knownClientApplications`在其应用程序清单中。  例如︰

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

此属性可更新资源[的应用程序的清单]通过[AAD-App-Manifest]，以及多层本机客户端调用 web API 示例在本文末尾[相关内容](#related-content)部分中所示。 下图概述了针对多层应用程序的同意︰

![同意在多层已知客户端应用程序][Consent-Multi-Tier-Known-Client] 

在不同的租户中注册应用程序的不同层情况类似的案例。  例如，请考虑创建 Office 365 Exchange 的联机 API 调用本机客户端应用程序的情况。  若要开发的本机应用程序中，和更高版本以本机应用程序运行在客户的租户，Exchange 联机服务主体必须存在。  在这种情况下客户将不得不购买在线交换主体在其组织中创建服务。  由 Microsoft 以外的组织建立的 API，如果需要为他们的客户能够同意他们到客户租户，例如，驱动器同意使用本文中描述的机制的 web 页的应用程序提供一种 API 的开发人员。  在组织中创建服务主体后，本机应用程序可以获取的 API 的标记。

下图概括的同意的情况下在不同租户中注册一个多层应用程序︰

![同意在多当事方的多层应用程序][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>正在撤消同意的情况下
用户和管理员可以撤销同意随时在您的应用程序︰

- 用户取消访问单个应用程序从其[访问的控制面板应用程序]中删除[AAD-Access-Panel]列表。
- 管理员撤销访问权限的应用程序从 Azure 广告使用[Azure 的传统门户]的 Azure AD 管理部分中删除[AZURE-classic-portal]。

如果管理员同意交由组织中的所有用户的应用程序，用户不能分别撤销访问权限。  只有管理员可以撤销访问权限，并只为整个应用程序。

### <a name="consent-and-protocol-support"></a>同意的情况下和协议支持
同意在 Azure AD OAuth，OpenID 连接，通过支持 WS 联合身份验证和 SAML 协议。  SAML 和 WS 联合身份验证协议不支持`prompt=admin_consent`参数，以便管理员同意的情况下，才可以通过 OAuth 和 OpenID 连接。

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>多租户应用程序和缓存的访问令牌
多租户应用程序还可以调用 Azure 广告由受保护的 Api 的访问令牌。  一个常见的错误时多租户应用程序中使用活动目录身份验证库 (ADAL) 是最初请求的标记使用 /common，用户会收到回复，然后请求该用户还在使用 /common 的后续标记。  因为从 Azure 广告响应来自于租户不/高速缓存的常见，ADAL 为来自组织的标记。 后续调用 /common 来获得用户的访问令牌未命中缓存项，并提示用户重新登录。  若要避免丢失缓存，确保为已登录的用户的后续调用对租户的终结点。

## <a name="related-content"></a>相关的内容

- [多租户应用程序示例][AAD-Samples-MT]
- [商标使用准则的应用程序][AAD-App-Branding]
- [Azure 广告开发人员指南 》][AAD-Dev-Guide]
- [应用程序对象和服务主体对象][AAD-App-SP-Objects]
- [与 Azure Active Directory 集成应用程序][AAD-Integrating-Apps]
- [同意框架概述][AAD-Consent-Overview]
- [Microsoft Graph API 权限范围][MSFT-Graph-AAD]
- [Azure 广告图形 API 权限范围][AAD-Graph-Perm-Scopes]

请使用下面的 Disqus 评论部分提供反馈，帮助我们改进和形状我们的内容。

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














