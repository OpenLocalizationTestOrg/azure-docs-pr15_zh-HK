<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 管理管理的域 |Microsoft Azure"
    description="管理 Azure Active Directory 域服务托管的域"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>管理 Azure Active Directory 域服务管理的域
本文介绍了如何管理 Azure 活动目录 (AD) 域服务托管的域。


## <a name="before-you-begin"></a>在开始之前
要执行本文中列出的任务，您需要︰

1. 有效**Azure 的订阅**。

2. **Azure 的广告目录**中的任何一个与内部目录或仅云目录同步。

3. Azure 的广告目录，必须启用**azure AD 域服务**。 如果您还没有这样做，请按照[入门指南](./active-directory-ds-getting-started.md)中列出的所有任务。

4. 从中您管理 Azure AD 域服务托管的域**加入域的虚拟机**。 如果没有虚拟机，请按照文[加入 Windows 虚拟机到托管域](./active-directory-ds-admin-guide-join-windows-vm.md)中列出的所有任务。

5. 需要在您的目录，以管理您的托管的域**属于 AAD DC 管理员组的用户帐户**的凭据。

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>您可以在管理域执行的管理任务
AAD DC 管理员组的成员被授予托管域上的特权，使他们能够执行下列任务︰

- 将计算机加入到托管域。

- 在管理域中配置内置 AADDC 计算机和 AADDC 用户容器的 GPO。

- 管理管理域的 DNS。

- 创建和管理自定义托管域上的组织单位 (Ou)。

- 获得管理访问权限的计算机加入到托管域。


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>在管理域没有管理员权限
由微软补丁、 监视和执行备份等活动管理域。 因此，域处于锁定状态，并且您有权在域中执行某些管理任务。 您不能执行的任务的一些示例如下。

- 您无权管理域的域管理员或企业管理员权限。

- 不能扩展托管域的架构。

- 您不能连接到使用远程桌面管理域的域控制器。

- 不能添加到托管域的域控制器。


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>任务 1-规定加入域的 Windows 服务器虚拟机来远程管理管理的域
可以使用熟悉的 Active Directory 管理工具如活动目录管理中心 (ADAC) 或 AD PowerShell 管理 azure AD 域服务托管的域。 租户管理员没有连接上通过远程桌面管理的域的域控制器的权限。 因此，AAD DC 管理员组的成员可以管理远程使用 AD 管理工具从 Windows 服务器/客户端计算机加入到托管域的托管的域。 AD 管理工具可以作为远程服务器管理工具 (RSAT) Windows 服务器和客户端计算机加入到托管域上的可选功能的一部分进行安装。

第一步是设置 Windows Server 虚拟机加入到托管域。 有关说明，请参阅文[加入 Windows Server 虚拟机到 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-windows-vm.md)。

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>远程客户端计算机 (例如，Windows 10) 从管理管理的域
此文章使用 Windows 服务器的虚拟机的说明进行操作来管理 AAD DS 管理域。 但是，您还可以选择使用 Windows 客户端 (例如，Windows 10) 虚拟机来执行此操作。

可以按照下面的说明在 TechNet 上的 Windows 客户端虚拟机上[安装远程服务器管理工具 (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) 。


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>任务 2 的虚拟机上安装 Active Directory 管理工具
执行以下步骤来加入域的虚拟机上安装活动目录管理工具。 [有关安装和使用远程服务器管理工具](https://technet.microsoft.com/library/hh831501.aspx)的详细信息，请参阅 Technet。

1. 导航到**虚拟机**在 Azure 的传统门户网站中的节点。 选择任务 1 中创建的虚拟机并单击**连接**窗口底部的命令栏上。

    ![连接到 Windows 虚拟机](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. 经典的门户会提示您打开或保存文件的扩展名为.rdp，用来连接到虚拟机。 单击以打开该文件，下载完成。

3. 在登录提示符下，使用属于 AAD DC 管理员组的用户的凭据。 例如，我们使用'bob@domainservicespreview.onmicrosoft.com'在我们的例子。

4. 从开始屏幕中，打开**服务器管理器**。 在服务器管理器窗口的中央窗格中，单击**添加角色和功能**。

    ![在虚拟机上启动服务器管理器](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. 在**开始之前****添加角色和功能向导**的页，单击**下一步**。

    ![在开始页之前](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. 在**安装类型**页上保留检查**基于角色或功能的安装**选项，并单击**下一步**。

    ![安装类型页](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. 在**选择服务器**页上选择服务器池，从当前的虚拟机并单击**下一步**。

    ![服务器选择页](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. 在**服务器角色**页中，单击**下一步**。 我们跳过此页，因为我们不在服务器上安装的任何角色。

9. 在**功能**页面上单击以展开**远程服务器管理工具**节点，然后单击以展开**角色管理工具**节点。 从角色管理工具的列表中选择**AD DS 和 AD LDS 工具**功能。

    ![功能的页](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. 在**确认**页中，AD LDS 工具功能在虚拟机上的安装 AD 与单击**安装**。 功能安装成功完成时，单击**关闭**以退出**添加角色和功能**向导。

    ![确认页](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>任务 3-连接到并浏览托管的域
现在，广告管理工具安装在加入域的虚拟机，我们可以使用这些工具来浏览和管理管理的域。

> [AZURE.NOTE] 您需要是 AAD DC 管理员组的成员才能管理托管的域。

1. 从开始屏幕中，单击**管理工具**。 您应该看到在虚拟机上安装的 AD 管理工具。

    ![在服务器上安装管理工具](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. 单击**Active Directory 管理中心**。

    ![Active Directory 管理中心](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. 要浏览域，请单击左窗格中 (例如，contoso100.com) 中的域名。 请注意两个分别名为 AADDC 计算机和 AADDC 用户的容器。

    ![ADAC-视图域](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. 单击名为**AADDC 的用户**查看所有用户和组属于托管域的容器。 您应该会看到用户帐户和组从 Azure 广告租户显示了此容器中。 注意，在此示例中，名为 bob 的用户和一个名为 AAD DC 管理员组的用户帐户有此容器中。

    ![ADAC-域用户](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. 单击名为**AADDC 的计算机**查看连接到此管理域的计算机的容器。 您应看到当前的虚拟机加入到域的条目。 加入到 Azure AD 域服务托管域的所有计算机的计算机帐户存储在此 AADDC 计算机容器中。

    ![ADAC 的加入域的计算机](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>相关的内容

- [Azure AD 域服务-快速入门指南](./active-directory-ds-getting-started.md)

- [将 Windows Server 虚拟机加入到 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-windows-vm.md)

- [部署远程服务器管理工具](https://technet.microsoft.com/library/hh831501.aspx)
