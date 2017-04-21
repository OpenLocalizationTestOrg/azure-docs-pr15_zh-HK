<properties
    pageTitle="Azure AD 连接同步︰ 如何更改了默认配置 |Microsoft Azure"
    description="演示如何对 Azure AD 连接同步中的配置进行更改。"
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


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD 连接同步︰ 如何对默认配置进行更改
本主题旨在逐步向您介绍如何更改默认的配置在 Azure AD 连接同步。 它提供一些常见方案的步骤。 了解这一点，您应能够对配置基于您自己的业务规则进行一些简单的更改。

## <a name="synchronization-rules-editor"></a>同步规则编辑器
同步规则编辑器用于查看和更改默认的配置。 可以在**Azure AD 连接**组下的开始菜单中找到它。  
![使用同步规则编辑器中的开始菜单](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

当您打开它时，您会看到默认的现成的规则。

![同步规则编辑器](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>在编辑器中导航
在编辑器顶部的下拉列表允许您快速地找到一个特定的规则。 例如，如果您想要查看规则包含属性代理地址所在，您将更改为以下的下拉列表︰  
![SRE 中筛选](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
要重置过滤并加载新配置，请按键盘上的**f5 键**。

到右上角，有一个按钮**添加新规则**。 此按钮用于创建您自己的自定义规则。

在底部，所选的同步规则上有按钮的作用。 **编辑**和**删除**执行您希望给他们。 **导出**生成 PowerShell 脚本重新创建同步规则。 此过程允许您将同步规则从一个服务器移动到另一个。

## <a name="create-your-first-custom-rule"></a>创建第一个自定义规则
最常见的更改是对属性流的更改。 源目录中的数据可能不是同 Azure 的广告一样。 在本节中的示例中，您要确保用户的名字始终处于**正确的大小写**。

### <a name="disable-the-scheduler"></a>禁用计划程序
[计划程序](active-directory-aadconnectsync-feature-scheduler.md)运行默认情况下每隔 30 分钟。 您想要确保它未启动时进行更改并解决您的新规则。 若要暂时禁用计划程序，PowerShell，启动并运行`Set-ADSyncScheduler -SyncCycleEnabled $false`

![禁用计划程序](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>创建规则

1. 单击**添加新规则**。
2. 在**描述**页上输入以下命令︰  
![入站筛选规则](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - 名称︰ 为规则指定一个描述性名称。
    - 说明︰ 一些澄清以便其他人能够了解的规则是什么。
    - 连接系统︰ 系统中找不到该对象。 在这种情况下，我们选择了 Active Directory 连接器。
    - 已连接的系统/元节对象类型︰ 分别选择**用户**和**人**。
    - 链接类型︰ 将此值更改为**加入**。
    - 优先级︰ 提供在系统中是唯一的值。 较低的数值表示较高的优先级。
    - 标记︰ 保留为空。 只有来自 Microsoft 的现成的规则应具有的值填充此框。
3. 在**调节筛选**页上，输入**givenName ISNOTNULL**。  
![入站规则作用域筛选器](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
此部分用于定义哪些对象的规则应该应用于。 如果保留为空，该规则将应用到所有用户对象。 但是，包括会议室、 服务帐户和其他非个人用户对象。
4. 在**加入规则**，将其留空。
5. 在**转换**页更改为**表达式**的 FlowType。 选择目标属性**givenName**中，然后在源输入`PCase([givenName])`。
![入站的规则转换](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
同步引擎是同时在函数名称和属性名称区分大小写。 如果键入错误的内容，添加规则时显示警告。 编辑器允许您保存并继续，因此必须要重新打开该规则并更正此规则。
6. 单击**添加**以保存该规则。

在系统中，应与其他同步规则可见新的自定义规则。

### <a name="verify-the-change"></a>验证更改
要与此新的更改，请确保它按预期方式工作，不会引发任何错误。 根据您所拥有的对象数，有两种不同的方法来执行此步骤。

1. 在所有的对象上运行完全同步
2. 在单个对象上运行预览和完全同步

从开始菜单启动**同步服务**。 本节中的步骤是所有在此工具中。

1. **所有对象的完全同步**  
选择顶部**的连接器**。 确定的连接线在前面的部分中，在 Active Directory 域服务，此种情况下进行更改并选择它。 从操作中选择**运行**并选择**完全同步**和**确定**。
![完全同步](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
中元节对象已更新。 您现在想要看一看在元节中的对象。

2. **预览和对单个对象的完全同步**  
选择顶部**的连接器**。 确定的连接线在前面的部分中，在 Active Directory 域服务，此种情况下进行更改并选择它。 选择**搜索连接器空间**。 使用范围以查找您要用于测试更改的对象。 选择对象，然后单击**预览**。 在新屏幕中，选择**提交预览**。
![提交预览](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
现在到了元节提交更改。

**在中元节对象的外观**  
您现在想要选取几个示例对象以确保该值应该是和应用该规则。 从顶部选择**元搜索**。 添加任何筛选器，您需要查找相关的对象。 从搜索结果中，打开对象。 看看这些属性值，还验证**同步规则**列作为应用该规则的预期中。  
![元搜索](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>允许计划程序
如果一切如预期的那样，您可以重新启用调度程序。 从 PowerShell，运行`Set-ADSyncScheduler -SyncCycleEnabled $true`。

## <a name="other-common-attribute-flow-changes"></a>其他公共属性流的更改
上一节描述如何更改属性流。 在此部分中，提供了一些附加的示例。 如何创建同步规则的步骤缩写形式，但在上一节中可以找到完整的步骤。

### <a name="use-another-attribute-than-the-default"></a>使用另一个属性，而不是默认
在 Fabrikam，没有本地字母表名字、 姓氏和显示名称的使用其中一个目录林。 扩展属性中可以找到这些属性中的字符表示形式。 在 Azure AD 中构建全局地址列表时，并且 Office 365 组织想要使用这些属性。

使用默认配置，将对象从本地林如下所示︰  
![属性流 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

若要创建与其他属性流规则，请执行以下操作︰

- 从开始菜单中启动**同步规则编辑器**。
- **入站**仍处于选定状态的左侧，单击按钮**添加新规则**。
- 为规则指定的名称和说明。 选择内部部署 Active Directory 和相关的对象类型。  在**链接类型**，选择**加入**。 优先级，来选择另一条规则不使用数字。 现成的规则开始 100，以便可以在本示例中使用的值 50。
![属性流 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- 将作用域保留为空 （即应适用于所有目录林中的用户对象）。
- 保留为空的联接规则 （即，让全新的规则处理所有联接）。
- 在变换，将创建以下流程︰  
![属性流 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- 单击**添加**以保存该规则。
- 转到**同步服务管理器**。 在**连接器**上选择的连接器添加规则的位置。 选择**运行**，并且**完全同步**。 完全同步重新计算使用当前规则的所有对象。

这是同一个对象与此自定义规则的结果︰  
![属性流 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>属性的长度
字符串属性的默认设置为可索引和最大长度为 448 个字符。 如果您正在使用可能包含更多的字符串属性，请确保为属性流中包括以下内容︰  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>更改 userPrincipalSuffix
在 Active Directory 中的范围内特性并非始终是已知用户和可能不适合作为登录 id。 Azure 的 AD 连接同步安装向导允许选择一个不同的特性，例如邮件。 但在某些情况下必须计算该属性。 例如，Contoso 公司有两个 Azure 的广告目录，一个用于生产，另一个用于测试。 他们希望用户在其测试租户用于另一个后缀的登录 id。  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

在此表达式中，将一切左侧的第一个@-sign(Word) 并使用一个固定的字符串连接。

### <a name="convert-a-multi-value-to-a-single-value"></a>将多值转换为单值
Active Directory 中的某些属性是多值架构中，即使它们看上去单个值在 Active Directory 用户和计算机。 例如，说明属性。  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

在此表达式中的特性具有值的情况下我们采取的第一项 （项） 的属性中、 移除前导空格和尾随空格 (Trim)，然后保持首先 448 个字符 （左） 在字符串中。

### <a name="do-not-flow-an-attribute"></a>不流特性
有关这部分的情况的背景信息，请参阅[控制特性的流程](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process)。

有两种流动特性的方法。 第一个是安装向导中提供，并允许您[删除选定的属性](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)。 如果从未进行过同步的属性之前，此选项效果。 但是，如果您开始同步此特性和以后使用这种功能中将其删除，然后管理特性和现有值同步引擎停止会留在 Azure 的广告。

如果您想要删除的属性的值并确保它不会在未来环绕，您需要创建自定义规则。

在 Fabrikam，我们已经意识到，一些我们同步到云环境的属性不应存在。 我们想要确保从 Azure AD 中删除这些属性。  
![扩展名不正确的特性](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- 创建一个新的入站的同步规则和填充描述![说明](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- 创建属性流类型**表达式**并具有源**AuthoritativeNull**。 文本的**AuthoritativeNull**表示，即使较低的优先级同步规则试图导入值，值应为空在 MV 中。
![扩展属性的转换](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- 保存同步规则。 启动**同步服务**，找到连接器，选择**运行**，并且**完全同步**。 此步骤将重新计算所有属性流。
- 验证计划中的更改将要导出的搜索连接器空间。
![分阶段的删除](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>下一步行动

- 阅读有关中[了解声明式配置](active-directory-aadconnectsync-understanding-declarative-provisioning.md)的配置模型详细信息。
- 了解有关详细[了解声明性资源调配表达式](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)中的表达式语言信息。

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
