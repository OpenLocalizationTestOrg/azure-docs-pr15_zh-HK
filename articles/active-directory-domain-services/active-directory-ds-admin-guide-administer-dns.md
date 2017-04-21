<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 管理 DNS 托管域 |Microsoft Azure"
    description="管理 Azure Active Directory 域服务托管域上的 DNS"
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

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>管理 Azure AD 域服务托管域上的 DNS
Azure Active Directory 域服务包括提供托管域 DNS 解析的 DNS （域名解析） 服务器。 有时，您可能需要在托管域上配置 DNS。 您可能需要创建未加入到域的计算机的 DNS 记录、 配置为负载平衡器的虚拟 IP 地址或设置外部 DNS 转发器。 由于这个原因，属于 AAD DC 管理员组的用户被授予 DNS 托管域上的管理特权。


## <a name="before-you-begin"></a>在开始之前
要执行本文中列出的任务，您需要︰

1. 有效**Azure 的订阅**。

2. **Azure 的广告目录**中的任何一个与内部目录或仅云目录同步。

3. Azure 的广告目录，必须启用**azure AD 域服务**。 如果您还没有这样做，请按照[入门指南](./active-directory-ds-getting-started.md)中列出的所有任务。

4. 从中您管理 Azure AD 域服务托管的域**加入域的虚拟机**。 如果没有虚拟机，请按照文[加入 Windows 虚拟机到托管域](./active-directory-ds-admin-guide-join-windows-vm.md)中列出的所有任务。

5. 需要在您的目录，以管理为您管理的域的 DNS 中**属于 AAD DC 管理员组的用户帐户**的凭据。

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>任务 1-规定加入域的虚拟机来远程管理受管理的域的 DNS
可以使用熟悉的 Active Directory 管理工具如活动目录管理中心 (ADAC) 或 AD PowerShell 远程管理 azure AD 域服务托管的域。 同样，可以使用 DNS 服务器管理工具远程管理受管理的域的 DNS。

在 Azure 的广告目录管理员没有连接上通过远程桌面管理的域的域控制器的权限。 AAD DC 管理员组的成员可以管理域使用远程 Windows 服务器/客户端计算机加入到管理域中的 DNS 服务器工具管理 DNS。 可以为远程服务器管理工具 (RSAT) Windows 服务器和客户端计算机加入到托管域上的可选功能的一部分安装 DNS 服务器工具。

第一项任务是设置 Windows Server 虚拟机加入到托管域。 有关说明，请参阅文[加入 Windows Server 虚拟机到 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-windows-vm.md)。


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>任务 2 的虚拟机上安装 DNS 服务器工具
执行以下步骤来加入域的虚拟机上安装 DNS 管理工具。 有关[安装和使用远程服务器管理工具](https://technet.microsoft.com/library/hh831501.aspx)的详细信息，请参阅 Technet。

1. 导航到**虚拟机**在 Azure 的传统门户网站中的节点。 选择任务 1 中创建的虚拟机并单击**连接**窗口底部的命令栏上。

    ![连接到 Windows 虚拟机](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. 经典的门户会提示您打开或保存文件的扩展名为.rdp，用来连接到虚拟机。 下载后，请单击文件。

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

9. 在**功能**页面上单击以展开**远程服务器管理工具**节点，然后单击以展开**角色管理工具**节点。 从角色管理工具的列表中选择**DNS 服务器工具**的功能。

    ![功能的页](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. 在**确认**页上，单击**安装**在虚拟机上安装 DNS 服务器工具的功能。 功能安装成功完成时，单击**关闭**以退出**添加角色和功能**向导。

    ![确认页](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>任务 3-启动 DNS 管理控制台来管理 DNS
既然上安装 DNS 服务器工具功能加入域的虚拟机，我们可以使用 DNS 工具来管理 DNS 托管的域上。

> [AZURE.NOTE] 您需要 AAD DC 管理员组的成员才能管理受管理的域的 DNS。

1. 从开始屏幕中，单击**管理工具**。 您应该看到在虚拟机上安装了**DNS**控制台。

    ![管理工具 — DNS 控制台](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. 单击**DNS**启动 DNS 管理控制台。

3. 在**连接到 DNS 服务器**对话框中，单击标题为**下列计算机**选项，输入托管域 (例如，contoso100.com) 的 DNS 域名称。

    ![DNS 控制台-连接到域](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. DNS 控制台连接到托管域。

    ![DNS 控制台的管理域](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. 您现在可以使用 DNS 控制台添加 DNS 条目的已启用 AAD 域服务的虚拟网络中的计算机。

> [AZURE.WARNING] 管理 DNS 使用 DNS 管理工具的管理域时要小心。 确保该您**不要删除或修改内置的 DNS 记录，由域中的域服务**。 内置的 DNS 记录包括域 DNS 记录、 名称服务器记录和其他记录用于直流位置。 如果您修改这些记录，域服务中断在虚拟的网络上。


请参阅[Technet 上的 DNS 工具文章](https://technet.microsoft.com/library/cc753579.aspx)有关管理 DNS 的详细信息。


## <a name="related-content"></a>相关的内容

- [Azure AD 域服务-快速入门指南](./active-directory-ds-getting-started.md)

- [将 Windows Server 虚拟机加入到 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-windows-vm.md)

- [管理 Azure AD 域服务管理的域](active-directory-ds-admin-guide-administer-domain.md)

- [DNS 管理工具](https://technet.microsoft.com/library/cc753579.aspx)
