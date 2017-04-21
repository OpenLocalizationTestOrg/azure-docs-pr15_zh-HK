<properties
    pageTitle="启用诊断在 Azure 云服务使用 PowerShell |Microsoft Azure"
    description="了解如何启用云服务使用 PowerShell 诊断"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>启用诊断中使用 PowerShell 的 Azure 云服务

您可以收集诊断数据，应用程序日志，如性能计数器等从云服务使用 Azure 诊断程序扩展。 本文介绍如何为云服务使用 PowerShell 启用 Azure 诊断程序扩展。  这篇文章所需的系统必备组件，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>启用诊断扩展部署云服务的一部分

这种方法的持续集成类型的方案可以作为部署云服务的一部分启用诊断程序扩展的位置很好。 创建新的云服务部署时可以通过*ExtensionConfiguration*参数传入到[新建 AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) cmdlet 启用诊断程序扩展。 *ExtensionConfiguration*参数采用诊断程序配置，可以使用[新建 AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) cmdlet 来创建的数组。

下面的示例演示如何启用云服务与 WebRole 和 WorkerRole 的诊断各有不同的诊断配置。

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

如果诊断程序配置文件使用存储帐户名称所指定的 StorageAccount 元素，然后新建 AzureServiceDiagnosticsExtensionConfig cmdlet 将自动使用该存储帐户。 为此，需要被同一订阅被部署在云服务中存储帐户。

从 Azure SDK 2.6 起由 MSBuild 生成的扩展配置文件发布目标输出将包含基于指定的服务配置文件 (.cscfg) 中的诊断程序配置字符串的存储帐户名。 下面的脚本演示如何分析从发布目标输出的扩展配置文件和部署云服务时配置的每个角色的诊断扩展。

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio 在线云服务的自动化 deploymnts 诊断程序扩展名使用类似的方法。 一个完整的示例，请参阅[发布 AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) 。

如果没有 StorageAccount 诊断程序配置中指定，则需要将 StorageAccountName 参数中传递给该 cmdlet。 如果指定 StorageAccountName 参数，则该 cmdlet 将总是使用指定的参数，而不是诊断程序的配置文件中指定的存储帐户。

如果诊断程序存储帐户处于其他订阅从云服务，则需要显式传递给该 cmdlet 的 StorageAccountName 和 StorageAccountKey 的参数中。 当诊断存储帐户都是在相同的订阅中，cmdlet 可以自动查询和设置注册表项值时启用诊断程序扩展，则不需要 StorageAccountKey 参数。 但是，如果诊断存储帐户处于其他订阅，然后 cmdlet 可能无法自动获取项并需要显式指定通过 StorageAccountKey 参数的键。

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>启用诊断扩展现有的云服务上

可以使用[一组 AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) cmdlet 启用或更新上云服务已在运行的诊断程序配置。


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>获取当前诊断扩展配置
使用[Get AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) cmdlet 来获取当前诊断程序配置为云服务。

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>删除诊断程序扩展名
若要关闭对云服务的诊断程序，您可以使用[删除 AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) cmdlet。

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

如果您启用了使用*集 AzureServiceDiagnosticsExtension*或*新建 AzureServiceDiagnosticsExtensionConfig*不使用*角色*参数，则可以移除扩展不使用*角色*参数使用*删除 AzureServiceDiagnosticsExtension*诊断程序扩展。 如果启用扩展时使用*角色*参数然后它时，必须同时使用删除扩展。

每个单个角色删除诊断扩展︰

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>下一步行动

- 使用 Azure 诊断技术以及其它技术解决问题的其他指南，请参阅[启用 Azure 云服务和虚拟机中的诊断程序](cloud-services-dotnet-diagnostics.md)。
- [诊断程序配置架构](https://msdn.microsoft.com/library/azure/dn782207.aspx)解释诊断扩展各种 xml 配置选项。
- 若要了解如何启用诊断程序扩展的虚拟机，请参阅[创建监视和诊断使用 Azure 资源管理器模板的虚拟 Windows 机器](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
