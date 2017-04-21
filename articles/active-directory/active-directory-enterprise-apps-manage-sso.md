<properties
    pageTitle="单在 Azure Active Directory 预览中的企业应用程序的登录管理 |Microsoft Azure"
    description="了解如何使用 Azure Active Directory 的企业应用程序的管理上的单一登录"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>管理企业应用程序中新的 Azure 门户的单一登录的预览︰

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-enterprise-apps-manage-sso.md)
- [Azure 的传统门户网站](active-directory-sso-integrate-saas-apps.md)

本文介绍如何使用[Azure 门户](https://portal.azure.com)管理单一登录应用程序设置，尤其是[Azure 活动目录 (AD Azure) 应用程序库](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)中添加了。 为单一登录 Azure AD 管理经验正在公共预览，和这篇文章描述的新功能，以及将只在预览期间的一些临时限制。 [在预览中是什么？](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>在新的门户中查找您的应用程序

9 月 2016 年所有应用程序已配置为单一登录在目录中，通过使用[Azure Active Directory 应用库](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)内[Azure 的传统门户网站](https://manage.windowsazure.com)目录管理员、 可现在查看和管理 Azure 门户中。

Azure 门户，[门户](https://portal.azure.com)中**其他服务**列表中找不到链接的**企业应用程序**部分中可以找到这些应用程序。 企业应用程序是应用程序已部署并正在由您的组织内的用户使用。

![刀片式服务器的企业应用程序][1]

选择以查看列表中的所有已配置，包括从库已添加的应用程序的应用程序的**所有应用程序**。 选择一个应用程序加载资源刀片式服务器为该应用程序，该应用程序，即可查看报告，并可管理的各种设置。

若要管理单一登录设置，选择**单一登录**。

![应用程序资源刀片式服务器][2]


##<a name="single-sign-on-modes"></a>单一登录模式

**单一登录**刀片式服务器开头的**模式**菜单，它允许单一登录模式进行配置。 可用的选项包括︰

* **基于 SAML 的登录**-该选项时可用的应用与 Azure Active Directory 使用 SAML 2.0 协议支持全联盟单一登录。

* **基于密码的登录**-该选项时可用 Azure 的广告支持密码表单填写此应用程序。

* **链接登录**-以前称为"现有单一登录"，该选项允许管理员在其用户的 Azure 广告接入面板或 Office 365 提供应用程序启动器中放置到此应用程序的链接。

有关这些模式的详细信息，请参阅[如何 does 单一登录使用 Azure Active Directory 工作](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)。


##<a name="saml-based-sign-on"></a>基于 SAML 的登录

**基于 SAML 的登录**选项显示刀片，分为四个部分︰

###<a name="domains-and-urls"></a>域和 Url
这是关于应用程序的域和 Url 的所有详细信息到 Azure 的广告目录的添加位置。 可以通过选择**显示高级 URL 设置**复选框查看所有可选的输入而直接在屏幕上，显示使单个登录工作应用程序所需的所有输入。 支持输入的完整列表包括︰

* **符号在 URL** – 用户转到登录到该应用程序的位置。 配置应用程序以执行服务提供程序启动单一登录，然后当用户导航到此 URL，服务提供商有必要重定向到 Azure 的广告进行身份验证和登录用户。 如果此字段已填充，然后 Azure 广告将使用此 URL 启动该应用程序从 Office 365 和 Azure AD 盖板。 如果省略此字段，然后 Azure 广告而执行身份标识提供程序的初始的登录应用程序何时启动 Office 365，Azure AD 后盖，或从 Azure 广告单一登录 URL。

* **标识符**-此 URI 应该唯一地标识应用程序的单一登录的配置。 这是 Azure 广告发送回应用程序作为群体参数的 SAML 令牌的值，应用程序需要对其进行验证。 此值显示为应用程序提供任何 SAML 元数据中的实体 ID。

* **回复 URL** -回复 URL 位于应用程序希望接收的 SAML 令牌。 这也称为断言使用者服务 (ACS) 的 URL。 这些输入后，单击下一步，前进到下一屏。 此屏幕提供有关哪些需要配置应用程序端，以使其可以接受 SAML 令牌从 Azure 的广告信息。

* **中继状态**-中继状态是可选参数可以帮助判断应用程序完成身份验证后将用户重定向的位置。 但是，一些应用程序以不同的方式使用此字段值通常是应用程序，一个有效的 URL （请参阅文档以了解详细信息的应用程序的单一登录）。 设置中继状态的能力是一项新功能所独有的 Azure 新门户。

###<a name="user-attributes"></a>用户属性
这是管理员可以查看和编辑属性发送的 SAML 令牌 Azure AD 颁发给应用程序每次用户登录位置。

对于第一个预览版本中，支持的只有可编辑属性是**用户标识符**属性。 此属性的值是唯一地标识应用程序中的每个用户的 Azure AD 中的字段。 例如，如果应用程序已部署用户名和唯一标识符作为使用的"电子邮件地址"，然后值将被设置为"user.mail"字段 Azure AD 中。

编辑其他属性将在后续的预览支持。

###<a name="saml-signing-certificate"></a>SAML 签名证书
此部分说明了 Azure 广告使用对 SAML 令牌颁发给每次该用户进行身份验证的应用程序进行签名的证书的详细信息。 它允许当前证书要检查的属性包括到期日期。

滚动更新证书的能力和管理额外的证书将在后续的预览版本中支持选项。 请注意，在完整的证书的管理，仍然可以[Azure 的传统门户网站](active-directory-sso-certs.md)中执行。

###<a name="application-configuration"></a>应用程序配置
最后一节提供的文档和/或配置应用程序本身使用 Azure Active Directory 为身份标识提供程序所需的控件。

**配置应用程序**弹出菜单中提供了新的简明而嵌入指令用于配置应用程序。 这是唯一的 Azure 新门户的另一个新功能。

> [AZURE.NOTE] 嵌入文档的完整示例，请参见 Salesforce.com 应用程序。 正在预览期间不断添加对其他应用程序的文档。

![嵌入的文档][3]

##<a name="password-based-sign-on"></a>基于密码的登录
如果应用程序支持，选择的基于密码的 SSO 模式并选择**保存**即时对其进行配置以执行基于密码的 SSO。 有关部署基于密码的 SSO 的详细信息，请参阅[如何 does 单一登录使用 Azure Active Directory 工作](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)。

![基于密码的登录][4]


##<a name="linked-sign-on"></a>链接的登录
如果应用程序支持，选择链接的 SSO 模式允许您输入所需的 Azure 广告接入面板或 Office 365 提供当用户单击此应用程序重定向到的 URL。 有关链接的 SSO （以前称为"现有 SSO"） 的详细信息，请参阅[如何 does 单一登录使用 Azure Active Directory 工作](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)。

![链接登录][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
