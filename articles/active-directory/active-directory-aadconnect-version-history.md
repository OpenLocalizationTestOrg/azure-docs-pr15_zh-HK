<properties
   pageTitle="Azure AD 连接︰ 版本发布历史记录 |Microsoft Azure"
   description="本主题列出了 Azure AD 连接和 Azure AD 同步的所有版本"
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
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD 连接︰ 版本版本历史记录

Azure Active Directory 团队定期更新新特性和功能 Azure AD 连接。 不是所有的新增功能是适用于所有访问群体。

本文旨在帮助您跟踪已发布的版本并了解您是否需要更新到最新版本，或不。

这是相关主题的列表︰

主题 |  
--------- | --------- |
从 Azure AD 连接升级步骤 | [从最新到的早期版本升级](active-directory-aadconnect-upgrade-previous-version.md)Azure AD 连接发布的不同方法。
所需的权限 | 应用更新，请参阅[用户帐户及权限](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)所需的权限
下载| [下载 Azure AD 连接](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
发布︰ 2016年 8 月

**已解决的问题︰**

- 同步时间间隔不发生更改直到下一同步周期完成后。
- Azure AD 连接向导不接受其用户名以一个下划线打头的 Azure AD 帐户 (\_)。
- Azure AD 连接向导无法验证 Azure 的广告客户提供帐户密码中包含特殊字符太多。 错误消息"无法验证的凭据。 意外的错误发生。" 将返回。
- 卸载暂存服务器禁用密码同步在 Azure AD 租户并导致密码同步的活动服务器失败。
- 密码同步失败不常见的情况中，存储在用户没有密码哈希时。
- 对于临时模式启用 Azure AD 连接服务器时，不被暂时禁用密码写回。
- Azure AD 连接向导不显示实际的密码同步和密码写回配置服务器时在调试模式。 它始终显示它们为已禁用。
- 服务器在调试模式下时，不保留配置更改密码同步和密码写回 Azure AD 连接向导。

**改进︰**

- 更新的开始 ADSyncSyncCycle cmdlet 以指示它是否能够成功启动新的同步周期或不。
- 终止同步周期及操作当前正在进行的添加的 Stop ADSyncSyncCycle cmdlet。
- 终止同步周期及操作当前正在进行的更新的停止 ADSyncScheduler cmdlet。
- 在 Azure AD 连接向导中配置[目录扩展](active-directory-aadconnectsync-feature-directory-extensions.md)，现在可以选择广告类型"电传字符串"属性。

## <a name="111890"></a>1.1.189.0
发布︰ 2016 6 月

**已解决的问题和改进︰**

- Azure AD 连接现在可以安装在与 FIPS 兼容的服务器上。
    - 密码同步，请参阅[密码同步和 FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- 解决了问题，其中一个 NetBIOS 名称无法解析为在 Active Directory 连接器 FQDN。

## <a name="111800"></a>1.1.180.0
发布︰ 2016 5 月

**新功能︰**

- 警告，并可以帮助您验证域，如果运行 Azure AD 连接之前没有做。
- 添加了对[Microsoft 云德国](active-directory-aadconnect-instances.md#microsoft-cloud-germany)的支持。
- 添加了对新 URL 要求与最新的[Microsoft Azure 政府云](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud)基础架构的支持。

**已解决的问题和改进︰**

- 添加筛选到同步规则编辑器以使其很容易地发现同步规则。
- 删除连接器空间时提高的性能。
- 同一对象被删除和添加在同一个运行 （称为删除/添加） 时解决问题。
- 禁用的同步规则将不再重新启用包含的对象和属性升级或目录架构刷新。

## <a name="111300"></a>1.1.130.0
发布︰ 2016 4 月

**新功能︰**

- 添加了对[目录扩展](active-directory-aadconnectsync-feature-directory-extensions.md)到多值属性的支持。
- 添加了对[自动升级](active-directory-aadconnect-feature-automatic-upgrade.md)将被视为有资格升级的详细配置变体的支持。
- 添加[自定义计划程序](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)为某些 cmdlet。

## <a name="111190"></a>1.1.119.0
发布︰ 2016 3 月

**已解决的问题︰**

- 建立确保密码同步后不能在 Windows Server 2008 (pre-R2) 上使用速成版安装不支持在此操作系统上。
- 从目录同步自定义筛选器配置的升级没有无法按预期工作。
- 在升级到较新版本时，并没有对配置的更改，不应安排完全导入/同步。

## <a name="111100"></a>1.1.110.0
发布︰ 2016年 2 月

**已解决的问题︰**

- 如果安装在默认的**C:\Program 文件**文件夹不是从早期版本升级而不起作用。
- 如果您安装并取消选择**启动同步进程.** 在安装向导结束时，重新运行安装向导将不允许计划程序。
- 计划程序不会像预期的那样在日期/时间格式不是美国 en 的服务器上。 它还将阻止`Get-ADSyncScheduler`以返回正确的时间。
- 如果您安装的选项和升级为使用 ADFS Azure AD 连接的早期版本，您无法再次运行安装向导。

## <a name="111050"></a>1.1.105.0
发布︰ 2016年 2 月

**新功能︰**

- 快速设置客户的[自动升级](active-directory-aadconnect-feature-automatic-upgrade.md)功能。
- 使用安装向导中的 MFA 和 PIM 的全局管理支持。
    - 您需要使您的代理服务器如果使用 MFA 还允许 https://secure.aadcdn.microsoftonline-p.com 的通信。
    - 您需要将 https://secure.aadcdn.microsoftonline-p.com 添加到 MFA 来正确地处理您的受信任的站点列表。
- 允许在初始安装后更改该用户的登录方法。
- 允许在安装向导中的[域和 OU 筛选](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)。 这也使到目录林并不是所有域都有连接。
- [计划](active-directory-aadconnectsync-feature-scheduler.md)程序内置到同步引擎。

**从预览升级为正式的功能︰**

- [设备写回](active-directory-aadconnect-feature-device-writeback.md)。
- [目录扩展](active-directory-aadconnectsync-feature-directory-extensions.md)。

**新的预览功能︰**

- 新的默认同步的周期间隔为 30 分钟。 用于将所有早期发行版本中的 3 个小时。 添加更改[计划程序](active-directory-aadconnectsync-feature-scheduler.md)行为的支持。

**已解决的问题︰**

- 验证 DNS 域页面总是不能识别的域。
- 当配置 ADFS 的域管理员凭据的提示。
- 内部部署 AD 帐户无法识别的安装向导，如果具有不同的 DNS 树，根域之外的域中。

## <a name="1091310"></a>1.0.9131.0
发布︰ 2015 年 12 月

**已解决的问题︰**

- 当您更改 AD DS，但有效的密码，不要设置密码时，密码同步可能无法工作。
- 在安装或卸载过程可能会失败对 Azure AD 身份验证代理服务器后，在配置页上的升级。
- 从使用完整的 Azure AD 连接的上一版本更新 SQL Server 将失败如果不能在 SQL 中的 SA。
- 从早期版本的 Azure AD 连接与远程更新 SQL Server 将显示错误"无法访问 ADSync SQL 数据库"。

## <a name="1091250"></a>1.0.9125.0
发布︰ 2015年 11 月

**新功能︰**

- 可以重新配置到 Azure AD 信任 ADF。
- 可以刷新 Active Directory 架构并重新生成同步规则。
- 可以禁用同步规则。
- 可以定义为新文本同步规则中的"AuthoritativeNull"。

**新的预览功能︰**

- [Azure AD 连接的运行状况同步](active-directory-aadconnect-health-sync.md)。
- 对[Azure AD 域服务](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)密码同步的支持。

**新的支持的方案︰**

- 支持多个内部 Exchange 组织。 有关更多信息，请参见[混合部署具有多个活动目录林](https://technet.microsoft.com/library/jj873754.aspx)。

**已解决的问题︰**

- 密码同步的问题︰
    - 一个对象，该对象将从范围外移到范围内将不会同步其密码。 该 incudes OU 和筛选属性。
    - 选择一个新的 OU 包含同步不需要完整的密码同步。
    - 启用已禁用的用户密码不同步。
    - 密码重试队列是无限和 5000 的对象淘汰了以前限制已被删除。
    - [提高了故障诊断](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization)。
- 不能连接到 Active Directory 与 Windows 服务器 2016年目录林功能级别。
- 不能更改用于在初始安装后筛选组的组。
- Azure AD 连接在服务器上为每个用户执行更改一次密码，用密码写启用将不会再创建新的用户配置文件。
- 不能使用长整型值的同步规则范围。
- 如果无法连接的域控制器"设备写回"的复选框是无效的。

## <a name="1086670"></a>1.0.8667.0
发布︰ 2015年 8 月

**新功能︰**

- 现在向所有的 Windows 服务器语言本地化 Azure AD 连接安装向导。
- 添加的支持帐户解锁时使用 Azure AD 密码管理。

**已解决的问题︰**

- 如果另一个用户继续安装而不是第一次启动安装的人会崩溃 azure AD 连接安装向导。
- 不，如果 Azure AD 连接无法彻底卸载 Azure AD 连接同步的上一安装，则可能要重新安装。
- 不能安装使用速成版安装，如果用户不是此目录林的根域中，或使用 Active directory 的非英语版本的 Azure AD 连接。
- 如果无法解决活动目录的用户帐户的 FQDN，则显示误导性的错误消息"无法提交架构"。
- 如果该向导更改了活动目录连接器上使用的帐户，向导将在后续运行失败。
- Azure AD 连接有时无法在域控制器上安装。
- 不能启用和禁用"转移模式"，如果已添加扩展属性。
- 密码写回失败在某些配置中由于错误的密码，在活动的目录连接器上。
- 如果 dn 属性筛选中使用，则无法升级目录同步。
- 过多 CPU 使用率时使用的密码重置。

**删除的预览功能︰**

- 预览功能[用户写回](active-directory-aadconnect-feature-preview.md#user-writeback)已暂时删除基于预览客户的反馈。 它将被重新添加以后当我们解决了提供的反馈。

## <a name="1086410"></a>1.0.8641.0
发布︰ 2015 6 月

**Azure AD 连接的初始版本。**

从 Azure AD 同步到 Azure AD 连接的更改的名称。

**新功能︰**

- [快速设置](./connect/active-directory-aadconnect-get-started-express.md)安装
- 可以[配置 ADF](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- 可以[从目录同步升级](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [防止意外删除](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- 引入[调试模式下](active-directory-aadconnectsync-operations.md#staging-mode)

**新的预览功能︰**

- [用户写回](active-directory-aadconnect-feature-preview.md#user-writeback)
- [组的写回](active-directory-aadconnect-feature-preview.md#group-writeback)
- [设备写回](active-directory-aadconnect-feature-device-writeback.md)
- [目录扩展](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
发布︰ 2015 5 月

**新的要求︰**

- Azure AD 同步现在要求安装的.Net framework 版本 4.5.1。

**已解决的问题︰**

- 从 Azure 广告的密码写回失败，servicebus 连接错误。

## <a name="104910413"></a>1.0.491.0413
发布︰ 2015 4 月

**已解决的问题和改进︰**

- 如果启用了回收站，在目录林中有多个域 Active Directory 连接器不正确处理删除。
- Azure 活动目录连接器已改进的导入操作的性能。
- 当一组超过了成员资格限制 （默认情况下，限制设置为 50 k 对象），Azure Active Directory 中已删除的组。 新的行为是，组将保持，不会引发错误和任何新的成员资格更改会被导出。
- 如果具有相同 DN 的分阶段的删除已存在的连接器空间中无法设置一个新的对象。
- 某些对象标记为正在同步在增量同步期间，虽然转移的对象上没有变化。
- 强制密码同步还会删除首选的 DC 列表。
- CSExportAnalyzer 有一些对象状态的问题。

**新功能︰**

- 现在，联接可以连接到在 MV 中"ANY"对象类型。

## <a name="104850222"></a>1.0.485.0222
发布︰ 2015年 2 月

**改进︰**

- 改进的导入性能。

**已解决的问题︰**

- 密码同步采用属性筛选使用的 cloudFiltered 属性。 筛选的对象不再将密码同步作用域中。
- 在极少数情况下，拓扑结构在其中有很多域控制器，密码同步不起作用。
- Azure AD/Intune 已启用"停止-服务器"时从 Azure AD 接口导入后设备管理。
- 加入来自同一林中的多个域的外部安全主体 (Fsp) 导致不明确联接错误。

## <a name="104751202"></a>1.0.475.1202
发布︰ 2014 年 12 月

**新功能︰**

- 它现在支持使用基于属性筛选的密码同步。 有关更多详细信息，请参阅[密码同步筛选](active-directory-aadconnectsync-configure-filtering.md)。
- 到 AD 写回特性 msDS-ExternalDirectoryObjectID。 这增加了对 Office 365 提供应用程序使用 OAuth2 在线访问，支持并部署混合 Exchange 部署中的邮箱。

**固定的升级问题︰**

- 新版的签到助手是在服务器上可用。
- 自定义安装路径用于安装 Azure AD 同步。
- 无效的自定义联接条件阻止升级。

**其他修补程序︰**

- 对于 Office 专业人员加上固定模板。
- 固定的安装问题引起的以破折号开头的用户名称。
- 固定在第二次运行安装向导时丢失的 sourceAnchor 设置。
- 密码同步的固定的 ETW 跟踪

## <a name="104701023"></a>1.0.470.1023
发布︰ 2014年 10 月

**新功能︰**

- 从多个密码同步内部到 Azure 的广告的广告。
- 本地化安装程序用户界面中的所有 Windows Server 语言。

**从 AADSync 1.0 GA 升级**

如果您已经安装的 Azure AD 同步，还有一个额外的步骤，您必须在您更改了任何现成的同步规则的情况下采取。 已升级到 1.0.470.1023 后释放的同步复制的规则进行修改。 对于每个修改同步规则执行以下操作︰

- 查找同步规则已修改并记下所做的更改。
- 删除同步规则。
- 查找新创建的 Azure AD 同步的同步规则，然后重新应用所做的更改。

**AD 帐户的权限**

AD 帐户必须被授予其他权限以便能够从 AD 中读取密码哈希值。 要授予的权限称为"复制目录更改"和"以所有形式复制目录更改"。 两个权限需要能够读取密码哈希值

## <a name="104190911"></a>1.0.419.0911
发布︰ 2014年 9 月

**首次发行的 Azure AD 同步。**

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
