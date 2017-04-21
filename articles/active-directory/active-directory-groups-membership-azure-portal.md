<properties
    pageTitle="管理您的组是 Azure Active Directory 预览中的成员的组 |Microsoft Azure"
    description="组可以包含其他组在 Azure Active Directory 中。 下面介绍了如何管理这些成员资格。"
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>管理您的组是 Azure Active Directory 预览中的成员的组

组可以包含其他组在 Azure Active Directory 预览。 [在预览中是什么？](active-directory-preview-explainer.md) 下面介绍了如何管理这些成员资格。

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>如何查找我的用户组的成员的组？

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

  ![打开用户管理](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  **用户和组**刀片式服务器上选择的**所有组**。

  ![打开刀片式服务器组](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. **用户和组的所有组**刀片式服务器上选择一个组。

5. **刀片式服务器选择****组的*组名*的组成员资格 * *。

  ![打开组成员身份刀片式服务器](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. 若要您的组的成员添加进来的另一个组，请在**组的组成员身份**刀片式服务器，选择**添加**命令。

7. 从**选择组**刀片式服务器，选择一个组，然后选择**选择**按钮底部的刀片式服务器。 可以一次添加到一个组的组。 **用户名**框筛选显示内容，基于匹配您的输入用户或设备名的任一部分。 在此框中不接受任何通配符。

  ![添加组成员身份](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. 若要删除您的组成员的其他组，请在**组的组成员身份**刀片式服务器，请选择一个组。

9. ***组名***刀片式服务器，选择**删除**命令，并确认您的选择在提示符处。

  ![删除成员身份命令](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. 当您完成更改为您的组的组成员身份时，则选择**保存**。


## <a name="additional-information"></a>其他信息

这些文章在 Azure Active Directory 提供附加信息。

* [现有的组，请参阅](active-directory-groups-view-azure-portal.md)
* [创建一个新组并添加成员](active-directory-groups-create-azure-portal.md)
* [管理组的设置](active-directory-groups-settings-azure-portal.md)
* [管理组的成员](active-directory-groups-members-azure-portal.md)
* [管理动态规则组中的用户](active-directory-groups-dynamic-membership-azure-portal.md)
