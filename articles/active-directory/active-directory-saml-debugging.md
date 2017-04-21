<properties 
    pageTitle="如何调试基于 SAML 的单一登录到 Azure Active Directory 中的应用程序 |Microsoft Azure" 
    description="了解如何调试基于 SAML 的单一登录与 Azure Active Directory 中的应用 " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>如何调试基于 SAML 的单一登录与 Azure Active Directory 中的应用

调试时基于 SAML 的应用程序集成，它很有帮助，若要使用类[Fiddler](http://www.telerik.com/fiddler)工具查看 SAML 请求，SAML 的响应，并颁发给应用程序的实际 SAML 令牌。 通过检查 SAML 令牌，您可以确保，所有必需的属性、 SAML 主题和颁发者 URI 中的用户名即将像预期的那样。

![][1]

从包含 SAML 令牌的 Azure 广告响应通常是 HTTP 302 重定向从 https://login.windows.net，并且将被发送到应用程序配置的**回复 URL**之后发生的。 
 
您可以通过选择以下行，然后选择查看 SAML 令牌**检查 > WebForms**在右侧面板中的选项卡。 从那里，用鼠标右键单击**SAMLResponse**值并选择**发送到 TextWizard**。 从**变换**菜单进行解码该标记，并查看其内容，然后选择**从 Base64** 。
 
**注意**︰ 若要查看此 HTTP 请求的内容，Fiddler 可能会提示您配置 HTTPS 通信，您需要做的解密。

## <a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [在 Azure Active Directory 应用库中的应用程序的单一登录配置](active-directory-saas-custom-apps.md)
- [如何自定义声明发出 SAML 令牌用于预先集成的应用程序](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
