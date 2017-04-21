<properties
    pageTitle="在 Azure AD 组管理 azure 活动目录 PowerShell 预览 cmdlet |Microsoft Azure"
    description="此页提供 PowerShell 的示例，以帮助您管理您在 Azure Active Directory 中的组"
    keywords="Azure 的广告，Azure Active Directory，PowerShell，组，组管理"
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
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory 预览 cmdlet 组管理

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-groups-create-azure-portal.md)
- [Azure 的传统门户网站](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

下面的文档将提供与如何使用 PowerShell 来管理您的组在 Azure 活动目录 (AD Azure) 的示例。  它还提供如何使用 Azure AD PowerShell 预览模块进行设置的信息。 首先，您必须[下载 Azure AD PowerShell 模块](http://go.microsoft.com/fwlink/p/?LinkId=828627)。

## <a name="installing-the-azure-ad-powershell-module"></a>Azure AD PowerShell 模块安装

要安装 AzureAD PowerShell 预览模块，请使用以下命令︰

    PS C:\Windows\system32> install-module azureadpreview

若要验证预览模块已安装，请使用下面的命令︰

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

现在您可以开始在模块中使用 cmdlet。 在 AzureAD 预览模块的 cmdlet 的完整说明，请参阅[在线参考文档](https://msdn.microsoft.com/library/azure/mt757216.aspx)。

## <a name="connecting-to-the-directory"></a>连接到目录
可以开始管理使用 Azure AD PowerShell 预览 cmdlet 组之前，您必须连接到要管理的目录的 PowerShell 会话。 若要执行此操作，请使用下面的命令︰

    PS C:\Windows\system32> Connect-AzureAD -Force

该 cmdlet 将提示您输入要用来访问您的目录的凭据。 在此示例中，我们将使用karen@drumkit.onmicrosoft.com访问演示目录。 该 cmdlet 将返回一条确认消息，以显示该会话已成功连接到您的目录︰

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

现在您可以开始使用 AzureAD 预览 cmdlet 来管理目录中的组。

## <a name="retrieving-groups"></a>检索组
从目录中检索现有的组，您可以使用 Get AzureADGroups cmdlet。 若要检索目录中的所有组，请使用不带参数的该 cmdlet:

    PS C:\Windows\system32> get-azureadgroup

该 cmdlet 将返回连接目录中的所有组。

可以使用对象 Id 的参数来检索特定组为其指定的组中的对象 Id:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

该 cmdlet 现在将返回的对象 Id 与您输入参数的值相匹配的组︰

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

您可以搜索特定的组使用的筛选器参数。 此参数采用 ODATA 筛选子句并返回所有组相匹配的筛选器，如以下示例所示︰

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

请注意，AzureAD PowerShell cmdlet 实现 OData 查询标准，更多的信息，请参阅[此处](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter)。

## <a name="creating-groups"></a>创建组
若要在目录中创建新组，请使用 New AzureADGroup cmdlet。 此 cmdlet 将创建一个名为"市场"的新安全组︰

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>更新组
若要更新现有的组，请使用组 AzureADGroup cmdlet。 在此示例中，我们要更改显示名称组属性的"Intune 管理员"。 首先，我们发现使用 Get AzureADGroup cmdlet 的组并使用显示名称属性进行筛选︰

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

接下来，我们正在为新的值"Intune 设备管理员"更改说明属性︰

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

现在如果我们再次发现组中，我们看到的说明属性将更新以反映新值︰

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>删除组
若要从目录中删除组，请使用删除 AzureADGroup cmdlet，如下所示︰

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>管理组的成员
如果您需要将新成员添加到组，请使用添加 AzureADGroupMember cmdlet。 此命令将成员添加到前面的示例中，我们使用 Intune 管理员组:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

的对象 Id 参数是我们想要将成员添加到组的对象 Id 和-RefObjectId 是我们想要作为成员添加到组的用户的对象 Id。

若要获取某个组的现有成员，使用 Get AzureADGroupMember cmdlet，如本示例所示︰

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

若要删除我们以前添加到组的成员，请使用删除 AzureADGroupMember cmdlet，如此处所示︰

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

若要验证的用户组 membership(s)，请使用选择 AzureADGroupIdsUserIsMemberOf cmdlet。 此 cmdlet 将作为它的参数对象 Id 的用户对其进行检查的组成员身份，并对其进行检查成员资格的组的列表。 提供复杂变量类型"Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck"的形式，因此我们首先必须创建一个变量与该类型必须是组的列表︰

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

接下来，我们将提供 groupIds 检查"GroupIds"此复杂变量的属性中的值︰

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

现在，如果我们想要检查的 ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea 与 $g 中的组的用户的组成员身份，我们应该使用︰

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


返回的值是该用户是其成员的组的列表。 您还可以应用此方法来检查组，选择 AzureADGroupIdsContactIsMemberOf，选择 AzureADGroupIdsGroupIsMemberOf 或选择 AzureADGroupIdsServicePrincipalIsMemberOf 使用给定列表的联系人、 组或服务主体成员资格

## <a name="managing-owners-of-groups"></a>管理组的所有者
若要向组中添加所有者，使用添加 AzureADGroupOwner cmdlet:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

的对象 Id 参数是我们想要添加的所有者组的对象 Id 和-RefObjectId 是我们想要为组的所有者添加的用户的对象 Id。

若要检索组的所有者，使用 Get AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

该 cmdlet 将返回为指定的组的所有者的列表︰

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

如果您想要从组中删除所有者，则使用删除 AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>下一步行动

在[Azure 活动目录 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=808260)，您可以找到更多 Azure 活动目录 PowerShell 文档。

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
