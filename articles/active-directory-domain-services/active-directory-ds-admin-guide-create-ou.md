<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 管理指南 |Microsoft Azure"
    description="在 Azure AD 域服务管理域中创建组织单位 (OU)"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>在 Azure AD 域服务管理域中创建组织单位 (OU)
Azure AD 域服务托管的域包含两个分别名为 AADDC 计算机和 AADDC 用户的内置容器。 AADDC 计算机容器已加入到托管域的所有计算机的计算机对象。 AADDC 用户容器中 Azure AD 租户包括用户和组。 有时，可能需要在部署工作负载管理域上创建服务帐户。 为此目的，可以在管理域中创建自定义组织单位 (OU) 并创建在该 OU 中的服务帐户。 这篇文章演示了如何在托管域中创建一个 OU。


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>用于远程管理加入域的虚拟机上安装 AD 管理工具
可以使用熟悉的 Active Directory 管理工具如活动目录管理中心 (ADAC) 或 AD PowerShell 远程管理 azure AD 域服务托管的域。 租户管理员没有连接上通过远程桌面管理的域的域控制器的权限。 管理托管的域，请加入到托管域的虚拟机上安装 AD 管理工具的功能。 请参考文说明[管理 Azure AD 域服务管理域](active-directory-ds-admin-guide-administer-domain.md)。

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>在托管的域中创建组织单位
现在，广告管理工具安装在加入域的虚拟机，我们可以使用这些工具在受管理的域中创建组织单位。 执行以下步骤︰

> [AZURE.NOTE] 只有 AAD DC 管理员组的成员具有所需的权限，若要创建自定义的 OU。 请确保您为此组所属的用户执行以下步骤。

1. 从开始屏幕中，单击**管理工具**。 您应该看到在虚拟机上安装的 AD 管理工具。

    ![在服务器上安装管理工具](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. 单击**Active Directory 管理中心**。

    ![Active Directory 管理中心](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. 若要查看域，请单击左窗格 (例如，contoso100.com) 中的域名。

    ![ADAC-视图域](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. 在右侧**的任务**窗格中，在域节点下，单击**新建**。 在此示例中，我们在右侧**的任务**窗格上的 contoso100(local) 节点下，单击**新建**。

    ![ADAC-新的 OU](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. 您会看到选项以创建一个组织单位。 单击以启动对话框中**创建的组织单位**的**组织单位**。

6. 在**创建组织单位**对话框中，指定新的 OU**名称**。 提供该 OU 的简短说明。 对于 OU，您还可以设置**管理者**字段。 若要创建自定义 OU，请单击**确定**。

    ![ADAC-创建 OU 对话框](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. 新创建的 OU 现在应显示 AD 管理中心 (ADAC) 中。

    ![ADAC 的 OU 创建](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>新创建的 Ou 的权限/安全性
默认情况下，创建自定义 OU 的用户 （AAD DC 管理员组的成员） 被授予 ou 的管理权限 （完全控制）。 然后，用户可以继续操作并将权限授予其他用户或随意 AAD DC 管理员组。 在下面的屏幕快照，用户可以看到'bob@domainservicespreview.onmicrosoft.com'谁创建了新的 MyCustomOU 部门被授予完全控制。

 ![ADAC-新 OU 安全](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>管理自定义 Ou 的说明
现在，您已经创建了自定义 OU，您可以继续操作并在此 OU 中创建的用户、 组、 计算机和服务帐户。 不能移动的用户或组从 AAD DC 用户 OU 到自定义 Ou。

> [AZURE.WARNING] Azure AD 租户中没有用户帐户、 组、 服务帐户和您在自定义 Ou 下创建的计算机对象。 换句话说，使用 Azure 广告图形 API 或 Azure AD 用户界面中不显示这些对象。 这些对象在 Azure AD 域服务托管域中才可用。


## <a name="related-content"></a>相关的内容

- [管理 Azure AD 域服务管理的域](active-directory-ds-admin-guide-administer-domain.md)

- [Active Directory 管理中心︰ 快速入门](https://technet.microsoft.com/library/dd560651.aspx)

- [服务帐户的逐步式指南](https://technet.microsoft.com/library/dd548356.aspx)
