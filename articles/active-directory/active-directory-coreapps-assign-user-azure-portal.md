<properties
    pageTitle="将用户或组分配给在 Azure Active Directory 预览中的企业应用程序 |Microsoft Azure"
    description="如何选择 Azure Active Directory 中为其指定的用户或组的企业应用程序"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>将用户或组分配给在 Azure Active Directory 预览中的企业应用程序

很容易地将用户或组分配给在 Azure 活动目录 (AD Azure) 预览中企业应用程序。 [在预览中是什么？](active-directory-preview-explainer.md) 您必须具有适当的权限来管理企业应用程序。 在当前的预览中，您必须是全局目录管理。

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>如何将用户的访问权限分配给企业应用程序？

1. 登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2. 选择**更多的服务**，在文本框中，输入 Azure Active Directory，然后选择**输入**。

3. 在**Azure 活动目录的*目录名称***选择刀片式服务器 （即，Azure AD 刀片式服务器所管理的目录），**企业应用程序 * *。

    ![打开企业应用程序](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. 在**企业级应用程序**刀片式服务器，选择**所有应用程序**。 您将看到您可以管理应用程序的列表。

5. 在**企业级应用程序的所有应用程序**刀片式服务器，选择应用程序。

6. ***应用程序名***刀片式服务器 （即，刀片具有标题中选定的应用程序的名称） 上选择**用户和组**。

    ![选择所有的应用程序命令](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. 在***应用程序名*****的用户和组分配**刀片式服务器，选择**添加**命令。

8. 在**添加任务**刀片式服务器，选择**用户和组**。

    ![将用户或组分配给应用程序](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. **用户和组**刀片式服务器中，从列表中选择一个或多个用户或组，然后选择底部刀片式服务器的**选择**按钮。

10. 在**添加任务**刀片式服务器，选择**角色**。 然后，在**选择角色**刀片式服务器，选择要应用于所选的用户或组的角色，然后选择**确定**按钮底部的刀片式服务器。

11. 在**添加任务**刀片式服务器，选择底部的刀片式服务器的**分配**按钮。 指派的用户或组将具有为此企业应用程序中的所选角色定义的权限。

## <a name="next-steps"></a>下一步行动

- [查看所有组](active-directory-groups-view-azure-portal.md)
- [从企业应用程序中删除用户或组分配](active-directory-coreapps-remove-assignment-azure-portal.md)
- [禁用用户登录为企业应用程序](active-directory-coreapps-disable-app-azure-portal.md)
- [更改名称或徽标的企业应用程序](active-directory-coreapps-change-app-logo-user-azure-portal.md)
