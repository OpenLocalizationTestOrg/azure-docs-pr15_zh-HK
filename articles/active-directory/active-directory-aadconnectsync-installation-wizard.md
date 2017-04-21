<properties
    pageTitle="Azure AD 连接同步︰ 第二次运行安装向导 |Microsoft Azure"
    description="解释如何安装向导工作第二次运行它时。"
    keywords="Azure AD 连接安装向导允许您配置第二次运行它的维护设置"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD 连接同步︰ 第二次运行安装向导
首次运行 Azure AD 连接安装向导，它引导您完成配置您的安装方式。 如果您再次运行安装向导，它提供了用于维护选项。

在**Azure AD 连接**的名为开始菜单中，可以找到安装向导。

![「 开始 」 菜单](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

启动安装向导时，您会看到这些选项的页面︰

![与其他任务的列表页](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

如果使用 Azure AD 连接安装了 ADF，有更多的选项。 在[ADFS 管理](active-directory-aadconnect-federation-management.md#ad-fs-management)记录具有 ADF 中的附加选项。

选择任务之一，然后单击**下一步**继续。

> [AZURE.IMPORTANT] 在您打开安装向导，同步引擎中的所有操作都将被都挂起。 请确保您已完成配置更改时，就立即关闭安装向导。

## <a name="view-current-configuration"></a>查看当前配置
此选项使您可以快速查看您的当前配置选项。

![所有选项及其状态的列表页](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

单击**上一步**返回。 如果选择**退出**，关闭安装向导。

## <a name="customize-synchronization-options"></a>自定义同步选项
此选项用于对同步配置进行更改。 您会看到选项自定义配置安装路径中的一个子集。 即使最初使用快速安装，您将会看到此选项。

- [添加多个目录](active-directory-aadconnect-get-started-custom.md#connect-your-directories)。 删除一个目录，请参阅[删除连接器](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete)。
- [更改域和 OU 筛选](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)。
- 删除组筛选。
- [更改可选功能](active-directory-aadconnect-get-started-custom.md#optional-features)。

从最初安装的其他选项不能更改，且不可用。 这些选项包括︰

- 更改要使用的范围内和 sourceAnchor 的特性。
- 更改从不同的目录林中对象的联接方法。
- 启用基于组的筛选。

## <a name="refresh-directory-schema"></a>刷新目录架构
如果您更改架构之一您在部署中使用此选项 AD DS 林。 例如，您可能安装 Exchange 或升级到 Windows Server 2012 架构与设备对象。 在这种情况下，您需要指示 Azure AD 连接从 AD DS 中重新读取该架构并更新它的高速缓存。 此操作还将重新生成同步规则。 如果您添加 Exchange 架构为例，交换同步规则将添加到配置中。

当选中此选项时，列出了在配置中的所有目录。 可以保留默认设置和刷新所有林状结构或取消选择其中的一些。

![环境中所有目录的列表页](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>配置临时模式
此选项允许您启用和禁用服务器上的临时模式。 在[操作](active-directory-aadconnectsync-operations.md#staging-mode)中找不到有关临时模式和使用方式的详细信息。

如果转移当前被启用或禁用显示的选项︰  
![此外显示当前状态的转移模式的选项](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

要更改状态，请选择此选项和选择或取消选择该复选框。  
![此外显示当前状态的转移模式的选项](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>更改用户登录
此选项允许您更改密码同步从联盟或其他的方式解决。 不能更改为**未配置**。

有关此选项的详细信息，请参阅[用户登录](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method)。

## <a name="next-steps"></a>下一步行动

- 了解有关使用 Azure AD 连接同步[了解声明式](active-directory-aadconnectsync-understanding-declarative-provisioning.md)调配的配置模型。

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
