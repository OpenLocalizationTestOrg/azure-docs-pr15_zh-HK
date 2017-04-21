<properties
   pageTitle="列出在 Azure Active Directory 应用程序库应用程序"
   description="如何列出在 Azure Active Directory 库支持单一登录的应用程序 |Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>列出在 Azure Active Directory 应用程序库应用程序

若要列出在[Azure AD 库](https://azure.microsoft.com/marketplace/active-directory/all/)单一登录使用 Azure 活动目录支持的应用程序，该应用程序首先需要实现以下集成模式之一︰

* **OpenID 连接**-Azure 用于身份验证和配置 Azure AD 同意 API 使用 OpenID 连接的广告与直接集成。 如果您刚开始集成和应用程序不支持 SAML，这是建议的模式。

* **SAML** – 应用程序已经具有配置第三方身份提供程序使用 SAML 协议的能力。

每个模式的列表要求如下。

##<a name="openid-connect-integration"></a>OpenID 连接集成

若要将与 Azure 广告，[开发人员说明](active-directory-authentication-scenarios.md)集成应用程序。 然后完成下面的问题，并将发送到waadpartners@microsoft.com。

* 提供您 Azure 的广告团队可用于测试的集成的应用程序测试租户或帐户的凭据。  

* 提供有关如何登录并将 Azure AD 实例连接到您的应用程序使用[Azure AD 同意框架](active-directory-integrating-applications.md#overview-of-the-consent-framework)Azure 的广告团队说明。 

* 提供所需的 Azure 的广告团队与您的应用程序的单一登录测试任何进一步说明。 

* 提供了以下信息︰

> 公司名称︰
> 
> 公司网站︰
> 
> 应用程序名称︰
> 
> 应用程序说明 （限制为 256 个字符）︰
> 
> （信息性） 的应用程序网站︰
> 
> 应用程序技术支持网站或联系信息︰
> 
> 该应用程序，应用程序详情，请访问 https://manage.windowsazure.com 中所示的客户机 ID:
> 
> 客户在哪里注册的应用程序注册 URL 和/或购买应用程序︰
> 
> 选择您的应用程序 （有关可用类别，请参阅 Azure 活动目录市场） 应列出在下的三个类别︰
> 
> 附加应用程序小图标 （PNG 文件，45px 45px，纯背景色的）︰
> 
> 附加应用程序大图标 （PNG 文件，215px 215px，纯背景色的）︰
> 
> 附加应用程序徽标 （PNG 文件，150px 的 122px，透明的背景色）︰

##<a name="saml-integration"></a>SAML 集成

任何支持 SAML 2.0 的应用程序可以直接使用 Azure AD 租户使用[这些说明来添加自定义应用程序](active-directory-saas-custom-apps.md)集成。 在测试您的应用程序集成使用 Azure 的广告，下面将信息发送到<waadpartners@microsoft.com>。

* 提供您 Azure 的广告团队可用于测试的集成的应用程序测试租户或帐户的凭据。  

* 您的应用程序作为描述[这里](active-directory-saas-custom-apps.md)提供 SAML 登录 URL、 颁发者 URL (实体 ID) 和回复 URL （断言使用者服务） 值。 如果您通常作为 SAML 的元数据文件的一部分提供了这些值，然后请发送，以及。

* 提供如何将 Azure AD 配置为使用 SAML 2.0 应用程序中标识提供程序的简短说明。 如果您的应用程序支持配置为标识提供程序通过自助服务管理门户的 Azure AD，则请确保上面提供的凭据包括能够对此进行设置。

* 提供了以下信息︰

> 公司名称︰
> 
> 公司网站︰
> 
> 应用程序名称︰
> 
> 应用程序说明 （限制为 256 个字符）︰
> 
> （信息性） 的应用程序网站︰
> 
> 应用程序技术支持网站或联系信息︰
> 
> 客户在哪里注册的应用程序注册 URL 和/或购买应用程序︰
> 
> 选择最多三个类别的应用程序 （有关可用类别，请参阅[Azure 活动目录市场](https://azure.microsoft.com/marketplace/active-directory/)） 出）︰
> 
> 附加应用程序小图标 （PNG 文件，45px 45px，纯背景色的）︰
> 
> 附加应用程序大图标 （PNG 文件，215px 215px，纯背景色的）︰
> 
> 附加应用程序徽标 （PNG 文件，150px 的 122px，透明的背景色）︰
