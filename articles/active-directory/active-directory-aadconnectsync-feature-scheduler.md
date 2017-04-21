<properties
   pageTitle="Azure AD 连接同步︰ 计划程序 |Microsoft Azure"
   description="本主题介绍在 Azure AD 连接同步的内置调度程序功能。"
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
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD 连接同步︰ 计划程序
本主题介绍在 Azure AD 连接同步中内置的计划程序 （也 同步引擎）。

与生成 1.1.105.0 （发布 2 月 2016年） 引入了此功能。

## <a name="overview"></a>概述
Azure AD 连接同步将同步您的内部目录使用调度程序中发生的更改。 有两个调度程序进程，一个用于密码同步，另一个对象/属性同步和维护任务。 本主题将介绍后一种。

在早期版本中的调度程序对象和属性的外部同步引擎，使用 Windows 任务计划程序或单独的 Windows 服务触发的同步过程。 计划程序是 1.1 版本内置同步引擎，并且允许某些自定义项。 新的默认同步频率为 30 分钟。

计划程序负责两项任务︰

- **同步周期**。 导入、 同步，并将更改导出过程。
- **维护任务**。 续订密钥和证书进行密码重置和设备注册服务 (DRS)。 清除旧的操作日志中的条目。

始终运行调度程序自身，但可以将它配置为仅运行一个或所有这些任务。 例如如果您需要有您自己的同步周期过程，则可以禁用该任务计划程序中的但仍然运行的维护任务。

## <a name="scheduler-configuration"></a>计划程序配置
若要查看您当前的配置设置，请转到 PowerShell 和运行`Get-ADSyncScheduler`。 它将显示类似下面这样︰

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

如果运行该 cmdlet 时，看到**cmdlet 的 sync 命令将不可用**，则不会加载 PowerShell 模块。 如果在域控制器或具有更高的 PowerShell 限制级别比默认设置的服务器上运行 Azure AD 连接，则可能发生此错误。 如果您看到此错误，然后运行`Import-Module ADSync`提供 cmdlet。

- **AllowedSyncCycleInterval**。 最常 Azure 的广告将使同步发生。 您不能比这更频繁地同步，仍会支持。
- **CurrentlyEffectiveSyncCycleInterval**。 当前正在生效时间表。 它将具有相同的 CustomizedSyncInterval 值 (如果设置) 如果不是比 AllowedSyncInterval 更频繁。 如果更改 CustomizedSyncCycleInterval，这将在下一同步周期后将生效。
- **CustomizedSyncCycleInterval**。 如果您希望 30 分钟运行任何其他频率而不是默认的计划程序，您将配置此设置。 计划程序上面图片中已设置改为运行的每个小时。 如果将其设置为一个值小于 AllowedSyncInterval，后者将使用。
- **NextSyncCyclePolicyType**。 增量或缩写。 定义如果下一次运行应该只处理增量更改，或如果下一次运行，应该执行完全导入并同步时，它会重新处理任何新的或更改的规则。
- **NextSyncCycleStartTimeInUTC**。 下一次计划程序将启动下一同步周期。
- **PurgeRunHistoryInterval**。 操作日志应保留的时间。 这些可以在同步服务管理器中查看。 默认设置为保留这些内容用于 7 天。
- **SyncCycleEnabled**。 指示如果调度程序正在运行同步，导入和导出进程作为其操作的一部分。
- **MaintenanceEnabled**。 如果启用了维护过程所示。 它将更新证书/密钥和清除操作日志。
- **IsStagingModeEnabled**。 显示是否启用[调试模式](active-directory-aadconnectsync-operations.md#staging-mode)。

您可以更改其中的某些设置与`Set-ADSyncScheduler`。 可以修改以下参数︰

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

计划程序配置存储在 Azure 的广告。 如果您有一台临时服务器，主服务器上的任何更改也将影响暂存服务器 （除了 IsStagingModeEnabled)。

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
语法︰`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d-天，HH-小时，毫米-分钟，ss-秒

示例︰`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
将计划程序来运行每 3 小时一次。

示例︰`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
将更改调度程序每天运行一次。

## <a name="start-the-scheduler"></a>启动计划程序
计划程序默认情况下将运行每隔 30 分钟。 在某些情况下您可能希望运行一个计划周期之间的同步周期，或者您需要运行一个不同的类型。

**增量同步周期**  
增量同步周期包括以下步骤︰

- 所有接头上的增量导入
- 所有接头上的增量同步
- 导出所有连接器

它可能是您必须紧急更改它必须立即同步这正是您需要手动运行一个周期。 如果您需要手动运行一个周期，然后从 PowerShell 运行`Start-ADSyncSyncCycle -PolicyType Delta`。

**完全同步周期**  
如果进行以下配置更改，您需要 （也运行一个完全同步周期 首次）︰

- 添加多个对象或属性将导入的源目录
- 对同步规则所做的更改
- 更改[筛选](active-directory-aadconnectsync-configure-filtering.md)，因此应包含不同数目的对象

如果您已进行了下列之一发生更改，则需要运行完全同步周期，因此同步引擎有机会重新进行合并的连接器空间。 完全同步周期包括以下步骤︰

- 所有接头上的完全导入
- 所有接头上的完全同步
- 导出所有连接器

要启动完全同步周期，请运行`Start-ADSyncSyncCycle -PolicyType Initial`PowerShell 提示符下。 这将启动完全同步周期。

## <a name="stop-the-scheduler"></a>停止计划程序
如果计划程序当前正在运行的同步周期可能需要将其停止。 例如，如果启动安装向导，则出现此错误︰

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

当运行同步周期时，您不能更改配置。 您可以等待，直到计划程序已完成该过程，但可以立即进行更改以便可以也停止。 停止当前的周期不是有害，仍未处理的任何更改将会处理与下一次运行。

1. 开始告诉调度程序停止其当前周期的 PowerShell cmdlet `Stop-ADSyncSyncCycle`。
2. 停止计划程序不会停止其当前任务的当前接头。 若要强制停止连接器，请执行以下操作︰ ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - 从开始菜单启动**同步服务**。 转到**连接器**、 突出显示与**运行**状态的连接器并选择**停止**操作。

计划程序仍处于活动状态，并将在下次有机会重新开始。

## <a name="custom-scheduler"></a>自定义计划程序
这一节中描述的 cmdlet 才可在生成[1.1.130.0](active-directory-aadconnect-version-history.md#111300)及更高版本。

如果内置的计划程序不能满足您的需求，您可以安排使用 PowerShell 的连接器。

### <a name="invoke-adsyncrunprofile"></a>调用 ADSyncRunProfile
您可以以这种方式连接器的启动配置文件︰

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

[同步服务管理器用户界面](active-directory-aadconnectsync-service-manager-ui.md)中找不到要使用的[连接器名称](active-directory-aadconnectsync-service-manager-ui-connectors.md)和[运行配置文件名称](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles)的名称。

![调用运行配置文件](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

`Invoke-ADSyncRunProfile` Cmdlet 是同步的即它不会返回控件直到连接器已完成操作的成功或出错。

当您计划您的连接器时，建议是它们按照以下顺序安排︰

1. （完全/增量）从内部目录，如活动目录导入
2. （完全/增量）导入从 Azure 的广告
3. （完全/增量）从本地目录，如 Active Directory 同步
4. （完全/增量）从 Azure 广告同步
5. 将导出到 Azure 的广告
6. 将导出到本地目录，如活动目录

如果查看内置的计划程序，则连接器将运行的顺序。

### <a name="get-adsyncconnectorrunstatus"></a>获得 ADSyncConnectorRunStatus
您还可以监视同步引擎空闲或忙碌。 如果同步引擎处于空闲状态，并且没有运行连接器，此 cmdlet 将返回空结果。 如果正在运行连接器，它将返回的连接器的名称。

```
Get-ADSyncConnectorRunStatus
```

![连接器运行状态](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
在上图中，第一行是从同步引擎空闲状态。 从 Azure AD 接口运行时第二行。

## <a name="scheduler-and-installation-wizard"></a>计划程序和安装向导
如果您启动安装向导，然后计划程序将暂时挂起。 这是因为我们假定您将进行配置更改，如果正在运行的同步引擎不能应用这些。 由于这个原因，不要让安装向导打开因为它将停止同步引擎执行所有同步操作。

## <a name="next-steps"></a>下一步行动
了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
