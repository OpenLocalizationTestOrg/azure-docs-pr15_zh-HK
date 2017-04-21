<properties
    pageTitle="Azure AD 连接︰ 入门使用快速设置 |Microsoft Azure"
    description="了解如何下载、 安装并运行 Azure AD 连接的安装向导。"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>要开始使用 Azure AD 连接使用快速设置
有一个单林拓扑和[密码同步](../active-directory-aadconnectsync-implement-password-synchronization.md)进行身份验证时使用 azure AD 连接**快速设置**。 **快速设置**是默认选项，用于最常见部署方案。 您是只短伸缩以扩展到云上部署目录。

在开始安装 Azure AD 连接之前，请确保下载[Azure AD 连接](http://go.microsoft.com/fwlink/?LinkId=615771)和中必备步骤以完成[Azure AD 连接︰ 硬件和系统必备组件](../active-directory-aadconnect-prerequisites.md)。

如果明确设置与您的拓扑结构不匹配，请参阅[相关的文档](#related-documentation)用于其他方案。

## <a name="express-installation-of-azure-ad-connect"></a>Azure AD 连接的快速安装
您可以查看操作中的[视频](#videos)部分中的步骤。

1. 以您想要安装在 Azure AD 连接的服务器的本地管理员身份登录。 您想要同步服务器的服务器上，应执行此操作。
2. 找到并双击**AzureADConnect.msi**。
3. 在欢迎屏幕上，选中同意许可条款，然后单击**继续**。  
4. 在快速设置屏幕中，单击**使用快速设置**。  
![欢迎使用 Azure AD 连接](./media/active-directory-aadconnect-get-started-express/express.png)
5. 在连接到 Azure 广告屏幕上，输入用户名和密码的全局管理员 Azure 广告。 单击**下一步**。  
![连接到 Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png)如果您收到错误，并且有连接问题，请参阅[连接问题的疑难解答](../active-directory-aadconnect-troubleshoot-connectivity.md)。
6. 在连接到 AD DS 屏幕，请输入企业管理员帐户的用户名和密码。 您可以输入域部分 NetBios 或 FQDN 格式，即 FABRIKAM\administrator 或 fabrikam.com\administrator。 单击**下一步**。  
![连接到 AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. [**Azure 广告登录配置**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration)页只显示是否您没有完成[系统必备组件](../active-directory-aadconnect-prerequisites.md)在[验证您的域](../active-directory-add-domain.md)。
![未验证的域](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
如果您看到此页面，然后查看标记**不会添加**并**不验证**每个域。 请确保已在 Azure AD 验证使用这些域。 当您验证您的域，请单击刷新符号。
8. 在准备配置屏幕中，单击**安装**。
    - （可选） 在配置页面已准备好，可以取消选中**启动同步进程一旦完成配置**复选框。 如果您想要执行其他配置，如[筛选](../active-directory-aadconnectsync-configure-filtering.md)，应取消选中此复选框。 如果取消选中此选项，向导将配置同步，但离开禁用计划程序。 它不会运行通过[重新运行安装向导](../active-directory-aadconnectsync-installation-wizard.md)的方式手动将其启用。
    - 如果您在部署 Active Directory 中交换，则还可以启用[**Exchange 混合部署**](https://technet.microsoft.com/library/jj200581.aspx)选项。 如果您计划让 Exchange 邮箱云和本地同时在两个，启用此选项。
![可以配置 Azure AD 连接](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. 安装完成后，单击**退出**。
10. 在安装完成后，注销并重新登录才能使用同步服务管理器或同步规则编辑器。

## <a name="videos"></a>视频

使用快速安装的视频，请参阅︰

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>下一步行动
现在，您已经安装的 Azure AD 连接您可以[验证安装和分配许可证](../active-directory-aadconnect-whats-next.md)。

了解有关安装启用了这些功能︰[自动升级](../active-directory-aadconnect-feature-automatic-upgrade.md)、[防止意外删除](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)，和[Azure AD 连接的状况](../active-directory-aadconnect-health-sync.md)。

了解更多有关这些常见的主题︰[调度程序和如何触发同步](../active-directory-aadconnectsync-feature-scheduler.md)。

了解有关[将您的内部标识使用 Azure Active Directory 集成](../active-directory-aadconnect.md)。

## <a name="related-documentation"></a>相关的文档

主题 |  
--------- | ---------
Azure AD 连接概述 | [与 Azure Active Directory 集成您的内部标识](../active-directory-aadconnect.md)
安装使用的自定义的设置 | [Azure AD 连接的自定义安装](active-directory-aadconnect-get-started-custom.md)
从目录同步升级 | [从 Azure AD 同步工具 （目录同步） 升级](active-directory-aadconnect-dirsync-upgrade-get-started.md)
用于安装的帐户 | [有关 Azure AD 连接的帐户和权限的详细信息](active-directory-aadconnect-accounts-permissions.md)
