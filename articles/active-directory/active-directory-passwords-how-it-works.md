<properties
    pageTitle="它的工作原理︰ Azure AD 密码管理 |Microsoft Azure"
    description="了解有关 Azure AD 密码管理，包括用户注册、 重新设置，并更改他们的密码，不同的组件和配置管理员，报告，和实现的内部部署 Active Directory 密码管理。"
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="how-password-management-works"></a>密码管理的工作原理

> [AZURE.IMPORTANT] **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。

下面介绍的几个逻辑组件包括 Azure Active Directory 中的密码管理。  单击每个链接以了解有关该组件的详细信息。

- [**密码管理配置门户**](#password-management-configuration-portal)-管理员可以控制密码通过导航到其目录[Azure 管理门户](https://manage.windowsazure.com)的配置选项卡的管理在其承租人的不同方面。
- [**用户注册门户**](#user-registration-portal)– 用户可用来注册此 web 门户通过重置密码。
- [**用户密码重置门户**](#user-password-reset-portal)--用户可以将自己用几种不同的挑战，根据由管理员控制密码重置策略的密码重置
- [**用户密码更改门户**](#user-password-change-portal)--用户可以更改自己的密码在任何时间通过输入旧密码，并选择使用此 web 门户的新密码
- [**密码管理报告**](#password-management-reports)– 管理员可以查看并分析其租户在密码重置和注册活动通过导航到其目录[Azure 管理门户](https://manage.windowsazure.com)中的"报表"选项卡上的"活动报表"部分
- [**Azure AD 连接的密码写回组件**](#password-writeback-component-of-azure-ad-connect)— 管理员可以有选择地启用密码写回功能时安装 Azure AD 连接，以实现管理的联合或密码同步来自云中的用户密码。

## <a name="password-management-configuration-portal"></a>密码管理配置门户
您可以配置通过导航到该目录的**配置**选项卡中的**用户密码重置策略**一节使用[Azure 管理门户](https://manage.windowsazure.com)的特定目录的密码管理策略。  在此配置页面中，您可以控制密码在您的组织中的管理方式的诸多方面包括︰

- 启用和禁用目录中的所有用户重置密码
- 设定的挑战 （一个或两个） 用户必须经过重置其密码
- 设置要为用户启用下面的选项从您组织中的挑战的特定类型︰
 - 移动电话 （通过文本或语音呼叫一个验证码）
 - 办公室电话 （语音呼叫）
 - 备选电子邮件 （通过电子邮件验证代码）
 - 安全问题 （基于知识的身份验证）
- 设置用户的问题数必须注册才能使用安全问题身份验证方法 （仅在启用了安全问题时可见）
- 设置用户的问题数重置使用 （仅在启用了安全问题时可见） 安全问题身份验证方法时必须提供
- 使用预存、 本地化，安全问题，用户可以在注册时使用的密码重置 （仅在启用了安全问题时可见）
- 定义自定义的安全问题，用户可以在注册时使用的密码重置 （仅在启用了安全问题时可见）
- 要求用户注册他们转到应用程序接入面板在[http://myapps.microsoft.com](http://myapps.microsoft.com)时，重置密码。
- 要求用户重新配置天数后确认其先前注册的数据经过 （仅在启用强制的注册时可见）
- 提供自定义的帮助台电子邮件或在他们有密码重置问题的情况下将显示给用户的 URL
- 启用或禁用密码写回功能 （如果已使用 AAD 连接部署密码写回）
- 查看密码写回代理的状态 （如果已使用 AAD 连接部署密码写回）
- 已 （在[Azure 管理门户网站](https://manage.windowsazure.com)的**通知**部分中可以找到） 重置其密码时启用用户的电子邮件的通知
- 其他管理员重置 （ [Azure 管理门户网站](https://manage.windowsazure.com)的**通知**部分中找到自己的密码时启用电子邮件通知管理员
- 品牌的用户密码重置门户和密码重置电子邮件与您的组织的徽标和名称，使用标记自定义功能 （ [Azure 管理门户](https://manage.windowsazure.com)**目录属性**一节中找到租户

若要了解有关您的组织中配置密码管理的更多信息，请参见[入门︰ Azure AD 密码管理](active-directory-passwords-getting-started.md)。

##<a name="user-registration-portal"></a>用户注册门户
用户还可以使用密码重置之前，必须使用正确的身份验证数据，以确保它们可以通过适当数量的由管理员定义的密码重置挑战更新云用户帐户。  管理员还可以定义此身份验证信息上代表其用户通过使用 Azure 或 Office web 门户目录同步 / Azure AD 连接或 Windows PowerShell。

但是，如果您而是会有用户注册他们自己的数据，我们还提供一个网页，以提供此信息，用户可以转到。  此页将允许用户指定根据密码的身份验证信息重置在其组织中已启用的策略。  一旦验证此数据，存储在他们的云用户帐户以后要使用的帐户恢复。 下面是什么注册门户外观如下所示︰

  ![][001]

有关详细信息，请参阅[入门︰ Azure AD 密码管理](active-directory-passwords-getting-started.md)和[的最佳做法︰ Azure AD 密码管理](active-directory-passwords-best-practices.md)。

##<a name="user-password-reset-portal"></a>用户密码重置门户
一旦启用自助服务密码重置、 设置您的组织的自助服务密码重置策略，并确保您的用户具有适当的联系人数据目录中，您组织中的用户将无法重设自己的密码会自动从工作或学校的帐户用于登录 （如[portal.microsoftonline.com](https://portal.microsoftonline.com)) 的任何 web 页。 在这些页面中，用户将看到**不能访问您的帐户吗？**链接。

  ![][002]

单击此链接将启动自助服务密码重置门户。

  ![][003]

若要了解有关如何用户可以重新设置自己的密码的详细信息，请参阅[入门︰ Azure AD 密码管理](active-directory-passwords-getting-started.md)。

##<a name="user-password-change-portal"></a>用户密码更改门户
如果用户想要更改自己的密码，他们可以做到在任何时候使用密码更改门户。  用户可以访问通过接入面板配置文件页面，或单击"更改密码"链接从 Office 365 提供应用程序中的密码更改门户。  在这种情况在密码过期后，用户也可能需要在登录的时候自动更改它们。

  ![][004]

在这两种情况下，如果已启用写回密码和或者联合用户或密码同步将这些更改后的密码写回到内部活动目录。 这是什么密码更改门户的外观如下所示︰

  ![][005]

若要了解有关用户如何更改他们自己的内部部署 Active Directory 密码的详细信息，请参阅[入门︰ Azure AD 密码管理](active-directory-passwords-getting-started.md)。

##<a name="password-management-reports"></a>密码管理报告
通过导航至**报告**选项卡，查看在**活动日志**部分，您将看到两个密码管理报告︰**密码重置活动**和**重新注册的活动设置的密码**。  使用这两种报告，可获得注册和使用密码重设您的组织中用户的视图。 下面是这些报告在[Azure 管理门户](https://manage.windowsazure.com)的外观︰

  ![][006]

有关详细信息，请参阅[获得的见解︰ Azure AD 密码管理报告](active-directory-passwords-get-insights.md)。

##<a name="password-writeback-component-of-azure-ad-connect"></a>密码写回分量的 Azure AD 连接
如果您的组织中的用户密码源于内部环境 （无论通过联盟或密码同步），可以安装最新版本的 Azure AD 连接启用更新直接从云的密码。  这意味着当用户忘记了或想要修改其 AD 密码，他们可以因此直接从网站。  这是在哪里可以找到密码写回 Azure AD 连接安装向导中︰

  ![][007]

Azure AD 连接的详细信息，请参阅[入门︰ Azure AD 连接](active-directory-aadconnect.md)。 有关密码写回的详细信息，请参阅[入门︰ Azure AD 密码管理](active-directory-passwords-getting-started.md)。


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>链接到密码重置文档
下面是 Azure AD 密码重置文档页的所有链接︰

* **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。
* [**入门**](active-directory-passwords-getting-started.md)-了解如何允许用户重置并更改其云或内部密码
* [**自定义**](active-directory-passwords-customize.md)-了解如何自定义的外观和感觉和行为与您的组织需要的服务
* [**最佳做法**](active-directory-passwords-best-practices.md)-了解如何快速部署和有效地管理您的组织中的密码
* [**深入**](active-directory-passwords-get-insights.md)的了解我们集成的报告功能
* [**常见问题解答**](active-directory-passwords-faq.md)-获取常见问题的答案
* [**故障排除**](active-directory-passwords-troubleshoot.md)-了解如何快速排查服务问题
* [**了解更多**](active-directory-passwords-learn-more.md)-转到服务的工作方式的技术细节的深



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
