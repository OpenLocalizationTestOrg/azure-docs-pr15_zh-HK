<properties
    pageTitle="Azure 的虚拟网络的 azure AD 域服务︰ 更新 DNS 设置 |Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure AD 域服务的 Azure 的虚拟网络的 DNS 更新设置

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>任务 4︰ 更新 Azure 的虚拟网络的 DNS 设置
在前面的配置任务，已成功启用 Azure AD 域服务为您的目录。 下一步的任务是确保在虚拟网络中的计算机可以连接并使用这些服务。 更新虚拟网络的 DNS 服务器设置，使其指向 Azure AD 域服务是在虚拟网络中可用的两个 IP 地址。

> [AZURE.NOTE] Azure AD 域服务显示在您的目录，**配置**选项卡上启用目录的 Azure AD 域服务后，记下 IP 地址。

执行下面的配置步骤，以更新 DNS 服务器设置为已启用 Azure AD 域服务的虚拟网络。

1. 导航到**Azure 的传统门户网站**([https://manage.windowsazure.com](https://manage.windowsazure.com))。

2. 选择左边窗格中的**网络**节点。

    ![虚拟网络节点](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. 在**虚拟网络**选项卡，选择启用了 Azure AD 域服务以查看其属性的虚拟网络。

4. 单击**配置**选项卡。

    ![虚拟网络节点](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. 在**DNS 服务器**部分中，输入 Azure AD 域服务的 IP 地址。

6. 请确保您输入的 IP 地址显示在您的目录**配置**选项卡上的**域服务**部分。

7. 若要保存该虚拟网络的 DNS 服务器设置，请单击任务窗格底部的页上的**保存**。

   ![更新 DNS 服务器设置为虚拟网络。](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] 在更新之后将 DNS 服务器设置为虚拟网络，可能需要一段时间的虚拟机网络以获取更新的 DNS 配置。 如果虚拟机不能连接到域，您可以刷新 DNS 缓存 （如。 ipconfig /flushdns) 在虚拟机上。 此命令将强制刷新虚拟机的 DNS 设置。


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>任务 5-启用密码同步的 Azure AD 域服务
下一步的配置任务是[启用密码同步的 Azure AD 域服务](active-directory-ds-getting-started-password-sync.md)。
