<properties
    pageTitle="将新用户添加到 Azure Active Directory 预览 |Microsoft Azure"
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="add-new-users-to-azure-active-directory-preview"></a>将新用户添加到 Azure Active Directory 预览

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-users-create-azure-portal.md)
- [Azure 的传统门户网站](active-directory-create-users.md)

本文介绍如何在 Azure 活动 Direstory (Azure AD) 预览您的组织中添加新用户。 [在预览中是什么？](active-directory-preview-explainer.md)

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

    ![打开用户管理](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  在**用户和组**刀片式服务器，选择**所有用户**，然后选择**添加**。

    ![选择添加命令](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  输入的用户**名称**和**用户名称**等详细信息。 用户名域名称部分必须是初始默认域名称"foo.onmicrosoft.com"的域名或经过验证的、 非联合域名如"contoso.com。"

5. 复制或否则注意生成的用户密码，这样，此过程完成后可以提供它给用户。

6. （可选） 您可以打开并填写**配置文件**刀片式服务器、**组**刀片式服务器或为用户**目录角色**刀片式服务器中的信息。 有关用户和管理员角色的详细信息，请参阅[在 Azure 的广告分配管理员角色](active-directory-assign-admin-roles.md)。

7.  在**用户**刀片式服务器，选择**创建**。

8. 安全地分发给新用户生成的密码，以便用户可以登录。

## <a name="whats-next"></a>接下来是什么

- [添加外部的用户](active-directory-users-create-external-azure-portal.md)
- [新的 Azure 门户中的用户的密码重置](active-directory-users-reset-password-azure-portal.md)
- [更改用户的工作信息](active-directory-users-work-info-azure-portal.md)
- [管理用户配置文件](active-directory-users-profile-azure-portal.md)
- [在 Azure 广告中删除用户](active-directory-users-delete-user-azure-portal.md)
- [在 Azure 广告将用户分配给角色](active-directory-users-assign-role-azure-portal.md)
