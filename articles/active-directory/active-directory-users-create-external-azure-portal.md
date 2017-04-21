<properties
    pageTitle="从其他目录中添加用户或合作伙伴公司在 Azure Active Directory 预览 |Microsoft Azure"
    description="介绍如何将用户添加或更改在 Azure Active Directory，包括外部和来宾用户的用户信息。"
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>从其他目录或合作伙伴公司在 Azure Active Directory 预览中的添加用户

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-users-create-external-azure-portal.md)
- [Azure 的传统门户网站](active-directory-create-users-external.md)

本文介绍了如何添加用户从其他目录在 Azure 活动目录 (AD Azure) 预览或合作伙伴公司。 [在预览中是什么？](active-directory-preview-explainer.md) 有关您的组织中添加新用户和添加具有 Microsoft 帐户的用户的信息，请参阅[添加新用户到 Azure 活动目录](active-directory-users-create-azure-portal.md)。 添加的用户没有管理员权限，默认情况下，但可以在任何时候为它们分配角色。

## <a name="add-a-user"></a>添加用户

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

    ![打开用户管理](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  **用户和组**刀片上, 选择**用户**，然后选择**添加**。

    ![选择添加命令](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. **用户**刀片上, 提供**名称**显示名称，用户的登录名的**用户名称**。

5. 复制或否则注意生成的用户密码，这样，此过程完成后可以提供它给用户。

6. 或者，选择**配置文件**添加用户第一次和姓、 职务和部门名称。
    
    ![打开用户配置文件](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - 选择要将用户添加到一个或多个组的**组**。

        ![将用户添加到组](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - 选择要从**角色**列表向角色分配用户的**组织角色**。 有关用户和管理员角色的详细信息，请参阅[在 Azure 的广告分配管理员角色](active-directory-assign-admin-roles.md)。

        ![将用户分配给角色](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. 选择**创建**。

8. 安全地分发给新用户生成的密码，以便用户可以登录。

> [AZURE.IMPORTANT] 如果您的组织使用多个域，您应了解以下问题，当您添加一个用户帐户︰
>
> - 若要跨域添加用户帐户使用同一用户主体名称 (UPN)，**第一个**添加，例如， geoffgrisso@contoso.onmicrosoft.com, **跟** geoffgrisso@contoso.com。
> - **不要**添加geoffgrisso@contoso.com在添加之前geoffgrisso@contoso.onmicrosoft.com。 此顺序很重要，并且可以麻烦撤消。

如果您更改其标识同步与您的内部部署 Active Directory 服务用户的信息，您不能更改 Azure 的传统门户网站中的用户信息。 若要更改用户信息，使用您的内部部署 Active Directory 管理工具。


## <a name="whats-next"></a>接下来是什么

- [添加用户](active-directory-users-create-azure-portal.md)
- [新的 Azure 门户中的用户的密码重置](active-directory-users-reset-password-azure-portal.md)
- [在 Azure 广告将用户分配给角色](active-directory-users-assign-role-azure-portal.md)
- [更改用户的工作信息](active-directory-users-work-info-azure-portal.md)
- [管理用户配置文件](active-directory-users-profile-azure-portal.md)
- [在 Azure 广告中删除用户](active-directory-users-delete-user-azure-portal.md)
