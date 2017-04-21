<properties
    pageTitle="实现使用 Azure AD 连接同步密码同步 |Microsoft Azure"
    description="提供了信息密码同步的工作原理以及如何启用它。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>实现使用 Azure AD 连接同步密码同步
本主题为您提供了您需要同步到一个基于云的 Azure 活动目录 (AD Azure) 从内部活动目录 (AD) 用户密码的信息。

## <a name="what-is-password-synchronization"></a>什么是密码同步
概率阻止从因忘记密码而完成工作的不同需要记忆的密码数相关。 您需要记住，越高的概率要忘记一个多个密码。 问题和有关重置密码和密码相关的其他问题的电话要求大多数技术支持资源。

密码同步是一种功能来同步到一个基于云的 Azure 活动目录 (AD Azure) 内部部署 Active Directory 中的用户密码。
此功能使您能够登录到 Azure Active Directory 服务 （如 Office 365、 Microsoft Intune、 CRM Online 和 Azure AD 域服务） 使用相同的密码用于登录到您的内部活动目录。

![什么是 Azure AD 连接](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

通过减少用户需要维护一个密码，密码同步可帮助您︰

- 提高用户生产效率
- 降低支持成本  

此外，如果您选择使用[**AD FS 的联合身份验证**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)，则 （可选） 可以作为备份密码同步您 AD FS 基础结构发生故障的情况下。

密码同步是由 Azure AD 连接同步目录同步功能的延伸。 若要在您的环境中使用密码同步，您需要︰

- Azure AD 安装连接  
- 配置您的部署之间的目录同步 AD 和 Azure 活动目录
- 启用密码同步

有关详细信息，请参阅[集成内部标识使用 Azure 活动目录](active-directory-aadconnect.md)

> [AZURE.NOTE] 有关配置为 FIPS 和密码同步的 Active Directory 域服务的详细信息，请参阅[密码同步和 FIPS](#password-synchronization-and-fips)。

## <a name="how-password-synchronization-works"></a>密码同步的工作原理
Active Directory 域服务的实际用户密码的哈希值表示形式存储密码。 哈希值是单向数学函数 （"*哈希算法*"） 的结果。 没有任何方法可以进行恢复密码的明文版本的单向函数的结果。 不能使用的密码哈希值以用于登录到您的内部网络。

若要同步您的密码，Azure AD 连接同步在内部部署 Active Directory 中提取您的密码哈希。 额外的安全处理应用于密码哈希之前就可以同步到 Azure 活动目录验证服务。 密码同步在每用户的基础上，按时间顺序排列。

密码同步过程的实际数据流动相类似的如显示名称或电子邮件地址的用户数据同步。 但是，通常比其他特性的标准目录同步窗口同步密码。 密码同步过程将运行每 2 分钟。 您不能修改此过程的频率。 当同步密码时，它会覆盖现有的云密码。

第一次启用密码同步功能，它将执行初始同步作用域中的所有用户的密码。 不能显式定义要同步的用户密码的一个子集。

内部密码更改时，更新的密码同步，往往在几分钟内。
密码同步功能自动重试失败的同步尝试。 如果尝试将密码同步期间发生错误，错误将记录在事件查看器中。

密码同步的当前登录的用户没有任何影响。
您登录到云服务的过程中发生的同步的密码更改不立即受影响当前云服务会话。 但是，当云服务要求您再次进行身份验证，您需要提供您的新密码。

> [AZURE.NOTE] 对象类型用户在 Active Directory 中仅支持密码同步。 它不支持 iNetOrgPerson 对象类型。

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>密码同步原理与 Azure AD 域服务
密码同步功能也可用于同步对[Azure AD 域服务](../active-directory-domain-services/active-directory-ds-overview.md)内部密码。 这种情况下允许与您在部署中可用的所有方法验证您的用户群中了 Azure AD 域服务广告。 这种情况下的经验是类似于在的内部部署环境中使用活动目录迁移工具 (ADMT)。

### <a name="security-considerations"></a>安全注意事项
当同步密码，密码的纯文本版本不会受到密码同步功能，到 Azure 的广告，或任何相关的服务。

另外，还有不要求在内部部署 Active Directory 以可逆加密格式存储密码。 摘要的 Active Directory 密码哈希值用于内部部署之间传输广告和 Azure 活动目录。 密码哈希值的摘要不能用于访问您的内部环境中的资源。

### <a name="password-policy-considerations"></a>密码策略注意事项
有两种类型的密码策略，受到了启用密码同步︰

1. 密码复杂性策略
2. 密码过期策略

**密码复杂性策略**  
当启用密码同步时，将内部部署 Active Directory 中的密码复杂性策略重写同步用户群中的复杂性策略。 内部活动目录的所有有效密码用于访问 Azure 的广告服务。

> [AZURE.NOTE] 在云环境中直接创建的用户的密码受到仍密码策略定义在云中。

**密码过期策略**  
如果用户密码同步的作用域中，云帐户密码设置为"*永不过期*"。
您可以继续登录到您的云服务使用内部部署环境中已过期的同步的密码。 下一次更改内部环境中的密码更新您云口令。

### <a name="overwriting-synchronized-passwords"></a>覆盖同步密码
管理员可以手动重置您的密码使用 Windows PowerShell。

在这种情况下，新的密码重写同步的密码并在云环境中定义的所有密码策略都应用到新的密码。

如果您更改内部密码，新密码同步到云中，并重写手动更新的密码。

## <a name="enabling-password-synchronization"></a>启用密码同步
自动启用密码同步，当您安装使用**快速设置**Azure AD 连接。 有关详细信息，请参阅[入门使用快速设置 Azure AD 连接](./connect/active-directory-aadconnect-get-started-express.md)。

如果安装 Azure AD 连接时，您可以使用自定义设置，则启用密码同步，用户在登录页上。 有关详细信息，请参阅[Azure AD 连接的自定义安装](./connect/active-directory-aadconnect-get-started-custom.md)。

![启用密码同步](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>密码同步和 FIPS
如果您的服务器已被锁定根据联邦信息处理标准 (FIPS)，MD5 已被禁用。

**若要启用 MD5 密码同步，请执行以下步骤︰**

1. 转到**%programfiles%\Azure AD Sync\Bin**。
2. 打开**miiserver.exe.config**。
3. 转到**运行时**配置/节点 （位于文件的末尾）。
4. 添加以下节点︰`<enforceFIPSPolicy enabled="false"/>`
5. 保存您的更改。

为了便于参考，此代码段是如何它应如下所示︰

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

有关安全和 FIPS 请参阅[AAD 密码同步、 加密和 FIPS 遵从性](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>故障排除密码同步
如果密码不同步像预期的那样，它可以是的那部分用户或所有用户。

- 如果有单个对象的问题，请参阅[疑难解答，没有同步密码的一个对象](#troubleshoot-one-object-that-is-not-synchronizing-passwords)。
- 如果您有一个问题没有密码同步位置，请参阅[无密码同步，解决问题](#troubleshoot-issues-where-no-passwords-are-synchronized)。

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>没有同步密码的一个对象疑难解答
通过检查对象的状态，可以很容易地解决密码同步问题。

在**Active Directory 用户和计算机**中启动。 查找用户，并验证未选定**用户必须更改在下一次登录的密码**。
![活动目录效率密码](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
如果选择它，然后要求用户登录和更改密码。 临时密码不会同步到 Azure 的广告。

如果它看起来在 Active Directory 中正确，然后下一步是按照同步引擎中的用户。 按照用户从内部部署 Active Directory 到 Azure 的广告，可以看到在该对象上是否具有描述性错误。

1. 启动**[同步服务管理器](active-directory-aadconnectsync-service-manager-ui.md)**。
2. 单击**连接器**。
3. 选择在用户所在的**Active Directory 连接器**。
4. 选择**搜索连接器空间**。
5. 找到要查找的用户。
6. 选择**沿袭**选项卡，请确保至少一个同步规则显示为**True**的**密码同步**。 在默认配置中，同步规则的名称是**在从广告的用户 AccountEnabled**。  
    ![沿袭关于用户的信息](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. 然后通过对 Azure AD 连接器空间元[跟随用户](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system)。 连接器空间对象应具有**密码同步**设置为**True**时出站规则。 在默认配置中，同步规则的名称是**出给 AAD 的用户加入**。  
    ![用户连接器空间属性](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. 若要查看上周的密码同步对象的详细信息，请单击**日志...**。  
    ![对象日志详细信息](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

状态列可以具有下列值︰

状态 | 说明
---- | -----
成功 | 已成功同步密码。
FilteredByTarget | 密码设置为**用户更改在下一次登录的密码**。 密码没有进行同步。
NoTargetConnection | 中元节或在 Azure AD 连接器空间中没有对象。
SourceConnectorNotPresent | 在内部部署 Active Directory 连接器空间中找到的对象。
TargetNotExportedToDirectory | 在 Azure AD 连接器空间中的对象尚未导出。
MigratedCheckDetailsForMoreInfo | 日志项生成 1.0.9125.0 之前就已创建并显示在其传统的状态。

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>解决的问题无密码同步位置
首先[获取密码同步设置的状态](#get-the-status-of-password-sync-settings)部分中运行脚本。 它使您的密码同步配置概述。  
![PowerShell 脚本输出密码同步设置](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
如果在 Azure AD 中不启用该功能，或者如果未启用同步信道状态，然后运行连接安装向导。 选择**自定义同步选项**，并且取消选择密码同步。 这种更改暂时禁用该功能。 然后再次运行该向导并重新启用密码同步。 运行该脚本，再次以验证配置正确。

该脚本显示没有心跳，然后运行该脚本[的所有密码完全同步触发器](#trigger-a-full-sync-of-all-passwords)。 此脚本还可以用于其他方案的配置正确，但密码不同步。

#### <a name="get-the-status-of-password-sync-settings"></a>获取密码同步设置的状态

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>触发所有密码完全的同步
可以触发完全同步的所有密码，使用下面的脚本︰

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>下一步行动

* [Azure AD 连接的同步︰ 自定义同步选项](active-directory-aadconnectsync-whatis.md)
* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)
