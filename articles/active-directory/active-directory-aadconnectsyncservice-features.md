<properties
    pageTitle="Azure AD 连接同步服务功能和配置 |Microsoft Azure"
    description="介绍了 Azure AD 连接同步服务的服务端功能。"
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
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD 连接同步服务功能

Azure AD 连接的同步功能有两个组件︰

- 内部组件命名**Azure AD 连接同步**，也称为**同步引擎**。
- 驻留在 Azure 广告也称为**Azure AD 连接同步服务**服务

本主题说明**Azure AD 连接同步服务**的以下功能的工作方式以及如何配置它们使用 Windows PowerShell。

通过[Azure 活动目录模块用于 Windows PowerShell](http://aka.ms/aadposh)配置这些设置。 下载并安装它分别从 Azure AD 连接。 本主题中介绍的这些 cmdlet 是在[2016 3 月发布版本 （9031.1）](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1)中引入的。 如果您没有在本主题中介绍的 cmdlet 或它们不会产生相同的结果，则请确保您运行最新版本。

若要查看您的 Azure 的广告目录中的配置，请运行`Get-MsolDirSyncFeatures`。  
![获得 MsolDirSyncFeatures 结果](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

许多这些设置只能更改通过 Azure AD 连接。

可以通过配置以下设置`Set-MsolDirSyncFeature`:

DirSyncFeature | 注释
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | 允许重复的另一个对象而不会导致整个对象导出过程时隔离特性。
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | 允许对象要联接的范围内，除了主 SMTP 地址。
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | 允许同步引擎更新托管许可 （非联盟） 用户的范围内特性。

启用功能之后，不能再禁用。

>[AZURE.NOTE] 从 8 月 24，2016年*重复特性可恢复性*功能默认启用的新 Azure 的广告目录。 此外将推出并在此日期之前创建的目录上启用此功能。 当您的目录即将获得启用此功能时，您将收到电子邮件通知。

以下设置 Azure AD 连接的配置，并且不能修改由`Set-MsolDirSyncFeature`:

DirSyncFeature | 注释
--- | ---
DeviceWriteback | [Azure AD 连接︰ 启用设备写回](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD 连接同步︰ 目录扩展](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [实现使用 Azure AD 连接同步密码同步](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [预览︰ 组写回](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | 目前尚不支持。

## <a name="duplicate-attribute-resiliency"></a>重复的属性可恢复性
而不是故障设置的对象重复 Upn/代理地址，重复的特性"隔离"，并指派临时值。 当解决冲突的方法时，临时 UPN 是自动更改为适当的值。 这种现象可以分别启用 UPN 和 proxyAddress。 有关详细信息，请参阅[标识同步和复制属性可恢复性](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)。

## <a name="userprincipalname-soft-match"></a>范围内软匹配
启用此功能后，软匹配被启用 UPN 除了[主 SMTP 地址](https://support.microsoft.com/kb/2641663)，这总是处于启用状态。 在 Azure AD 与内部用户匹配现有的云用户用于软匹配。

如果需要匹配部署在云环境中创建的现有客户的广告帐户并不使用 Exchange 联机，然后此功能非常有用。 在此方案中，您通常没有云在设置 SMTP 属性的一个原因。

此功能是在默认情况下，对于新创建的 Azure 的广告目录。 您可以看到是否启用此功能为您通过运行︰  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

如果 Azure 的广告目录的情况下，不启用此功能，然后您可以启用它通过运行︰  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>同步范围内更新
从历史上看，更新到使用同步服务从内部已被阻止，除非同时满足两个条件︰

- 用户管理 （非联合）。
- 用户尚未分配许可证。

有关详细信息，请参阅[在 Office 365，Azure 或 Intune 用户名不匹配的本地 UPN 或替代登录 ID](https://support.microsoft.com/kb/2523192)。

启用此功能允许同步引擎更新已更改的内部部署并且使用密码同步时的范围。 如果您使用联合身份验证，则不支持此功能。

此功能是在默认情况下，对于新创建的 Azure 的广告目录。 您可以看到是否启用此功能为您通过运行︰  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

如果 Azure 的广告目录的情况下，不启用此功能，然后您可以启用它通过运行︰  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

在启用此功能后后, 现有范围内值将保留为-是。 在范围内属性在内部部署的下一处修订，对用户的正常增量同步将更新 UPN。  

## <a name="see-also"></a>请参见

- [Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)
- [将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
