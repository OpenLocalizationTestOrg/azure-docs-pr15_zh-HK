<properties
   pageTitle="Azure AD 连接︰ 自动升级 |Microsoft Azure"
   description="本主题介绍在 Azure AD 连接同步的内置自动升级功能。"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD 连接︰ 自动升级
与生成 1.1.105.0 （发布 2 月 2016年） 引入了此功能。

## <a name="overview"></a>概述
确保您的 Azure AD 连接安装始终最新的从未如此容易与**自动升级**功能。 快速安装默认情况下启用此功能和目录同步升级。 发布新版本时，会自动升级您的安装。

默认情况下，为以下启用自动升级︰

- 快速设置安装和目录同步升级。
- 使用 SQL Express LocalDB，即快速设置始终使用。 目录同步与 SQL Express 还使用 LocalDB。
- AD 帐户是默认的 MSOL_ 帐户创建的快速设置和目录同步。
- 中元节有少于 100000 个对象。

可以使用 PowerShell cmdlet 查看当前状态的自动升级`Get-ADSyncAutoUpgrade`。 它具有以下状态︰

状态 | 注释
---- | ----
启用 | 启用了自动升级。
挂起 | 仅系统设置。 系统将不再有资格接受自动升级。
已禁用 | 已禁用自动升级。

您可以**启用**和**禁用**与之间更改`Set-ADSyncAutoUpgrade`。 只有系统应设置**挂起**的状态。

自动升级升级基础结构使用 Azure AD 连接的状况。 对于工作的自动升级，请确保 Url 中打开您的代理服务器的**Azure AD 连接健康** [Office 365 的 Url 和 IP 地址范围](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)中所述。

如果在服务器上运行**同步服务管理器**用户界面，然后升级挂起，直到用户界面被关闭。

## <a name="troubleshooting"></a>故障排除
如果您连接的安装不会升级本身像预期的那样，然后按照以下步骤来找出什么可能是错误的。

首先，您不应期望自动升级来尝试新版本发布的第一天。 所以别害怕如果不立即升级您的安装尝试升级之前没有故意的随机性。

如果您认为事情不是这样的然后第一次运行`Get-ADSyncAutoUpgrade`以确保启用自动升级。

然后，请确保您的代理服务器或防火墙中打开所需的 Url。 自动更新使用 Azure AD 连接运行状况的[概述](#overview)中所述。 如果您使用代理服务器，请确保已配置为使用[代理服务器](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)运行状况。 此外测试[健康连接](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)到 Azure 的广告。

通过对 Azure AD 验证连接，就可以观察多的日志事件。 启动事件查看器并在**应用程序**事件日志中查找。 添加**AD 连接升级 Azure**源事件日志筛选器和事件的 id 范围**300 399**。  
![事件日志筛选器自动升级](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

现在您可以看到多与自动升级的状态相关联的日志事件。  
![事件日志筛选器自动升级](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

结果代码都有一个前缀与状态的概览。

结果代码前缀 | 说明
--- | ---
成功 | 安装已成功升级。
UpgradeAborted | 临时情况停止升级。 将再次重试，期望它以后成功。
UpgradeNotSupported | 系统具有阻止了自动升级的系统配置。 将重试的状态会更改，但预期是必须手动升级系统。

这里是您找到最常见的消息的列表。 它不会列出所有，但结果消息应该清楚有什么问题。

结果消息 | 说明
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | 无法写入注册表。
UpgradeAbortedInsufficientDatabasePermissions | 内置管理员组没有对数据库的权限。 手动升级到最新版本的 Azure AD 连接来解决这个问题。
UpgradeAbortedInsufficientDiskSpace | 没有足够的磁盘空间以支持升级。
UpgradeAbortedSecurityGroupsNotPresent | 未能找到并解决同步引擎使用的所有安全组。
UpgradeAbortedServiceCanNotBeStarted | NT 服务**Microsoft Azure AD 同步**启动失败。
UpgradeAbortedServiceCanNotBeStopped | NT 服务**Microsoft Azure AD 同步**停止失败。
UpgradeAbortedServiceIsNotRunning | **Microsoft Azure AD 同步**NT 服务未在运行。
UpgradeAbortedSyncCycleDisabled | 在[计划程序](active-directory-aadconnectsync-feature-scheduler.md)中的 SyncCycle 选项已被禁用。
UpgradeAbortedSyncExeInUse | [同步服务管理器用户界面](active-directory-aadconnectsync-service-manager-ui.md)是在服务器上打开的。
UpgradeAbortedSyncOrConfigurationInProgress | 运行安装向导或同步日程安排超出了计划。
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | 您有您自己的自定义规则添加到配置中。
UpgradeNotSupportedDeviceWritebackEnabled | 已启用[设备的写回](active-directory-aadconnect-feature-device-writeback.md)功能。
UpgradeNotSupportedGroupWritebackEnabled | 已启用[组的写回](active-directory-aadconnect-feature-preview.md#group-writeback)功能。
UpgradeNotSupportedInvalidPersistedState | 安装未明确设置或目录同步升级。
UpgradeNotSupportedMetaverseSizeExceeeded | 中元节有 10 万多个对象。
UpgradeNotSupportedMultiForestSetup | 您要连接到多个目录林。 Express 安装程序只能连接到一个目录林。
UpgradeNotSupportedNonLocalDbInstall | 不使用 SQL Server Express LocalDB 数据库。
UpgradeNotSupportedNonMsolAccount | [AD 连接器帐户](active-directory-aadconnect-accounts-permissions.md#active-directory-account)不是默认的 MSOL_ 帐户了。
UpgradeNotSupportedStagingModeEnabled | 服务器设置是在[调试模式](active-directory-aadconnectsync-operations.md#staging-mode)。
UpgradeNotSupportedUserWritebackEnabled | 已启用的[用户写回](active-directory-aadconnect-feature-preview.md#user-writeback)功能。

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
