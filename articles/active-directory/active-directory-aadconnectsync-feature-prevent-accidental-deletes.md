<properties
   pageTitle="Azure AD 连接同步︰ 防止意外删除 |Microsoft Azure"
   description="本主题介绍在 Azure AD 连接防止意外删除 （防止意外删除） 功能。"
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
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD 连接同步︰ 防止意外删除
本主题介绍在 Azure AD 连接防止意外删除 （防止意外删除） 功能。

当安装 Azure AD 连接，防止意外删除是默认情况下启用并配置为不允许与 500 多个删除导出。 此功能旨在保护您免受意外配置更改和更改到您的内部目录，将影响许多用户和其他对象。

当看到包括太多的 delete 时的常见方案︰

- 对[筛选](active-directory-aadconnectsync-configure-filtering.md)整个[OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering)或[域](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering)未更改。
- 一个 OU 中的所有对象都将被都删除。
- OU 被重命名，以便在它的所有对象都被都认为是超出同步范围。

500 个对象的默认值可以更改使用 PowerShell 使用`Enable-ADSyncExportDeletionThreshold`。 您应该配置此值以适合您组织的规模。 由于同步计划程序运行时间间隔为 30 分钟，值是在 30 分钟内检测到删除的数目。

如果有太多删除转移到 Azure AD 中导出，然后停止导出并收到这样一封电子邮件︰

![防止意外删除电子邮件](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hello （技术联系人）。（时） 标识同步服务检测到删除的数目超过 （单位名称） 的配置的删除阈值。要删除的运行此标识同步发送了总 （数字） 的对象。这达到或超过 （数字） 对象的配置的删除阈值值。我们需要您提供确认这些删除应进行处理之后，我们将继续进行。请防止意外删除此电子邮件中列出的错误有关的详细信息，参阅。*

您还可以查看状态`stopped-deletion-threshold-exceeded`查看时在**同步服务管理器**用户界面中的导出配置文件。
![防止意外删除同步服务管理器用户界面](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

如果这是意外，调查，并采取纠正措施。 要查看将要被删除的对象，请执行以下操作︰

1. 从开始菜单启动**同步服务**。
2. 转到**连接器**。
3. 与类型**Azure 活动目录**中选择连接线。
4. 在右侧的**操作**，选择**搜索连接器空间**。
5. 在弹出窗口中的在**作用域**下，选择**断开连接后**，选出在过去的时间。 单击**搜索**。 此页提供所有对象将要删除的视图。 通过单击每个项，您可以获取有关该对象的其他信息。 您还可以单击要添加附加的属性网格中可见的**列设置**。

![搜索连接器空间](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

如果需要所有删除，然后执行下列操作︰

1. 若要暂时禁用此保护，并让这些经历，运行 PowerShell cmdlet 的删除︰ `Disable-ADSyncExportDeletionThreshold`。 提供的 Azure AD 全局管理员帐户和密码。
![凭据](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. 使用 Azure Active Directory 连接器仍处于选中状态，选择**运行**该操作并选择**导出**。
3. 若要重新启用保护，运行 PowerShell cmdlet: `Enable-ADSyncExportDeletionThreshold`。

## <a name="next-steps"></a>下一步行动

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
