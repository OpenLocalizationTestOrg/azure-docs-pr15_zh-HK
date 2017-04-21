<properties
    pageTitle="Azure 的 Active Directory B2C︰ 常见问题解答 |Microsoft Azure"
    description="关于 Azure 活动目录 B2C 常见问题"
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
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure 的 Active Directory B2C︰ 常见问题解答

此页回答有关 Azure 活动目录 (AD Azure) B2C 的常见问题。 请检查更新。

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>可以在我现有的员工基于 Azure AD 租户使用 Azure AD B2C 功能？

目前 Azure AD B2C 功能不能打开在您现有的 Azure AD 租户。 我们建议您创建一个单独的租户使用 Azure AD B2C 的功能来管理您的使用者。

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>可以使用 Azure AD B2C 提供社交登录 （Facebook 和 Google +） 到 Office 365？

能与 Microsoft Office 365 azure AD B2C。 一般情况下，它不能用于提供对任何 SaaS 应用程序 （Office 365、 销售队伍、 工作日等） 进行身份验证。 它提供了身份和访问管理只面向使用者的 web 和移动应用程序，并不适用于员工或合作伙伴方案。

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>在 Azure AD B2C 中的本地帐户是什么？ 它们如何不同于工作或学校的帐户在 Azure 的广告？

使用窗体的电子邮件地址 （除了使用 Microsoft 的现有帐户的用户） 租户的每个用户签署在 Azure AD 租户， `<xyz>@<tenant domain>`，其中`<tenant domain>`是租户或初始验证域之一`<...>.onmicrosoft.com`域。 这种类型是帐户的工作或学校的帐户。

在 Azure AD B2C 租户，大多数应用程序需要使用任何任意的电子邮件地址进行登录的用户 (例如， joe@comcast.net, bob@gmail.com, sarah@contoso.com,或jim@live.com)。 这种类型是帐户的本地帐户。 今天，我们还支持任意用户名称 （就是普通的字符串） 作为本地帐户 （例如，李先生、 bob、 林莎一起或马）。 可以在 Azure AD B2C 服务中选择这两种本地帐户类型之一。

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>哪些社会身份提供程序是否支持现在？ 您打算在将来支持哪些？

我们目前支持 Facebook、 Google +，LinkedIn 和 Amazon。 我们将根据客户的要求其他流行的社会身份提供程序的支持。

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>可以将配置详细信息使用者收集各种社会身份提供程序的范围？

否，但此功能是在我们的规划图上。 我们支持一套社会身份提供程序使用的默认作用域包括︰

- Facebook︰ 电子邮件
- Google +: 电子邮件
- Microsoft 帐户︰ openid 电子邮件配置文件
- Amazon︰ 配置文件
- LinkedIn: r_emailaddress r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>我的应用程序是否需要为它使用 Azure AD B2C 在 Azure 上运行？

否，可以承载您的应用程序中任何位置 （云或内部）。 它需要与 Azure AD B2C 交互就是发送和接收 HTTP 请求公开访问终结点上的能力。

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>我有多个 Azure AD B2C 承租人。 在 Azure 门户网站上，如何可以管理他们？

每个 Azure AD B2C 租户 Azure 的门户网站上都有自己的 B2C 功能刀片式服务器。 请参阅[Azure AD B2C︰ 注册您应用程序](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)以了解如何巡航到 Azure 的门户网站上的一个特定租户 B2C 功能刀片式服务器。 在 Azure 的门户网站上的 Azure AD B2C 目录之间切换并不能保证 B2C 功能在大多数浏览器打开刀片式服务器。

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>如何自定义验证的电子邮件 (内容和"从:"字段) 发送的 Azure AD B2C？

[公司品牌功能](../active-directory/active-directory-add-company-branding.md)可用于自定义验证电子邮件的内容。 具体而言，可自定义电子邮件这两个元素︰

- **横幅徽标**︰ 显示在右下角。
- **背景色**︰ 显示在顶部。

    ![自定义的验证电子邮件屏幕抓图](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

电子邮件签名中包含提供当您首次创建 B2C 租户的 B2C 租户的名称。 您可以更改使用这些指令的名称︰

- 作为订阅管理员登录到[Azure 的传统门户网站](https://manage.windowsazure.com/)。
- 导航到 B2C 租户。
- 在**配置**选项卡上单击。
- 更改**目录属性**部分下的**名称**字段。
- 单击页面底部的**保存**。

目前，并没有办法更改"从:"字段中的电子邮件。 如果您感和完全自定义的验证电子邮件的正文中此功能，请不要投票[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails)上的功能。

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>如何可以将迁移我现有的用户名、 密码和配置文件从数据库到 Azure AD B2C？

可以使用 Azure 广告图形 API 来编写迁移工具。 [Graph API 示例](active-directory-b2c-devquickstarts-graph-dotnet.md)的详细信息，请参阅。 我们将在以后提供各种迁移选项和工具的-瓜。

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>在 Azure AD B2C 中的本地帐户的用途是什么密码策略？

本地帐户的 Azure AD B2C 密码策略基于 Azure 的广告策略。 Azure AD B2C 的注册、 注册或登录和密码重置策略使用的"强"密码强度和不过期密码。 阅读更多详细信息的[Azure AD 密码策略](https://msdn.microsoft.com/library/azure/jj943764.aspx)。

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>可以使用 Azure AD 连接迁移到 Azure AD B2C 我内部部署 Active Directory 存储的消费者身份？

否，Azure AD 连接不是使用 Azure AD B2C。 我们将在以后提供各种迁移选项和工具的-瓜。

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C 是否使用如 Microsoft Dynamics CRM 系统？

目前没有。 集成这些系统是在我们的规划图上。

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>没有 Azure AD B2C 使用 SharePoint 部署 2016年或更早版本？

目前没有。 Azure AD B2C 没有支持的门户网站和电子商务应用程序基于 SharePoint 内部需要的 SAML 1.1 标记。 请注意，Azure AD B2C 不是 SharePoint 外部合作伙伴共享方案;看到[Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) 。

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>我应使用 Azure AD B2C 或 B2B 管理外部标识？

阅读这篇文章有关[外部标识](../active-directory/active-directory-b2b-compare-external-identities.md)以了解更多关于应用于外部标识方案的相应功能。

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Azure AD B2C 提供哪些报告和审核功能？ 它们与 Azure AD 津贴相同？

否，Azure AD B2C 不支持 Azure AD 津贴作为一组相同的报告。 基本报告和审核 Api 很快将同时发布 azure AD B2C。

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>可以进行本地化提供通过 Azure AD B2C 的页面的用户界面 支持哪些语言？

目前，Azure AD B2C 只优化英语。 我们打算尽快推出本地化功能。

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>可以在我服务的 Azure AD B2C 的注册和登录页面上使用我自己的 Url？ 例如，可以更改 URL 从 login.microsoftonline.com 到 login.contoso.com？

目前没有。 此功能是在我们的规划图上。 此外请注意，验证您您租户在 Azure 的传统门户网站上的**域**选项卡中的域将不这样做。

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>如何删除我的 Azure AD B2C 租户？

请按照以下步骤删除 Azure AD B2C 租户︰

- 按照以下[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)到 Azure 的门户网站上。
- 导航到**应用程序**，**标识提供程序**和**所有策略**刀片式服务器并删除每个中的所有条目。
- 现在登录到[Azure 的传统门户网站](https://manage.windowsazure.com/)作为订阅管理员。 （这是相同的工作或学校帐户或用来注册 azure 的 Microsoft 帐户相同。
- 导航到 Active Directory 扩展在左侧，单击 B2C 租户。
- 单击**用户**选项卡。
- 把 （不包括您当前正在为，亦即订阅管理员用户） 选择每个用户。 单击页面底部的**删除**，单击**是**时提示您。
- 单击**应用程序**选项卡。
- **显示**下拉字段中选择**我公司拥有的应用程序**并单击该复选标记。
- 您会看到调用下面列出的**b2c 扩展应用程序**的应用程序。 单击页面底部的**删除**，单击**是**时提示您。
- 重新定位到 Active Directory 扩展并选择 B2C 租户。
- 单击页面底部的**删除**。 按照完成屏幕上的说明。

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>是否可以获得 Azure AD B2C 企业移动套件的一部分？

否，Azure AD B2C 为即付即用 Azure 服务并不是企业移动套件的一部分。

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>如何报告 Azure AD B2C 的问题？

请参阅[支持活动目录 B2C 的 Azure 的请求的文件](active-directory-b2c-support.md)。

## <a name="more-information"></a>详细信息

您还可以查看当前[服务限制、 限制和约束](active-directory-b2c-limitations.md)。
