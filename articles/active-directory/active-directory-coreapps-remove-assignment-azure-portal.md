<properties
    pageTitle="从 Azure Active Directory 预览中的企业应用程序中删除用户或组分配 |Microsoft Azure"
    description="如何从 Azure Active Directory 中的企业应用程序中删除用户或组的访问权限分配"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>从 Azure Active Directory 预览中的企业应用程序中删除用户或组分配

很容易地删除用户或组访问权限分配给一个企业应用程序在 Azure 活动目录 (AD Azure) 预览。 [在预览中是什么？](active-directory-preview-explainer.md) 您必须具有适当的权限来管理企业应用程序。 在当前的预览中，您必须是全局目录管理。

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>如何删除用户或组分配？

1. 登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2. 选择**更多的服务**，在文本框中，输入**Azure Active Directory** ，然后选择**输入**。

3. 在**Azure 活动目录的*目录名称***选择刀片式服务器 （即，Azure AD 刀片式服务器所管理的目录），**企业应用程序 * *。

    ![打开企业应用程序](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. 在**企业级应用程序**刀片式服务器，选择**所有应用程序**。 您将看到您可以管理应用程序的列表。

5. 在**企业级应用程序的所有应用程序**刀片式服务器，选择应用程序。

6. ***应用程序名***刀片式服务器 （即，刀片具有标题中选定的应用程序的名称） 上选择**用户和组**。

    ![选择用户或组](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. 在***应用程序名*****的用户和组分配**刀片式服务器，选择一种更多的用户或组，然后选择**删除**命令。 确认在提示符下决策。

    ![选择删除命令](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>下一步行动

- [查看所有组](active-directory-groups-view-azure-portal.md)
- [将用户或组分配给一个企业应用程序](active-directory-coreapps-assign-user-azure-portal.md)
- [禁用用户登录为企业应用程序](active-directory-coreapps-disable-app-azure-portal.md)
- [更改名称或徽标的企业应用程序](active-directory-coreapps-change-app-logo-user-azure-portal.md)
