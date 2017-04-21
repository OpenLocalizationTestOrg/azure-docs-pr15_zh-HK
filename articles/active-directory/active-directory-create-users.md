<properties
    pageTitle="将新用户添加到 Azure Active Directory |Microsoft Azure"
    description="解释如何将新用户添加或更改在 Azure Active Directory 中的用户信息。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>将新用户或使用 Microsoft 帐户的用户添加到 Azure 活动目录

添加用户来填充您的目录。 本文介绍了如何在您的组织中添加新用户和添加具有 Microsoft 帐户的用户。 从 Azure Active Directory 中的其他目录中添加用户或从合作伙伴公司中添加用户的更多信息，请参阅[添加用户从其他目录或在 Azure Active Directory 中的合作伙伴公司](active-directory-create-users-external.md)。 添加的用户没有管理员权限，默认情况下，但可以在任何时候为它们分配角色。

## <a name="add-a-user"></a>添加用户

1. 登录到[Azure 的传统门户网站](https://manage.windowsazure.com)的目录是全局管理员帐户。
2. **活动目录**中选择，然后选择组织目录的名称。
3. 选择**用户**选项卡，然后在命令栏中，选择**添加用户**。
4. **告诉我们有关此用户**在页上，在**类型的用户**，可以选择︰

    - **组织中的新用户**--在您的目录中添加新的用户帐户。
    - **与现有 Microsoft 帐户的用户**– 将现有 Microsoft 使用者帐户添加到您的目录 （例如，Outlook 帐户）

5. 根据**用户类型**，输入的用户名称 （新用户） 或电子邮件地址 （用于与 Microsoft 帐户的用户）。
6. 在用户**配置文件**页上，提供了第一个和最后一个名称、 一个用户友好的名称和**角色**列表中的用户角色。 有关用户和管理员角色的详细信息，请参阅[在 Azure 的广告分配管理员角色](active-directory-assign-admin-roles.md)。 指定是否**启用多因素身份验证**的用户。
7. 在**获得临时密码**页上，选择**创建**。

> [AZURE.IMPORTANT] 如果您的组织使用多个域，您应了解以下问题，当您添加一个用户帐户︰
>
> - 若要跨域添加用户帐户使用同一用户主体名称 (UPN)，**第一个**添加，例如， geoffgrisso@contoso.onmicrosoft.com, **跟** geoffgrisso@contoso.com。
> - **不要**添加geoffgrisso@contoso.com在添加之前geoffgrisso@contoso.onmicrosoft.com。 此顺序很重要，并且可以麻烦撤消。

## <a name="change-user-information"></a>更改用户信息

您可以更改任何用户属性除外的对象 id。

1. 打开您的目录。
2. 选择**用户**选项卡，然后选择您想要更改的用户的显示名称。
3. 完成所做的更改，然后单击**保存**。

如果您要更改的用户与您的内部部署 Active Directory 服务同步，您不能更改使用该过程的用户信息。 若要更改的用户，使用您的内部部署 Active Directory 管理工具。

## <a name="guest-user-management-and-limitations"></a>来宾用户管理和限制

来宾帐户是曾获邀到您的目录访问 SharePoint 文档、 应用程序或其他 Azure 的资源的其他目录中的用户。 您的目录中的来宾帐户具有其基本的用户类型属性设置为"来宾"。 普通用户 （特别是，您的目录成员） 有用户类型属性中"成员"。

客人的目录中有一组有限的权限。 这些权限限制为客人发现目录中其他用户的有关信息的能力。 但是，来宾用户可以仍然交互的用户和组与正在使用的资源。 来宾用户可以︰

- 查看其他用户和组所分配到的 Azure 订阅关联
- 查看用户所属的组的成员
- 查找目录中的其他用户，如果用户知道该用户的完整电子邮件地址
- 只有一组有限的他们查找-限制为显示名称、 电子邮件地址、 用户主体名称 (UPN) 和缩略图照片的用户的属性，请参阅
- 获取已验证域的目录中的列表
- 同意向其授予您目录中的成员具有相同的访问权的应用程序

## <a name="set-guest-user-access-policies"></a>设置来宾用户访问策略

目录的**配置**选项卡包括用于控制访问来宾用户的选项。 这些选项可以更改目录的全局管理员只能在 Azure 的传统门户网站中。 目前，没有 PowerShell 或者 API 方法。

在 Azure 的经典门户打开**配置**选项卡，选择**Active Directory**，然后选择的目录的名称。

![在 Azure Active Directory 配置选项卡][1]

然后您可以编辑的选项用于控制来宾用户访问。

![来宾用户的访问控制选项][2]


## <a name="whats-next"></a>接下来是什么

- [从其他目录或在 Azure Active Directory 中的合作伙伴公司中添加用户](active-directory-create-users-external.md)
- [管理 Azure 的广告](active-directory-administer.md)
- [在 Azure AD 管理密码](active-directory-manage-passwords.md)
- [在 Azure AD 管理组](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
