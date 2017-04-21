<properties
    pageTitle="Azure AD 连接同步︰ 配置筛选 |Microsoft Azure"
    description="解释如何配置在 Azure AD 连接同步筛选。"
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
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD 连接同步︰ 配置筛选
使用筛选，您可以控制哪些对象应出现在 Azure 广告从本地目录。 默认配置将配置目录林中的所有域中的所有对象。 一般情况下，这是推荐的配置。 最终用户使用 Office 365 的工作负载，如 Exchange 联机和 Skype 的业务，以便它们可以发送电子邮件，并调用每个人受益完整的全局地址列表。 使用默认配置中，这些参数将被处理的交换或 Lync 内部实现相同的体验。

在某些情况下，它需要进行一些更改为默认配置。 下面是一些示例︰

- 您计划使用[多 Azure 的广告目录拓扑结构](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory)。 然后您需要将应用筛选器以控制哪些对象应同步到特定 Azure 的广告目录。
- 试运行的 Azure 或 Office 365 和 Azure AD 中只需要的那部分用户。 在小型试验中，并不重要，能够完成全局地址列表演示功能。
- 您有多个服务帐户和您不希望在 Azure AD 中其他非个人帐户。
- 出于法规遵从性方面的考虑不删除任何用户帐户内部。 只禁用它们。 但在 Azure AD 只希望活动帐户存在。

本文介绍了如何配置不同的筛选方法。

> [AZURE.IMPORTANT]Microsoft 不支持修改或 Azure AD 连接同步正式记录这些操作以外的操作。 任何这些操作可能会导致出现不一致或不受支持的状态，Azure AD 连接同步的因此，Microsoft 不能提供的技术支持，对于这种部署。

## <a name="basics-and-important-notes"></a>重要说明和基本要素
在 Azure AD 连接同步中，则可以启用任何时候过滤。 如果使用默认配置的目录同步启动，然后配置筛选，筛选掉的对象不再同步到 Azure 的广告。 进行此更改后，Azure AD 中删除 Azure AD 以前被同步，但随后被筛选的任何对象。

在开始做之前更改为筛选，请确保您[禁用计划的任务](#disable-scheduled-task)以便不会意外地导出您尚未尚未验证正确的更改。

由于筛选可以同时删除多个对象，想要确保将新筛选器正确，您才能将更改导出到 Azure 的广告。 完成这些配置步骤后，强烈建议将导出并到 Azure AD 进行更改之前请按照[验证步骤](#apply-and-verify-changes)。

若要保护您免受无意中删除多个对象，[防止意外删除](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)功能默认是打开的。 如果您删除多个对象，因为内容筛选 (500，默认情况下)，您需要按照允许转到 Azure 广告删除这篇文章中的步骤。

如果您使用版本 11 月 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250))，对筛选器的配置进行了更改，并使用密码同步之前，则需要在完成配置后触发完全同步的所有密码。 有关如何触发一个密码的步骤完全同步，请参阅[触发器完全同步的所有密码](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords)。 如果您是在 1.0.9125 或更高版本，然后定期**完全同步**操作还计算如果应同步密码和不再需要此额外步骤。

如果**用户**对象是无意中删除在 Azure AD 中由于筛选错误，可以 Azure AD 中重新创建用户对象，通过删除筛选配置，然后再次同步您的目录。 此操作将用户从回收站还原在 Azure 的广告。 但是，您不能撤消删除其他对象类型。 例如，如果您意外地删除安全组，并曾到 ACL 组和其 Acl 的资源不能恢复。

Azure AD 连接仅删除它有一度被认为是在作用域中的对象。 如果有 Azure 的广告是由另一个同步引擎创建的对象，这些对象不是在作用域中添加筛选不要删除它们。 例如，如果您使用一个目录同步服务器启动它在 Azure AD 中创建整个目录的完整副本和从开始启用了筛选功能的并行安装新的 Azure AD 连接同步服务器，它不会删除多余的对象创建的目录同步。

安装或升级到较新版本的 Azure AD 连接时，将保留筛选配置。 它始终是最佳实践验证，不会无意中更改配置升级后到新版本运行前第一次同步周期。

如果您有多个林，然后在本主题中所述的筛选配置必须应用于每个目录林 （假定您希望对所有这些相同的配置）。

### <a name="disable-scheduled-task"></a>禁用计划的任务
要禁用内置的计划程序触发的同步周期每隔 30 分钟，请执行以下步骤︰

1. 转到 PowerShell 提示。
2. 运行`Set-ADSyncScheduler -SyncCycleEnabled $False`禁用计划程序。
3. 本主题中所述进行更改。
4. 运行`Set-ADSyncScheduler -SyncCycleEnabled $True`若要重新启用调度程序。

**如果您使用之前 1.1.105.0 Azure AD 连接生成**  
要禁用计划的任务触发的同步周期每 3 小时一次，请执行以下步骤︰

1. 从开始菜单启动**任务计划程序**。
2. 直接在**任务计划程序库**中，查找名为**Azure AD 同步调度程序**，单击鼠标右键，并选择**禁用**的任务。  
![任务计划程序](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. 现在您可以进行配置更改并从**同步服务管理器**控制台手动运行同步引擎。

完成所有筛选更改后，不要忘记是后退和**启用**任务再次。

## <a name="filtering-options"></a>筛选选项
下面的筛选配置类型可以应用于该目录同步工具︰

- [**基于组**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)︰ 基于一个组筛选仅可配置在使用安装向导的初始安装。 它进一步不包括在本主题中。

- [**基于域**](#domain-based-filtering)︰ 该选项使您能够选择到 Azure AD 同步的域。 它还允许您添加和删除同步引擎配置域如果后安装 Azure AD 连接同步到您的内部部署基础结构进行更改。

- [**基于组织单位 –**](#organizational-unitbased-filtering): 此筛选选项使您能够选择的 Ou 同步到 Azure 的广告。 此选项是选定 Ou 中的所有对象类型。

- [**基于属性 –**](#attribute-based-filtering): 此选项允许您筛选基于对象上的属性值的对象。 此外，您还可以针对不同的对象类型不同的筛选器。

您可以同时使用多个筛选选项。 例如，可以使用基于 OU 的筛选以仅包括对象一个 OU 中并在同一时间基于属性的筛选来筛选对象进一步。 当您使用多个筛选的方法时，筛选器将使用筛选器之间的逻辑 AND。

## <a name="domain-based-filtering"></a>基于域的筛选
本节提供配置域筛选器的步骤。 如果添加或删除域林中，Azure AD 连接安装后，您还必须更新筛选配置。

若要更改域基于筛选的首选的方法是通过运行安装向导和更改[域和 Ou 筛选](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)。 安装向导将自动执行本主题中介绍的所有任务。

如果您由于某种原因无法运行安装向导，只应执行以下步骤。

基于域的筛选配置包括下列步骤︰

- [选择的域](#select-domains-to-be-synchronized)应包含在同步中。
- 对于每个已添加或删除的域，调整[运行配置文件](#update-run-profiles)。
- [应用和验证更改](#apply-and-verify-changes)。

### <a name="select-domains-to-be-synchronized"></a>选择要同步的域
**要设置的域筛选器，请执行以下步骤︰**

1. 登录到正在使用的是**ADSyncAdmins**安全组的成员帐户运行 Azure AD 连接同步的服务器。
2. 从开始菜单启动**同步服务**。
3. 选择**连接符**，在**接口**列表中，选择该连接线与**Active Directory 域服务**的类型。 从**操作**，选择**属性**。  
![连接器的属性](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. 单击**配置目录分区**。
5. 在**目录分区中选择**列表中，选择，然后根据需要取消选择域。 验证选中了您想要同步的分区。  
![分区](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
如果您更改了您的内部的 AD 架构和添加或移除目录林的域，然后单击**刷新**按钮以获取更新后的列表。 刷新时，将要求您输入凭据。 为内部部署活动目录具有读取访问权限提供任何凭据。 它并没有为将被预填充在对话框中的用户。  
![需要刷新](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. 操作完成后，单击**确定**关闭**属性**对话框。 如果您已经从林中删除域，指出已删除的域的一个弹出消息，该配置将被清理。
7. 继续调整[运行配置文件](#update-run-profiles)。

### <a name="update-run-profiles"></a>更新运行配置文件
如果更新域筛选器，您还需要更新运行配置文件。

1. 在**接口**列表中，确保选择您在上一步中更改的连接器。 从**操作**，选择**配置运行配置文件**。  
![连接器运行配置文件](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

您需要调整以下配置文件︰

- 完全导入
- 完全同步
- 增量导入
- 增量同步
- 导出

对于每个五个配置文件，可采取下列步骤为每个**添加**的域︰

1. 选择运行配置文件，然后单击**新的步骤**。
2. 在**配置步骤**页中，**类型**下拉列表，为正在配置的配置文件中选择步骤类型具有相同的名称。 然后单击**下一步**。  
![连接器运行配置文件](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. 在**连接器配置**页上，在**分区**下拉列表，选择已添加到您的域筛选器的域的名称。  
![连接器运行配置文件](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. 若要关闭**配置运行配置文件**对话框中，单击**完成**。

对于每个五个配置文件，可采取下列步骤为每个**删除**的域︰

1. 选择运行配置文件。
2. 如果**分区**特性的**值**是一个 GUID，选择运行步骤，然后单击**删除步骤**。  
![连接器运行配置文件](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

结果应该是您想要同步的每个域，应列为每个运行的配置文件中的一个步骤。

若要关闭**配置运行配置文件**对话框中，单击**确定**。

- 若要完成配置，[应用和验证更改](#apply-and-verify-changes)。

## <a name="organizational-unitbased-filtering"></a>组织单位基于筛选
更改 OU 基于筛选的首选的方法是通过运行安装向导和更改[域和 Ou 筛选](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)。 安装向导将自动执行本主题中介绍的所有任务。

如果您由于某种原因无法运行安装向导，只应执行以下步骤。

**配置组织基于单位的筛选，请执行以下步骤︰**

1. 登录到正在使用的是**ADSyncAdmins**安全组的成员帐户运行 Azure AD 连接同步的服务器。
2. 从开始菜单启动**同步服务**。
3. 选择**连接符**，在**接口**列表中，选择该连接线与**Active Directory 域服务**的类型。 从**操作**，选择**属性**。  
![连接器的属性](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. 单击**配置目录分区**，选择想要配置的域，然后单击**容器**。
5. 出现提示时，为内部活动目录具有读取访问权限提供任何凭据。 它并没有为将被预填充在对话框中的用户。
6. 在**选择容器**对话框中，清除不希望同步与云目录，然后单击**确定**的 Ou。  
![OU](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - Windows 10 到 Azure AD 成功同步的计算机，应选择**计算机**容器。 如果您加入域的计算机位于其他 Ou 中，请确保选择。
  - 如果您有多个目录林有信任关系，则应选择**ForeignSecurityPrincipals**容器。 这个容器允许跨林的安全组成员身份解析。
  - 如果您已启用设备的写回功能，则应选择**RegisteredDevices** OU。 如果要使用另一个写回功能，如组写回，确保这些位置的选择。
  - 选择任何其他 OU 的用户、 iNetOrgPersons、 组、 联系人和计算机位于何处。 在图片中，所有这些都位于 ManagedObjects OU。
7. 操作完成后，单击**确定**关闭**属性**对话框。
8. 若要完成配置，[应用和验证更改](#apply-and-verify-changes)。

## <a name="attribute-based-filtering"></a>基于属性的筛选
请确保在 11 月 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) 或以后为这些步骤以处理生成。

基于属性筛选是非常灵活的方式对筛选器对象。 [声明性资源调配](active-directory-aadconnectsync-understanding-declarative-provisioning.md)的电源可用于控制的几乎每一个方面，当对象应同步到 Azure 的广告。

可以应用筛选[入站](#inbound-filtering)到了元节的 Active Directory 中和[出站](#outbound-filtering)中元节到 Azure 的广告上。 建议应用，因为它是最简单的方法维护在入站筛选。 如果有必要，可以执行计算前加入多个林中的对象应仅使用出站筛选。

### <a name="inbound-filtering"></a>入站筛选
根据筛选入站使用默认的配置对象转到 Azure 广告必须元节属性 cloudFiltered 的值未设置要同步。 如果此特性的值设置为**True**，将不同步对象。 它应设置为**False**设计。 若要确保其他规则能够提供一个值，此属性只能具有值**True**则为**空**（不存在）。

在入站筛选，使用的**范围**来确定哪些对象应该或不应该同步。 这是您在此处进行调整以适应贵公司的要求。 作用域模块具有**组**和**子句**来确定同步规则应在范围内。 一**组**包含一个或多个**子句**。 还有多个子句和逻辑之间或多个组之间的逻辑 AND。

让我们看一个示例︰  
![作用域](./media/active-directory-aadconnectsync-configure-filtering/scope.png)这应该读作**(部门 = IT) 或者 (部门销售和 c = = 美国)**。

中的示例和执行以下步骤，用户对象用作示例，但您时可以使用的所有对象类型。

在下面的示例中，优先级值启动 500。 此值确保后 （更低的优先级，更高的数值） 的现成的规则评估这些规则。

#### <a name="negative-filtering-do-not-sync-these"></a>负的筛选，"同步这些"
在以下示例中，筛选出 （不同步） **extensionAttribute15**其中有**非同步**的值的所有用户。

1. 登录到正在使用的是**ADSyncAdmins**安全组的成员帐户运行 Azure AD 连接同步的服务器。
2. 从开始菜单中启动**同步规则编辑器**。
3. 请确保**入站**处于选中状态并单击**添加新规则**。
4. 如为规则指定一个描述性名称，"*在从广告 – 用户 DoNotSyncFilter*"。 选择作为**MV 对象类型**的正确目录林中，**用户**作为**CS 对象类型**和**人**。 作为**链接类型**，选择**加入**和下的优先顺序键入当前未由另一个同步规则 (例如 500)，然后单击**下一步**。  
![入站 1 说明](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. 在**调节筛选器**中，单击**添加组**，单击**添加子句**，并在属性中选择**ExtensionAttribute15**。 确保操作员设置为**相等**，请在值框中键入值**NoSync** 。 单击**下一步**。  
![入站 2 的范围](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. 保留为空，**加入**规则，然后单击**下一步**。
7. 单击**添加转换**、 选择**FlowType**为**常数**、 选择目标属性**cloudFiltered**和在源文本框中，键入**，则返回 True**。 单击**添加**以保存该规则。  
![3 转换站](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. 若要完成配置，[应用和验证更改](#apply-and-verify-changes)。

#### <a name="positive-filtering-only-sync-these"></a>正筛选，"只能同步这些项"
表达积极筛选可能会更具挑战性，因为您必须还考虑不明显会议室等要同步的对象。

积极筛选选项需要两个同步规则。 一个 （或多个） 与要同步的对象，第二个的正确范围全部捕获同步规则筛选出您尚未标识作为应该同步的对象的所有对象。

在以下示例中，只同步用户的部门属性都有**销售**的值的对象。

1. 登录到正在使用的是**ADSyncAdmins**安全组的成员帐户运行 Azure AD 连接同步的服务器。
2. 从开始菜单中启动**同步规则编辑器**。
3. 请确保**入站**处于选中状态并单击**添加新规则**。
4. 为规则指定一个描述性的名称，如"*中广告 – 从用户销售同步*"。 选择作为**MV 对象类型**的正确目录林中，**用户**作为**CS 对象类型**和**人**。 作为**链接类型**，选择**加入**和优先级中键入一个值当前未由另一个同步规则 (例如 501)，，然后单击**下一步**。  
![入站 4 说明](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. 在**调节筛选器**中，单击**添加组**，单击**添加子句**，在属性中选择**部门**。 确保操作员设置为**相等**，请在值框中键入值**销售**。 单击**下一步**。  
![入站 5 范围](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. 保留为空，**加入**规则，然后单击**下一步**。
7. 单击**添加转换**、 选择**FlowType**为**常数**、 选择目标属性**cloudFiltered**和在源文本框中，键入**假**。 单击**添加**以保存该规则。  
![入站 6 转换](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
这是一种特殊情况，其中 cloudFiltered 显式设置为 False。

    我们现在必须创建全部捕获同步规则。

8. 如为规则指定一个描述性名称，"*在从广告 – 用户全部捕获筛选器*"。 选择作为**MV 对象类型**的正确目录林中，**用户**作为**CS 对象类型**和**人**。 作为**链接类型**，选择**加入**输在优先级中当前未使用的另一项同步规则 (例如 600) 的值。 您已选择优先值 （低优先级） 高于先前同步规则但还留留出空间，以便当您想要开始同步其他部门，我们可以以后再添加更多的筛选同步规则。 单击**下一步**。  
![入站 7 说明](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. 保留**调节筛选器**为空，然后单击**下一步**。 空筛选器指示应将该规则应用于所有对象。
10. 保留为空，**加入**规则，然后单击**下一步**。
11. 单击**添加转换**、 选择**FlowType**为**常数**、 选择目标属性**cloudFiltered**和在源文本框中，键入**，则返回 True**。 单击**添加**以保存该规则。  
![3 转换站](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. 若要完成配置，[应用和验证更改](#apply-and-verify-changes)。

如果需要您可以创建多个规则的第一种类型在我们同步中包含多个对象的位置。

### <a name="outbound-filtering"></a>出站筛选
在某些情况下，有必要进行筛选，只有在中元节对象加入之后。 可能，例如，需要从资源林和帐户目录林以确定是否要同步的对象范围内特性看邮件属性。 在这些情况下，您可以创建出站规则上的筛选。

在此示例中，您更改筛选因此只有用户邮件和范围内，以结束@contoso.com进行同步︰

1. 登录到正在使用的是**ADSyncAdmins**安全组的成员帐户运行 Azure AD 连接同步的服务器。
2. 从开始菜单中启动**同步规则编辑器**。
3. 在**规则类型**下单击**出站**。
4. 查找名为**AAD – 用户加入的 SOAInAD 给出**的规则。 单击**编辑**。
5. 在弹出窗口中，回答**是**以创建规则的副本。
6. 在**描述**页中，将优先变为未使用的值，例如 50。
7. 在左侧的导航栏上，单击**调节筛选器**。 单击**添加子句**，请在属性选择**邮件**、 操作员选择**ENDSWITH**和中值类型**@contoso.com**。 在属性选择**范围内**，操作员选择**ENDSWITH**和值类型，请单击**添加子句** **@contoso.com**。
8. 单击**保存**。
9. 若要完成配置，[应用和验证更改](#apply-and-verify-changes)。

## <a name="apply-and-verify-changes"></a>应用并验证更改
所做的配置更改后，必须将它们应用于系统中已经存在的对象中。 它还可以，应该处理对象当前没有同步引擎中的，同步引擎需要读取源系统重新验证其内容。

如果您更改了配置使用**域**或**组织单位**筛选，则需要进行**完全导入**跟**增量同步**。

如果您更改了使用**属性**筛选配置，则需要执行**完全同步**。

执行以下步骤︰

1. 从开始菜单启动**同步服务**。
2. 选择**连接符**，在**接口**列表中，选择发生更改之前的配置连接器。 从**操作**，选择**运行**。  
![运行连接器](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. 在**运行配置文件**，请选择上一节中提到的操作。 如果您需要运行两个操作，运行第二个，第一个完成后 （**状态**列中为所选连接线**空闲**）。

同步后，所有更改都转移要导出。 实际上在 Azure AD 中进行更改之前，您希望验证所有这些更改正确。

1. 启动命令提示符并转到`%Program Files%\Microsoft Azure AD Sync\bin`
2. 运行︰`csexport "Name of Connector" %temp%\export.xml /f:x`  
可以在同步服务中找到的连接器的名称。 它有一个名称类似于"contoso.com – AAD"Azure 的广告。
3. 运行︰`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. 您现在有一个文件命名为 export.csv，可以进行检查，在 Microsoft Excel 中的 %temp%。 该文件包含要导出的所有更改。
5. 对数据或配置进行必要的更改，并运行这些步骤再次 （导入、 同步，并验证） 直到预期将要导出的更改。

当您感到满意时，则将所做的更改导出到 Azure 的广告。

1. 选择**连接符**，在**接口**列表中，选择 Azure AD 连接器。 从**操作**，选择**运行**。
2. 在**运行配置文件**，请选择**导出**。
3. 如果配置更改删除多个对象，然后错误在导出时看到数大于所配置的阈值 （默认情况下 500）。 如果您看到此错误，然后您需要暂时禁用[防止意外删除](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)功能。

现在是时候来重新启用调度程序。

1. 从开始菜单启动**任务计划程序**。
2. 直接在**任务计划程序库**中，查找名为**Azure AD 同步调度程序**、 右键单击，然后选择**启用**的任务。

## <a name="next-steps"></a>下一步行动
了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
