<properties
    pageTitle="开始使用 Azure 堆栈密钥存储库 |Microsoft Azure"
    description="开始使用 Azure 堆栈密钥存储库"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>要开始使用密钥存储库

本部分介绍的步骤创建一个电子仓库，管理密钥和密码以及授权用户或应用程序的调用存储库中的 Azure 堆栈中的操作。 以下步骤假定存在的租户订阅和 KeyVault 服务注册该订阅中。 所有的示例命令基于可用的 KeyVault cmdlet Azure PowerShell SDK 的一部分。

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>启用保险存储操作的租户订阅 

您可以签发可针对任何存储区的操作之前，您需要确保保险存储操作中启用了您的订购。 您可以通过以下的 PowerShell 命令确认的︰

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

上述命令的输出应该报告每个行的"注册"状态"已的登记"。

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 如果不是这种情况，您应该调用下面的命令来注册您的订阅中的 KeyVault 服务︰

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

而以下是该命令的输出︰
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] 如果您收到错误消息:"*订阅未注册 Azure 密钥存储库*"文件时，调用 KeyVault 的 cmdlet，请确认您已启用 KeyVault 资源提供程序按上面的说明。


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>在 Azure 堆栈来存储和管理加密密钥和密码创建加固的容器 （存储库）

为了创建一个电子仓库，承租人应首先创建资源组。 以下 PowerShell 命令该资源组中创建的资源组，然后一个电子仓库。 该示例还包括该 cmdlet 的典型输出。

### <a name="creating-a-resource-group"></a>创建资源组︰

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

输出︰

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>创建电子仓库︰


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

输出︰

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
此 cmdlet 的输出显示刚刚创建了密钥存储库的属性。 两个最重要的属性是︰

-   **电子仓库名称**︰ 在本例中，这是**vault010**。 您将使用此名称的其他密钥存储库的 cmdlet。

-   **保险存储 URI**︰ 在本例中，这是 https://vault010.vault.azurestack.local。 使用的 REST API，通过存储库的应用程序必须使用此 URI。

Azure 帐户已被授权对此密钥存储库执行任何操作。 没有其他人了。


## <a name="operating-on-keys-and-secrets"></a>工作项和机密信息

创建后的存储库，请按照以下步骤来创建管理密钥和密码︰

### <a name="creating-a-key"></a>创建密钥

为了创建一个密钥，使用按下面的示例中**添加 AzureKeyVaultKey** 。 之后成功的密钥创建 cmdlet 将输出新创建的关键细节。

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

下面是*添加 AzureKeyVaultKey* cmdlet 的输出︰

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
您现在可以引用此键创建或上载到 Azure 密钥存储库，通过它的 URI。 使用**keyVaultKeyName001/https://vault010.vault.azurestack.local:443/键**始终获得最新的版本;然后使用**https://vault010.vault.azurestack.local:443/键/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff**获得该特定版本。

### <a name="retrieving-a-key"></a>检索一个键

使用**Get AzureKeyVaultKey**检索键并按下面的示例的详细信息︰

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

以下是获取 AzureKeyVaultKey 的输出

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>设置一个秘密

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
输出

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>检索一个秘密

    Get-AzureKeyVaultSecret -VaultName vault010

输出

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

现在，您的密钥存储库和密钥或密码已准备好要使用的应用程序。
您必须授权使用它们的应用程序。

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>授权应用程序要使用的密钥或密码 

若要授权访问密钥或密钥存储库中的应用程序，使用集-**AzureRmKeyVaultAccessPolicy** cmdlet。

例如，如果电子仓库名称是*ContosoKeyVault*和您要授权的应用程序具有*8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*，一个*客户机 ID*和您要授权解密并使用您的存储库中的密钥对进行签名的应用程序，运行以下命令︰

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

如果您想授权读取机密信息在您的存储库中的同一个应用程序，运行以下命令︰

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>下一步行动
[部署虚拟机使用密钥存储库密码](azure-stack-kv-deploy-vm-with-secret.md)

[部署虚拟机的证书密钥存储库](azure-stack-kv-push-secret-into-vm.md)