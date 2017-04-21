<properties 
    pageTitle="PowerShell 脚本来创建应用程序的见解资源" 
    description="自动创建应用程序建议的资源。" 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShell 脚本来创建应用程序的见解资源

*在预览是应用程序的见解。*

当您想要监视新的应用程序-或新版本的应用程序-与[Visual Studio 应用程序见解](https://azure.microsoft.com/services/application-insights/)时，您设置 Microsoft Azure 中的新资源。 此资源是从您的应用程序的遥测数据是分析并显示在其中。 

您可以通过使用 PowerShell 自动化创建了一个新的资源。

例如，如果正在开发移动设备的应用程序，很可能，在任何时候，有几个已发布的版本的应用程序正在使用您的客户。 不想遥测结果得到混合起来的不同版本。 因此您获得您的生成过程以创建为每个生成新的资源。

## <a name="script-to-create-an-application-insights-resource"></a>要创建应用程序的见解资源脚本

请参见相关的 cmdlet 规范︰

* [新 AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [新 AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShell 脚本*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>如何处理 iKey

每个资源都由其检测参数 (iKey) 标识。 IKey 是该资源创建脚本的输出。 生成脚本应提供在您的应用程序中嵌入到应用程序的见解 SDK iKey。

有两种方法，使 iKey SDK:
  
* 在[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* 或在[初始化代码](app-insights-api-custom-events-metrics.md)中︰ 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>请参见

* [从模板创建应用程序的见解和 web 测试资源](app-insights-powershell.md)
* [设置监视使用 PowerShell Azure 诊断程序](app-insights-powershell-azure-diagnostics.md) 
* [通过使用 PowerShell 设置警报](app-insights-powershell-alerts.md)

 