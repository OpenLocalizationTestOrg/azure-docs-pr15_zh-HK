<properties
    pageTitle="Azure AD 连接︰ 在同步过程中解决错误 |Microsoft Azure"
    description="介绍如何解决与 Azure AD 连接的同步过程中遇到错误。"
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>在同步过程中解决错误
从 Windows 服务器活动目录 (AD DS) 到 Azure 活动目录 (AD Azure) 同步标识数据时，可能会发生错误。 本文概述了不同类型的同步错误，一些可能导致这些错误和潜在的方法来修复错误的方案。 这篇文章包含常见的错误类型，并可能无法涵盖所有可能的错误。

 本文假定读者熟悉基础[设计概念 Azure 的 AD 和 Azure AD 连接](active-directory-aadconnect-design-concepts.md)。

与最新版本的 Azure AD 连接\(8 月 2016年或更高版本\)，同步错误的报告是[Azure 门户](https://aka.ms/aadconnecthealth)中可用的 Azure AD 连接健康同步的一部分。


从 9 月 1，2016 [Azure 活动目录复制属性恢复](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)功能将启用默认情况下，所有*新*Azure 活动目录承租人。 此功能将自动启用现有租户在接下来的月份。

Azure AD 连接将保持同步目录中执行 3 种类型的操作︰ 导入、 同步和导出。 错误可以发生在所有的操作。 这篇文章主要着重在导出到 Azure 的广告错误。

## <a name="errors-during-export-to-azure-ad"></a>导出到 Azure 广告期间发生的错误
以下各节介绍了不同类型的 Azure 广告使用 Azure AD 连接器上的导出操作期间可能会发生同步错误。 此连接器可以识别的名称格式正在"contoso。*onmicrosoft.com*"。
导出到 Azure 广告期间发生的错误，指示该操作\(添加、 更新、 删除等\)Azure AD 连接尝试\(同步引擎\)Azure 活动目录失败。

![导出错误概述](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>数据不匹配错误
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>说明
- 当 Azure AD 连接\(同步引擎\)指示 Azure Active Directory 来添加或更新对象，Azure 的广告匹配传入 Azure AD 中使用**sourceAnchor**属性设置为**immutableId**属性的对象的对象。 此匹配被称为**硬匹配**。
- 当 Azure 广告**找不到**任何与**immutableId**相匹配的对象特性与传入对象，此对象的**sourceAnchor**属性设置一个新的对象中前, 它将回来可用于查找匹配的代理地址和范围内的属性。 此匹配被称为**软匹配**。 软匹配用于匹配表示部署在同一个实体 （用户、 组） 的新对象是在同步期间添加/更新 Azure 的广告 （即在 Azure AD 来源） 中已有的对象。
- **InvalidSoftMatch**错误发生时硬匹配不到任何匹配的对象**和**软匹配查找匹配的对象，但该对象具有不同的值的*immutableId*比传入对象的*SourceAnchor*，建议匹配对象的同步与内部部署 Active Directory 中的另一个对象。

换句话说，为了使软匹配工作，要与软匹配的对象不应具有任何值为*immutableId*。 如果*immutableId*有任何对象与设置值是无法硬匹配，但满足软匹配条件中，该操作会导致 InvalidSoftMatch 同步错误。

Azure Active Directory 架构不允许两个或多个对象具有下列属性的相同值。 \(这不是一个详尽的列表。\)

- 代理地址
- 范围内
- onPremisesSecurityIdentifier
- 对象 Id

>[AZURE.NOTE] [Azure AD 属性重复属性恢复](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)功能是也被作为默认行为的 Azure Active Directory 推出。  这将通过使 Azure 的广告更具弹性的方式处理重复的代理地址和范围内属性中部署 AD 环境存在的减少 Azure AD 连接 （以及其他同步客户端） 可以看到同步错误的数。 此功能不能解决重复错误。 因此数据仍然需要解决。 但它允许资源调配的新对象的否则阻止 Azure AD 中置备由于重复值。 它还可以减少同步错误返回到同步客户端数。
如果为您的组织启用了此功能，则不看新对象的资源调配过程中，发生 InvalidSoftMatch 同步错误。


#### <a name="example-scenarios-for-invalidsoftmatch"></a>InvalidSoftMatch 的示例方案
1. 在 Active Directory 中存在两个或多个对象具有同一个代理地址属性的值。 获取在 Azure AD 配置只有一个。
2. 在 Active Directory 中存在两个或多个对象具有相同的值的范围。 获取在 Azure AD 配置只有一个。
3. 添加了在内部部署中代理地址属性的值相同的 Active Directory 的 Azure Active Directory 中的现有对象。 在场所添加的对象没有获取设置 Azure Active Directory 中。
4. 对象已添加在场所具有相同值的范围内特性的活动目录的 Azure Active Directory 中的帐户。 该对象未获取设置 Azure Active Directory 中。
5. 同步的帐户被移动从林 A 林 B.Azure AD 连接 （同步引擎） 使用 ObjectGUID 属性来计算 SourceAnchor。 目录林移动后，SourceAnchor 的值是不同的。 新对象 （来自林 B) 未能在 Azure AD 同步与现有对象。
6. 同步的对象意外地删除了从在活动目录和一个新的对象创建在 Active Directory 中为同一个实体 （例如用户） 而不删除此帐户在 Azure Active Directory 中的场所。 新帐户与现有的 Azure 的 AD 对象同步失败。
7. Azure AD 连接已卸载并重新安装。 在重新安装时，SourceAnchor 作为选择了不同的特性。 以前有同步的所有对象都停止同步出现 InvalidSoftMatch 错误。

#### <a name="example-case"></a>本文的示例︰
1. **Bob 史密斯**是 contoso.com 的 Azure 从部署 Active Directory**上的 Active Directory 中的同步的用户
2. Bob Smith**范围内**设为**bobs@contoso.com**。
3. **"abcdefghijklmnopqrstuv = ="**是**SourceAnchor**计算通过使用 Bob Smith **objectGUID**开的 Azure AD 连接部署 Active Directory，这是**immutableId**为 Bob Smith 在 Azure Active Directory 中。
4. 小明还具有以下**代理地址**属性的值︰
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  一个新的用户，**王俊元泰勒**，加入上部署 Active Directory。
6. Bob 泰勒**范围内**设为**bobt@contoso.com**。
7. **"abcdefghijkl0123456789 = =""**是**sourceAnchor**计算通过使用 Bob 泰勒**objectGUID**开的 Azure AD 连接部署 Active Directory。 Bob 泰勒对象尚未尚未到 Azure Active Directory 同步。
8. Bob 泰勒有以下代理地址属性的值
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. 在同步期间，Azure AD 连接将识别 Bob 泰勒在内部部署 Active Directory 的加法，并询问 Azure 的广告，来进行相同的更改。
10. Azure 的广告将首先执行硬匹配。 也就是说，它将搜索如果没有任何对象与 immutableId 等于"abcdefghijkl0123456789 = ="。 随着 Azure AD 中的没有其他对象将具有该 immutableId，硬匹配将失败。
11. Azure 广告然后将尝试以软匹配 Bob 泰勒。 也就是说，它将搜索如果具有代理地址的任何对象相等于三个值，包括smtp:bob@contoso.com
12. Azure 的广告会发现 Bob Smith 对象以匹配软匹配条件。 但此对象包含的值的 immutableId ="abcdefghijklmnopqrstuv = ="。 指示此对象是从另一个对象的内部部署 Active Directory 同步。 因此，Azure 的广告不能软-匹配这些对象，会导致**InvalidSoftMatch**同步错误。

#### <a name="how-to-fix-invalidsoftmatch-error"></a>如何修复 InvalidSoftMatch 错误
InvalidSoftMatch 错误的最常见原因是两个具有不同 SourceAnchor \(immutableId\)具有代理地址和/或范围内的属性，它们用于在 Azure 广告软匹配过程中使用的值相同。 为解决无效的软匹配

1.  标识复制代理地址、 范围内或其他导致错误的属性值。 此外将确定哪些两个\(或多个\)冲突中涉及的对象。 由[Azure AD 连接的运行状况同步](https://aka.ms/aadchsyncerrors)生成的报告可帮助您确定两个对象。
2. 确定哪个对象应继续具有重复的值和对象不应该。
3. 不应具有该值的对象删除重复的值。 注意您应对象来自的目录中进行更改。 在某些情况下，您可能需要删除冲突的对象之一。
4. 如果您在部署 AD 中作了更改，让 Azure AD 连接同步更改。

请注意在 Azure AD 连接健康同步的同步错误报告每隔 30 分钟更新一次，并包括最新的同步尝试中的错误。

>[AZURE.NOTE] ImmutableId，根据定义，不应更改该对象的生存期中。 如果没有配置 Azure AD 连接一些构思从上面的列表中的方案，最终可能会在 Azure AD 连接位置计算有不同的 AD 对象表示相同的 SourceAnchor 值的情况下具有您希望继续使用现有的 Azure AD 对象的实体 （同一用户/组/联系人等）。

#### <a name="related-articles"></a>相关的文章
- [重复或无效的属性防止 Office 365 中的目录同步](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>说明
如果 Azure 广告尝试柔和匹配两个对象，则可能，两个不同的"对象类型"（如用户、 组、 联系人等） 具有用于执行软匹配的属性相同的值。 Azure AD 中不允许重复的这些特性，该操作可能会导致在"ObjectTypeMismatch"同步错误。

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>ObjectTypeMismatch 错误的示例方案
- Office 365 中创建启用邮件安全组。 管理上部署 AD （即不到 Azure AD 尚未同步） 与代理地址属性的值相同的 Office 365 组添加新的用户或联系人。

#### <a name="example-case"></a>本文的示例

1. 管理税务部门在 Office 365 中创建新的安全组启用邮件服务和提供电子邮件地址为tax@contoso.com。 这将指定此组具有的值的代理地址属性**smtp:tax@contoso.com**
2. 新用户加入 Contoso.com 并为内部部署为 proxyAddress 的用户创建帐户**smtp:tax@contoso.com**
3. Azure AD 连接将同步新的用户帐户，它会在"ObjectTypeMismatch"错误。

#### <a name="how-to-fix-objecttypemismatch-error"></a>如何修复 ObjectTypeMismatch 错误
ObjectTypeMismatch 错误的最常见原因是两个对象 （用户、 组、 联系人等） 的不同类型的代理地址属性的值相同。 为解决 ObjectTypeMismatch:

1.  标识复制代理地址 （或其他属性） 导致错误的值。 此外将确定哪些两个\(或多个\)冲突中涉及的对象。 由[Azure AD 连接的运行状况同步](https://aka.ms/aadchsyncerrors)生成的报告可帮助您确定两个对象。
2. 确定哪个对象应继续具有重复的值和对象不应该。
3. 不应具有该值的对象删除重复的值。 注意您应对象来自的目录中进行更改。 在某些情况下，您可能需要删除冲突的对象之一。
4. 如果您在部署 AD 中作了更改，让 Azure AD 连接同步更改。 在 Azure AD 连接健康同步的同步错误报告获取更新每隔 30 分钟，包括最新的同步尝试中的错误。


## <a name="duplicate-attributes"></a>重复属性
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>说明
Azure Active Directory 架构不允许两个或多个对象具有下列属性的相同值。 这就是 Azure 广告中的每个对象都不得不在给定实例具有这些属性中的唯一值。

- 代理地址
- 范围内

如果 Azure AD 连接尝试将新对象添加或更新现有对象具有以上特性的已分配给另一个 Azure Active Directory 中的对象的值时，该操作将导致"AttributeValueMustBeUnique"同步错误。
#### <a name="possible-scenarios"></a>可能的方案︰
1. 重复的值分配给已同步对象与另一个同步对象的冲突。

#### <a name="example-case"></a>本文的示例︰
1. **Bob 史密斯**是 contoso.com 的 Azure 从部署 Active Directory 上的 Active Directory 中的同步的用户
2. 内部部署的 Bob Smith**范围内**设为**bobs@contoso.com**。
3. 小明还具有以下**代理地址**属性的值︰
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. 一个新的用户，**王俊元泰勒**，加入上部署 Active Directory。
5. Bob 泰勒**范围内**设为**bobt@contoso.com**。
6. **Bob 泰勒**有以下**代理地址**属性 i 的值。 smtp:bobt@contoso.com二。 smtp:bob.taylor@contoso.com
7. Bob 泰勒对象已成功同步使用 Azure 的广告。
8. 管理员决定使用下面的值更新 Bob 泰勒**代理地址**属性︰ 我。 **smtp:bob@contoso.com**
9. Azure 的广告会尝试更新 Bob 泰勒对象在 Azure 广告上面的值，但该操作将失败，代理地址值已分配给 Bob Smith，从而导致"AttributeValueMustBeUnique"错误。

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>如何修复 AttributeValueMustBeUnique 错误
AttributeValueMustBeUnique 错误的最常见原因是两个具有不同 SourceAnchor \(immutableId\)具有相同值的代理地址和/或范围内的属性。 为解决 AttributeValueMustBeUnique 错误

1.  标识复制代理地址、 范围内或其他导致错误的属性值。 此外将确定哪些两个\(或多个\)冲突中涉及的对象。 由[Azure AD 连接的运行状况同步](https://aka.ms/aadchsyncerrors)生成的报告可帮助您确定两个对象。
2. 确定哪个对象应继续具有重复的值和对象不应该。
3. 不应具有该值的对象删除重复的值。 注意您应对象来自的目录中进行更改。 在某些情况下，您可能需要删除冲突的对象之一。
4. 如果您在部署 AD 中进行更改，使 Azure AD 连接，同步错误以获得固定的更改。

#### <a name="related-articles"></a>相关的文章
-[重复或无效的属性防止 Office 365 中的目录同步](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>数据验证失败
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>说明
Azure 的 Active Directory 实施允许数据将写入该目录之前对数据本身的各种限制。 这是为了确保最终用户获得最佳使用的应用程序依赖于这些数据时体验。
#### <a name="scenarios"></a>方案
一。 范围内的属性值包含无效/不受支持的字符。
b。 该范围内的属性不符合所需的格式。
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>如何修复 IdentityDataValidationFailed 错误

一。 确保范围内特性具有支持字符和所需的格式。

#### <a name="related-articles"></a>相关的文章
- [准备供应通过目录同步到 Office 365 的用户]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>说明
这是在非常具体的情况下，用户范围内的后缀从一个联盟域更改为另一个联盟的域时将导致**"DataValidationFailed"**同步错误。

#### <a name="scenarios"></a>方案
同步用户，范围内后缀是从一个联盟域更改内部部署另一个联盟的域。 例如，*范围内 =bob@contoso.com*已更改为*范围内 = bob@fabrikam.com *。

#### <a name="example"></a>示例
1. Bob Smith Contoso.com，帐户被添加为范围内与 Active Directory 中的新用户bob@contoso.com
2. Bob 移到名为 Fabrikam.com 的 Contoso.com 不同部门和他范围内更改为bob@fabrikam.com
3. Contoso.com 和 fabrikam.com 域是联盟与 Azure Active Directory 域。
4. 王俊元的范围内得不到更新，并产生"DataValidationFailed"同步错误。

#### <a name="how-to-fix"></a>如何解决
如果从已更新的用户范围内后缀bob@ **contoso.com**到bob@ **fabrikam.com**，其中**contoso.com**和**fabrikam.com**都是**联盟的域**，然后按照以下步骤来解决同步时出错

1. 更新用户的范围内，在从 Azure ADbob@contoso.com到bob@contoso.onmicrosoft.com。 用 Azure AD PowerShell 模块，可以使用下面的 PowerShell 命令︰`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. 允许在下一同步周期尝试同步。 这种时间同步将会成功，它将更新 Bob 给范围内的bob@fabrikam.com像预期的那样。


## <a name="largeobject"></a>大对象
### <a name="description"></a>说明
当属性超过允许的大小限制、 长度限制或设置 Azure Active Directory 架构计数限制时，同步操作将导致**大对象**或**ExceededAllowedLength**同步错误。 该错误通常发生下列属性

- 用户证书
- thumbnailPhoto
- 代理地址

### <a name="possible-scenarios"></a>可能的情况
1. Bob 的用户证书属性来存储分配给 Bob 的证书太多。 它们中可能包含较旧的、 过期的证书。
2. 王俊元的 thmubnailPhoto 设置在 Active Directory 中是太大，无法同步在 Azure 的广告。
3. 在 Active Directory 中的代理地址属性自动填充，期间获得分配对象 > 500 的代理地址。

### <a name="how-to-fix"></a>如何解决

1. 确保允许限制中引起错误的属性。

## <a name="related-links"></a>相关的链接
- [在 Active Directory 管理中心查找 Active Directory 对象](https://technet.microsoft.com/library/dd560661.aspx)
- [如何查询 Azure Active Directory 对象使用 Azure 活动目录 PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
