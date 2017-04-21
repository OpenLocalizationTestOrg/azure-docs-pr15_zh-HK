<properties
   pageTitle="品牌对于应用程序的准则 |Microsoft Azure"
   description="全面的 Azure Active Directory 面向开发人员的资源指南"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>商标使用准则的应用程序


本主题讨论使用 Azure 活动目录 (AD Azure) 开发应用程序时，应使用商标使用准则。 这些指南将帮助指导您的客户想要使用他们工作或学校的帐户，在 Azure 广告，管理或其个人帐户注册并登录到您的应用程序时。

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>个人帐户与工作或 microsoft 的学校科目

Microsoft 管理两种类型的用户帐户︰

- **个人帐户**（以前称为 Windows Live™ ID）。 这些帐户表示*单个*用户和微软之间的关系，并用于访问 Microsoft 中的使用者的设备和服务。 这些帐户被供个人使用。

- **工作或学校的帐户。** 这些帐户是由 Microsoft 管理使用 Azure Active Directory 的组织代表。 这些帐户用于从 Microsoft Office 365 和其他商业服务登录。

Microsoft 工作或学校的帐户通常分配给最终用户 （员工、 学生、 联邦雇员） 通过其组织 （公司、 学校、 政府机关）。 这些帐户在云中，在 Azure 的广告平台，直接掌握了，或者从本地目录中，如 Windows 服务器的 Active Directory 同步到 Azure 的广告。 Microsoft 是*管理员*的工作或学校的科目，但拥有和控制的组织帐户。

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>指的 Azure 的广告在您的应用程序中的帐户

Microsoft 不公开最终用户对 Azure 或 Active Directory 品牌名称，既不应该您。

- 一旦用户登录，应使用组织的名称和徽标尽可能多地。 这是优于使用通用术语"组织"等。

- 当用户没有登录时，您就应当为其帐户"工作或学校帐户"，然后使用 Microsoft 徽标来传达这些帐户由 Microsoft 管理。 不要使用如"企业客户"、"企业往来帐户"或"公司帐户"的条款，它创建用户混淆。

## <a name="user-account-pictogram"></a>用户帐户 pictogram
在早期版本的这些原则，我们建议使用"蓝色工牌"pictogram。 根据用户和开发者的反馈，我们现在建议使用 Microsoft 徽标的相反。 这将帮助用户了解，他们可以重复使用它们将使用具有 Office 365 或其他 Microsoft 业务服务以登录到您的应用程序的帐户。

## <a name="signing-up-and-signing-in-with-azure-ad"></a>注册和签名用 Azure 的广告

您的应用程序可能会显示注册和登录的单独的路径，以下各节提供了可视化指导，这两种情况。

**如果您的应用程序支持最终用户符号向上 （例如免费试用版或 freemium 模型）**︰ 可以显示**登录**按钮，使用户能够访问您的应用程序与他们工作的帐户或其个人帐户。 Azure 的广告将显示在访问您的应用程序在第一次同意提示。

**如果您的应用程序需要的权限，只有管理员可以表示同意，或如果您的应用程序要求组织授权**︰ 应该分开，从用户登录的管理员购置。 **"获取此应用程序"按钮**将重定向管理员登录，然后要求他们授予代表组织中的用户同意的情况。 这有抑制到您的应用程序的最终用户同意的情况下提示的优点。

## <a name="visual-guidance-for-app-acquisition"></a>应用程序获取可视化指导

"获取该应用程序"链接必须将用户重定向到 Azure AD 授予访问权限 （授权） 页，允许组织管理员联系以授予您的应用程序有权访问由 Microsoft 的组织的数据。 [Azure Active Directory 集成应用程序](active-directory-integrating-applications.md)文章讨论了如何请求访问的详细信息。

管理员表示同意在您的应用程序后，他们可以选择将其添加到其用户的 Office 365 提供应用程序启动器体验 （从 waffle 和[https://portal.office.com/myapps](https://portal.office.com/myapps)的情况下可访问）。 如果您想要公布这种能力，可以使用术语像"将此应用程序添加到您的组织"，并显示此类按钮︰

![应用程序类型和方案](./media/active-directory-branding-guidelines/add-to-my-org.png)

但是，我们建议您编写说明性的文本，而不是依靠按钮。 例如︰
> *如果您已经使用 Office 365 或其他 microsoft 的业务服务，您可以只需授予 < your_app_name > 访问组织的数据。这将允许您的用户可以访问其现有的工作帐户 < your_app_name >。*


## <a name="visual-guidance-for-sign-in"></a>登录程序的可视化指南
您的应用程序应显示一个符号按钮将用户重定向到登录端点对应于使用 Azure 广告与集成的协议中。 下一节详细说明该按钮应如下所示。

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictogram 和"使用 Microsoft 签名"
它是 Microsoft 徽标的关联，您的应用程序可能支持唯一代表其他标识提供程序之间的 Azure 广告"号在 with Microsoft"条款。 如果您没有足够的空间对于"号在 with Microsoft"，则可以将其缩短为"登录"。

![应用程序类型和方案](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![应用程序类型和方案](./media/active-directory-branding-guidelines/sign-in-light.png)

此外可以为按钮使用深色配色方案。

![应用程序类型和方案](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![应用程序类型和方案](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>正确和错误的品牌

**请**使用"工作或学校帐户"结合"号在 with Microsoft"按钮提供补充说明，以帮助最终用户识别是否他们可以使用它。 **不要**使用其他术语，如"企业客户"、"企业往来帐户"或"公司帐户"。

**不要**使用"Office 365 ID"或"Azure ID"。 Office 365 也是使用 microsoft 这不使用 Azure AD 身份验证提供者的名称。

**不**改变 Microsoft 徽标。

**不**公开到 Azure 或 Active Directory 品牌的最终用户。 它不过是确定与开发人员、 IT 专业人员和管理员使用这些术语。

## <a name="navigation-dos-and-donts"></a>导航须知

**请**提供便于用户注销并切换到另一个用户帐户。 大多数人有一个个人帐户从 Microsoft/Facebook/Google/Twitter，而人们往往与多个组织。 支持多个已登录的用户即将登场。
