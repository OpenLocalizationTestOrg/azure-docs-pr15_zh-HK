<properties
    pageTitle="使用证书使用 Azure 堆栈密钥存储库中部署虚拟机 |Microsoft Azure"
    description="了解如何部署一个虚拟机，并注入 Azure 堆栈密钥存储区中的证书"
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
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>创建虚拟机并将包括从密钥存储库中检索到证书

Azure 堆栈中的虚拟机部署到 Azure 资源管理器中，并可以将证书存储在 Azure 堆栈密钥存储库中。 然后 Azure 堆栈 （Microsoft.Compute 通过特定的资源提供） 推动它们到您的 Vm 部署 Vm 时。 可以在多种方案，包括 SSL，加密和基于证书的身份验证中使用证书。

通过使用此方法，您可以将安全证书。 现在是不是在虚拟机映像中，或在应用程序的配置文件或其他不安全的位置。 通过设置适当的访问策略密钥存储库，您可以控制谁访问到您的证书。 另一个好处是，您可以管理 Azure 堆栈密钥存储库中的一个位置中的所有证书。

以下是该过程的概述︰

-   您需要以.pfx 格式的证书。

-   创建密钥存储库 （使用一个模板或以下示例脚本）。

-   请确保 EnabledForDeployment 开关已打开。

-   作为一个秘密上载证书。

## <a name="deploying-vms"></a>部署虚拟机

此示例脚本创建密钥存储库中，并将存储在本地目录，到作为一个秘密密钥存储库的.pfx 文件中存储的证书。

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

.Pfx 文件和存储文件内容 base64 编码作为 JSON 对象中读取该脚本的第一部分。 然后 JSON 对象也是 base64 编码。

接下来，创建新的资源组，并创建密钥存储库。 请注意在新建 AzureKeyVault 命令中的最后一个参数-EnabledForDeployment，这为授予访问权限 Azure （特别是对 Microsoft.Compute 的资源提供程序） 来读取机密密钥从保险存储部署。

上次执行的命令只是将 base64 编码 JSON 对象密钥存储库中存储为一个秘密。

以下是上述脚本的示例输出︰

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

现在，我们已准备好部署的虚拟机模板。 请注意从输出 （如在前面的输出以绿色突出显示） 的机密的 URI。

您将需要一个模板位于此处。 （除了通常的 VM 参数） 的感兴趣的参数是电子仓库名称、 保险存储资源组和秘密的 URI。 当然，也可以从 GitHub 上下载并根据需要进行修改。

此 VM 部署时，Azure 将证书注入 VM。
在 Windows 上，.pfx 文件中的证书添加不能导出的私钥。 证书添加到 LocalMachine 证书的位置，与用户提供的证书存储区中。 在 Linux 上，证书文件位于 /var/lib/waagent 目录下，文件名&lt;UppercaseThumbprint&gt;.crt 的 X509 证书文件和&lt;UppercaseThumbprint&gt;.prv 专用密钥。
这两个文件是.pem 格式。

应用程序通常使用指纹查找证书，而不需要修改。

## <a name="retiring-certificates"></a>撤销的证书


在前面的部分中，我们向您展示如何将一个新证书推送到您现有的虚拟机。 但旧证书仍在虚拟机中，不能删除。 为了提高安全性，可以更改旧密码的属性为已禁用，以便即使旧模板试图创建 VM 与旧版本的证书，它将。 下面是您如何设置特定的机密版本被禁用︰

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>结论


使用此新方法时，证书可以分开从 VM 映像或应用程序负载。 因此，我们已删除的暴露的一个点。

证书还可以更新，而无需重新生成虚拟机映像或应用程序部署程序包上载到密钥存储库。 应用程序仍然需要通过提供与此新证书版本新的 URI。

通过将证书从虚拟机或应用程序有效载荷分离开来，我们现在有减少的数目将可以直接访问该证书的人员。

此外，您现在拥有密钥存储库来管理所有证书，包括随着时间的推移部署的所有版本中的一个方便的位置。

## <a name="next-steps"></a>下一步行动

[部署虚拟机使用密钥存储库密码](azure-stack-kv-deploy-vm-with-secret.md)

[允许应用程序访问密钥存储库](azure-stack-kv-sample-app.md)