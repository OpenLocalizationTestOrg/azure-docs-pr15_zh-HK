<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 将 RHEL 虚拟机加入到管理域 |Microsoft Azure"
    description="Red Hat 企业 Linux 虚拟机加入 Azure AD 域服务"
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

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>将 Red Hat 企业 Linux 7 虚拟机加入到管理域
这篇文章演示了如何将 Red Hat 企业 Linux (RHEL) 7 虚拟机加入到 Azure AD 域服务托管域。

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat 企业 Linux 虚拟机的资源调配
执行以下步骤来配置 RHEL 7 虚拟机使用 Azure 的门户。

1. 登录到[Azure 的门户](https://portal.azure.com)。

    ![Azure 门户的仪表板](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. 在左窗格中单击**新建**，键入搜索栏的**Red Hat** ，如下面的屏幕快照中所示。 Red Hat 企业 Linux 的条目显示在搜索结果中。 单击**红帽企业 Linux 7.2**。

    ![在结果中选择 RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. 在**内容**窗格中的搜索结果中应列出 Red Hat 企业 Linux 7.2 图像。 单击以查看有关虚拟机映像的详细信息的**Red Hat 企业 Linux 7.2** 。

    ![在结果中选择 RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. 在**Red Hat 企业 Linux 7.2**窗格，您应看到有关虚拟机映像的详细信息。 在**选择一种部署模式**下拉列表中，选择**经典**。 然后单击**创建**按钮。

    ![查看图像详细信息](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. 在**创建 VM**的窗格中，输入新的虚拟机**的主机名**。 此外指定**用户名**字段和**密码**的本地管理员的用户名称。 您还可以选择使用 SSH 密钥进行身份验证的本地管理员用户。 选择**定价层**的虚拟机。

    ![创建虚拟机的基本详细信息](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. 单击**可选配置**。 在**可选配置**窗格上，单击**网络**。

    ![创建虚拟机-配置虚拟网络](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. 这将打开一个名为**网络**的窗格。 在**网络**窗格上，单击以选择应在其中部署 Linux 虚拟机的虚拟网络的**虚拟网络**。 这将打开**虚拟网络**窗格。 在**虚拟网络**窗格上，选择**使用现有的虚拟网络**选项。 然后选择 Azure AD 域服务是可用的虚拟网络。 在此示例中，我们选择在 'MyPreviewVNet' 虚拟网络。

    ![创建虚拟机-选择虚拟网络](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. 在**可选配置**窗格上，单击**确定**按钮。

    ![创建虚拟机的虚拟网络选择](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. 现在您就可以创建虚拟机。 在**创建 VM**窗格中，单击**创建**按钮。

    ![创建虚拟机-准备就绪](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. 基于 RHEL 7.2 图像的新虚拟机的部署应开始。

  ![创建虚拟机的部署已启动](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. 几分钟后，该虚拟机应部署成功，并已准备好使用。

  ![创建虚拟机的部署](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>远程连接到该新调配的 Linux 虚拟机
Azure 中提供了 RHEL 7.2 虚拟机。 下一步的任务是远程连接到虚拟机。

**连接到 RHEL 7.2 虚拟机**请按照本文[如何登录到运行 Linux 的虚拟机](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)中的说明进行操作。

其余的步骤假定您使用 PuTTY SSH 客户机连接到 RHEL 虚拟机。 有关详细信息，请参阅[PuTTY 下载页面](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

1. 打开的 PuTTY 程序。

2. 输入新建 RHEL 虚拟机的**主机名**。 在此示例中，我们的虚拟机都具有主机名 contoso rhel.cloudapp.net。 如果您不确定您的虚拟机的主机名，请参阅在 Azure 的门户网站上的 VM 仪表板。

    ![PuTTY 连接](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. 登录到虚拟机使用的虚拟机创建时，您指定的本地管理员凭据。 在此示例中，我们使用本地管理员帐户"mahesh"。

    ![PuTTY 登录](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>在 Linux 虚拟机上安装必需的软件包
连接到虚拟机之后下, 一步任务是安装软件包所需的域加入虚拟机上。 执行以下步骤︰

1. **安装 realmd:**Realmd 包用于加入域。 在 PuTTY 终端，请键入以下命令︰

    sudo yum 安装 realmd

    ![安装 realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    几分钟后，应在虚拟机上安装 realmd 软件包。

    ![realmd 安装](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **安装 sssd:**Realmd 包取决于 sssd 来执行域联接操作。 在 PuTTY 终端，请键入以下命令︰

    sudo yum 安装 sssd

    ![安装 sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    几分钟后，应在虚拟机上安装 sssd 软件包。

    ![realmd 安装](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **安装 kerberos:**在 PuTTY 终端，请键入以下命令︰

    sudo yum 安装 krb5 工作站 krb5 库

    ![Kerberos 安装](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    几分钟后，应在虚拟机上安装 realmd 软件包。

    ![Kerberos 安装](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>将 Linux 虚拟机加入到托管域
现在 Linux 虚拟机上安装了所需的程序包下, 一步的任务是将虚拟机加入到托管域。

1. 发现的 AAD 域服务托管的域。 在 PuTTY 终端，请键入以下命令︰

    sudo 领域发现 CONTOSO100.COM

    ![领域发现](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    如果**领域发现**找不到您的托管的域，确保从虚拟机 (尝试 ping) 可访问域。 还要确保到托管的域位于同一虚拟网络实际上部署虚拟机。

2. 初始化 kerberos。 在 PuTTY 终端，键入下面的命令。 确保您指定属于 AAD DC 管理员组的用户。 只有这些用户可以将计算机加入到托管域。

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    确保您指定的域名以大写字母的形式，其他 kinit 失败。

3. 将计算机加入到域中。 在 PuTTY 终端，键入下面的命令。 指定在上一步 (kinit) 中指定的同一用户。

    sudo 领域联接 — 详细的 CONTOSO100.COM U'bob@CONTOSO100.COM'

    ![领域联接](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

您应该收到一条消息 （"成功地注册机领域中"） 机器时成功加入到托管域。


## <a name="verify-domain-join"></a>验证域联接
您可以快速验证是否计算机成功加入到托管域。 连接到新域加入 RHEL VM 使用 SSH 和域用户帐户，然后检查是否已正确解决用户帐户。

1. 在 PuTTY 终端，键入下面的命令以连接到新域加入 RHEL 虚拟机使用 SSH。 使用域帐户属于托管的域 (例如，'bob@CONTOSO100.COM'在这种情况。)

    ssh-l bob@CONTOSO100.COM contoso rhel.cloudapp.net

2. 在 PuTTY 终端，键入下面的命令以查看主目录在正确初始化。

    pwd

3. 在 PuTTY 终端，键入下面的命令以查看是否被正确地解析的组成员身份。

    标识

这些命令的示例输出如下所示︰

![验证域联接](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>加入域的疑难解答
请参阅[故障排除域联接](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join)项目。


## <a name="related-content"></a>相关的内容
- [Azure AD 域服务-快速入门指南](./active-directory-ds-getting-started.md)

- [将 Windows Server 虚拟机加入到 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-windows-vm.md)

- [如何登录到运行 Linux 的虚拟机](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)。

- [Kerberos 安装](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [红帽企业 Linux 7-Windows 集成指南](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
