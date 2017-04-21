<properties 
    pageTitle="启动和停止虚拟机使用 Azure 自动化-PowerShell 流 |Microsoft Azure"
    description="包括运行手册来启动和停止虚拟机经典的 Azure 自动化方案的图形版本。"
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure 的自动化方案的启动和停止虚拟机

此 Azure 自动化方案包括运行手册来启动和停止经典的虚拟机。  可用于此方案如下︰  

- 在您自己的环境中使用而无需修改运行手册。 
- 修改要执行的自定义的功能运行手册。  
- 从另一个 runbook 作为整体解决方案的一部分调用运行手册。 
- 使用运行手册作为教程学习创作概念的 runbook。 

> [AZURE.SELECTOR]
- [图形](automation-solution-startstopvm-graphical.md)
- [PowerShell 工作流](automation-solution-startstopvm-psworkflow.md)

这是这种情况下的 PowerShell 流 runbook 版本。 此外，还可以使用[图形化运行手册](automation-solution-startstopvm-graphical.md)。

## <a name="getting-the-scenario"></a>获取方案

该方案由两个 PowerShell 工作流运行手册，您可以从下面的链接下载。  看到这种情况下，图形化运行手册的链接的[图形版本](automation-solution-startstopvm-graphical.md)。

| Runbook | 链接 | 类型 | 说明 |
|:---|:---|:---|:---|
| 开始-AzureVMs | [启动 Azure 经典的虚拟机](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell 工作流 | 经典的所有虚拟机在都启动 Azure 的 subscriptionor 所有虚拟机使用特定的服务名称。 |
| Stop AzureVMs | [停止 Azure 经典的虚拟机](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell 工作流 | 停止自动帐户中的所有虚拟机或使用特定的服务名称的所有虚拟机。  |


## <a name="installing-and-configuring-the-scenario"></a>安装和配置方案

### <a name="1-install-the-runbooks"></a>1.安装运行手册

在下载后运行手册，您可以导入它们使用[导入 Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)中的过程。

### <a name="2-review-the-description-and-requirements"></a>2.审查的说明和要求
运行手册包含注释的帮助文本，包括说明和所需的资产。  此外可以从这篇文章相同的信息。 

### <a name="3-configure-assets"></a>3.资产配置
运行手册要求以下的资产，您必须创建和填充适当的值。

| 资产类型 | 资产名称 | 说明 |
|:---|:---|:---|:---|
| 凭据 | AzureCredential | 包含有权启动和停止虚拟机在 Azure 订阅帐户的凭据。  或者，您可以**凭据****添加 AzureAccount**活动的参数中指定凭据的另一种资产。 |
| 变量 | AzureSubscriptionId | 包含 Azure 订阅的订阅 ID。 |

## <a name="using-the-scenario"></a>使用方案

### <a name="parameters"></a>参数

运行手册每个具有下列参数。  必须提供所有必需的参数值，并可以选择提供值的其它参数根据您的要求。

| 参数 | 类型 | 必填字段 | 说明 |
|:---|:---|:---|:---|
| 服务名 | 字符串 | 不 | 如果未提供值，则使用该服务名称的所有虚拟机的启动或停止。  如果未提供值，Azure 的订阅中的所有经典虚拟机的启动或停止。 |
| AzureSubscriptionIdAssetName | 字符串 | 不 | 包含[变量的资产](#installing-and-configuring-the-scenario)包含 Azure 订阅的订阅 ID 的名称。  如果不指定值，则使用*AzureSubscriptionId* 。  |
| AzureCredentialAssetName | 字符串 | 不 | 包含[凭据资产](#installing-and-configuring-the-scenario)包含 runbook 使用的凭据名称。  如果不指定值，则使用*AzureCredential* 。  |

### <a name="starting-the-runbooks"></a>开始运行手册

可用于任何一种方法[开始在 Azure 自动化 runbook](automation-starting-a-runbook.md)在这种情况下启动运行手册之一。

下面的示例命令使用 Windows PowerShell 运行**StartAzureVMs**来启动与该服务名称*MyVMService*的所有虚拟机。

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>输出

运行手册将为每个虚拟机，该值指示[输出一条消息，](automation-runbook-output-and-messages.md)指示已成功提交的开始或停止指令。  您可以查找特定字符串的输出来确定每个 runbook 的结果中。  下表中列出了可能的输出字符串。

| Runbook | 条件 | 消息 |
|:---|:---|:---|
| 开始-AzureVMs | 虚拟机已在运行  | MyVM 已在运行 |
| 开始-AzureVMs | 已成功提交的虚拟机的启动请求 | MyVM 已开始 |
| 开始-AzureVMs | 虚拟机启动请求失败  | MyVM 将无法启动 |
| Stop AzureVMs | 虚拟机已停止  | MyVM 已停止 |
| Stop AzureVMs | 停止虚拟机已成功提交的请求 | MyVM 已停止 |
| Stop AzureVMs | 为虚拟机停止请求失败  | MyVM 无法停止 |

例如，下面的代码段，从 runbook 尝试与该服务名称*MyServiceName*开始的所有虚拟机。  如果任何开始请求失败，则可以执行错误时操作。 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>详细的分解

下面是在这种情况下运行手册的细目。  此信息可用于自定义运行手册，或者只是从中了解创作您自己的自动化方案。

### <a name="parameters"></a>参数

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

获取[输入的参数](#using-the-scenario)的值来启动工作流。  如果没有提供资源名称将使用默认名称。

### <a name="output"></a>输出

    # Returns strings with status messages
    [OutputType([String])]

这行声明的 runbook 输出将是一个字符串。  这不是必需的但最佳做法是，当 runbook 用作[子 runbook](automation-child-runbooks.md) ，以便父 runbook 知道期望的输出类型。

### <a name="authentication"></a>身份验证

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

下面的几行设置[凭据](automation-configuring.md#configuring-authentication-to-azure-resources)和 Azure 将用剩下的 runbook 的订阅。
首先我们使用**Get AutomationPSCredential**以获取资产持有用来启动和停止虚拟机在 Azure 订阅的访问凭据。 然后，**添加 AzureAccount**使用此资产设置凭据。  输出被赋予一个虚拟变量，以便它不包括在 runbook 输出。  

使用**Get AutomationVariable**和订阅集与**选择 AzureSubscription**然后检索订阅 id 变量的资产。

### <a name="get-vms"></a>获取虚拟机

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**获得 AzureVM**用于检索 runbook 将使用虚拟机。  如果**站点**输入变量中提供一个值，则将检索虚拟机仅使用该服务名称。  如果**服务名**为空，将检索所有虚拟机。

### <a name="startstop-virtual-machines-and-send-output"></a>启动/停止虚拟机并将输出发送

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

下面的几行单步执行每个虚拟机。  首先检查以查看是否正在运行或停止，这取决于 runbook 虚拟机的**电源状态**。  如果已在目标状态中，然后会将消息发送到输出，并且在 runbook 结束。  如果没有，则**开始 AzureVM**或**停止 AzureVM**用于尝试启动或停止虚拟机存储到一个变量请求的结果。  一条消息，然后发送输出指定要启动或停止的请求是否已成功提交。


## <a name="next-steps"></a>下一步行动

- 若要了解有关使用子运行手册的详细信息，请参阅[Azure 自动化中的子运行手册](automation-child-runbooks.md) 
- Runbook 执行和记录，以帮助解决在学习有关输出消息的详细信息，请参阅[Runbook 输出和 Azure 自动化中的消息](automation-runbook-output-and-messages.md)
