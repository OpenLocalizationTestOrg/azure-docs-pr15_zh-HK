<properties
    pageTitle="Azure AD 域服务︰ 创建 AAD DC 管理员组 |Microsoft Azure"
    description="要开始使用 Azure Active Directory 域服务"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>开始使用 Azure AD 域服务

本文讲解启用 Azure AD 租户的 Azure AD 域服务所必需的配置任务。

## <a name="task-1-create-the-aad-dc-administrators-group"></a>任务 1︰ 创建 AAD DC 管理员组
第一项任务是在 Azure Active Directory 租户中创建一个管理组。 这种特殊的管理组称为**AAD DC 管理员**。 此组的成员授予到 Azure AD 域服务托管域加入域的计算机的管理权限。 在加入域的计算机上，此组添加到管理员组。 此外，该组的成员可以使用远程桌面进行远程连接到加入域的计算机。  

> [AZURE.NOTE] 您没有创建使用 Azure AD 域服务的托管域的域管理员或企业管理员权限。 在托管域，这些特权服务保留和程序变为不可提供给组织内的用户。 但是，可以使用特殊的管理员用户组创建此配置任务，在执行某些特权的操作。 这些操作包括将计算机加入到域，属于管理员组在加入域的计算机上配置组策略等。

在此配置任务中，您创建的管理组和目录中的一个或多个用户添加到组。 执行以下步骤来为 Azure AD 域服务创建的管理组︰

1. 导航到**Azure 的传统门户网站**([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. 选择左边窗格中的**活动目录**节点。

3. 选择要为其启用 Azure AD 域服务 Azure AD 租户 （目录）。 您只能创建一个域，每个 Azure 的广告目录。

    ![选择 Azure 的广告目录](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. 单击**组**选项卡。

5. 若要将组添加到 Azure AD 租户，单击页面底部的任务窗格中**添加组**。

    ![添加组按钮](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. 创建一个名为**AAD DC 管理员**组。 将**组类型**设置为**安全**。

    > [AZURE.WARNING] 要启用 Azure AD 域服务中的访问受管理的域，使用该确切名称创建一个组。

    ![创建管理员用户组](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. 让其他人了解使用此组授予管理权限在 Azure AD 域服务，请添加此组的描述。

8. 在创建组之后，单击以查看此组的属性组的名称。 若要将用户添加为该组的成员，请单击底部面板上的**添加成员**按钮。

    ![添加组成员按钮](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. 在**添加成员**对话框中，选择应是此组的成员并完成后，请选中该复选框的用户。

    ![将用户添加到管理员组](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>任务 2︰ 创建或选择 Azure 的虚拟网络
下一步的配置任务是[创建或选择 Azure 的虚拟网络](active-directory-ds-getting-started-vnet.md)。
