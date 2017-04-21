<properties
    pageTitle="Azure AD 连接同步︰ 了解声明性资源调配表达式 |Microsoft Azure"
    description="介绍了资源调配的声明性表达式。"
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
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD 连接同步︰ 了解声明性资源调配表达式
声明配置 Forefront 标识管理器 2010 年第一次引入基于 azure AD 连接同步。 它允许您实现完整标识集成的业务逻辑，而无需编写编译后的代码。

声明性资源调配的核心部分是属性流中使用的表达式语言。 所用的语言是 Microsoft® Visual Basic® 的应用程序 (VBA) 的一个子集。 在 Microsoft Office 中使用这种语言并具有经验的 VBScript 的用户也将识别它。 提供声明性表达式语言只使用函数并不是结构化的语言。 没有任何方法或语句。 改为明确程序流到嵌套函数。

有关更多详细信息，请参阅[Office 2013 的语言参考 Visual Basic for Applications 欢迎](https://msdn.microsoft.com/library/gg264383.aspx)。

特性是强类型。 函数只接受正确类型的属性。 这也是区分大小写的。 函数名称和属性名称必须具有正确的大小写，或将引发错误。

## <a name="language-definitions-and-identifiers"></a>语言的定义和标识符

- 函数具有跟括号中的参数的名称︰ FunctionName （参数 1 的参数 N）。
- 属性由方括号: [属性名称]
- 参数由百分号: %参数名称 %
- 字符串常量用引号引起来︰ 例如，"Contoso"(注意︰ 必须使用直引号""不弯引号和"")
- 数字值是表示不带引号，并应为十进制。 为前缀的十六进制值 & h。 例如，98052，& h f F
- 布尔值表示具有常量︰ 真、 假。
- 用他们的姓名以表示内置常量和原义字符︰ 空，CRLF，IgnoreThisFlow

### <a name="functions"></a>函数
声明性资源调配使用许多功能使能够转换属性值。 可以嵌套这些函数，以便将结果从一个函数中传递给另一个函数。

`Function1(Function2(Function3()))`

[函数参考](active-directory-aadconnectsync-functions-reference.md)中找不到函数的完整列表。

### <a name="parameters"></a>参数
参数定义连接器或管理员使用 PowerShell。 参数通常包含从一个系统到另一个系统是不同的值，例如位于用户的域的名称。 属性流中，可以使用这些参数。

活动目录连接器为入站同步规则提供以下参数︰

| 参数名称 | 注释 |
| --- | --- |
| Domain.Netbios | 当前正在导入域，例如 FABRIKAMSALES 的 Netbios 格式 |
| Domain.FQDN | 当前正在导入域，例如 sales.fabrikam.com 的 FQDN 格式 |
| Domain.LDAP | LDAP 格式的当前正在导入域，例如 DC = 销售额，DC = fabrikam，DC = com |
| Forest.Netbios | Netbios 名称格式的目录林中当前正在导入，例如 FABRIKAMCORP |
| Forest.FQDN | 林名称当前正在导入，例如 fabrikam.com 的 FQDN 格式 |
| Forest.LDAP | LDAP 名称格式的目录林中当前正在导入，例如 DC = fabrikam，DC = com |

系统提供了以下参数，用于获取当前正在运行的接口的标识符︰  
`Connector.ID`

下面是域的一个示例填充用户所在的 netbios 名称与元节属性域︰  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>运算符
可以使用下列运算符︰

- **比较**︰ <，< =、 <>、 =、 >、 > =
- **数学**: +，-， \*，-
- **字符串**︰ & （连接）
- **逻辑**︰ & & （和），| |（或者）
- **求值顺序**: （)

运算符从左到右进行计算，并具有相同的计算优先级。 也就是说， \* （倍频） 则不会计算前-（减）。 2\*(5 + 3) 2 相同\*5 + 3。 用括号 （） 来更改求值顺序，从左到右计算顺序不合适时。

## <a name="multi-valued-attributes"></a>多值的属性
这些函数可以应用于单值和多值属性。 对于多值属性，该函数通过每个值进行操作，并将相同的函数应用于每个值。

例如︰  
`Trim([proxyAddresses])`不要在 proxyAddress 属性中的每个值的修剪。  
`Word([proxyAddresses],1,"@") & "@contoso.com"`每个值与@-sign,替换与域@contoso.com。  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`SIP 地址查找并删除这些值。

## <a name="next-steps"></a>下一步行动

- 阅读有关中[了解声明式配置](active-directory-aadconnectsync-understanding-declarative-provisioning.md)的配置模型详细信息。
- 请参阅如何声明性资源调配是使用过的全新理解[的默认配置](active-directory-aadconnectsync-understanding-default-configuration.md)。
- 了解如何更改实际使用声明性资源调配中[如何对默认配置进行更改](active-directory-aadconnectsync-change-the-configuration.md)。

**概述主题**

- [Azure AD 连接同步︰ 了解并自定义同步](active-directory-aadconnectsync-whatis.md)
- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

**参考主题**

- [Azure AD 连接同步︰ 函数参考](active-directory-aadconnectsync-functions-reference.md)
