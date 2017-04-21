<properties
   pageTitle="Azure AD 连接: 从早期版本升级 |Microsoft Azure"
   description="解释升级到最新版本的 Azure 活动目录连接，包括就地升级和迁移摆动的不同方法。"
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
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>从以前的版本的 azure AD 连接︰ 升级为最新
本主题介绍可用于将 Azure AD 连接安装升级到最新版本的不同方法。 我们建议您保持自己当前的 Azure AD 连接版本。 进行大量的配置更改时也可使用[旋转迁移](#swing-migration)中描述的步骤。

若要从目录同步升级，看到[从 Azure AD 同步工具 （目录同步） 升级](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)。

有几个不同的策略，以升级 Azure AD 连接。

方法 | 说明
--- | ---
[自动升级](active-directory-aadconnect-feature-automatic-upgrade.md) | 为客户提供快速的安装，这是最简单的方法。
[就地升级](#in-place-upgrade) | 如果您有一台服务器，安装就地升级的同一台服务器上。
[Swing 迁移](#swing-migration) | 使用两个服务器，可以准备的一个新版本或配置服务器，并更改活动的服务器，当您准备好。

所需的权限，请参阅[升级所需的权限](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)。

## <a name="in-place-upgrade"></a>就地升级
就地升级适用于从 Azure AD 同步或 Azure AD 连接。 它不适用于目录同步或 FIM + Azure AD 接口解决方案。

当您有一台服务器和小于大约 100000 个对象时，此方法是首选。 如果有现成的同步规则的任何改动，完全导入和完全同步后出现升级。 这样可确保，新配置应用于所有现有对象的系统中。 同步引擎的范围中，这可能需要几个小时，具体取决于对象的数目。 普通的增量同步时间表，默认情况下，每隔 30 分钟会暂停，但密码同步将继续。 可以考虑就地升级，在周末期间。 如果没有现成的配置与新的 Azure AD 连接版本的更改，正常增量导入/同步将改为启动。  
![就地升级](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

如果对现成的同步规则进行了更改，这些将被重新设置为上升级的默认配置。 若要确保升级之间保留为您的配置，确保所做的更改，[更改默认配置的最佳操作](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)中所述。

## <a name="swing-migration"></a>Swing 迁移
如果您有复杂的部署或非常多的对象，可能不切实际要执行就地升级上实时系统。 这可能对于某些客户需要多天，在此期间将处理任何增量更改。 当您计划您的配置进行了重大更改，您想要试验一下之前这些推到云时，也使用此方法。

这些方案的推荐的方法是使用 swing 迁移。 您需要 （至少） 两个服务器、 一个活动和一个临时服务器。 活动服务器 （蓝色实线在下面的图片） 负责进行的活动生产负载。 临时服务器 （紫色虚线下面的图片中） 与新版本的配置已准备好和完全准备好后，此服务器将成为活动。 前一个活动的服务器，现在与旧版本的安装，配置测试服务器和升级。

两台服务器可以使用不同的版本。 例如，打算退役的活动服务器可以使用 Azure AD 同步和新的暂存服务器可以使用 Azure AD 连接。 如果您使用 swing 迁移来开发新的配置是最好的两个服务器上具有相同的版本。  
![临时服务器](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

注意︰ 它已注意到一些客户希望有三个或四个服务器为此方案。 临时服务器升级时，则表明您没有在[灾难恢复](active-directory-aadconnectsync-operations.md#disaster-recovery)情况下的备份服务器。 使用三个或四个服务器，一组使用新版本的主/备用服务器可以准备，确保总是有准备接管临时服务器。

这些步骤还可用于移动从 Azure AD 同步或 FIM + Azure AD 接口解决方案。 这些步骤不适用于目录同步，但在[升级 Azure Active Directory 同步 （目录同步）](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)找不到相同摆动 （也称作并行部署） 的迁移方法步骤与用于目录同步。

### <a name="swing-migration-steps"></a>Swing 迁移步骤

1. 如果您在两台服务器上使用 Azure AD 连接并计划才做的配置更改，请确保您的活动服务器和暂存服务器都使用相同的版本。 这样便于以后比较差异。 如果您从 Azure AD 同步升级，这些服务器会有不同的版本。 如果您要从旧 Azure AD 连接版本升级的则最好开头两个服务器使用相同的版本，而不是必需的。
2. 如果临时服务器不具有它所做的一些自定义的配置，请按照[移动从活动到临时服务器的自定义配置](#move-custom-configuration-from-active-to-staging-server)下的步骤。
3. 如果您要从 Azure AD 连接的早期版本进行升级，升级到最新版临时服务器。 如果从 Azure AD 同步移动，然后在临时服务器上安装 Azure AD 连接。
4. 让临时服务器上运行完全导入和完全同步同步引擎。
5. 验证新的配置未导致使用中[验证服务器的配置](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server)下**验证**的步骤的任何意外的更改。 如果某些东西并不像预期的、 正确的、 运行导入和同步，并验证直到数据看上去不错。 主题中的链接，可以找到这些步骤。
6. 切换活动服务器将临时服务器。 这是最后一步**切换活动服务器**中[验证服务器的配置](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server)。
7. 如果您要升级 Azure AD 连接，升级现在在调试模式下到最新版本的服务器。 按照相同的步骤操作以获取数据和配置升级。 如果您是从 Azure AD 同步升级，您现在可以关闭并停止使用旧的服务器。

### <a name="move-custom-configuration-from-active-to-staging-server"></a>将从活动的自定义配置移动到暂存服务器
如果对活动服务器进行了配置更改，您需要确保相同的更改将应用到阶段服务器。

您已创建的自定义同步规则可以使用 PowerShell 移动。 其他更改必须应用这两种系统上相同的方式并不能被迁移。

事情必须确保配置两个服务器上相同的方式︰

- 对同一个目录林的连接。
- 所有域和 OU 筛选。
- 同一个可选功能，如密码同步和密码写回。

**移动同步规则**  
要移动自定义同步规则，请执行以下操作︰

1. 在活动服务器上打开**同步规则编辑器**。
2. 选择您的自定义规则。 单击**导出**。 这将打开记事本窗口。 PS1 扩展名保存的临时文件。 这使得 PowerShell 脚本。 Ps1 文件复制到测试服务器。  
![同步规则导出](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. 连接器 GUID 临时服务器上不同，必须进行更改。 获取的 GUID、 启动**同步规则编辑器**，选择一个现成的规则表示同一个已连接的系统中，并单击**导出**。 从阶段服务器 GUID 替换 PS1 文件中的 GUID。
4. PowerShell 提示时，请运行 PS1 文件。 这将在阶段服务器上创建自定义同步规则。
5. 如果您有多个自定义规则，重复的所有自定义规则。

## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
