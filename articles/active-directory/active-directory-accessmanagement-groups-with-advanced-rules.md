
<properties
    pageTitle="使用属性可创建高级的规则 |Microsoft Azure"
    description="如何-的创建高级的规则组包括支持表达式规则运算符和参数。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules"></a>使用属性可创建高级的规则

Azure 的传统门户网站可为您提供创建高级的规则以启用 Azure 活动目录 (AD Azure) 组更复杂的基于属性的动态成员的能力。  

当用户更改任何属性，系统计算所有动态组规则目录以查看用户的属性更改将触发了任何组中添加或删除。 如果用户满足一组中的规则，它们是作为成员添加到该组。 如果他们不能再满足它们是成员的一组规则，它们作为成员组中被删除的。

## <a name="to-create-the-advanced-rule"></a>创建高级的规则

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com)中，选择**活动目录**，，然后打开您的组织的目录。

2. 选择**组**选项卡，然后打开要编辑的组。

3. 选择**配置**选项卡，选择**高级的规则**选项，然后在文本框中输入高级的规则。

## <a name="constructing-the-body-of-an-advanced-rule"></a>创建高级规则的主体

有关动态组的成员可以创建高级的规则是本质上是一个二进制表达式，由三部分组成，true 或 false 的结果会导致。 三个部分是︰

- 左侧的参数
- 二元运算符
- 右边的常数

完整的高级的规则类似于下图: (leftParameter binaryOperator"RightConstant")，其中左和右括号所需的整个二进制表达式、 双引号内所需的适当的常量，和左侧参数的语法是 user.property。 高级的规则可以包含多个二进制表达式分隔-和，-或，和没有逻辑运算符。
结构合理的高级规则的示例如下︰

- (user.department-eq"销售")-或 （user.department-eq"市场"）
- (user.department-eq"销售")-和-不 (user.jobTitle-包含"SDE")

支持的参数表达式规则运算符的完整列表，请参见以下各节。

高级规则体的总长度不能超过 2048 个字符。

> [AZURE.NOTE]
>字符串和正则表达式操作区分大小写。 您还可以执行空检查，例如为常数时，使用 $null，user.department-eq $null。
包含引号的字符串"应使用转义 ' 字符，例如，user.department eq \`"销售"。

## <a name="supported-expression-rule-operators"></a>受支持的表达式规则运算符
下表列出了所有受支持的表达式规则运算符和其语法使用高级规则的正文中︰

| 运算符        | 语法         |
|-----------------|----------------|
| 不等于      | -ne            |
| 等于          | -eq            |
| 开头 | -notStartsWith |
| 以此开头     | -startsWith    |
| 不包含    | -notContains   |
| 包含        | -包含      |
| 不匹配       | -notMatch      |
| 匹配项           | -与匹配         |


## <a name="query-error-remediation"></a>查询错误修正
下表列出了潜在的错误以及如何会对其进行更正

| 查询解析错误     | 错误用法       | 更正后的使用情况             |
|-----------------------|-------------------|-----------------------------|
| 错误︰ 不支持的属性。                                      | (user.invalidProperty-eq"值")       | (user.department-eq"值")<br/>属性应与[支持的属性列表](#supported-properties)中的一个。                          |
| 错误︰ 属性上不支持运算符。                       | (user.accountEnabled-包含 true)                                                                               | （user.accountEnabled-eq 真）<br/>属性属于类型的布尔值。 使用受支持的运算符 (-eq 或-ne) 从上面的列表中的 boolean 类型的值类型。                                                                                                                                   |
| 错误︰ 查询编译错误。                                      | (user.department-eq"销售")-和 (user.department-eq"市场")(user.userPrincipalName-match"*@domain.ext") | (user.department-eq"销售")-和 （user.department-eq"市场"）<br/>逻辑运算符应匹配上面的所支持的属性列表中的一个。(user.userPrincipalName-匹配".*@domain.ext")or(user.userPrincipalName-匹配"@domain.ext$")Error正则表达式中。 |
| 错误︰ 二进制表达式不是以正确的格式。                     | (user.department-eq"销售")(user.department-eq"销售")(user.department-eq"销售")                             | （user.accountEnabled-eq 真）-和 (user.userPrincipalName-包含"alias@domain")<br/>查询有多个错误。 括号不在正确的位置。                                                                                                                            |
| 错误︰ 设置动态成员身份时出现未知的错误。 | (user.accountEnabled eq"True"AND user.userPrincipalName-包含"alias@domain")                               | （user.accountEnabled-eq 真）-和 (user.userPrincipalName-包含"alias@domain")<br/>查询有多个错误。 括号不在正确的位置。                                                                                                                            |

## <a name="supported-properties"></a>受支持的属性
以下是您可以使用高级规则中的用户属性︰

### <a name="properties-of-type-boolean"></a>布尔值类型的属性

允许的运算符

* -eq


* -ne


| 属性     | 允许的值  | 使用                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | 真正的假      | user.accountEnabled-eq 真）  |
| dirSyncEnabled | 真正的假空 | （user.dirSyncEnabled-eq 真） |

### <a name="properties-of-type-string"></a>类型字符串的属性

允许的运算符

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -包含


* -notContains


* -与匹配


* -notMatch

| 属性                 | 允许的值                                                                                        | 使用                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| 市/县                       | 任何字符串值或 $null                                                                           | (user.city-eq"值")                                   |
| 国家/地区                    | 任何字符串值或 $null                                                                            | (user.country-eq"值")                                |
| 部门                 | 任何字符串值或 $null                                                                          | (user.department-eq"值")                             |
| 显示名称                | 任何字符串值                                                                                 | (user.displayName-eq"值")                            |
| facsimileTelephoneNumber   | 任何字符串值或 $null                                                                           | (user.facsimileTelephoneNumber-eq"值")               |
| givenName                  | 任何字符串值或 $null                                                                           | (user.givenName-eq"值")                              |
| 职务                   | 任何字符串值或 $null                                                                           | (user.jobTitle-eq"值")                               |
| 邮件                       | 任何字符串值或 $null （用户的 SMTP 地址）                                                  | (user.mail-eq"值")                                   |
| mailNickName               | （邮件别名的用户） 的任何字符串值                                                            | (user.mailNickName-eq"值")                           |
| 移动                     | 任何字符串值或 $null                                                                           | (user.mobile-eq"值")                                 |
| 对象 Id                   | 用户对象的 GUID                                                                            | (user.objectId-eq"1111111-1111年-1111年-1111年-111111111111") |
| passwordPolicies           | 无 DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration、 DisableStrongPassword |   (user.passwordPolicies-eq"DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | 任何字符串值或 $null                                                                            | (user.physicalDeliveryOfficeName-eq"值")             |
| 邮政编码                 | 任何字符串值或 $null                                                                            | (user.postalCode-eq"值")                             |
| preferredLanguage          | ISO 639-1 代码                                                                                        | (user.preferredLanguage-eq"EN-US")                      |
| sipProxyAddress            | 任何字符串值或 $null                                                                            | (user.sipProxyAddress-eq"值")                        |
| 状态                      | 任何字符串值或 $null                                                                            | (user.state-eq"值")                                  |
| 街道地址              | 任何字符串值或 $null                                                                            | (user.streetAddress-eq"值")                          |
| 姓氏                    | 任何字符串值或 $null                                                                            | (user.surname-eq"值")                                |
| telephoneNumber            | 任何字符串值或 $null                                                                            | (user.telephoneNumber-eq"值")                        |
| usageLocation              | 两个字母表示的国家/地区代码                                                                           | (user.usageLocation-eq"美国")                             |
| 范围内          | 任何字符串值                                                                                     | (user.userPrincipalName-eq"alias@domain")               |
| 用户类型                   | 访客成员 $null                                                                                    | (user.userType-eq"成员")                              |

### <a name="properties-of-type-string-collection"></a>类型字符串集合的属性

允许的运算符

* -包含


* -notContains

| Poperties      | 允许的值                        | 使用                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | 任何字符串值                      | (user.otherMails-包含"alias@domain")           |
| 代理地址 | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-包含"SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>扩展属性和自定义属性
动态成员身份规则并支持扩展属性和自定义属性。

扩展属性的同步完成从窗口服务器 AD 的前提上，并采取"ExtensionAttributeX"，其中 X 等于 1-15 的格式。
使用扩展属性规则的一个示例是

（user.extensionAttribute15 eq"市场部"）

在部署 Windows 服务器 AD 或从连接的 SaaS 应用程序的自定义属性的同步完成从和格式的"user.extension_[GUID]\__ [属性]"，其中 [GUID] 是在 AAD AAD 中创建该特性的应用程序的唯一标识符，而 [属性] 按照当初创建的是属性的名称。
规则所使用的自定义属性的一个示例是

user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

可以通过查询用户目录中找到的自定义属性名称的属性使用图形浏览器和搜索该属性名。

## <a name="direct-reports-rule"></a>直接报告规则
现在，您可以使用填充基于用户的管理器属性组中的成员。

**要将组配置为"管理员"组**

1. 在 Azure 经典门户中，**活动目录**，请单击，然后单击组织的目录的名称。

2. 选择**组**选项卡，然后打开要编辑的组。

3. 选择**配置**选项卡，然后选择**高级规则**。

4. 键入以下语法规则︰

    直接报告的*{obectID_of_manager} 的直接下属*。 直接下属的有效规则的一个示例是

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    其中，"62e19b97-8b3d-4d4a-a106-4ce66896a863"是管理器的对象 Id。 Azure ad 用户页面中的管理器的用户**配置文件选项卡**上，可以找到的对象 ID。

3. 在保存该规则，满足该规则的所有用户将被都加入作为组的成员。 它可以需要几分钟时间，首次填充组。


## <a name="using-attributes-to-create-rules-for-device-objects"></a>使用属性来创建设备对象的规则

您还可以创建选择组中的成员资格的设备对象的规则。 可以使用下列设备属性︰

| 属性              | 允许的值                  | 使用                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| 显示名称             | 任何字符串值                | (device.displayName-eq"杨志 Iphone")                       |
| deviceOSType            | 任何字符串值                | (device.deviceOSType-eq"IOS")                             |
| deviceOSVersion         | 任何字符串值                | （设备。OSVersion-eq"9.1")                                |
| isDirSynced             | 真正的假空                 | (device.isDirSynced-eq"true")                             |
| isManaged               | 真正的假空                 | (device.isManaged-eq"假")                              |
| isCompliant             | 真正的假空                 | (device.isCompliant-eq"true")                             |
| deviceCategory          | 任何字符串值                | (device.deviceCategory eq"")                              |
| deviceManufacturer      | 任何字符串值                | (device.deviceManufacturer-eq"Microsoft")                 |
| deviceModel             | 任何字符串值                | (device.deviceModel-eq"IPhone"7 +)                        |
| deviceOwnership         | 任何字符串值                | (device.deviceOwnership eq"")                             |
| 域名称              | 任何字符串值                | (device.domainName-eq"contoso.com")                       |
| enrollmentProfileName   | 任何字符串值                | (device.enrollmentProfileName eq"")                       |
| isRooted                | 真正的假空                 | (device.deviceOSType-eq"true")                            |
| managementType          | 任何字符串值                | (device.managementType eq"")                              |
| organizationalUnit      | 任何字符串值                | (device.organizationalUnit eq"")                          |
| deviceId                | 有效 deviceId                | (device.deviceId-eq"d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [AZURE.NOTE]
> 不能使用 Azure 的经典门户中的"简单规则"下拉列表创建这些设备规则。


## <a name="additional-information"></a>其他信息
这些文章在 Azure Active Directory 提供附加信息。

* [疑难解答动态组的成员身份](active-directory-accessmanagement-troubleshooting.md)

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [配置组设置 azure Active Directory cmdlet](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
