<properties
    pageTitle="设置应用程序在 Azure 的见解使用 PowerShell |Microsoft Azure"
    description="自动化应用程序理解的管道配置 Azure 诊断。"
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>使用 PowerShell 设置 Azure 的 web 应用程序的应用程序的见解

[Microsoft Azure](https://azure.com)可以[配置为发送 Azure 诊断](app-insights-azure-diagnostics.md) [Visual Studio 应用程序理解](app-insights-overview.md)。 诊断程序将与 Azure 云服务和 Azure 虚拟机。 他们补充从发送应用程序使用应用程序的见解 SDK 中的遥测数据。 作为自动化在 Azure 创建新资源的过程的一部分，您可以配置使用 PowerShell 的诊断程序。

## <a name="azure-template"></a>碧空模板

如果 web 应用程序是 Azure 中并创建使用 Azure 资源管理器模板资源，可以通过将此添加到资源节点配置应用程序的见解︰

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-理解应用程序资源的名称
* `myWebAppName`-使 web 应用程序的 id


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>启用诊断扩展部署云服务的一部分

`New-AzureDeployment` Cmdlet 都有一个参数`ExtensionConfiguration`，其中包含了一系列诊断程序的配置。 可以使用创建这些`New-AzureServiceDiagnosticsExtensionConfig`cmdlet。 例如︰

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>启用诊断扩展现有的云服务上

在现有的服务，使用`Set-AzureServiceDiagnosticsExtension`。

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>获取当前诊断扩展配置

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>删除诊断程序扩展名

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

如果您启用了使用诊断程序扩展名`Set-AzureServiceDiagnosticsExtension`或`New-AzureServiceDiagnosticsExtensionConfig`不使用角色参数，则可以移除扩展使用`Remove-AzureServiceDiagnosticsExtension`不使用角色参数。 如果启用扩展时使用角色参数然后它时，必须同时使用删除扩展。

每个单个角色删除诊断扩展︰

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>请参见

* [监视 Azure 云服务应用程序与应用程序的见解](app-insights-cloudservices.md)
* [Azure 诊断发送到应用程序的见解](app-insights-azure-diagnostics.md)
* [自动化配置警报](app-insights-powershell-alerts.md)

