<properties
   pageTitle="事件 azure Active Directory 的审计报告 |Microsoft Azure"
   description="审核事件可用于查看和下载从 Azure 活动目录"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory 审核报告事件

*本文档是 [Azure 活动目录报告指南] (活动-目录的报告-guide.md) 的一部分。*

Azure 活动目录审核报告可帮助客户确定他们 Azure Active Directory 中出现的特权的操作。 特权的操作包括提升或做的更改 （例如，角色创建密码重置），更改策略配置 （例如密码策略） 或更改目录配置 （例如，更改域联盟设置）。 报告提供了事件名称，参与者执行操作，受更改的日期和时间 （UTC) 的目标资源的审核记录。 [查看审核日志](active-directory-reporting-azure-portal.md)中所述，客户将能够通过[Azure 门户](https://portal.azure.com/)，其 Azure 活动目录检索审核事件的列表。


## <a name="list-of-audit-report-events"></a>审核报告事件列表
<!--- audit event descriptions should be in the past tense --->

事件                               | 事件描述
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**用户事件**                      |
添加用户                             | 将某个用户添加到目录中。
删除用户                          | 从目录中删除用户。
设置许可证属性               | 在目录中的用户设置许可属性。
重置用户密码                  | 重置目录中的用户的密码。
更改用户密码                 | 更改该目录中的用户的密码。
更改用户许可证                  | 更改分配给该目录中的用户许可证。 若要查看哪些许可证已更新，请看下面的[更新用户](#update-user-attributes)属性
更新用户                          | 更新用户目录中。 [请参阅下面的](#update-user-attributes)属性，可更新。
设置强制更改用户密码       | 设置强制用户更改其密码登录上的属性。
更新用户凭据        | 用户更改密码  
**组事件**                     |
添加组                            | 在目录中创建一个组。
更新组                         | 更新目录中的组。 要查看组属性已更新，请参阅下面的部分中的[组属性审核](#update-group-attributes)
删除组                         | 从目录中删除一个组。
将成员添加到组            | 添加到目录中的组的成员。
从组中删除成员         | 从目录中的组中删除成员。
CreateGroupSettings              | 创建的组设置
UpdateGroupSettings          | 更新组设置。 若要查看已更新哪些组设置，请参阅下面的部分中的[组属性审核](#update-group-attributes)
DeleteGroupSettings            | 已删除的组设置
SetGroupLicense                  | 设置组许可证。
SetGroupManagedBy              | 设置要管理的用户组
AddGroupMember               | 向组添加的成员
RemoveGroupMember            | 从组中删除成员
AddGroupOwner                | 添加到组的所有者
RemoveGroupOwner                 | 从组中删除的所有者
**应用程序事件**               |
添加服务主体                | 添加到目录服务主体。
删除服务主体             | 从目录中删除服务主体。
添加服务主体凭据    | 为服务主体的添加的凭据。
删除服务主体凭据 | 从主体服务的已删除的凭据。
添加委派条目                 | 在目录中创建的[OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) 。
设置委派条目                 | 更新目录中的[OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) 。
删除委派条目              | 删除目录中的[OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) 。
**角色的事件**                      |
添加到角色的角色成员              | 将某个用户添加到目录角色。
从角色中删除角色成员         | 从目录角色中删除用户。
设置公司联系人信息      | 设置联系人公司级别的首选项。 这包括有关 Microsoft Online Services 的市场营销，以及技术通知的电子邮件地址。
添加委派条目                 | 在目录中创建的[OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) 。
设置委派条目                 | 更新目录中的[OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) 。
删除委派条目              | 删除目录中的[OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) 。
AddSevicePrincipalOwner          | 添加的所有者更改为服务主体。
RemoveSevicePrincipalOwner   | 从服务主体的所有者。
AddApplication  | 添加应用程序。
UpdateApplication | 更新应用程序。 若要查看已更新的应用程序设置，请参阅下面的部分中的[应用程序属性审核](#update-application-attributes)
DeleteApplication | 删除应用程序。
RestoreApplication|还原应用程序。
AddApplicationOwner   |     添加到应用程序所有者。
RemoveApplicationOwner    |     从应用程序中删除所有者。
**角色的事件**                         |
添加到角色的角色成员                 | 将某个用户添加到目录角色。
从角色中删除角色成员            | 从目录角色中删除用户。
AddRoleDefinition               | 添加的角色定义。
UpdateRoleDefinition                | 更新角色定义。 若要查看哪些角色设置已更新，请参阅下面的部分中的[角色定义属性审核](#update-role-definition-attributes)
DeleteRoleDefinition                | 已删除的角色定义。
AddRoleAssignmentToRoleDefinition | 向角色定义添加的角色分配。
RemoveRoleAssignmentFromRoleDefinition | 角色定义中移除的角色分配。
AddRoleFromTemplate     | 从模板添加的角色。
UpdateRole  | 已更新的角色。
AddRoleScopeMemberToRole    | 向角色添加作用域的成员。
RemoveRoleScopedMemberFromRole  | 从角色中删除指定了作用域的成员。
**设备事件 （所有的新事件）**                    |
AddDevice               | 添加的设备。
UpdateDevice            | 最新的设备。 要查看设备属性已更新，请参阅下面的部分中的[设备属性 Audited](#update-device-attributes)
DeleteDevice            | 已删除的设备。
AddDeviceConfiguration      | 添加的设备配置。
UpdateDeviceConfiguration   | 更新的设备配置。 若要查看哪些设备配置属性已更新，请参阅下面的部分中的[设备配置属性 Audited](#update-device-configuration-attributes)
DeleteDeviceConfiguration   | 已删除的设备配置。
AddRegisteredOwner     | 添加到设备的注册的所有者。
AddRegisteredUsers     | 已注册的用户添加到设备中。
RemoveRegisteredOwner    | 从设备中删除已注册的所有者。
RemoveRegisteredUsers    | 从设备中删除已注册的用户。
RemoveDeviceCredentials  | 删除设备的凭据。
**B2B 的事件**                       |
批量上载的邀请。              | 管理员已上载文件包含到伙伴用户发送邀请。
批次处理的邀请。             | 已处理包含伙伴用户邀请的文件。
邀请外部用户。                | 外部用户已邀请到该目录。
将外部用户邀请兑换成现金。         | 外部用户已兑现他们邀请到的目录。
外部用户添加到组中。          | 外部用户具备成员资格到目录中的组。
分配给应用程序的外部用户。 | 外部用户已指配给直接访问应用程序。
病毒的租户创建。               | 通过邀请兑换已在 Azure 广告创建新租户。
病毒的用户创建。                 | 用户已创建在现有租户在 Azure AD 通过邀请兑换。
**管理单位 （所有的新事件）**             |
AddAdministrativeUnit   |   添加管理单元。
UpdateAdministrativeUnit|   更新管理单元。 要查看管理单元的属性已更新，请参阅下面的部分中的[管理单元属性审核](#update-administrative-unit-attributes)
DeleteAdministrativeUnit|   删除管理单元。
AddMemberToAdministrativeUnit|  将成员添加到管理单元。
RemoveMemberFromAdministrativeUnit|     从管理单元中删除成员。
**目录事件**                 |
公司向中添加合作伙伴               | 添加到目录的一个伙伴。
从公司中删除合作伙伴          | 从目录中删除一个合作伙伴。
DemotePartner   |   对合作伙伴进行降级。
将域添加到公司                | 将域添加到目录。
从公司中删除域           | 从目录中删除域。
更新域                        | 更新域目录上。 要查看域属性已更新，请参阅下面的部分中的[域属性审核](#update-domain-attributes)
设置域身份验证            | 更改默认域设置的公司。
设置公司联系人信息      | 设置联系人公司级别的首选项。 这包括有关 Microsoft Online Services 的市场营销，以及技术通知的电子邮件地址。
设置域的联盟    | 更新域的联合身份验证设置。
验证域                        | 已验证域的目录上。
确认电子邮件已验证的域         | 已验证在使用电子邮件验证目录域。
公司集 DirSyncEnabled 标志   | 设置为 Azure AD 同步启用目录的属性。
设置密码策略                  | 设置用户密码的长度和字符限制。
集公司信息              | 更新公司级别信息。 [获得 MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) PowerShell cmdlet 的更多详细信息，请参阅。
SetCompanyAllowedDataLocation  |    允许数据位置的一组公司。
SetCompanyDirSyncEnabled    |   设置 DirSyncEnabled 标志。
SetCompanyDirSyncFeature |  设置目录同步功能。
SetCompanyInformation |   集公司信息。
SetCompanyMultiNationalEnabled    |     设置公司跨国功能已启用。
SetDirectoryFeatureOnTenant   |     在客户端上设置目录功能。
SetTenantLicenseProperties  |   设置租户许可属性。
CreateCompanySettings   |   创建公司设置
UpdateCompanySettings   |   更新公司设置。 若要查看哪些公司属性已更新，请参阅下面的部分中[公司属性审核](#update-company-attributes)
DeleteCompanySettings   |   删除公司设置
SetAccidentalDeletionThreshold   |  将意外删除阈值设置。
SetRightsManagementProperties   |   设置权限管理属性。
PurgeRightsManagementProperties     |   权限管理属性中清除。
UpdateExternalSecrets   |   更新外部的秘密
**策略事件 （所有的新事件）**           |
AddPolicy   |   添加策略。
UpdatePolicy    |   更新策略。
DeletePolicy    |   删除策略。
AddDefaultPolicyApplication     |   将策略添加到应用程序。
AddDefaultPolicyServicePrincipal    |   将策略添加到服务主体。
RemoveDefaultPolicyApplication  |   从应用程序移除策略。
RemoveDefaultPolicyServicePrincipal     |   删除服务主体的策略。
RemovePolicyCredentials     |   删除策略的凭据。

## <a name="audit-report-retention"></a>审计报告保留
在 Azure 广告审查报告事件保留 180 天。 关于保留报表的详细信息，请参阅[Azure 活动目录报告保留策略](active-directory-reporting-retention.md)。

客户感兴趣较长保留期内存储的审核事件，对于报告 API 可以用于定期将审核事件拉入一个单独的数据存储区。 有关详细信息，请参阅[入门报告 API](active-directory-reporting-api-getting-started.md) 。

## <a name="properties-included-with-each-audit-event"></a>属性包含与每个审核事件

属性      | 说明
------------- | --------------------------------------------------------------
日期和时间 | 审核事件发生的时间与日期
演员         | 用户或服务执行操作的主体
操作        | 已执行的操作
目标        | 在执行该操作的服务主体的用户


## <a name="update-user-attributes"></a>"更新用户"属性
"更新用户"审核事件包括有关哪些用户属性已更新的其他信息。 对于每个属性，请将包含的以前值和新值。

属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | 用户可以登录。
AssignedLicense                     | 已分配给用户的所有许可证。
AssignedPlan                        | 从而从许可证分配给用户的服务计划。
LicenseAssignmentDetail             | 许可证分配给用户的详细信息。 例如，如果涉及基于组的授权，这包括授予许可证组。
移动                              | 用户的移动电话。
OtherMail                           | 用户的备用电子邮件地址。
OtherMobile                         | 用户可选的移动电话。
StrongAuthenticationMethod          | 由用户配置的多因素身份验证，如语音呼叫、 SMS 或验证代码从移动应用程序的验证方法的列表。
StrongAuthenticationRequirement     | 如果实施了多因素身份验证，启用或禁用该用户的。
StrongAuthenticationUserDetails     | 用户的电话号码，备用电话号码和电子邮件地址，用于多因素身份验证和密码重置验证。
StrongAuthenticationPhoneAppDetail  | 注册要执行 2FA 登录详细信息 forof 电话应用程序
TelephoneNumber                     | 用户的电话号码。
AlternativeSecurityId               | 对象的可选的安全 ID。
CreationType                        |（可以通过邀请或病毒） 的用户的创建方法。
InviteTicket                        |邀请用户票证的列表。
InviteReplyUrl                      |在邀请接受答复的 url 的列表。
InviteResources                     |用户已邀请到的资源的列表。
LastDirSyncTime                     |上次更新对象由于同步从权威 （客户端） 的目录。
MSExchRemoteRecipientType           |映射为 MSO 收件人类型。 请参阅 [MSO 收件人类型] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx 的收件人类型
PreferredDataLocation               |用户、 组、 联系人的公用文件夹的首选的位置或设备的数据。
代理地址                      |按其 Exchange Server 收件人对象识别外部邮件系统中的地址。
StsRefreshTokensValidFrom           |执行刷新令牌的吊销信息-这次被视为前发出任何 STS 刷新令牌过期。
范围内                   |为互联网式登录名的用户的 UPN。
UserState                           |用户 PendingApproval/PendingAcceptance/接受/PendingVerification 的状态。
UserStateChangedOn                  |到 UserState 上次更改的时间戳。 用于触发工作流生命周期。
用户类型                            |用户的类型。 成员 (0)、 访客 (1) Viral(2)。

## <a name="update-group-attributes"></a>"更新组"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
分类            | 为统一组 （HBI、 MBI 等） 分类。
说明               | 有关该对象的描述性短语读。  
显示名称               |显示名称对象
DirSyncEnabled            |指示是否同步发生时从权威 （客户端） 的目录。
GroupLicenseAssignment    |许可证分配的组
GroupType                 |类型的组，统一 (0)
IsMembershipRuleLocked    |指示将 MembershipRule 属性设置的自助组管理服务和用户不能更改。 仅适用于其中 GroupType 包括 GroupType.DynamicMembership 组
IsPublic                  |用于指示组是否公钥/私钥的标志。
LastDirSyncTime           |上次同步从权威的结果更新该对象 （内部部署客户） 目录。
邮件                      |主要电子邮件地址
MailEnabled               |指示此组是否有电子邮件功能。
MailNickname              |通讯簿对象的名字对象，它前面的电子邮件名称的部分通常"@"符号。   
MembershipRule            |一个字符串，该字符串表示的自助组管理服务用于确定哪些成员应属于此组的条件。 请参阅 IsMembershipRuleLocked。 仅适用于其中 GroupType 包括 GroupType.DynamicMembership 组。
MembershipRuleProcessingState| 一个 enum 值，用于定义通过自助服务组管理服务定义的处理此组的成员身份状态。 仅适用于其中 GroupType 包括 GroupType.DynamicMembership 组。
代理地址            |按其 Exchange Server 收件人对象识别外部邮件系统中的地址。
RenewedDateTime           |时间戳的最近续订一个组的记录。   
SecurityEnabled           |指示是否在组中的成员资格可能影响授权决定。
WellKnownObject           |标记的目录对象，将它指定为预定义设置之一。

## <a name="update-device-attributes"></a>"更新设备"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | 表示安全主体可以进行身份验证。
CloudAccountEnabled | 表示安全主体可以进行身份验证。 当设备掌握在内部由 InTune 写入。
CloudDeviceOSType | 设备类型如 Windows RT，iOS 操作系统基础。 设置通过云服务 （如 Intune) 时，此特性成为权威在目录中的 DeviceOSType。
CloudDeviceOSVersion | 操作系统的版本。 设置通过云服务 （如 Intune) 时，此特性成为权威在目录中的 DeviceOSVersion。
CloudDisplayName | LDAP 显示名称属性的值。 设置通过云服务 （如 Intune) 时，此属性将成为权威显示名称的目录中。
CloudCreated |指示该对象是否由云服务。
CompliantUntil | 直到什么时候设备被视为符合标准。
DeviceMetadata |该设备的自定义元数据
DeviceObjectVersion | 此特性用于标识设备的架构版本。
DeviceOSType |设备类型如 Windows RT，iOS 操作系统基础。 编写的注册服务，应由 MDM 更新管理服务或 STS 亮管理服务。
DeviceOSVersion |操作系统的版本。 编写的注册服务，应由 MDM 更新管理服务或 STS 亮管理服务。
DevicePhysicalIds |多值的属性用于存储物理设备的标识符。 这可能包括 BIOS Id，TPM 指纹、 硬件特定 Id 等。
DirSyncEnabled | 指示是否从权威 （客户的内部部署版本） 进行同步目录。
显示名称 |显示名称对象
IsCompliant | 此特性用于管理设备的移动设备管理状态。
IsManaged |此特性用于指示设备由云 mdm。
LastDirSyncTime |上次更新对象由于同步从权威 （位于本地客户） 目录。

## <a name="update-device-configuration-attributes"></a>"更新设备配置"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |一个设备可以是之前处于非活动状态的最大天数被视为删除。
RegistrationQuota   |用来限制单个用户所允许的设备注册数量的策略。

## <a name="update-service-principal-configuration-attributes"></a>"更新服务主体配置"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |表示安全主体可以进行身份验证。
AppPrincipalId | 对于安全主体外部的应用程序定义的标识。
显示名称 |显示名称对象
ServicePrincipalName    | 服务主体名称，包含"名称/机构"位置名称指定应用程序类别值和机构至少包含主机 [: 端口] 或"名称"为服务主体指定标识符。

## <a name="update-app-attributes"></a>"更新应用程序"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |（重定向 Url） 分配给服务主体的地址集。
应用程序标识                               |应用程序的应用程序 ID   
AppIdentifierUri | 应用程序 URI，它标识该应用程序。  它通常是应用程序访问 URL。
AppLogoUrl |CDN 中存储应用程序徽标图像的 url。
AvailableToOtherTenants | 真正的应用程序是多租户应用程序 （如可由其他租户）。
显示名称 | 应用程序名称显示名称
权利 |应用程序权限的列表。
ExternalUserAccountDelegationsAllowed |该标志指明是否资源应用程序是可信的可以创建外部用户帐户的委派条目。
GroupMembershipClaims |组成员资格索赔策略。
PublicClient | 如果客户端无法保持机密 （亦即 OAuth2.0 中的非机密客户）
RecordConsentConditions | 同意条件，按合同条款定义的类型︰ 无 (0) SilentConsentForPartnerManagedApp(1)。 此值将公开图形 API 架构中，并且只能由租户管理员设置/更改。
RequiredResourceAccess |RequiredResourceAccess 属性的值的 XML 内容。
WebApp |如果为 true，则指示此应用程序是 web 应用程序。
WwwHomepage |主网页。

## <a name="update-role-attributes"></a>"更新角色"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |（重定向 Url） 分配给服务主体的地址集。
BelongsToFirstLoginObjectSet |如果为 true，则指示此对象属于启用的新租户的第一个管理员登录所需的对象的集合。
内建 |指示是否由系统拥有对象的生存期。
说明 | 有关该对象的描述性短语读。  
显示名称 |显示名称对象
MailNickname | 通讯簿对象的名字对象，它前面的电子邮件名称的部分通常"@"符号。
RoleDisabled |指明该角色是否应忽略为进行访问权限检查。
RoleTemplateId | 角色模板的标识。
ServiceInfo |由 MOAC 和/或其他服务 （相同或不同的服务类型实例） 的特定于服务的提供信息。
TaskSetScopeReference |标识设置和一套与角色或 RoleTemplate 关联的作用域。
ValidationError |发布由联合服务描述与有关的属性或链接对象管理员操作来解决非瞬态、 服务特定错误的信息。
WellKnownObject |标记的目录对象，将它指定为预定义设置之一。

## <a name="update-role-definition-attributes"></a>"更新角色定义"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |为安全主体指派此 RoleDefinition 时可以引用的授权范围的集合。
显示名称     |显示名称对象
GrantedPermissions  |该 RoleDefinition 由授予的权限。


## <a name="update-administrative-unit-attributes"></a>"更新管理单位"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
说明 |   当您更改管理的单位的描述时，都会更新此属性。
显示名称 |   一个管理单元的名称更改时，都会更新此属性。

## <a name="update-company-attributes"></a>"更新公司"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | 公司的用户可设置的位置。
AuthorizedServiceInstance   | 一个计划可能部署到其中的服务实例的名称。
DirSyncEnabled |指示是否从权威 （客户的内部部署版本） 进行同步目录。
DirSyncStatus |指示是否在此租户上下文中的地址簿对象的同步发生从权威 （客户的内部部署版本） 目录中。公司对象的 DirSyncEnabled 属性的扩展。
DirSyncFeatures |位标志，来跟踪有关租户的启用和禁用目录同步功能集。
DirectoryFeatures |启用/禁用目录功能。
DirSyncConfiguration |包含特定于当前租户的所有目录同步配置。
显示名称 |显示名称对象
IsMnc |一个布尔型标志设置为"true"iff 跨国公司功能启用了该公司。
ObjectSettings | 适用于该对象的作用域设置的集合。
PartnerCommerceUrl |合作伙伴的商务网站的 URL。
PartnerHelpUrl |合作伙伴的帮助网站的 URL。
PartnerSupportEmail | 合作伙伴支持通过电子邮件发送的 URL。
PartnerSupportTelephone |URL 的伙伴支持电话。
PartnerSupportUrl |合作伙伴的支持网站的 URL。
StrongAuthenticationDetails |与 StrongAuthentication 相关的详细信息。
StrongAuthenticationPolicy |该公司的强身份验证策略。
TechnicalNotificationMail |电子邮件地址通知与公司有关的技术问题。
TelephoneNumber |ITU 建议的 E.123 所遵守的电话号码。
TenantType |租户的类型。 如果未指定此值，租户是一家公司。 否则，可能的值为︰ MicrosoftSupport (0)、 SyndicatePartner (1)、 (2) BreadthPartner BreadthPartnerDelegatedAdmin (3) (4) ResellerPartnerDelegatedAdmin ValueAddedResellerPartnerDelegatedAdmin (5)。
VerifiedDomain |DNS 域名绑定到一家公司一套。

## <a name="update-domain-attributes"></a>"更新域"属性
属性                           | 说明
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
功能    |如果有，位描述域的能力的标志。
默认 |表示域是否为默认值;例如，默认范围内后缀时管理员在 MOAC 中创建一个新用户。
初始 |表示域是否是公司的初始域通过 OCP 分配。 在初始域是唯一子域的 Microsoft 联机域;e.g.contoso3.microsoftonline.com。
LiveType    |类型的相应 Windows Live 命名空间，如果有的话。
名称    | 终结点的标识符。
PasswordNotificationWindowDays  |数天前密码过期用户会收到通知。
PasswordValidityPeriodDays  | 必须更改密码，则适用于之前的天数。

审计记录是很多的法规遵从性要求的必需的控件。 对于使用 Azure 活动目录审核报表以满足他们的法规遵从性要求的客户，建议客户提交一份本帮助主题与客户的导出的审计报告，以帮助解释报告的详细信息的副本。 如果审计人员想了解 Azure 目前满足法规遵从性要求，直接到微软 Azure 信任中心的[法规遵从性页](https://azure.microsoft.com/support/trust-center/compliance/)审核员。
