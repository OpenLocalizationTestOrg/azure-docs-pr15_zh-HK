<properties
    pageTitle="在 Azure Active Directory 预览中创建新组 |Microsoft Azure"
    description="如何在 Azure 活动目录中创建一个组并向组中添加用户 （成员）"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>在 Azure Active Directory 预览中创建新组

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-groups-create-azure-portal.md)
- [Azure 的传统门户网站](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

本文介绍如何创建和填充 Azure 活动目录 (AD Azure) 预览中的新组。 [在预览中是什么？](active-directory-preview-explainer.md) 一组用于执行管理任务，如同时将许可证或权限分配给用户或设备数。

## <a name="how-do-i-create-a-group"></a>如何创建一组？

1. 登录到[Azure 的门户网站](https://portal.azure.com)的目录是全局管理员帐户。

2. 选择**更多的服务**，在文本框中，输入**用户和组**，然后选择**输入**。

  ![打开用户管理](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. **用户和组**刀片式服务器上选择的**所有组**。

  ![打开刀片式服务器组](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. 在**用户和组的所有组**刀片式服务器，选择**添加**命令。

  ![选择添加命令](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. 在**组**刀片式服务器，添加名称和组的说明。

6. 若要选择要添加到组的成员，在**成员资格类型**框中，选择**分配**，然后选择**成员**。 如何动态地管理组的成员身份有关的详细信息，请参阅[使用属性来创建高级的规则的组成员身份](active-directory-groups-dynamic-membership-azure-portal.md)。

  ![选择要添加的成员](./media/active-directory-groups-create-azure-portal/select-members.png)

5. 在**成员**刀片式服务器，选择一个或多个用户或设备添加到组并选择**选择**按钮底部的刀片式服务器将它们添加到组。 **用户名**框筛选显示内容，基于匹配您的输入用户或设备名的任一部分。 在此框中不接受任何通配符。

6. 完成将成员添加到组后，**组**刀片在选择**创建**。    

  ![创建组确认](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>其他信息

这些文章在 Azure Active Directory 提供附加信息。

* [现有的组，请参阅](active-directory-groups-view-azure-portal.md)
* [管理组的设置](active-directory-groups-settings-azure-portal.md)
* [管理组的成员](active-directory-groups-members-azure-portal.md)
* [管理组的成员身份](active-directory-groups-membership-azure-portal.md)
* [管理动态规则组中的用户](active-directory-groups-dynamic-membership-azure-portal.md)
