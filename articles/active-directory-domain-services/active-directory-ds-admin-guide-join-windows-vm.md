<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 将一个 Windows 服务器的虚拟机加入到管理域 |Microsoft Azure"
    description="将 Windows Server 虚拟机加入到 Azure AD 域服务"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>将 Windows Server 虚拟机加入到管理域

> [AZURE.SELECTOR]
- [Azure 经典门户-Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell 的窗口](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

本文介绍了如何加入到 Azure AD 域服务托管域，使用 Azure 的经典门户运行 Windows Server 2012 R2 的虚拟机。


## <a name="step-1-create-the-windows-server-virtual-machine"></a>步骤 1︰ 创建 Windows 服务器的虚拟机
按照[创建虚拟机运行 Windows Azure 的传统门户网站在](../virtual-machines/virtual-machines-windows-classic-tutorial.md)本教程中的说明。 请务必确保这个新创建的虚拟机加入到相同的虚拟网络启用了 Azure AD 域服务。 不启用快速创建选项可以将虚拟机与虚拟网络联接。 因此，您需要使用剪辑库中的选项来创建虚拟机。

执行以下步骤来创建 Windows 虚拟机加入虚拟网络中启用了 Azure AD 域服务。

1. 在 Azure 经典门户中，在底部窗口中的命令栏上单击**新建**。

2. 在**计算**，单击**虚拟机**，然后单击**剪辑库中**。

3. 第一个屏幕可以为您的虚拟机可用的图像中**选择图像**。 选取适当的图像。

    ![选择图像](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. 第二个屏幕，您可以选择计算机名称、 大小、 和管理权限的用户名和密码。 使用层和运行您的应用程序或工作负载所需的大小。 您在此处选择的用户名已在计算机上的本地管理员用户。 不要输入下面的域用户帐户凭据。

    ![配置虚拟机](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. 第三个屏幕允许您配置的网络、 存储和可用性的资源。 请确保您选择启用了**区域中的相似性组/虚拟网络**下拉列表中的 Azure AD 域服务的虚拟网络。 **云服务的 DNS 名称**指定为相应的虚拟机。

    ![选择虚拟机的虚拟网络](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    请确保您将虚拟机加入到已启用 Azure AD 域服务的相同的虚拟网络。 因此，虚拟机可以看到域并执行任务，例如加入域。 如果您选择在不同的虚拟网络中创建虚拟机，连接到虚拟网络中启用了 Azure AD 域服务的虚拟网络。

6. 第四个屏幕允许您安装虚拟机代理配置某些可用的扩展。

    ![完成](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. 创建虚拟机后，传统门户将列出新的虚拟机的**虚拟机**节点下。 同时自动启动虚拟机和云服务，并且其状态被列为**运行**。

    ![虚拟机已启动并正在运行](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>步骤 2︰ 连接到 Windows 服务器的虚拟机使用本地管理员帐户
现在，我们将连接到新创建 Windows Server 虚拟机，加入到域。 使用时创建虚拟机，可以连接到该指定的本地管理员凭据。

执行以下步骤来连接到虚拟机。

1. 导航到**虚拟机**中经典的门户网站的节点。 选择您在步骤 1 中创建的虚拟机并单击**连接**窗口底部的命令栏上。

    ![连接到 Windows 虚拟机](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. 经典的门户会提示您打开或保存文件的扩展名为.rdp，用来连接到虚拟机。 单击以打开该文件，下载完成。

3. 在登录提示符下，输入您的**本地管理员凭据**，指定在创建虚拟机。 例如，我们已经在此示例中使用 localhost\mahesh。

此时，您应该登录到新创建的 Windows 虚拟机以使用本地管理员凭据。 下一步是将虚拟机加入到域。


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>第 3 步︰ 连接到 AAD DS 托管域 Windows Server 虚拟机
执行以下步骤来加入到 AAD DS 管理域的 Windows 服务器的虚拟机。

1. 连接到 Windows 服务器，如步骤 2 所示。 从开始屏幕中，打开**服务器管理器**。

2. 在服务器管理器窗口的左窗格中，单击**本地服务器**。

    ![在虚拟机上启动服务器管理器](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. 在**属性**部分中单击**工作组**。 在**系统属性**属性页中单击**更改**加入到域。

    ![系统属性页](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. 指定 Azure AD 域服务的域名管理域中的**域**文本框，然后单击**确定**。

    ![指定要加入的域](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. 提示您输入您的凭据来加入到域中。 请确保该您**指定为属于 AAD DC 管理员组用户的凭据**。 只有此组的成员有权把计算机加入到托管域。

    ![指定凭据，以便加入域](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. 您可以用下列方法之一指定凭据︰

    - UPN 格式︰ 指定用户帐户的 UPN 后缀 Azure AD 中进行配置。 在此示例中，bob 的用户的 UPN 后缀为'bob@domainservicespreview.onmicrosoft.com'。

    - SAMAccountName 格式︰ 可以以 SAMAccountName 格式指定帐户名。 在此示例中，bob 的用户将需要输入 CONTOSO100\bob。

        > [AZURE.NOTE] **我们建议使用 UPN 格式指定的凭据。** SAMAccountName 可能会自动生成用户的 UPN 前缀是否很长 （例如，joereallylongnameuser）。 如果多个用户具有相同的 UPN 前缀 (例如，bob) 在 Azure AD 租户，它们的 SAMAccountName 格式可能是自动生成的服务。 在这些情况下，UPN 格式可用于可靠地登录到域中。

7. 成功加入域后，您将看到下面的消息，欢迎您加入该域。 重新启动虚拟机完成域联接操作。

    ![欢迎到域](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>加入域的疑难解答
### <a name="connectivity-issues"></a>连接问题
如果虚拟机是找不到域，请参阅下面的疑难解答步骤︰

- 确保虚拟机连接到的相同的虚拟网络已启用域中的服务。 如果不是，虚拟机无法与此域连接，因此无法加入到域中。

- 如果虚拟机连接到另一个虚拟的网络，请确保该虚拟网络相连的虚拟网络已启用域服务的。

- 尝试 ping 使用托管的域 (例如，ping contoso100.com) 的域名的域。 如果你无法做到这一点，尝试 ping 启用 Azure AD 域服务的位置的域显示在页上的 IP 地址 (例如，ping 10.0.0.4)。 如果您不能 ping IP 地址而不是在域，可能未正确配置 DNS。 您可能不具有配置域的 IP 地址与虚拟网络的 DNS 服务器。

- 请尝试刷新虚拟机 (ipconfig /flushdns) 上的 DNS 解析程序缓存。

如果您要求加入到域的凭据的对话框中，您没有连接问题。


### <a name="credentials-related-issues"></a>凭据相关问题
如果您在遇到使用凭据的问题，并且无法加入到域中，则请参阅以下步骤。

- 请尝试使用 UPN 格式指定凭据。 如果有多个具有相同的 UPN 前缀，您组织中的用户或 UPN 前缀过长 SAMAccountName 为您的帐户可能会自动生成。 因此，您的帐户的 SAMAccountName 格式可能不同于您期望或内部域中使用。

- 尝试使用属于计算机加入托管域 AAD DC 管理员组的用户帐户的凭据。

- 确保您有依照入门指南 》 中概述的步骤[启用密码同步](active-directory-ds-getting-started-password-sync.md)。

- 确保您使用的用户的 UPN，Azure AD 中进行配置 (例如，'bob@domainservicespreview.onmicrosoft.com')进行登录。

- 请确保您有等待足够长的时间完成的入门指南中指定的密码同步。


## <a name="related-content"></a>相关的内容

- [Azure AD 域服务-快速入门指南](./active-directory-ds-getting-started.md)

- [管理 Azure AD 域服务管理的域](./active-directory-ds-admin-guide-administer-domain.md)
