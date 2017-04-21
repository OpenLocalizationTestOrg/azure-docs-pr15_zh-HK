<properties
    pageTitle="Azure AD 连接同步︰ 了解默认配置 |Microsoft Azure"
    description="本文介绍了 Azure AD 连接同步中的默认配置。"
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD 连接同步︰ 了解默认配置
本文介绍了现成的配置规则。 它将记录的规则，这些规则如何影响配置。 它还引导您完成同步 Azure AD 连接的默认配置。 目标是确保读者了解如何配置模型，名为声明的资源调配正在一个真实世界的例子。 本文假定您已经安装并配置 Azure AD 连接同步使用安装向导。

要了解配置模型的详细信息，请阅读[理解声明性资源调配](active-directory-aadconnectsync-understanding-declarative-provisioning.md)。

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>从内部到 Azure AD 的现成的规则
下面的表达式可以找到现成的配置中。

### <a name="user-out-of-box-rules"></a>用户全新的规则
这些规则也适用于 iNetOrgPerson 对象类型。

用户对象必须满足以下操作以进行同步︰

- 必须有 sourceAnchor。
- 在 Azure AD 中创建了对象后，不能更改 sourceAnchor。 如果的值为内部部署更改，对象将停止同步直到 sourceAnchor 会返回到其以前的值。
- 必须填充 accountEnabled (userAccountControl) 的特性。 与内部部署 Active Directory，此属性始终是存在和填充。

下列用户对象到 Azure AD**不**同步︰

- `IsPresent([isCriticalSystemObject])`. 确保在活动目录中，如内置管理员帐户的许多现成的对象，都不会同步。
- `IsPresent([sAMAccountName]) = False`. 请确保没有使用 sAMAccountName 属性与用户对象不同步。 这种情况下实际上只会出现在从 NT4 升级的域中。
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. 不会同步 Azure AD 连接同步及其更早版本所使用的服务帐户。
- 不会同步将不能工作在 Exchange 的联机的 Exchange 帐户。
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- 不会同步将不能工作在 Exchange 的联机的对象。
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
此位掩码 (& H21C07000) 将筛选出的下列对象︰
    - 已启用邮件的公用文件夹
    - 系统助理邮箱
    - 邮箱数据库 （系统邮箱）
    - 通用安全组 （不是适用于用户，但由于遗留原因存在）
    - 非通用组 （不是适用于用户，但由于遗留原因存在）
    - 邮箱计划
    - 发现邮箱
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. 不同步复制受害者的任何对象。

下列属性规则适用︰

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. SourceAnchor 属性从链接邮箱不发表了。 我们假定如果已找到链接的邮箱，以后加入的实际帐户。
- 与 Exchange 相关的属性仅同步如果**mailNickName**属性的值。
- 当有多个林时，然后按以下顺序消耗特性︰
    1. 从与启用的帐户目录林提供相关要签入的属性 （例如范围内）。
    2. 可以在 Exchange 全球通讯簿 （全局地址列表） 中找到的属性是从 Exchange 邮箱与林发表了。
    3. 如果找不到没有邮箱，这些特性可以来自任何林。
    4. 与 Exchange 相关属性 （在全球通讯簿中不可见的技术属性） 提供从林其中`mailNickname ISNOTNULL`。
    5. 如果有多个目录林将满足其中一项规则，然后用于决定哪些林是属性创建顺序 （日期/时间） （目录林） 的连接器。

### <a name="contact-out-of-box-rules"></a>现成的规则，请与联系
联系人对象必须满足以下操作以进行同步︰

- 必须已启用邮件的联系人。 使用下面的规则对其进行验证︰
    - `IsPresent([proxyAddresses]) = True)`. 代理地址属性必须被填充。
    - 在代理地址属性或邮件属性找不到主电子邮件地址。 是否存在@用来验证这些内容的电子邮件地址。 这两个规则之一必须评估为 True。
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. 是否有一个包含的项"SMTP:"如果没有，可@字符串中找到吗？
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. 邮件属性填充，如果是，可以将@字符串中找到吗？

以下联系人对象到 Azure AD**不**同步︰

- `IsPresent([isCriticalSystemObject])`. 确保没有标记为关键的联系人对象进行同步。 不应该是任何使用默认配置。
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. 这些对象不会起作用在 Exchange 的联机。
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. 不同步复制受害者的任何对象。

### <a name="group-out-of-box-rules"></a>现成的规则进行分组
组对象必须满足以下操作以进行同步︰

- 必须有不超过 50000 的成员。 这一数目是内部组中的成员数。
    - 如果它有更多的成员之前启动第一次同步，不同步组。
    - 如果成员数目增长从它最初创建时，然后当它到达 50000 成员停止同步的成员计数再次低于 50000 之前。
    - 注意︰ 通过 Azure 的广告还实施 50000 的成员计数。 您将不能同步组有更多的成员，即使您修改或删除该规则。
- 如果组是一个**通讯组**，则它还必须是启用邮件。 实施此规则的[联系人现成的规则](#contact-out-of-box-rules)，请参阅。

下面的组对象到 Azure AD**不**同步︰

- `IsPresent([isCriticalSystemObject])`. 确保在活动目录中，如内置管理员组的许多现成的对象，都不会同步。
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. 传统使用的目录同步的组。
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. 角色组。
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. 不同步复制受害者的任何对象。

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal 的全新规则
Fsp 将加入"任何"(\*) 中元节对象。 事实上，该连接只发生的用户和安全组。 此配置可确保跨目录林的成员身份解析并在 Azure AD 中正确地重新表达。

### <a name="computer-out-of-box-rules"></a>计算机的全新规则
计算机对象必须满足以下操作以进行同步︰

- `userCertificate ISNOTNULL`. 只有 Windows 10 计算机填充该属性。 此属性的值的所有计算机对象进行都同步。

## <a name="understanding-the-out-of-box-rules-scenario"></a>了解的全新规则方案
在此示例中，我们将使用只有一个帐户目录林 (A) 部署、 一个资源目录林 (R) 和一个 Azure 的广告目录。

![图片与方案说明](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

在此配置中，假定没有启用的帐户中的帐户林和资源林中具有链接邮箱已禁用的帐户。

我们与默认配置的目标是︰

- 从与启用的帐户目录林同步特性相关的登录。
- 从林中具有邮箱同步可以在全球通讯簿 （全局地址列表） 中找到的属性。 如果找不到没有邮箱，则使用任何其他目录林。
- 如果找到链接的邮箱，则必须找到要导出到 Azure 的 AD 对象链接启用的帐户。

### <a name="synchronization-rule-editor"></a>同步规则编辑器
可以查看和使用该工具同步规则编辑器 (SRE) 更改配置，可以在开始菜单中找到它的快捷方式。

![同步规则编辑器图标](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

SRE 中是一个资源工具包工具并安装使用 Azure AD 连接同步。 能够启动它，您必须是 ADSyncAdmins 组的成员。 它在启动时，您会看到类似下面这样︰

![入站同步规则](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

在此窗格中，您将看到所有同步规则创建针对您的配置。 表中的每一行是一个同步规则。 在规则类型下的左侧，列出两种不同类型︰ 入站和出站。 入站和出站时从视图中元节。 您主要将焦点在本概述中的入站规则。 实际的同步规则列表在 AD 中取决于检测到的架构。 在上图中，帐户目录林 (fabrikamonline.com) 没有任何服务，例如交换和 Lync，并已为这些服务创建任何同步规则。 但是，在资源目录林 (res.fabrikamonline.com) 您找到同步规则获得这些服务。 这些规则的内容是根据检测到的版本不同。 例如，在部署中使用 Exchange 2013 有比配置 Exchange 2010/2007年中的多个属性流。

### <a name="synchronization-rule"></a>同步规则
同步规则是一组满足某个条件时流动特性的配置对象。 它还用于描述连接器空间中的对象与中元节，称为**联接**或**匹配**的对象。 同步规则具有优先值，指示它们之间的彼此。 具有较低的数字值的同步规则具有较高的优先级和属性流冲突，优先级更高获胜的冲突解决方法。

例如，查看同步规则**在从广告 – 用户 AccountEnabled**。 标记为 SRE 中的这行并选择**编辑**。

由于此规则是一个现成的规则，当您打开该规则时收到警告。 您不应任何[现成的规则所做更改](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)，因此需要您的意图是什么。 在这种情况下，您只想查看规则。 选择**否**。

![同步警告的规则](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

同步规则有四个配置部分︰ 说明，范围筛选、 联接规则和转换。

#### <a name="description"></a>说明
第一节提供了基本信息，如名称和说明。

![描述选项卡同步规则编辑器 ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

您还发现此规则，关于哪些连接的系统的相关的信息的对象类型将应用到所连接的系统中，元节对象类型。 元节对象类型源对象类型是用户、 iNetOrgPerson 或联系人始终是无关的人。 元节对象类型应该永远不会更改，因此它创建作为泛型类型。 链接类型可以将联接、 StickyJoin 或规定。 此设置与加入规则明一同工作，稍后介绍。

您还可以查看此同步规则用于密码同步。 如果用户在此同步规则的范围内，该密码从内部同步到云 （假定已启用密码同步功能） 中。

#### <a name="scoping-filter"></a>作用域筛选器
作用域筛选器部分用于配置时应该应用同步规则。 因为您正在查看同步规则的名称指示已启用用户应该只应用，因此 AD 属性**userAccountControl**不能有 2 位配置范围设置。 同步引擎在 AD 中找到用户，它会应用此同步规则，如果**userAccountControl**设置为十进制值 512 （启用普通用户）。 如果用户的**userAccountControl**设置为 514 （禁用普通用户） 不适用于该规则。

![在规则编辑器中同步的作用域选项卡 ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

作用域筛选器具有组可以嵌套的子句。 要应用的同步规则必须满足组内的所有条款。 当定义了多个组时，至少一个组必须满足有关要应用的规则。 即逻辑或组和逻辑之间计算并进行评估组内。 在出站同步规则**出 AAD – 组加入给**找不到此配置的一个示例。 有几种同步筛选器组、 安全组，例如一个 (`securityEnabled EQUAL True`)，另一个用于通讯组 (`securityEnabled EQUAL False`)。

![在规则编辑器中同步的作用域选项卡 ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

此规则用于定义哪些组应调配到 Azure 的广告。 通讯组必须启用与 Azure 的广告，要同步邮件但安全组的电子邮件不是必需的。

#### <a name="join-rules"></a>加入规则
第三部分用来配置中元节对象中的连接器空间对象的关联方式。 您已在前面的规则没有任何配置对于加入规则，相反要看一看**在从广告 – 用户加入**。

![联接，同步规则编辑器中的规则选项卡 ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

连接规则的内容取决于在安装向导中选择匹配选项。 对于入站规则，评估启动与源连接器空间中的对象，每个组中的联接规则按顺序计算。 如果源对象计算匹配元节使用一种连接规则中的一个对象，对象被联接。 如果所有规则中已经过都评估和没有匹配项，则使用描述页上的链接类型。 如果此配置**设置**的设置，然后创建一个新对象是在目标中，元节。 若要提供一个新的对象，到了元节是也称为**项目**到了元节对象。

联接规则只计算一次。 当联接的连接器空间对象和元节对象时，它们联接只要保持同步规则的范围是仍然满意。

在评估时同步规则，只有一个同步规则定义的联接规则必须在范围内。 如果联接规则与多个同步规则找到一个对象，则会引发错误。 由于这个原因，最好是能够与多个同步规则时对象作用域中定义的联接一个同步规则。 在 Azure AD 连接同步现成的配置中，这些规则可以通过查看名称找到并查找这些词结尾的名称**加入**。 同步的规则没有任何定义的联接规则适用于另一个同步规则结合在一起的对象或设置新目标中的对象的属性流。

如果您看一下上面的图片，您可以看到规则正试图加入**objectSID** **msExchMasterAccountSid** （交换） 和**msRTCSIP OriginatorSid** (Lync)，这是我们期望的帐户资源林拓扑结构。 查找所有林状结构在相同的规则。 假设是每个林可能是帐户或资源目录林。 如果您有帐户，住在单个目录林并不一定要加入，这种配置也适用。

#### <a name="transformations"></a>转换
转换部分定义联接对象和作用域筛选器，则满足时将应用于目标对象的所有属性流。 回溯到**在从广告 – 用户 AccountEnabled**同步规则中，您发现了下列转换︰

![转换选项卡同步规则编辑器 ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

若要将此配置放在上下文中，在客户资源目录林部署中，它应启用的帐户在帐户目录林和已禁用的帐户中找到资源目录林 Exchange 和 Lync 设置。 您正在查看的同步规则包含用于登录所需的属性和这些属性应该从林中流启用的帐户的位置。 所有这些属性流被一起放入一个同步规则。

转换可以具有不同的类型︰ 常数、 直和表达式。

- 常流总是流向一个硬编码值。 在上面的例子中，它始终设置**为**元节中的值属性命名为**accountEnabled**。
- 直接流量始终流动中为目标属性的源的属性的值-是。
- 第三种流类型是表达式，它允许使用更高级的配置。

表达式语言是 VBA (的 Visual Basic for Applications)，因此人们使用的 Microsoft Office 体验或 VBScript 将识别的格式。 属性括在方括号内，[属性名称]。 属性名和函数名区分大小写，但同步规则编辑器中计算表达式，并提供一条警告，如果表达式是无效。 在单独的一行，包含嵌套函数表示的所有表达式。 若要显示的配置语言的强大功能，下面是流为 pwdLastSet，但插入的其他意见︰

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

属性流的表达式语言的详细信息，请参阅[了解声明性资源调配表达式](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)。

### <a name="precedence"></a>优先级
您现在已看好某些个别的同步规则，但规则配置中协同工作。 在某些情况下，从多个同步规则到同一目标属性提供属性值。 在这种情况下，属性的优先级用于确定哪个特性获胜。 例如，看一下属性 sourceAnchor。 此属性是一个重要的特性，能够登录到 Azure 的广告。 此属性在两个不同的同步规则中，可以找到属性流**在从广告 – 用户 AccountEnabled**和**在从广告 – 用户常见**。 由于同步规则的优先级，sourceAnchor 属性是发表了从林中启用帐户具有第一次加入的元节对象的多个对象时。 如果没有已启用的帐户，则同步引擎将使用全部捕获同步规则**在从广告 – 用户通用**。 此配置可以确保，即使对于已禁用的帐户，还有 sourceAnchor。

![入站同步规则](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

通过在安装向导中组设置了同步规则的优先级。 组中的所有规则都有相同的名称，但它们连接到不同的连接目录。 安装向导为该规则提供了**在从广告 – 用户加入**最高优先级，和循环转移所有连接广告目录。 它然后继续下一步的按预定义顺序的规则组。 组内连接器向导中添加的顺序添加规则。 如果通过向导添加了另一个接头，同步规则进行重新排序，每个组中最后一个插入新连接器的规则。

### <a name="putting-it-all-together"></a>放在一起
我们现在知道足够的关于同步规则，以便能够了解配置不同的同步规则的工作原理。 如果您看一下用户和贡献元节的属性，按下列顺序应用规则︰

名称 | 注释
:------------- | :-------------
在从广告 – 用户联接 | 加入与元节的连接器空间对象的规则。
在广告 – 从用户帐户启用 | 为登录到 Azure 广告所需属性和 Office 365。 我们希望这些属性中启用帐户。
在从广告 – 从 Exchange 用户常用 | 在全局地址列表中找到的属性。 我们假定在林中我们发现用户的邮箱中的数据质量最好。
在从广告 – 用户常用 | 在全局地址列表中找到的属性。 万一找不到邮箱，联接的任何其他对象可以提供的属性值。
在从广告 – 用户交换 | 已检测到 Exchange 时才存在。 它排列 Exchange 基础结构的所有属性。
在从广告 – 用户 Lync | 已检测到 Lync 时才存在。 排列所有基础结构 Lync 特性。

## <a name="next-steps"></a>下一步行动

- 阅读有关中[了解声明式配置](active-directory-aadconnectsync-understanding-declarative-provisioning.md)的配置模型详细信息。
- 了解有关详细[了解声明性资源调配表达式](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)中的表达式语言信息。
- 继续阅读[了解用户和联系人](active-directory-aadconnectsync-understanding-users-and-contacts.md)中的现成的配置工作原理
- 了解如何更改实际使用声明性资源调配中[如何对默认配置进行更改](active-directory-aadconnectsync-change-the-configuration.md)。

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
