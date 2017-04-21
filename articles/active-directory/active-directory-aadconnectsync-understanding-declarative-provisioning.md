<properties
    pageTitle="Azure AD 连接同步︰ 了解声明式配置 |Microsoft Azure"
    description="介绍了在 Azure AD 连接声明性配置的配置模型。"
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD 连接同步︰ 了解声明性资源调配
本主题说明在 Azure AD 连接的配置模型。 该模型称为声明性资源调配，它使您可以轻松地更改配置。 本主题中描述的很多事情都高级，并且不要求对于大多数客户方案。

## <a name="overview"></a>概述
声明提供程序处理来自源连接目录对象并确定方式的对象和属性应转换从源到目标。 对象处理在同步管道中，管道是相同的入站和出站规则。 元节从连接器空间是一个入站的规则和出站规则到连接器空间是中元节。

![同步管道](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

管道有几个不同的模块。 每个负责对象同步过程中的一个概念。

![同步管道](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- 源，源对象
- [作用域](#scope)，查找范围中的所有同步规则
- [加入](#join)，连接器空间和元节之间的确定关系
- [转换](#transform)、 计算应如何转换特性和流量
- [优先](#precedence)解决冲突属性贡献
- 目标，目标对象

## <a name="scope"></a>作用域
作用域模块正在评估对象并确定作用域中，并应包括处理过程中的规则。 这取决于对象上的属性值，计算不同同步规则范围内。 例如，没有 Exchange 邮箱的已禁用的用户有不同的规则，比的已启用邮箱用户。  
![作用域](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

范围定义为组和子句。 子句是组内。 组中的所有子句之间使用逻辑 AND。 例如，(部门 IT 和国家/地区 = = 丹麦)。 逻辑 OR 组之间使用。

![作用域](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
这张图片中的范围应该读作 (部门 IT 和国家/地区 = = 丹麦) 或者 (国家/地区 = 瑞典)。 如果组 1 或组 2 计算结果为 true，那么该规则是在范围内。

作用域模块支持以下操作。

操作 | 说明
--- | ---
等，不等 | 如果值是该属性的值相等的计算结果字符串比较。 多值属性，请参阅 ISIN 和 ISNOTIN。
小于二 LESSTHAN_OR_EQUAL | 如果值是计算结果的字符串比较早于该属性中的值。
包含，NOTCONTAINS | 如果值内可能会找到某个位置值属性中的计算结果字符串比较。
STARTSWITH NOTSTARTSWITH | 计算值是否在属性中的值的开头的字符串比较。
ENDSWITH NOTENDSWITH | 如果值是属性中的值的最终计算结果的字符串比较。
大于 GREATERTHAN_OR_EQUAL | 如果值大于该属性中的值计算结果的字符串比较。
ISNULL ISNOTNULL | 如果该特性不存在计算结果的对象。 如果该特性不存在，因此空，规则是在范围内。
ISIN ISNOTIN | 如果存在中定义的属性值的计算结果。 此操作是多值的相等和不等的变化。 应在属性是多值的属性，如果任何属性值中可以找到的值，然后规则位于范围。
ISBITSET ISNOTBITSET | 如果设置了特定位的计算结果。 例如，可用来评估在 userAccountControl 查看用户启用或禁用的位。
ISMEMBEROF ISNOTMEMBEROF | 为连接器空间中的一组 DN 应包含的值。 如果对象是指定组的成员，规则是在范围内。

## <a name="join"></a>加入
同步管道中的加入模块负责在目标中查找源中的对象与对象之间的关系。 在入站规则，这种关系是在元节中查找与对象的关系连接器空间中的对象。  
![Cs 和 mv 之间加入](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
目标是中元节，另一个接头，通过创建对象时它应该与之关联，请参阅。 例如，帐户资源林中帐户目录林中的用户应加入与资源目录林中的用户。

入站规则上主要使用联接同一元节对象的连接器空间对象联接在一起。

联接被指一个或多个组。 在一个组，则必须子句。 组中的所有子句之间使用逻辑 AND。 逻辑 OR 组之间使用。 组是从上到下的顺序处理。 一组有目标中找到一个匹配对象，被不评估任何其他连接规则。 如果零个或多个对象位于，处理将继续到下一组规则。 因此，规则应以最明确的第一顺序创建和结束时更模糊。  
![加入定义](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
这张图片中的联接从上到下处理。 首次同步管道查看是否有匹配的雇员 id。 如果不是，第二条规则可以看到如果可以使用帐户名来联接在一起的对象。 如果不匹配或者，第三个也是用户的最后规则将是用户的更模糊匹配使用的名称。

如果所有的联接规则中已经过评估并不是正好有一个匹配项，则使用**描述**页上的**链接类型**。 如果此选项设置为**资源调配**，被创建新目标中的对象。  
![供应或联接](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

对象只应有一个单个同步规则与联接规则范围内。 如果有多个同步规则定义联接的位置，就会出错。 优先权不用于解决加入冲突。 对象必须具有一个连接规则范围内的属性具有相同的入站/出站方向流动。 如果您需要流动特性对入站和出站到同一个对象，您必须具有入站和出站同步规则与联接。

出站连接有特殊的行为时它会尝试调配到目标连接器空间对象。 DN 属性用于首先尝试反向联接。 如果具有相同 DN 的目标连接器空间中已存在对象，这些对象相连。

一旦新的同步规则进入范围时，是只计算连接模块。 当对象已加入时，不脱离即使不再满足联接条件。 如果您想要 disjoin 对象，加入这些对象的同步规则必须超出范围。

### <a name="metaverse-delete"></a>元节删除
元节对象仍保持原样存在与**链接类型**的作用域中的一个同步规则设置为**规定**或**StickyJoin**。 不允许连接线设置一个新的对象，到了元节，但它已加入使用 StickyJoin，它之前，必须删除在源中元节对象被删除。

元节对象中删除时，所有标记为要**设置**出站同步规则关联的对象标记为删除。

## <a name="transformations"></a>转换
转换用于定义如何特性应流从源到目标。 流可以具有以下**流类型**之一︰ 直接、 常数或表达式。 直接流流动特性值为-与任何其他转换。 一个常量的值设置指定的值。 表达式使用声明性资源调配的表达式语言来表达转换应如何。 [了解资源调配的声明性表达式语言](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)主题中找不到表达式语言的详细信息。

![供应或联接](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

在最初创建该对象时，才应设置该属性定义**应用一次**复选框。 例如，可以使用此配置来设置新的用户对象的初始密码。

### <a name="merging-attribute-values"></a>合并属性值
在属性流没有确定如果应从几个不同的连接器合并多值的属性的设置。 默认值是**更新**，即具有最高优先级的同步规则应该获胜。

![合并类型](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

还有**合并**和**MergeCaseInsensitive**。 这些选项允许您合并来自不同源的值。 例如，它可以用于合并来自几个不同的目录林的成员或代理地址属性。 使用此选项时，对象的作用域中的所有同步规则必须都使用相同的合并类型。 不能定义从一个接口的**更新**，并从另一个**合并**。 如果您尝试，您将收到错误。

**合并**和**MergeCaseInsensitive**之间的差别是如何处理重复的属性值。 同步引擎确保重复的值不会插入到目标属性。 与**MergeCaseInsensitive**，重复的值的差异与在的情况下不会出现。 例如，您不应看到两个"SMTP:bob@contoso.com"和"smtp:bob@contoso.com"目标属性中。 精确的值和多值只搜索**合并**，只是在不同情况下可能会出现。

**替换**选项是相同**更新**，但不是使用它。

### <a name="control-the-attribute-flow-process"></a>控件属性流过程
当多个入站的同步规则被配置为参与同一元节属性，然后优先级用于确定获奖者。 具有最高优先级 （最低数字值） 的同步规则要贡献值。 对于出站规则会发生相同情况。 同步与最高优先级 wins 规则并参与到连接目录的值。

在某些情况下，而不是参与某个值，同步规则应确定的其他规则的行为方式。 有这种情况下使用一些特殊文本。

为入站同步规则，文本**为空**可表示流没有参与任何值。 优先级较低的另一个规则可以提供一个值。 如果没有规则提供一个值，则将移除该元属性。 对于出站规则，如果**空**后的最终值同步的所有规则都已都处理，则该值将移除连接目录中。

文本的**AuthoritativeNull**是类似于**空**，但有没有较低的优先顺序规则可能产生一个值的差异。

属性流还可以使用**IgnoreThisFlow**。 它是类似于空在这个意义上，它指示没有任何贡献。 不同之处在于，它不会在目标中删除已存在的值。 正像属性流从未存在。

下面是一个示例︰

在*了解到 AD-用户交换混合*可以发现以下流︰  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
该表达式应读作︰ 如果用户邮箱位于 Azure 的广告，然后流 Azure 广告与广告中的特性。 如果不是，不流回 Active Directory 的任何内容。 在这种情况下，它会在 AD 中保留现有值。

### <a name="importedvalue"></a>ImportedValue
由于属性名必须括在引号，而不是方括号，函数 ImportedValue 是不同于其他所有功能︰  
`ImportedValue("proxyAddresses")`.

通常在同步期间属性将使用所需的值，即使还没有已导出或导出 （"顶部的塔"） 过程中接收到的错误。 入站的同步假定尚未尚未最终到达连接的目录属性达到它。 在某些情况下，很重要，只同步已确认通过连接目录 （"全息图和增量导入塔"） 的值。

现成的同步规则中找不到此函数的示例，*在从广告 – 从 Exchange 用户通用*。 已确认成功导出值时，仅应混合交换中同步 exchange 联机添加的值︰  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>优先级
当多个同步规则试图参与到目标相同的属性值时，优先值用于确定入选方。 具有优先级最高，最低的数字值，规则将参与冲突中的属性。

![合并类型](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

这种排序可用于定义更精确的一小部分的对象的属性流。 例如，出-的-框-规则确保启用的帐户 (**用户 AccountEnabled**) 中的属性具有来自其他帐户的优先级。

优先级可以定义连接器之间。 它具有更好的数据，第一次参与值使连接器。

### <a name="multiple-objects-from-the-same-connector-space"></a>从相同的连接器空间中的多个对象
如果您有加入同一元节对象的同一连接器空间中的多个对象，必须调整优先级。 如果多个对象在同一同步规则的范围内，则不能确定优先级同步引擎。 源对象应该参与到了元节中的值不明确。 此配置被报告为不明确，即使源中的属性具有相同的值。  
![连接到同一个 mv 对象的多个对象](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

这种情况下，您需要更改同步规则的范围，使源对象作用域中具有不同的同步规则。 它允许您定义不同的优先级。  
![连接到同一个 mv 对象的多个对象](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>下一步行动

- 了解有关详细[了解声明性资源调配表达式](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)中的表达式语言信息。
- 请参阅如何声明性资源调配是使用过的全新理解[的默认配置](active-directory-aadconnectsync-understanding-default-configuration.md)。
- 了解如何更改实际使用声明性资源调配中[如何对默认配置进行更改](active-directory-aadconnectsync-change-the-configuration.md)。
- 继续读取用户和联系人在如何协同工作[了解用户和联系人](active-directory-aadconnectsync-understanding-users-and-contacts.md)。

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

**参考主题**

- [Azure AD 连接同步︰ 函数参考](active-directory-aadconnectsync-functions-reference.md)
