<properties
    pageTitle="Azure AD 连接同步︰ 到 Azure Active Directory 同步属性 |Microsoft Azure"
    description="列出对 Azure Active Directory 同步的属性。"
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
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD 连接同步︰ 到 Azure Active Directory 同步属性
本主题列出了通过 Azure AD 连接同步所同步的特性。  
属性分组相关 Azure 的 AD 的应用程序。

## <a name="attributes-to-synchronize"></a>要同步的属性
一个常见的问题是*什么是最少的属性要同步的列表*。 默认的和建议的做法是保留默认的属性，以便可以在云中构建完整的全球通讯簿 （全局地址列表） 并获取 Office 365 的工作负荷中的所有功能。 在某些情况下，有一些属性，您的组织不希望同步到云因为这些属性包含敏感或 PII （个人身份信息） 的数据，在此示例如下所示︰  
![不正确的特性](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

在这种情况下，启动与本主题中的属性列表并识别那些可能包含敏感或 PII 数据并不能同步的属性。 安装使用[AD Azure 应用程序和属性筛选](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)过程，然后取消选择这些属性。

>[AZURE.WARNING] 当取消选择属性，应该谨慎并只取消选择绝对不能同步这些属性。 取消选择其他属性上的功能可能有负面影响。

## <a name="office-365-proplus"></a>Office 365 ProPlus

| 属性名称| 用户| 注释 |
| --- | :-: | --- |
| accountEnabled| X| 如果启用了帐户，定义。|
| cn| X|  |
| 显示名称| X|  |
| objectSID| X| 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| pwdLastSet| X| 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。 使用密码同步和联盟。|
| sourceAnchor| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| usageLocation| X| 机械的属性。 用户的国家/地区。 使用许可分配。|
| 范围内| X| UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|

## <a name="exchange-online"></a>联机交换

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | 如果启用了帐户，定义。|
| 助手| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| cn| X|  | X|  |
| co| X| X|  |  |
| 公司| X| X|  |  |
| 国家/地区代码| X| X|  |  |
| 部门| X| X|  |  |
| 说明| X| X| X|  |
| 显示名称| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| 住宅电话| X| X|  |  |
| 信息| X| X| X| 此属性当前未使用组。|
| 首字母缩写| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| 管理器| X| X|  |  |
| 成员|  |  | X|  |
| 移动| X| X|  |  |
| msDS HABSeniorityIndex| X| X| X|  |
| msDS PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| 此属性当前未使用由 Exchange 联机。 |
| msExchExtensionCustomAttribute2| X| X| X| 此属性当前未使用由 Exchange 联机。 |
| msExchExtensionCustomAttribute3| X| X| X| 此属性当前未使用由 Exchange 联机。 |
| msExchExtensionCustomAttribute4| X| X| X| 此属性当前未使用由 Exchange 联机。 |
| msExchExtensionCustomAttribute5| X| X| X| 此属性当前未使用由 Exchange 联机。 |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| 页导航| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| 邮政编码| X| X|  |  |
| 代理地址| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。 使用密码同步和联盟。|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| 从 groupType 派生|
| sn| X| X|  |  |
| sourceAnchor| X| X| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| st| X| X|  |  |
| 街道地址| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| 标题| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | 机械的属性。 用户的国家/地区。 使用许可分配。|
| 用户证书| X| X|  |  |
| 范围内| X|  |  | UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint 在线

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | 如果启用了帐户，定义。|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| cn| X|  | X|  |
| co| X| X|  |  |
| 公司| X| X|  |  |
| 国家/地区代码| X| X|  |  |
| 部门| X| X|  |  |
| 说明| X| X| X|  |
| 显示名称| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| 住宅电话| X| X|  |  |
| 信息| X| X| X|  |
| 首字母缩写| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| 邮件| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| 管理器| X| X|  |  |
| 成员|  |  | X|  |
| 称谓| X| X|  |  |
| 移动| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| 页导航| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| 邮政编码| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| 代理地址| X| X| X|  |
| pwdLastSet| X|  |  | 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。 使用密码同步和联盟。|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| 从 groupType 派生|
| sn| X| X|  |  |
| sourceAnchor| X| X| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| st| X| X|  |  |
| 街道地址| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| 标题| X| X|  |  |
| unauthOrig| X| X| X|  |
| url| X| X|  |  |
| usageLocation| X|  |  | 机械的属性。 用户的国家/地区。 使用许可分配。|
| 范围内| X|  |  | UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync 联机

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | 如果启用了帐户，定义。|
| c| X| X|  |  |
| cn| X|  | X|  |
| co| X| X|  |  |
| 公司| X| X|  |  |
| 部门| X| X|  |  |
| 说明| X| X| X|  |
| 显示名称| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| 住宅电话| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| 邮件| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| 管理器| X| X|  |  |
| 成员|  |  | X|  |
| 移动| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP ApplicationOptions| X|  |  |  |
| msRTCSIP DeploymentLocator| X| X|  |  |
| msRTCSIP 行| X| X|  |  |
| msRTCSIP OptionFlags| X| X|  |  |
| msRTCSIP OwnerUrn| X|  |  |  |
| msRTCSIP PrimaryUserAddress| X| X|  |  |
| msRTCSIP UserEnabled| X| X|  |  |
| objectSID| X|  | X| 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| 邮政编码| X| X|  |  |
| preferredLanguage| X|  |  |  |
| 代理地址| X| X| X|  |
| pwdLastSet| X|  |  | 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。 使用密码同步和联盟。|
| securityEnabled|  |  | X| 从 groupType 派生|
| sn| X| X|  |  |
| sourceAnchor| X| X| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| st| X| X|  |  |
| 街道地址| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| 标题| X| X|  |  |
| usageLocation| X|  |  | 机械的属性。 用户的国家/地区。 使用许可分配。|
| 范围内| X|  |  | UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | 如果启用了帐户，定义。|
| cn| X|  | X| 常见的名称或别名。 最常用的 [邮件] 值前缀。|
| 显示名称| X| X| X| 一个字符串，表示通常显示为好记的名称 （姓） 的名称。|
| 邮件| X| X| X| 完整的电子邮件地址。|
| 成员|  |  | X|  |
| objectSID| X|  | X| 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| 代理地址| X| X| X| 机械的属性。 使用 Azure 的广告。 包含该用户的所有辅助的电子邮件地址。|
| pwdLastSet| X|  |  | 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。|
| securityEnabled|  |  | X| 从 groupType 派生。|
| sourceAnchor| X| X| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| usageLocation| X|  |  | 机械的属性。 用户的国家/地区。 使用许可分配。|
| 范围内| X|  |  | 此 UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|

## <a name="intune"></a>Intune

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | 如果启用了帐户，定义。|
| c| X| X|  |  |
| cn| X|  | X|  |
| 说明| X| X| X|  |
| 显示名称| X| X| X|  |
| 邮件| X| X| X|  |
| mailnickname| X| X| X|  |
| 成员|  |  | X|  |
| objectSID| X|  | X| 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| 代理地址| X| X| X|  |
| pwdLastSet| X|  |  | 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。 使用密码同步和联盟。|
| securityEnabled|  |  | X| 从 groupType 派生|
| sourceAnchor| X| X| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| usageLocation| X|  |  | 机械的属性。 用户的国家/地区。 使用许可分配。|
| 范围内| X|  |  | UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|

## <a name="dynamics-crm"></a>Dynamics CRM

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | 如果启用了帐户，定义。|
| c| X| X|  |  |
| cn| X|  | X|  |
| co| X| X|  |  |
| 公司| X| X|  |  |
| 国家/地区代码| X| X|  |  |
| 说明| X| X| X|  |
| 显示名称| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| 管理器| X| X|  |  |
| 成员|  |  | X|  |
| 移动| X| X|  |  |
| objectSID| X|  | X| 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| physicalDeliveryOfficeName| X| X|  |  |
| 邮政编码| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。 使用密码同步和联盟。|
| securityEnabled|  |  | X| 从 groupType 派生|
| sn| X| X|  |  |
| sourceAnchor| X| X| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| st| X| X|  |  |
| 街道地址| X| X|  |  |
| telephoneNumber| X| X|  |  |
| 标题| X| X|  |  |
| usageLocation| X|  |  | 机械的属性。 用户的国家/地区。 使用许可分配。|
| 范围内| X|  |  | UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|

## <a name="3rd-party-applications"></a>第三方应用程序
这组为一套用作一般工作负荷或应用程序所需的最小属性的特性。 对于未在另一节中列出的工作负荷或非 Microsoft 应用程序可以使用它。 其显式使用以下方法︰

- Yammer （只有用户使用）
- [提供的资源，如 SharePoint 的混合企业到企业 (B2B) 跨组织协作方案](http://go.microsoft.com/fwlink/?LinkId=747036)

此组是一组如果不用于支持 Office 365、 动态或 Intune Azure 的广告目录，可以使用的属性。 有少数的核心特性。

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | 如果启用了帐户，定义。|
| cn| X|  | X|  |
| 显示名称| X| X| X|  |
| givenName| X| X|  |  |
| 邮件| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| 成员|  |  | X|  |
| objectSID| X|  |  | 机械的属性。 AD 用户标识符用于维护同步之间 Azure 广告和广告。|
| 代理地址| X| X| X|  |
| pwdLastSet| X|  |  | 机械的属性。 知道何时要使之无效的已颁发的令牌的使用。 使用密码同步和联盟。|
| sn| X| X|  |  |
| sourceAnchor| X| X| X| 机械的属性。 要维护添加和 Azure 的广告之间的关系的不可变的标识符。|
| usageLocation| X|  |  | 机械的属性。 用户的国家/地区。 使用许可分配。|
| 范围内| X|  |  | UPN 是用户的登录 ID。 通常作为 [电子邮件] 相同的值。|

## <a name="windows-10"></a>Windows 10
加入域的 Windows 10 computer(device) 同步到 Azure AD 的某些属性。 有关方案的详细信息，请参阅[连接到 Windows 10 的 Azure AD 的加入域的设备体验](active-directory-azureadjoin-devices-group-policy.md)。 这些属性始终同步，Windows 10 不显示为一个应用程序，您可以取消选择。 通过让用户填充的属性证书标识 Windows 10 加入域的计算机。

| 属性名称| 设备| 注释 |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| 加入域的计算机的硬编码值。 |
| 显示名称 | X| |
| ms-DS-CreatorSID | X| 也称为 registeredOwnerReference。|
| objectGUID | X| 也称为 deviceID。|
| objectSID | X| 也称为 onPremisesSecurityIdentifier。|
| 操作系统 | X| 也称为 deviceOSType。|
| operatingSystemVersion | X| 也称为 deviceOSVersion。|
| 用户证书 | X| |

这些**用户**的属性是您选择的其他应用程序的补充。  

| 属性名称| 用户| 注释 |
| --- | :-: | --- |
| domainFQDN| X| 也称为 dnsDomainName。 如 contoso.com。|
| domainNetBios| X| 也称为 netBiosName。 例如，CONTOSO。|

## <a name="exchange-hybrid-writeback"></a>交换混合写回
这些属性被写回从 Azure AD 内部部署 Active Directory 时您选择启用**Exchange 混合**。 根据您的 Exchange 版本，可能同步特性较少。

| 属性名称| 用户| 联系人| 组| 注释 |
| --- | :-: | :-: | :-: | --- |
| msDS ExternalDirectoryObjectID| X|  |  | 从 cloudAnchor 派生在 Azure 的广告。 此属性是新在 Exchange 2016。|
| msExchArchiveStatus| X|  |  | 在线存档︰ 使客户能够将邮件存档。|
| msExchBlockedSendersHash| X|  |  | 过滤︰ 将写回到内部筛选和在线安全和阻止发件人数据从客户端程序。|
| msExchSafeRecipientsHash| X|  |  | 过滤︰ 将写回到内部筛选和在线安全和阻止发件人数据从客户端程序。|
| msExchSafeSendersHash| X|  |  | 过滤︰ 将写回到内部筛选和在线安全和阻止发件人数据从客户端程序。|
| msExchUCVoiceMailSettings| X|  |  | 启用统一消息 (UM)-在线语音邮件︰ 由 Microsoft Lync Server 集成到 Lync 服务器指示内部用户的在线服务有语音邮件。|
| msExchUserHoldPolicies| X|  |  | 诉讼封存︰ 诉讼封存正在使云服务来确定哪些用户。|
| 代理地址| X| X| X| 仅从 Exchange 联机地址插入 x500。|

## <a name="device-writeback"></a>设备写回
在 Active Directory 中创建设备对象。 这些对象可以是设备加入到 Azure 广告或加入域的 Windows 10 计算机。

| 属性名称| 设备| 注释 |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| 显示名称 | X| |
| dn | X| |
| msDS CloudAnchor | X| |
| msDS DeviceID | X| |
| msDS DeviceObjectVersion | X| |
| msDS DeviceOSType | X| |
| msDS DeviceOSVersion | X| |
| msDS DevicePhysicalIDs | X| |
| msDS KeyCredentialLink | X| 仅与 Windows 服务器 2016 AD 架构 |
| msDS IsCompliant | X| |
| msDS 启用 | X| |
| msDS IsManaged | X| |
| msDS RegisteredOwner | X| |


## <a name="notes"></a>备注

- 当使用一个备用的 ID，Azure AD 属性 onPremisesUserPrincipalName 同步内部属性范围。 同步示例邮件的备选 ID 属性，Azure AD 属性范围。
- 在列表中的上面，对象类型**用户**也适用于对象类型**iNetOrgPerson**。

## <a name="next-steps"></a>下一步行动
了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
