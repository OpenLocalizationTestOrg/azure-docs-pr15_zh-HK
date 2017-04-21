## <a name="setting-up-powershell-for-resource-manager-templates"></a>为资源管理器模板设置 PowerShell

使用 Azure PowerShell 与资源管理器之前，您需要具有右 Windows PowerShell 和 Azure PowerShell 版本。

### <a name="verify-powershell-versions"></a>验证 PowerShell 版本

请验证您具有 Windows PowerShell 3.0 或 4.0 版本。 若要查找 Windows PowerShell 的版本，请在 Windows PowerShell 命令提示符下键入该命令。

    $PSVersionTable

您将收到下面的信息类型︰

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


检查**PSVersion**的值是 3.0 或 4.0。 如果没有，请参阅[Windows 管理框架 3.0](http://www.microsoft.com/download/details.aspx?id=34595)或[Windows 管理框架 4.0](http://www.microsoft.com/download/details.aspx?id=40855)。

### <a name="set-your-azure-account-and-subscription"></a>设置 Azure 帐户和订阅

如果您没有订阅了 Azure，可以激活您的[MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或符号组成的[免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

使用此命令打开 Azure PowerShell 命令提示符并登录到 Azure。

    Login-AzureRmAccount

如果您有多个 Azure 的订阅，您可以列出 Azure 订阅使用此命令。

    Get-AzureRmSubscription

您将收到下面的信息类型︰

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

可以通过在 Azure PowerShell 命令提示符下运行以下命令来设置当前 Azure 订阅。 引号，包括的所有内容替换 < 和 > 字符，用正确的名称。

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

关于 Azure 预订和帐户的详细信息，请参阅[如何︰ 连接到您的订阅](powershell-install-configure.md#Connect)。
