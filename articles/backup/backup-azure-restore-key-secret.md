<properties
    pageTitle="还原密钥存储库密钥和密钥加密的虚拟机使用 Azure 备份 |Microsoft Azure"
    description="了解如何还原密钥存储库的密钥和密钥使用 PowerShell 的 Azure 备份"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>对于加密的虚拟机使用 Azure 备份恢复密钥存储库密钥和密码
这篇文章谈到使用 Azure VM 备份实用程序还原加密 Azure Vm，如果您的密钥和密钥在密钥存储库中不存在。 如果您想要恢复 VM 的维护一份的密钥 （密钥加密密钥） 和密码 （BitLocker 加密密钥），还可以使用下列步骤。

## <a name="pre-requisites"></a>必备组件

1. **备份加密的虚拟机**-加密 Azure Vm 已备份使用 Azure 备份。 有关如何备份加密的 Azure 虚拟机的详细信息，请参阅文章[使用 PowerShell 的 Azure 虚拟机的备份和恢复管理](backup-azure-vms-automation.md)。

2. **配置 Azure 密钥存储库**– 确保密钥存储库的密钥和密码需要恢复已存在。 有关密钥存储库管理的详细信息，请参阅文章[开始使用 Azure 密钥存储库](../key-vault/key-vault-get-started.md)。

## <a name="setup-recovery-services-vault"></a>安装程序恢复服务存储库 
使用以下步骤可以登录到 PowerShell 和设置恢复服务存储库上下文

### <a name="log-in-to-azure-powershell"></a>登录到 Azure PowerShell 

登录到使用以下 cmdlet 的 Azure 帐户

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>设置故障恢复服务存储库上下文

登录后，请使用以下 cmdlet 以获取可用的订阅列表

```
PS C:\> Get-AzureRmSubscription
```

选择在哪些资源是可用的订阅

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

设置存储库上下文使用恢复服务存储库位置备份启用加密的虚拟机

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>恢复点 

表示加密 Azure 的虚拟机存储库中选择容器

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

使用此容器，得到相应的虚拟机的项

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

获取变量的 rp 中选定的备份项恢复点的数组

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>还原加密的虚拟机
使用下列步骤还原加密的虚拟机，其密钥和密码。

### <a name="restore-key"></a>恢复密钥

阵列 $rp 以上，时间与最新的恢复点，位于索引 0 处的按相反的顺序排序。 例如︰ $rp [0] 就会选择最新的恢复点。

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
此 cmdlet 成功运行后，blob 文件获取生成在指定的文件夹在计算机上运行它。 该 blob 文件表示密钥加密密钥以加密的形式。

将密钥备份还原到密钥存储库使用以下 cmdlet。 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>恢复密码

从上面获得的恢复点还原机密数据

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
在 vault.azure.net 之前的文本表示原始密钥存储库名称。 机密信息后的文本 / 表示密钥的名称。 

以防您想要使用相同的密钥名称从上面运行该 cmdlet 的输出中获取机密的名称和值。 在其他情况下，下面的 $secretname 应更新以使用新的密钥名称。 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

VM 需要以及恢复的情况下，则设置标记为机密。 标记 DiskEncryptionKeyFileName 中，值应该包含您打算使用的密钥的名称。 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
DiskEncryptionKeyFileName 值是秘密获取上面的名称相同。 DiskEncryptionKeyEncryptionKeyURL 的值可以还原备份密钥并使用[Get AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet 后获得密钥存储库中   

机密密钥存储库返回一组

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>将虚拟机恢复
上述 PowerShell cmdlet 帮助您将恢复密钥和密钥后到密钥存储库，如果您已备份加密的虚拟机使用 Azure VM 备份。 还原后它们，请参阅文章[管理 Azure 使用 PowerShell 的虚拟机的备份和恢复](backup-azure-vms-automation.md)来恢复加密的虚拟机。
