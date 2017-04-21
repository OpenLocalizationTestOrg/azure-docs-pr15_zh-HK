<properties
    pageTitle="将用户分配到 Azure Active Directory 预览中的管理员角色 |Microsoft Azure"
    description="解释如何更改在 Azure Active Directory 中的用户管理信息"
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

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>将用户分配到 Azure Active Directory 预览中的管理员角色

本文介绍了如何将管理角色分配到 Azure 活动目录 (AD Azure) 预览中的用户。 [在预览中是什么？](active-directory-preview-explainer.md) 有关在您的组织中添加新用户的信息，请参阅[添加新用户到 Azure 活动目录](active-directory-users-create-azure-portal.md)。 添加的用户没有管理员权限，默认情况下，但可以在任何时候为它们分配角色。

## <a name="assign-a-role-to-a-user"></a>为用户分配角色

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

    ![打开用户管理](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  在**用户和组**刀片式服务器，选择**所有用户**。

    ![打开所有用户刀片式服务器](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. **用户和组的所有用户**刀片式服务器中，从列表中选择一个用户。

5. 在所选用户的刀片式服务器，选择**目录角色**，，然后将用户分配给角色从**目录角色**列表。 有关用户和管理员角色的详细信息，请参阅[在 Azure 的广告分配管理员角色](active-directory-assign-admin-roles.md)。

      ![将用户分配给角色](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. 选择**保存**。


## <a name="whats-next"></a>接下来是什么

- [添加用户](active-directory-users-create-azure-portal.md)
- [新的 Azure 门户中的用户的密码重置](active-directory-users-reset-password-azure-portal.md)
- [更改用户的工作信息](active-directory-users-work-info-azure-portal.md)
- [管理用户配置文件](active-directory-users-profile-azure-portal.md)
- [在 Azure 广告中删除用户](active-directory-users-delete-user-azure-portal.md)
