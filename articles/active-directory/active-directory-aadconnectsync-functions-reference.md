<properties
    pageTitle="Azure AD 连接同步︰ 函数引用 |Microsoft Azure"
    description="在 Azure AD 连接同步的声明性设置表达式的引用。"
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD 连接同步︰ 函数参考

在 Azure AD 连接，函数用于在同步期间处理特性值。  
使用以下格式表示函数的语法︰  
`<output type> FunctionName(<input type> <position name>, ..)`

如果某个函数重载方法，接受多个语法，列出了所有有效的语法。  
强类型的函数，它们确认，类型传递中匹配的记录的类型。  
如果类型不匹配，将引发错误。

类型表示使用以下语法︰

- **bin** – 二进制文件
- **布尔值**-布尔值
- **dt** — UTC 日期/时间
- **枚举**– 已知常数的枚举
- **exp** – 表达式，表达式的计算结果为布尔值应
- **mvbin** — Multi-Valued 二进制
- **mvstr** — Multi-Valued 字符串
- **mvref** — Multi-Valued 的引用
- **num** – 数字
- **ref** – 引用
- **str** -字符串
- **var** – （几乎） 任何其他类型的变体
- **void** – 不返回值

使用类型**mvbin**、 **mvstr**和**mvref**函数可以仅适用于多值属性。 **Bin**和**str**， **ref**函数处理单值和多值属性。

## <a name="functions-reference"></a>功能参考

函数的列表 | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**转换** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**日期 / 时间** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [现在](#now)
[NumFromDate](#numfromdate) |  
**目录** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**评估** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**数学** |  
[BitAnd](#bitand) | [BitOr](#bitor) | [RandomNum](#randomnum)
**多值** |  
[包含](#contains) | [计数](#count) | [项目](#item) | [ItemOrNull](#itemornull)
[加入](#join) | [RemoveDuplicates](#removeduplicates) | [拆分](#split) |
**程序流** |  
[错误](#error) | [IIF](#iif)  | [开关](#switch)
**文本** |  
[GUID](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [LCase](#lcase)
[向左](#left) | [长度](#len) | [LTrim](#ltrim) | [中期](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [替换](#replace)
[ReplaceChars](#replacechars) | [向右](#right) | [RTrim](#rtrim) | [修剪](#trim)
[UCase](#ucase) | [单词](#word)

----------
### <a name="bitand"></a>BitAnd

**说明︰**  
BitAnd 函数设置的值指定的位数。

**语法︰**  
`num BitAnd(num value1, num value2)`

- 值 1，值 2︰ 应该在一起用于启用和禁用的数值

**注释︰**  
此函数将两个参数转换为二进制表示形式，并设置了一个位︰

- 如果一个或两个*掩码*和*标记*中的相应位均为 0-0
- 1-如果两个相应的位为 1。

换句话说，它在所有情况下，除非这两个参数的相应位后 1 返回 0。

**示例︰**  
`BitAnd(&HF, &HF7)`  
由于十六进制"F"和"F7"计算此值，则返回 7。

----------
### <a name="bitor"></a>BitOr

**说明︰**  
BitOr 函数设置的值指定的位数。

**语法︰**  
`num BitOr(num value1, num value2)`

- 值 1，值 2︰ 应该用 or 连在一起的数字值

**注释︰**  
此函数将两个参数转换为二进制表示形式，并设置了一个位为 1，如果一个或两个掩码和标记中的相应位都为 1，并为 0 如果两个相应的位为 0。 换句话说，它在所有情况下除外，这两个参数的相应位均为 0 返回 1。

----------
### <a name="cbool"></a>CBool

**说明︰**  
CBool 函数返回一个布尔值，根据计算出的表达式

**语法︰**  
`bool CBool(exp Expression)`

**注释︰**  
如果表达式为非零值，则 CBool 返回 True，否则返回 False。

**示例︰**  
`CBool([attrib1] = [attrib2])`  

返回 True，如果两个属性具有相同的值。

----------
### <a name="cdate"></a>CDate

**说明︰**  
CDate 函数从字符串返回 UTC 日期时间。 日期时间不同步是本机属性类型，但使用的某些功能。

**语法︰**  
`dt CDate(str value)`

- 字符串与日期、 时间和选择时区值︰

**注释︰**  
返回的字符串总是以 utc 格式。

**示例︰**  
`CDate([employeeStartTime])`  
返回日期时间根据员工的开始时间

`CDate("2013-01-10 4:00 PM -8")`  
返回日期时间代表"2013年-01-11 12:00 AM"

----------
### <a name="cguid"></a>CGuid

**说明︰**  
CGuid 函数将 GUID 的字符串表示形式转换为其二进制表示形式。

**语法︰**  
`bin CGuid(str GUID)`

- 在此模式中设置格式的字符串︰ xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx 或 {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>包含

**说明︰**  
包含函数查找的字符串内的多值属性

**语法︰**  
`num Contains (mvstring attribute, str search)`-区分大小写  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-区分大小写

- 特性︰ 要搜索的多值的属性。
- 搜索︰ 在属性中查找的字符串。
- Casetype︰ 输入或区分大小写。

返回多值属性中找到的字符串中的索引。 如果未找到该字符串，则返回 0。

**注释︰**  
对于多值的字符串属性，搜索查找子字符串的值。  
对于引用属性，搜索的字符串必须与要视为匹配的值完全匹配。

**示例︰**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
如果代理地址属性都有一个主电子邮件地址 (用大写字母表示"SMTP:")，然后将 proxyAddress 属性返回，否则返回一个错误。

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**说明︰**  
ConvertFromBase64 函数将指定的 base64 编码值转换为一个正则字符串。

**语法︰**  
`str ConvertFromBase64(str source)`-假定 Unicode 编码  
`str ConvertFromBase64(str source, enum Encoding)`

- 来源︰ Base64 编码的字符串  
- 编码︰ Unicode，ascii 码 UTF8

**示例**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

这两个示例返回"*世界，你好 ！*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**说明︰**  
ConvertFromUTF8Hex 函数将指定的 UTF8 编码的十六进制值转换为字符串。

**语法︰**  
`str ConvertFromUTF8Hex(str source)`

- 来源︰ UTF8 2 字节编码的字符串

**注释︰**  
此函数，并在 ConvertFromBase64([],UTF8) 的结果是友好的 DN 属性之间的区别。  
这种格式使用 Azure Active Directory 为 DN。

**示例︰**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
返回"*世界，你好 ！*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**说明︰**  
ConvertToBase64 函数将一个字符串转换为 Unicode base64 字符串。  
将一个整数数组的值转换为其用 base64 数字编码的等效字符串表示。

**语法︰**  
`str ConvertToBase64(str source)`

**示例︰**  
`ConvertToBase64("Hello world!")`  
返回"SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**说明︰**  
ConvertToUTF8Hex 函数将一个字符串转换为 UTF8 编码的十六进制值。

**语法︰**  
`str ConvertToUTF8Hex(str source)`

**注释︰**  
此函数的输出格式使用 Azure Active Directory 作为 DN 属性格式。

**示例︰**  
`ConvertToUTF8Hex("Hello world!")`  
返回 48656C6C6F20776F726C6421

----------
### <a name="count"></a>计数

**说明︰**  
计数函数返回多值特性中的元素数

**语法︰**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**说明︰**  
CNum 函数采用一个字符串并返回数值的数据类型。

**语法︰**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**说明︰**  
将字符串转换为引用属性

**语法︰**  
`ref CRef(str value)`

**示例︰**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**说明︰**  
CStr 函数将转换为字符串数据类型。

**语法︰**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- 值︰ 可以是数值、 引用属性或布尔值。

**示例︰**  
`CStr([dn])`  
无法返回"cn = Joe，dc = contoso，dc = com"

----------
### <a name="dateadd"></a>DateAdd

**说明︰**  
返回包含已添加指定的时间间隔的日期。

**语法︰**  
`dt DateAdd(str interval, num value, dt date)`

- 间隔︰ 字符串表达式，表示您想要添加的时间间隔。 该字符串必须具有下列值之一︰
 - yyyy 年
 - 问︰ 季度
 - 米月
 - y 的年份的第几天
 - d 一天
 - w 工作日
 - ww 周
 - h
 - n 分钟
 - s 第二
- 值︰ 您想要添加的单位数。 它可以是正数 （此时将获取今后的日期） 或负数 （此时将获取过去的日期）。
- 日期︰ 日期时间表示要向其添加间隔的日期。

**示例︰**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
添加 3 个月，并返回表示"2001年-04-01"的日期时间。

----------
### <a name="datefromnum"></a>DateFromNum

**说明︰**  
DateFromNum 函数将一个值中广告的日期格式为 DateTime 类型。

**语法︰**  
`dt DateFromNum(num value)`

**示例︰**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
返回日期时间表示 2012年-01-01 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**说明︰**  
DNComponent 函数返回指定 DN 组件从左值。

**语法︰**  
`str DNComponent(ref dn, num ComponentNumber)`

- dn: 引用特性来解释
- ComponentNumber: DN，返回该组件

**示例︰**  
`DNComponent([dn],1)`  
如果 dn"cn = Joe，ou =......，"它返回乔

----------
### <a name="dncomponentrev"></a>DNComponentRev

**说明︰**  
DNComponentRev 函数返回的值指定 DN 组件从右 （结束）。

**语法︰**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- dn: 引用特性来解释
- ComponentNumber-返回的 DN 中的组件
- 选项︰ DC – 忽略所有组件"dc ="

**示例︰**  
如果 dn"cn = Joe，ou = 亚特兰大，ou = 正式上市时，ou = 美国，dc = contoso，dc = com"然后  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
同时返回我们。

----------
### <a name="error"></a>错误

**说明︰**  
误差函数用于返回自定义错误。

**语法︰**  
`void Error(str ErrorMessage)`

**示例︰**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
如果不存在属性帐户名，则该对象将引发错误。

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**说明︰**  
EscapeDNComponent 函数的 DN 的一部分，其进行转义，因此它可以表示在 LDAP 中。

**语法︰**  
`str EscapeDNComponent(str value)`

**示例︰**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
确保显示名称属性中是否必须在 LDAP 中转义的字符可以在 LDAP 目录中创建该对象。

----------
### <a name="formatdatetime"></a>FormatDateTime

**说明︰**  
使用 FormatDateTime 函数来设置字符串到日期时间格式与指定的格式

**语法︰**  
`str FormatDateTime(dt value, str format)`

- 值︰ 日期时间格式中的值
- 格式︰ 一个字符串，表示要转换为的格式。

**注释︰**  
可能的值的格式可以在以下位置找到︰[用户定义的日期/时间格式 （格式功能）](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**示例︰**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
在"2007年-12-25"的结果。

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
可能会导致"20140905081453.0Z"

----------
### <a name="guid"></a>GUID

**说明︰**  
GUID 的函数生成一个新的随机 GUID

**语法︰**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**说明︰**  
IIF 函数返回一组可能的值基于指定的条件之一。

**语法︰**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- 条件︰ 任何值或表达式，可以计算为真或假。
- valueIfTrue︰ 如果条件的计算结果为 true，则返回的值。
- valueIfFalse︰ 如果条件的计算结果为 false，则返回的值。

**示例︰**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 如果用户是拘留，返回具有"t-"它，其他的开头添加的用户的别名返回是用户的别名。

----------
### <a name="instr"></a>InStr

**说明︰**  
InStr 函数在字符串中查找子字符串的第一个匹配项

**语法︰**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- stringcheck︰ 要搜索字符串
- stringmatch︰ 要查找的字符串
- 启动︰ 起始位置来查找子字符串
- 比较︰ vbTextCompare 或 vbBinaryCompare

**注释︰**  
返回子找不到或找不到 0 的位置的位置。

**示例︰**  
`InStr("The quick brown fox","quick")`  
为 5 Evalues

`InStr("repEated","e",3,vbBinaryCompare)`  
计算结果为 7

----------
### <a name="instrrev"></a>InStrRev

**说明︰**  
InStrRev 函数在字符串中查找子字符串的最后一个匹配项

**语法︰**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- stringcheck︰ 要搜索字符串
- stringmatch︰ 要查找的字符串
- 启动︰ 起始位置来查找子字符串
- 比较︰ vbTextCompare 或 vbBinaryCompare

**注释︰**  
返回子找不到或找不到 0 的位置的位置。

**示例︰**  
`InStrRev("abbcdbbbef","bb")`  
返回 7

----------
### <a name="isbitset"></a>IsBitSet

**说明︰**  
设置该函数 IsBitSet 测试中是否有一些

**语法︰**  
`bool IsBitSet(num value, num flag)`

- 值︰ 一个数值，它是 evaluated.flag︰ 有位要进行求值的数值

**示例︰**  
`IsBitSet(&HF,4)`  
因为在"F"的十六进制值设置位"4"，返回 True

----------
### <a name="isdate"></a>IsDate

**说明︰**  
如果该表达式可以是计算结果为 DateTime 类型，然后 IsDate 函数的计算结果为 True。

**语法︰**  
`bool IsDate(var Expression)`

**注释︰**  
用来确定是否 cdate （） 都不会成功。

----------
### <a name="isempty"></a>IsEmpty

**说明︰**  
如果属性 CS 或 MV 中出现，但计算结果为一个空字符串，然后 IsEmpty 函数计算结果为 True。

**语法︰**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**说明︰**  
如果字符串无法转换为 GUID，然后 IsGuid 函数求值为 true。

**语法︰**  
`bool IsGuid(str GUID)`

**注释︰**  
GUID 指以下其中一种模式的字符串︰ xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx 或 {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

用来确定是否 CGuid() 就可以成功。

**示例︰**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
如果 StrAttribute 有 GUID 格式，返回二进制表示形式，否则返回空值。

----------
### <a name="isnull"></a>IsNull

**说明︰**  
如果表达式计算结果为 Null，然后 IsNull 函数返回 true。

**语法︰**  
`bool IsNull(var Expression)`

**注释︰**  
属性中，Null 表示缺少的特性。

**示例︰**  
`IsNull([displayName])`  
如果该特性不存在 MV 的 CS 中，则返回 True。

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**说明︰**  
如果表达式为 null 或空字符串，IsNullOrEmpty 函数返回 true。

**语法︰**  
`bool IsNullOrEmpty(var Expression)`

**注释︰**  
对于属性，这将计算结果为 True 如果属性不存在或存在，但将为空字符串。  
此函数的反函数被命名为 IsPresent。

**示例︰**  
`IsNullOrEmpty([displayName])`  
如果该特性不存在，或将 MV 的 CS 中为空字符串，则返回 True。

----------
### <a name="isnumeric"></a>IsNumeric

**说明︰**  
IsNumeric 函数返回一个布尔值，该值指示是否可将表达式计算为数字类型。

**语法︰**  
`bool IsNumeric(var Expression)`

**注释︰**  
用于确定 CNum() 可以成功分析表达式。

----------
### <a name="isstring"></a>IsString

**说明︰**  
如果表达式可以计算为字符串类型，然后 IsString 函数求值为 True。

**语法︰**  
`bool IsString(var expression)`

**注释︰**  
用于确定 CStr() 可以成功分析表达式。

----------
### <a name="ispresent"></a>IsPresent

**说明︰**  
如果表达式计算结果为一个字符串，不为 Null 且不为空，然后 IsPresent 函数返回 true。

**语法︰**  
`bool IsPresent(var expression)`

**注释︰**  
此函数的反函数被命名为 IsNullOrEmpty。

**示例︰**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>项目

**说明︰**  
Item 函数返回多值字符串/属性中的一项。

**语法︰**  
`var Item(mvstr attribute, num index)`

- 特性︰ 多值的属性
- 索引︰ 对多值字符串中项的索引。

**注释︰**  
因为后一种功能返回多值属性中的项目索引，Item 函数非常有用以及包含函数。

如果索引超出范围，则将引发错误。

**示例︰**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
返回主电子邮件地址。

----------
### <a name="itemornull"></a>ItemOrNull

**说明︰**  
ItemOrNull 函数返回多值字符串/属性中的一项。

**语法︰**  
`var ItemOrNull(mvstr attribute, num index)`

- 特性︰ 多值的属性
- 索引︰ 对多值字符串中项的索引。

**注释︰**  
因为后一种功能返回多值属性中的项目索引，ItemOrNull 函数非常有用以及包含函数。

如果索引超出范围，则返回 Null 值。

----------
### <a name="join"></a>加入

**说明︰**  
Join 函数采用一个多值的字符串，并返回具有指定插入的每个项之间的分隔符的单值的字符串。

**语法︰**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- 特性︰ 包含要合并的多值的属性。
- 分隔符︰ 用于分隔返回的字符串中的子字符串的任何字符串。 如果省略，空格字符 ("") 使用。 如果 Delimiter 是零长度字符串 ("") 或无，列表中的所有项都连接不使用分隔符。

**备注**  
之间的联接和拆分功能没有奇偶校验。 Join 函数采用一个字符串数组，然后用分隔符字符串，返回单个字符串联接起来。 Split 函数采用一个字符串并将其分开在分隔符，返回的字符串数组。 但是，关键差别是联接可以使用任何分隔符字符串的字符串串联，拆分只可以单独使用一个字符分隔符的字符串。

**示例︰**  
`Join([proxyAddresses],",")`  
无法返回︰"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**说明︰**  
LCase 函数将字符串中的所有字符都转换为小写。

**语法︰**  
`str LCase(str value)`

**示例︰**  
`LCase("TeSt")`  
返回"test"。

----------
### <a name="left"></a>向左

**说明︰**  
Left 的函数返回从字符串的左侧指定的数目的字符。

**语法︰**  
`str Left(str string, num NumChars)`

- 字符串︰ 要返回的字符的字符串
- NumChars︰ 标识的编号的字符数返回从字符串的开头 （左）

**注释︰**  
一个字符串，包含字符串中的第一个 numChars 字符︰

- 如果 numChars = 0，返回空字符串。
- 如果 numChars < 0，返回输入的字符串。
- 如果字符串为 null，则返回空字符串。

如果字符串包含的字符数小于数字中指定 numChars，则返回一个字符串 （即，含有参数 1 中的所有字符） 的字符串相同。

**示例︰**  
`Left("John Doe", 3)`  
返回"Joh"。

----------
### <a name="len"></a>长度

**说明︰**  
Len 函数返回字符串中的字符数。

**语法︰**  
`num Len(str value)`

**示例︰**  
`Len("John Doe")`  
返回 8

----------
### <a name="ltrim"></a>LTrim

**说明︰**  
LTrim 函数从字符串中删除前导空格。

**语法︰**  
`str LTrim(str value)`

**示例︰**  
`LTrim(" Test ")`  
返回"Test"

----------
### <a name="mid"></a>中期

**说明︰**  
Mid 函数从字符串中的指定位置中返回指定的数目的字符。

**语法︰**  
`str Mid(str string, num start, num NumChars)`

- 字符串︰ 要返回的字符的字符串
- 启动︰ 在要返回的字符的字符串中定位标识起始编号
- NumChars︰ 标识的编号的字符数返回从字符串中的位置

**注释︰**  
返回从字符串中的位置开始的 numChars 个字符。  
一个包含从字符串中的位置开始的 numChars 字符的字符串︰

- 如果 numChars = 0，返回空字符串。
- 如果 numChars < 0，返回输入的字符串。
- 如果开始 > 字符串的长度，返回输入的字符串。
- 如果启动 < = 0，返回输入的字符串。
- 如果字符串为 null，则返回空字符串。

如果没有空位置开始、 从字符串的剩余的字符返回尽可能多的字符。

**示例︰**  
`Mid("John Doe", 3, 5)`  
返回"hn 不"。

`Mid("John Doe", 6, 999)`  
返回"Doe"

----------
### <a name="now"></a>现在

**说明︰**  
Now 函数返回当前日期和时间，您的计算机的系统日期和时间的指定日期时间。

**语法︰**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**说明︰**  
NumFromDate 函数返回广告的日期格式的日期。

**语法︰**  
`num NumFromDate(dt value)`

**示例︰**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
返回 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**说明︰**  
PadLeft 函数左游戏板的使用提供的填充字符的指定长度的字符串。

**语法︰**  
`str PadLeft(str string, num length, str padCharacter)`

- 字符串︰ 要填充的字符串。
- 长度︰ 一个整数，表示所需的字符串的长度。
- padCharacter︰ 一个字符将用作填充字符组成的字符串

**注释︰**

- 如果长度小于字符串的长度，则 padCharacter （左） 的字符串直到它具有长度等于长度的开头重复追加。
- PadCharacter 可以是空白字符，但不是能为空值。
- 如果字符串的长度是等于或大于长度，则返回字符串保持不变。
- 如果字符串长度大于或等于长度，则返回一个字符串与字符串完全相同。
- 如果字符串的长度小于长度，按所需长度的新字符串，则返回包含字符串填充 padCharacter。
- 如果字符串为 null，则该函数返回空字符串。

**示例︰**  
`PadLeft("User", 10, "0")`  
返回"000000User"。

----------
### <a name="padright"></a>PadRight

**说明︰**  
PadRight 函数右-垫的使用提供的填充字符的指定长度的字符串。

**语法︰**  
`str PadRight(str string, num length, str padCharacter)`

- 字符串︰ 要填充的字符串。
- 长度︰ 一个整数，表示所需的字符串的长度。
- padCharacter︰ 一个字符将用作填充字符组成的字符串

**注释︰**

- 如果长度小于字符串的长度，则 padCharacter 反复追加到字符串的末尾 （右图） 直到它具有长度相等的长度。
- padCharacter 可以是空白字符，但不是能为空值。
- 如果字符串的长度是等于或大于长度，则返回字符串保持不变。
- 如果字符串长度大于或等于长度，则返回一个字符串与字符串完全相同。
- 如果字符串的长度小于长度，按所需长度的新字符串，则返回包含字符串填充 padCharacter。
- 如果字符串为 null，则该函数返回空字符串。

**示例︰**  
`PadRight("User", 10, "0")`  
返回"User000000"。

----------
### <a name="pcase"></a>PCase

**说明︰**  
PCase 函数将字符串中的每个空格分隔单词的第一个字符转换为大写形式，和其他所有字符都转换为小写。

**语法︰**  
`String PCase(string)`

**注释︰**

- 此功能当前不能提供正确的大小写转换为全部大写首字母缩略词如一个词。

**示例︰**  
`PCase("TEsT")`  
返回"Test"。

`PCase(LCase("TEST"))`  
返回"Test"

----------
### <a name="randomnum"></a>RandomNum

**说明︰**  
RandomNum 函数以指定的间隔间返回一个随机数。

**语法︰**  
`num RandomNum(num start, num end)`

- 启动︰ 数字标识的随机值的下限来生成
- 结束︰ 数字标识的随机值的上限值来生成

**示例︰**  
`Random(100,999)`  
可以返回 734。

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**说明︰**  
RemoveDuplicates 函数采用一个多值的字符串，并确保每个值都是唯一的。

**语法︰**  
`mvstr RemoveDuplicates(mvstr attribute)`

**示例︰**  
`RemoveDuplicates([proxyAddresses])`  
返回一个净化的 proxyAddress 属性位置已删除所有重复值。

----------
### <a name="replace"></a>替换

**说明︰**  
Replace 函数替换为另一个字符串的字符串的所有匹配项。

**语法︰**  
`str Replace(str string, str OldValue, str NewValue)`

- 字符串︰ 要替换中的值的字符串。
- OldValue: 字符串来搜索和替换。
- 其中︰ 要替换的字符串。

**注释︰**  
该函数可以识别下面的特殊名字对象︰

- \n-新行
- \r – 回车
- \t – 选项卡

**示例︰**  
`Replace([address],"\r\n",", ")`  
CRLF 替换一个逗号和一个空格，并可能导致"一个 Microsoft 方式，雷蒙德，华盛顿，美国"

----------
### <a name="replacechars"></a>ReplaceChars

**说明︰**  
ReplaceChars 函数替换 ReplacePattern 字符串中找到的字符的所有匹配项。

**语法︰**  
`str ReplaceChars(str string, str ReplacePattern)`

- 字符串︰ 要替换字符的字符串。
- ReplacePattern︰ 一个字符串，包含要替换的字符与词典。

格式是 {source1}: {target1}，{source2}: {target2}，{sourceN}，{targetN} 源所在的字符查找和替换的字符串的目标。

**注释︰**

- 该函数将定义源的每个匹配项并将它们替换为目标。
- 源必须是一个 (unicode) 字符。
- 源不能为空或长度超过一个字符 （分析错误）。
- 目标可以有多个字符，例如 ö:oe、 β:ss。
- 目标可以为空，指示应移除字符。
- 源是区分大小写，并且必须完全匹配。
- ，（逗号） 和: （冒号） 是保留的字符，不能使用此函数替换。
- 空格和其他白色的 ReplacePattern 字符串中的字符将被忽略。

**示例︰**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
返回 Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
返回"ONeil"，一个刻度定义将被删除。

----------
### <a name="right"></a>向右

**说明︰**  
Right 函数返回从字符串的右边 （结束） 指定的数量的字符。

**语法︰**  
`str Right(str string, num NumChars)`

- 字符串︰ 要返回的字符的字符串
- NumChars︰ 标识的编号的字符数返回从字符串的末尾 （右图）

**注释︰**  
从字符串的最后一个位置则返回 NumChars 字符。

一个字符串，包含字符串中的最后一个 numChars 字符︰

- 如果 numChars = 0，返回空字符串。
- 如果 numChars < 0，返回输入的字符串。
- 如果字符串为 null，则返回空字符串。

如果字符串包含的字符数小于数字中指定 NumChars，则返回的字符串与字符串完全相同。

**示例︰**  
`Right("John Doe", 3)`  
返回"Doe"。

----------
### <a name="rtrim"></a>RTrim

**说明︰**  
使用 RTrim 函数从字符串中移除尾随空格。

**语法︰**  
`str RTrim(str value)`

**示例︰**  
`RTrim(" Test ")`  
返回"Test"。

----------
### <a name="split"></a>拆分

**说明︰**  
Split 函数获取用分隔符分隔的字符串，并将其多值的字符串。

**语法︰**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- 值︰ 定界符以分隔的字符串。
- 分隔符︰ 单要用作分隔符的字符。
- 限制︰ 可以返回的值的最大数目。

**示例︰**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
返回带有 2 个元素的多值的字符串 proxyAddress 属性很有用。

----------
### <a name="stringfromguid"></a>StringFromGuid

**说明︰**  
StringFromGuid 函数采用二进制的 GUID，并将其转换为一个字符串

**语法︰**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**说明︰**  
StringFromSid 函数将一个字节数组，包含安全标识符的字符串。

**语法︰**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>开关

**说明︰**  
Switch 函数用于返回单个值，根据评估情况。

**语法︰**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- expr︰ 要计算的变量表达式。
- 值︰ 相应的表达式为 True 时要返回的值。

**注释︰**  
开关函数的参数列表由表达式和值对组成。 从左到右，计算表达式，并返回结果为 True 的第一个表达式相关联的值。 如果组成部分没有正确配对，则会发生运行时错误。

例如，如果表达式 1 为 True，则开关返回值 1。 如果 expr 1 是假，但 expr 2 是真，交换机返回值 2，等等。

交换机返回 Nothing 如果︰

- 表达式都真。
- 第一个真正的表达式都有相应的值为 Null。

开关计算所有表达式，即使它只返回一个它们。 因此，应该注意的令人不快的副作用。 例如，如果任何表达式的计算结果除零错，就会出错。

值也可以是错误函数，将返回一个自定义字符串。

**示例︰**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
返回在一些大城市中所说的语言，否则会返回一个错误。

----------
### <a name="trim"></a>修剪

**说明︰**  
Trim 函数删除前导空格和尾随空格的字符串。

**语法︰**  
`str Trim(str value)`  

**示例︰**  
`Trim(" Test ")`  
返回"Test"。

`Trim([proxyAddresses])`  
移除前导空格和尾随空格在 proxyAddress 属性中的每个值。

----------
### <a name="ucase"></a>UCase

**说明︰**  
UCase 函数将字符串中的所有字符都转换为大写形式。

**语法︰**  
`str UCase(str string)`

**示例︰**  
`UCase("TeSt")`  
返回"TEST"。

----------
### <a name="word"></a>单词

**说明︰**  
Word 函数返回基于参数描述的分隔符使用与单词数，返回的字符串中包含的单词。

**语法︰**  
`str Word(str string, num WordNumber, str delimiters)`

- 字符串︰ 要返回从单词的字符串。
- WordNumber︰ 应返回标识哪些单词数的数字。
- 分隔符︰ 一个字符串，表示用于标识词 delimiter(s)

**注释︰**  
每个字符串之一中的分隔符字符分隔的字符串中的字符被视为单词︰

- 如果 < 1 号，返回空字符串。
- 如果字符串为 null，则返回空字符串。

如果字符串包含小于号词，或字符串中不包含由分隔符标识任何单词，则返回一个空字符串。

**示例︰**  
`Word("The quick brown fox",3," ")`  
返回"棕色"

`Word("This,string!has&many separators",3,",!&#")`  
将返回"有"

## <a name="additional-resources"></a>其他资源

* [了解资源调配的声明性表达式](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD 连接的同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
