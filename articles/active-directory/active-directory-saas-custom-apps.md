<properties 
    pageTitle="在 Azure Active Directory 应用库中的应用程序的单一登录配置 |Microsoft Azure" 
    description="了解如何为自助服务连接到 Azure Active Directory 使用 SAML 和基于密码的 SSO 应用程序" 
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

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>在 Azure Active Directory 应用库中的应用程序的单一登录配置

这篇文章是关于一种功能，使管理员能够配置单一登录应用程序不存在于 Active Directory 的 Azure 应用程序库*无需编写代码*。 此功能从技术预览上于 2015 年 11 月 18 日发布，并且包含在[Azure 活动目录津贴](active-directory-editions.md)。 如果您改为寻找如何与代码通过 Azure AD 集成自定义应用程序的开发人员指南，请参阅[Azure AD 身份验证方案](active-directory-authentication-scenarios.md)。

Azure Active Directory 应用库提供已知支持单一登录使用 Azure 活动目录的一种形式，如[本文](active-directory-appssoaccess-whatis.md)所述的应用程序的列表。 一旦您 （作为组织中 IT 专家或系统集成商） 找到您想要连接的应用程序，您可以开始通过遵循分步说明在 Azure 管理门户中显示以启用单一登录。

使用[Azure 活动目录特优](active-directory-editions.md)许可证的客户也获得这些附加功能︰

* 自助服务的任何应用程序都支持 SAML 2.0 身份提供程序 （SP 启动或 IdP 启动） 的集成
* 自助服务的有基于 HTML 的登录页面使用[基于密码的 SSO](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)任何 web 应用程序的集成
* 自助服务的用户资源调配 （[此处所述](active-directory-scim-provisioning.md)） 对使用 SCIM 协议的应用程序的连接
* 将链接添加到[Office 365 提供应用程序启动程序](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/)或[Azure 广告接入面板](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)的任何应用程序的能力

这可以包括不仅 SaaS 应用程序使用，但有没有尚未被上-boarded 到 Azure AD 应用程序库中，而且您的组织已经部署到服务器控制云或内部部署中的第三方 web 应用程序。

这些功能，也称为*应用程序集成模板*用于支持 SAML、 SCIM 或基于表单的身份验证，并且还包括灵活的选项和与广大应用程序的兼容性设置的应用程序提供基于标准的连接点。 

##<a name="adding-an-unlisted-application"></a>添加未列出的应用程序

要连接的应用程序使用的应用程序集成模板，Azure 管理门户使用 Azure Active Directory 管理员帐户，登录，并浏览到**Active Directory > [目录] > 应用程序**部分中，选择**添加**，然后**添加应用程序从库**。 

![][1]

在应用程序库中，您可以添加未列出的应用程序，使用**自定义**类别，在左边，或通过选择如果您所需的应用程序找不到显示在搜索结果中**列出的应用程序添加**链接。 在输入后您的应用程序的名称，您可以配置单一登录选项和行为。 

**快速提示**︰ 作为最佳实践，使用搜索功能来检查是否该应用程序已存在应用程序库中。 如果找到该应用程序，其说明中提到了"单一登录"，然后该应用程序已经支持联盟单一登录。 

![][2]

添加这样的应用程序提供了对可用的预集成的应用程序的一个非常相似的体验。 若要开始，请选择**配置单一登录**。 下一个屏幕提供以下三个选项用于配置单一登录，以下各节所述。

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure 的广告单登录

选择此选项可以配置基于 SAML 的应用程序的身份验证。 这就要求应用程序支持 SAML 2.0 中，并且您应该如何使用 SAML 功能的应用程序，然后继续收集信息。 选择**下一步**之后, 将提示您输入三个不同的 Url 对应于应用程序的 SAML 端点。 

![][4]
 
它们是︰

* **号上的 URL (SP 启动只)** – 用户转到登录到该应用程序的位置。 如果应用程序被配置为执行服务提供程序启动单一登录，然后当用户导航到此 URL，服务提供商将执行需要重定向到 Azure 的广告进行身份验证和登录的用户。 如果此字段已填充，然后 Azure 广告将使用此 URL 启动该应用程序从 Office 365 和 Azure AD 盖板。 如果此字段是 ommited，那么 Azure 广告将改为执行身份标识提供程序的初始的登录应用程序何时启动 Office 365，Azure AD 后盖，或从 Azure 广告单一登录 URL (copiable 从仪表板选项卡)。

* 正在配置**颁发者 URL**的 URL 应识别的应用程序的单一登录的颁发者。 这是 Azure 广告发送回应用程序作为**群体**参数的 SAML 令牌的值，应用程序需要对其进行验证。 此值显示为应用程序提供任何 SAML 元数据中的**实体标识**。 检查应用程序的 SAML 有关内容的详细信息的文档是实体 ID 或用户值。 下面是读者 URL SAML 令牌返回到应用程序中的显示方式的示例︰

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **回复 URL** -回复 URL 位于应用程序希望接收的 SAML 令牌。 这也称为**断言使用者服务 (ACS) 的 URL**。 检查应用程序的 SAML 有关什么是其 SAML 令牌回复或 ACS URL 的详细信息的文档。
 这些输入后，单击**下一**步，前进到下一屏。 此屏幕提供有关哪些需要配置应用程序端，以使其可以接受 SAML 令牌从 Azure 的广告信息。 

![][5]

哪些值是所需将应用程序而异，因此检查应用程序的 SAML 文档了解详细信息。 **登录**和**注销**服务都解析为同一个终结点，是 Azure AD 实例 SAML 请求处理终结点的 URL。 颁发者 URL 是显示为"颁发者"在 SAML 令牌颁发给应用程序的值。 

在配置您的应用程序之后，请单击**下一页**按钮，然后**完成**关闭对话框。 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>将用户和组分配给 SAML 应用程序 

一旦您的应用程序已配置为基于 SAML 的身份提供程序使用 Azure 的广告，它是几乎准备好测试。 为安全控件，Azure 的广告不会发出一个标记，该标记允许其登录到应用程序，除非它们已被授予访问权限使用 Azure 的广告。 用户可能被授予访问权限，直接或通过他们的成员的组。 

若要将用户或组分配给您的应用程序，单击**分配用户**按钮。 选择用户或组要指派，，然后选择**分配**按钮。 

![][6]

指定用户将允许 Azure 广告发出的用户，以及导致此应用程序的用户访问权面板中显示的图块的标记。 如果用户使用 Office 365，Office 365 提供应用程序启动器中也会出现应用程序图块。 

您可以上载应用程序的应用程序的**配置**选项卡上使用**上载徽标**按钮图块徽标。 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>自定义发出 SAML 令牌中的声明 

当用户对应用程序进行身份验证时，则 Azure 广告将有关的用户的唯一标识它们 SAML 令牌发布到应用程序，其中包含的信息 （或声明）。 默认情况下这将包括该用户的用户名、 电子邮件地址、 名字和姓氏。 

您可以查看或编辑发送到应用程序的**属性**选项卡下的 SAML 令牌中的声明。 

![][7]

有两种原因，您可能需要编辑发出 SAML 令牌中的声明: • 该应用程序已被编写为需要一组不同的 Uri 索赔或索赔值 •Your 应用程序已经部署到需要 NameIdentifier 自称是 Azure Active Directory 中存储的用户名 （也称为用户主要名称） 以外的内容。 

有关如何添加和编辑这些方案中的索赔，检查出该[声明自定义的文章](active-directory-saml-claims-customization.md)。 

###<a name="testing-the-saml-application"></a>测试 SAML 应用程序 

一旦已在 Azure 的广告，在应用程序配置 SAML Url 和证书，用户或组已分配给该应用程序在 Azure，并索赔已审查和编辑，如有必要，然后用户就可以登录到该应用程序。 

若要测试，只是符号-到 Azure AD 访问面板在 https://myapps.microsoft.com 使用分配给该应用程序的用户帐户，然后单击要甩掉单一登录过程的应用程序图块上。 或者，您可以直接浏览到应用程序和登录的登录 URL 从那里。 

用于调试的提示，请参阅[文章如何基于 SAML 的单一登录应用程序调试](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>密码单一登录

选择此选项可配置的[基于密码的单一登录](active-directory-appssoaccess-whatis.md)web 应用程序具有 HTML 登录页。 基于密码的 SSO，也称为密码保险存储，使您能够管理用户访问和 web 应用程序不支持联合身份验证的密码。 它也是有用的几个用户需要共享单个帐户，例如为您的组织的社会媒体应用程序帐户。 

选择**下一步**之后, 将提示您输入的登录页的基于 web 的应用程序的 URL。 请注意，这必须是包括用户名和密码的输入的字段的页。 输入后，Azure 广告将启动一个进程，来分析输入用户名和密码输入的登录页。 如果此过程不成功，然后它指导您完成安装浏览器扩展 （需要 Internet Explorer，镶边，或者 Firefox），您可以手动捕获字段的替代过程。

一旦捕获登录页面时，可能会指派用户和组，可以像常规[密码 SSO 应用程序](active-directory-appssoaccess-whatis.md)设置凭据策略。

注意︰ 您可以上载应用程序的应用程序的**配置**选项卡上使用**上载徽标**按钮图块徽标。 

##<a name="existing-single-sign-on"></a>现有单一登录

选择此选项可添加到您的组织的 Azure 广告接入面板或 Office 365 门户应用程序的链接。 您可以使用这将链接添加到自定义 web 应用程序当前正在使用 Azure Active Directory 联合身份验证服务 （或其他联合身份验证服务） 而非 Azure AD 身份验证。 或者，可以将深层链接添加到特定 SharePoint 页面或只是想要显示在用户的访问面板上其他 web 页。 

选择**下一步**之后, 将提示您输入要链接到的应用程序的 URL。 完成后，可能给应用程序，显示应用程序会导致[Office 365 提供应用程序启动程序](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/)或[Azure 广告接入面板](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)的用户指派用户和组。

注意︰ 您可以上载应用程序的应用程序的**配置**选项卡上使用**上载徽标**按钮图块徽标。

## <a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [如何自定义声明发出 SAML 令牌用于预先集成的应用程序](active-directory-saml-claims-customization.md)
- [基于 SAML 的单一登录疑难解答](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
