<properties
    pageTitle="管理 Azure Active Directory 预览中某个组的成员 |Microsoft Azure"
    description="如何给用户和设备的 Azure Active Directory 中的组成员"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>管理 Azure Active Directory 预览中某个组的成员

本文介绍了如何管理 Azure 活动目录 (AD Azure) 预览中某个组的成员。 [在预览中是什么？](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>如何查找成员并对其进行管理？

1.  登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2.  选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

  ![打开用户管理](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  **用户和组**刀片式服务器上选择的**所有组**。

  ![打开刀片式服务器组](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. **用户和组的所有组**刀片式服务器上选择一个组。

5. **组的*组名*上**刀片式服务器选择**成员 * *。

  ![打开成员刀片式服务器](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. 若要将成员添加到组，请在**组的成员**刀片式服务器，选择**添加成员**。

  ![添加成员命令](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. 在**成员**刀片式服务器，选择一个或多个用户或设备添加到组并选择**选择**按钮底部的刀片式服务器将它们添加到组。 **用户名**框筛选显示内容，基于匹配您的输入用户或设备名的任一部分。 在此框中不接受任何通配符。

8. 要删除成员的组，**组的成员**刀片式服务器，请选择一个成员。

9. 在***成员名称***刀片式服务器，选择**删除**命令，并确认您的选择在提示符下。

  ![删除成员命令](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. 当您完成更改的组的成员时，则选择**保存**。


## <a name="additional-information"></a>其他信息

这些文章在 Azure Active Directory 提供附加信息。

* [现有的组，请参阅](active-directory-groups-view-azure-portal.md)
* [创建一个新组并添加成员](active-directory-groups-create-azure-portal.md)
* [管理组的设置](active-directory-groups-settings-azure-portal.md)
* [管理组的成员身份](active-directory-groups-membership-azure-portal.md)
* [管理动态规则组中的用户](active-directory-groups-dynamic-membership-azure-portal.md)
