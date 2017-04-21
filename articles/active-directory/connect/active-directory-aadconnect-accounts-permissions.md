<properties
   pageTitle="Azure AD 连接︰ 用户帐户及权限 |Microsoft Azure"
   description="本主题介绍使用和创建帐户和所需的权限。"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD 连接︰ 用户帐户及权限
Azure AD 连接安装向导提供了两种不同的路径︰

- 在快速设置向导需要更多特权，使它可以轻松地设置您的配置而无需创建用户或单独配置的权限。

- 在自定义设置向导为您提供更多的选择和选项，但有些情况下，您需要确保您具有正确的权限您自己。

## <a name="related-documentation"></a>相关的文档
如果未读取有关[集成内部标识使用 Azure 活动目录](../active-directory-aadconnect.md)的文档下, 表提供了指向相关主题的链接。

主题 |  
--------- | ---------
安装使用快速设置 | [Azure AD 连接的快速安装](active-directory-aadconnect-get-started-express.md)
安装使用自定义设置 | [Azure AD 连接的自定义安装](active-directory-aadconnect-get-started-custom.md)
从目录同步升级 | [从 Azure AD 同步工具 （目录同步） 升级](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>快速设置安装
在快速设置，安装向导会询问 AD DS 企业管理员凭据以便内部活动目录可以配置为使用 Azure AD 连接所需的权限。 如果要从目录同步升级，AD DS 企业管理员凭据用于重置目录同步使用的帐户的密码。 您还需要 Azure AD 全局管理员凭据。

向导页  | 收集的凭据 | 所需权限| 用于
------------- | ------------- |------------- |-------------
N/A|运行安装向导的用户| 本地服务器的管理员联系| <li>创建作为[同步引擎服务帐户](#azure-ad-connect-sync-service-account)使用本地帐户。
连接到 Azure 的广告| Azure 的广告目录凭据 | 在 Azure 广告中的全局管理员角色 | <li>启用在 Azure 的广告目录同步。</li>  <li>正在同步操作在 Azure 的广告中使用[Azure AD 帐户](#azure-ad-service-account)的创建。</li>
连接到 AD DS | 内部部署 Active Directory 凭据 | 企业管理员 (EA) 在 Active Directory 中组的成员| <li>在活动目录中创建一个[帐户](#active-directory-account)并授予它的权限。 这创建了帐户用于读取和写入过程中同步目录信息。</li>

### <a name="enterprise-admin-credentials"></a>企业管理员凭据
这些凭据仅在安装过程中使用，并在安装完成后使用。 它是企业管理员，并不是域管理员，以确保在 Active Directory 中的权限可以设置中的所有域。

### <a name="global-admin-credentials"></a>全局管理员凭据
这些凭据仅在安装过程中使用和不使用安装完成后。 它用于创建用于对 Azure AD 同步更改的[Azure AD 帐户](#azure-ad-service-account)。 在 Azure AD 帐户作为一项功能也使得同步。

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>快速设置帐户权限创建 AD DS
用于读取和写入到 AD DS 中创建的[帐户](#active-directory-account)具有以下权限创建快速设置时︰

权限 | 用于
---- | ----
<li>复制目录的更改</li><li>复制目录更改所有 | 密码同步
读写用户的所有属性 | 导入和交换的混合
读取/写入所有属性 iNetOrgPerson | 导入和交换的混合
读取/写入所有属性都组 | 导入和交换的混合
读取/写入所有属性都联系 | 导入和交换的混合
重置密码 | 启用密码写回的准备工作

## <a name="custom-settings-installation"></a>自定义设置安装
当使用自定义设置，必须在安装前创建用来连接到活动目录的帐户。 [创建 AD DS 帐户](#create-the-ad-ds-account)中找不到您必须授予此帐户的权限。

向导页  | 收集的凭据 | 所需权限| 用于
------------- | ------------- |------------- |-------------
N/A | 运行安装向导的用户|<li>本地服务器的管理员联系</li><li>如果使用完整的 SQL Server，则用户必须在 SQL 系统管理员 (SA)</li>| 默认情况下，创建作为[同步引擎服务帐户](#azure-ad-connect-sync-service-account)使用本地帐户。 当管理员没有指定特定的帐户只能创建该帐户。
安装同步服务服务帐户选项 | 广告或本地用户帐户凭据 | 用户权限授予由安装向导 | 如果管理员指定的帐户，该帐户用作同步服务的服务帐户。
连接到 Azure 的广告 | Azure 的广告目录凭据| 在 Azure 广告中的全局管理员角色| <li>启用在 Azure 的广告目录同步。</li>  <li>正在同步操作在 Azure 的广告中使用[Azure AD 帐户](#azure-ad-service-account)的创建。</li>
连接您的目录 | 部署的每个目录林，到 Azure AD 连接 Active Directory 凭据 | 的权限取决于功能启用并[创建 AD DS 帐户](#create-the-ad-ds-account)，请参阅 |此帐户用于读取和写入过程中同步目录信息。
AD FS 服务器 | 对于列表中每个服务器，向导会收集凭据时运行该向导的用户的登录凭据不足，无法连接 | 域管理员 | 安装和配置 AD FS 服务器角色。
Web 应用程序代理服务器 |对于列表中每个服务器，向导会收集凭据时运行该向导的用户的登录凭据不足，无法连接 | 目标计算机上的本地管理员 | 安装和配置的 WAP 服务器角色。
代理服务器信任的凭据 |联合身份验证服务信任凭据 （凭据代理用于注册 FS 的信任证书 |是 AD FS 服务器的本地管理员的域帐户 | 初始注册 FS WAP 信任证书。
AD FS 服务帐户页中，"使用域用户帐户选项" | AD 用户帐户凭据 | 域用户 | 提供的凭据的 AD 用户帐户用作 AD FS 服务的登录帐户。

### <a name="create-the-ad-ds-account"></a>创建 AD DS 帐户
安装 Azure AD 连接时，**连接您的目录**页指定的帐户在 Active Directory 中必须存在并具有所需权限授予。 安装向导不验证在同步过程中只找到权限和任何问题。

所需的权限取决于可选功能，则启用。 如果您有多个域，则必须为林中的所有域授予权限。 如果不启用这些功能，默认的**域用户**权限就足够了。

功能 | 权限
------ | ------
密码同步 | <li>复制目录的更改</li>  <li>复制目录更改所有
混合的 Exchange 部署 | 为用户、 组和联系人记录在[交换混合回写](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback)的属性的写入权限。
密码写回 | 属性为用户记录中[的密码管理入门](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions)的写入权限。
设备写回 | 使用 PowerShell 脚本[设备写回](../active-directory-aadconnect-feature-device-writeback.md)所述授予的权限。
组的写回 | 读取、 创建、 更新和删除通讯组应该位于该 OU 中的组对象。

## <a name="upgrade"></a>升级
当从 Azure AD 连接的一个版本升级到新版本时，您需要下列权限︰

主体 | 所需权限 | 用于
---- | ---- | ----
运行安装向导的用户 | 本地服务器的管理员联系 | 更新的二进制文件。
运行安装向导的用户 | ADSyncAdmins 的成员 | 对同步规则和其他配置进行更改。
运行安装向导的用户 | 如果您使用完整的 SQL 服务器︰ DBO （或类似） 的同步引擎数据库 | 进行数据库级别的更改，例如更新与新列的表。

## <a name="more-about-the-created-accounts"></a>有关创建帐户的详细信息

### <a name="active-directory-account"></a>活动目录帐户
如果使用快速设置，然后创建帐户用于同步的 Active Directory 中。 位于用户容器中的目录林根域中创建的帐户，并有其名称前缀**MSOL_**。 使用长时间复杂密码不过期创建该帐户。 如果您有在您的域中的密码策略，请确保长和复杂的密码就会被允许为此帐户。

![AD 帐户](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure AD 连接同步服务帐户
（除非您指定该帐户以使用自定义设置中），将由安装向导创建本地服务帐户。 该帐户是前缀**AAD_** ，并用于实际的同步服务来运行方式。 如果您在域控制器上安装了 Azure AD 连接，在域中创建帐户。 如果您使用运行 SQL 服务器或如果您使用代理服务器要求身份验证的远程服务器，则必须在域中位于**AAD_**服务帐户。

![同步服务帐户](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

使用长时间复杂密码不过期创建该帐户。

此帐户用于将其他帐户密码存储在安全的方式。 加密数据库中存储这些其他帐户密码。 使用 Windows 数据保护 API (DPAPI) 加密服务私钥加密受保护的私钥的加密密钥。 因为 Windows 将再销毁加密密钥，出于安全考虑，不应重置服务帐户的密码。

如果您使用完整的 SQL Server，则该服务帐户是数据库的同步引擎创建的 DBO。 该服务将无法按预期使用的任何其他权限。 此外会创建 SQL 登录名。

此外授予对文件、 注册表项和其他对象与同步引擎相关权限帐户。

### <a name="azure-ad-service-account"></a>Azure AD 服务帐户
在 Azure 的广告帐户创建同步服务的使用。 通过其显示名称可以识别该帐户。

![AD 帐户](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

用户名称的第二部分，可以标识的服务器使用的帐户的名称。 在图片中，服务器名称是 FABRIKAMCON。 如果有临时服务器，每台服务器都有自己的帐户。 在 Azure AD 中没有 10 个同步服务帐户的限制。

用长时间复杂密码不过期，创建服务帐户。 它被授予特殊角色都只具有权限执行目录同步任务的**目录同步帐户**。 这种特殊的内置角色不能被授予 Azure AD 连接向导和 Azure 门户显示此角色的**用户**帐户。

![广告客户角色](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>下一步行动

了解有关[将您的内部标识使用 Azure Active Directory 集成](../active-directory-aadconnect.md)。
