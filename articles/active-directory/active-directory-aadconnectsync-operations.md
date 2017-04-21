<properties
   pageTitle="Azure AD 连接同步︰ 操作任务和注意事项 |Microsoft Azure"
   description="本主题介绍 Azure AD 连接同步以及如何准备运行该组件的操作任务。"
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

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD 连接同步︰ 操作任务和考虑
本主题的目的是为了说明 Azure AD 连接同步的操作任务。

## <a name="staging-mode"></a>调试模式下
调试模式下可用于多个方案，包括︰

-   高可用性。
-   测试和部署新的配置更改。
-   引入一个新的服务器并停止使用旧。

临时模式中的服务器，可以对配置进行更改，并使服务器处于活动状态之前预览更改。 它还允许您运行完全导入和验证在生产环境中进行这些更改之前，需要进行所有更改的完全同步。

在安装期间，可以选择要在**调试模式下**的服务器。 此操作使服务器处于活动状态的导入和同步，但它不会运行任何输出。 在临时模式下的服务器未运行密码同步或回写密码，即使您在安装过程中选择这些功能。 禁用临时模式时，服务器开始导出、 启用密码同步，并使密码写回。

您仍然可以通过使用同步服务管理器强制导出。

在临时模式下的服务器将继续从 Active Directory 和 Azure 广告接收更改。 通过另一台服务器的职责，它始终有一份最新的更改和速度非常快，需要。 如果您的主服务器进行了配置更改，则您有责任对在调试模式下的服务器进行相同的更改。

对于这种旧的同步技术的知识转移模式是不同的由于服务器有其自己的 SQL 数据库。 此体系结构允许临时模式服务器位于不同的数据中心。

### <a name="verify-the-configuration-of-a-server"></a>验证服务器的配置
若要应用此方法，请执行以下步骤︰

1. [准备](#prepare)
2. [导入和同步](#import-and-synchronize)
3. [验证](#verify)
4. [切换活动服务器](#switch-active-server)

#### <a name="prepare"></a>准备

1. Azure AD 连接，安装**调试模式下**，选择和取消选择安装向导中的最后一页上**开始同步**。 这种模式允许您手动运行同步引擎。
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. 号关闭/登录并从 「 开始 」 菜单选择**同步服务**。

#### <a name="import-and-synchronize"></a>导入和同步

1. 选择**连接线**，并选择类型为**Active Directory 域服务**的第一个连接器。 单击**运行**中，选择**完全导入**和**确定**。 对于此类型的所有连接器执行这些步骤。
2. 选择连接器类型**Azure Active Directory (Microsoft)**。 单击**运行**中，选择**完全导入**和**确定**。
3. 请确保连接器选项卡仍处于选中状态。 每个连接器使用**Active Directory 域服务**类型，单击**运行**，选择**增量同步**和**确定**。
4. 选择连接器类型**Azure Active Directory (Microsoft)**。 单击**运行**中，选择**增量同步**和**确定**。

现在有转移出口变为 Azure 的 AD 和本地 AD （如果您使用的 Exchange 混合部署）。 下面的步骤允许您检查什么是将要更改之前实际上开始导出到的目录。

#### <a name="verify"></a>验证

1. 启动命令提示符并转到`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. 运行︰`csexport "Name of Connector" %temp%\export.xml /f:x`  
可以在同步服务中找到的连接器的名称。 它有一个名称类似于"contoso.com – AAD"Azure 的广告。
3. 运行︰`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. 您有一个文件命名为 export.csv，可以进行检查，在 Microsoft Excel 中的 %temp%。 该文件包含要导出的所有更改。
5. 对数据或配置进行必要的更改，然后运行这些步骤再次 （导入和同步和验证） 之前预计将要导出的更改。

**了解 export.csv 文件**

大部分文件是很容易理解。 若要了解的内容有些缩写︰

- OMODT – 对象修改类型。 指示是否在对象级别的操作是添加、 更新或删除。
- AMODT – 属性修改类型。 指示在属性级别的操作是添加、 更新或删除。

如果特性值是多值属性，将显示不是每个更改。 仅添加和移除的值的个数是可见的。

#### <a name="switch-active-server"></a>切换活动服务器

1. 在当前处于活动状态的服务器，关闭服务器 （DirSync/FIM/Azure AD 同步） 以便它不会导出到 Azure AD 或者将其设置在调试模式下 （Azure AD 连接）。
2. 在**调试模式下**的服务器上运行安装向导并禁用**调试模式**。
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>灾难恢复
实现设计的组成部分是计划怎么办没有灾难失去同步服务器的位置。 有不同的模型使用与要使用哪一种取决于若干因素，包括︰

-   在 Azure AD 在停机过程中未对对象可以进行更改的容差是什么？
-   如果您使用密码同步，是否用户接受他们有在 Azure 的广告中使用旧密码，以防它们更改内部？
-   有数据实时操作，例如密码写回表的相关性材料吗？

根据对这些问题以及您的组织策略的回答，可以来实现以下策略之一︰

-   当需要重建。
-   有一个备用的备用服务器，称为**调试模式**。
-   使用虚拟机。

如果您不使用内置的 SQL Express 数据库，然后还应检查[SQL 高可用性](#sql-high-availability)部分。

### <a name="rebuild-when-needed"></a>在需要时重新生成
一个可行的战略是在需要时将服务器重建规划。 通常情况下，安装可以在几小时内完成初始导入和同步该同步引擎和。 如果没有可用的备用服务器，就可以暂时使用的域控制器承载同步引擎。

同步引擎服务器不存储与对象有关的任何状态，以便可以从 Active Directory 和 Azure AD 中的数据重建数据库。 **SourceAnchor**属性用于联接对象从内部部署和云。 如果重建与现有对象内部和云的服务器，然后同步引擎匹配这些对象一起再次重新安装上。 需要进行记录和保存的事情是到服务器，例如筛选和同步规则所做的配置更改。 在开始进行同步之前，必须重新应用这些自定义配置。

### <a name="have-a-spare-standby-server---staging-mode"></a>有备用的备用服务器的调试模式下
如果您有更复杂的环境，然后让一个或多个备用服务器建议。 在安装期间，可以使服务器能够在**调试模式**。

有关详细信息，请参阅[调试模式](#staging-mode)。

### <a name="use-virtual-machines"></a>使用虚拟机
通用和受支持的方法是在虚拟机中运行的同步引擎。 如果主机有一个问题，图像与同步引擎服务器可以迁移到其他服务器中。

### <a name="sql-high-availability"></a>SQL 的高可用性
如果您没有使用附带了 Azure AD 连接 SQL Server Express，应该也考虑对 SQL Server 的高可用性。 支持仅高可用性解决方案是 SQL 群集。 不受支持的解决方案包括镜像和总在。

## <a name="next-steps"></a>下一步行动

**概述主题**  

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)  
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)  
