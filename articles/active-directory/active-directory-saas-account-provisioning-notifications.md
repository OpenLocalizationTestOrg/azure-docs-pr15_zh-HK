<properties
    pageTitle="帐户设置通知 |Microsoft Azure"
    description="了解如何确保向用户提供有关的问题，需要引起您的注意来启用帐户调配通知通知您。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>帐户设置通知

与用户供应，您可以自动化管理第三方的 SaaS 应用程序中的用户的过程。 <br>
虽然这是一个自动化的过程，此过程与您交互时常是必需的。 <br>
此情况，例如，当密码的帐户已配置为交换数据与第三方的 SaaS 应用程序已过期。 

通过启用帐户调配的通知，您可以确保通知到用户供应有关的问题，需要引起您的注意。

激活或停用帐户调配通知为您的用户配置第三方 SaaS 应用程序的一部分。

![用户资源调配][1] 



要激活帐户设置通知，请选中**确认**对话框页的相关复选框，然后键入收件人的电子邮件别名。

![帐户设置通知][2]
 


您可以输入通讯组列表作为收件人，则为但是，值得注意的通知电子邮件包含指向只是 Azure AD 管理员可以访问的报告。

如果您有帐户调配启用通知，您将收到电子邮件为用户提供相关的关键问题。 但是，为了避免电子邮件重载，将只收到一个通知电子邮件，每天每个 SaaS 应用程序已启用的通知电子邮件。


##<a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [自动化用户资源调配/撤消对 SaaS 应用程序](active-directory-saas-app-provisioning.md)
- [为用户提供自定义属性映射](active-directory-saas-customizing-attribute-mappings.md)
- [编写属性映射的表达式](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [作用域为用户提供的筛选器](active-directory-saas-scoping-filters.md)
- [如何使用 SCIM 以使用户和应用程序从 Azure 的 Active Directory 组的自动资源调配](active-directory-scim-provisioning.md)
- [有关如何集成 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png