<properties
    pageTitle="使用 Powershell 见解应用程序中设置警报"
    description="自动化应用程序理解，以便获取有关指标更改电子邮件的配置。"
    services="application-insights"
    documentationCenter=""
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

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>使用 PowerShell 见解应用程序中设置警报

您可以自动配置的[警报](app-insights-alerts.md)在[Visual Studio 应用程序理解](app-insights-overview.md)。

此外，可以[设置 webhooks 自动对警报的响应](../monitoring-and-diagnostics/insights-webhooks-alerts.md)。

## <a name="one-time-setup"></a>一次性设置

如果未使用 PowerShell Azure 订阅之前︰

想要运行的脚本的计算机上安装的 Azure Powershell 模块。

 * 安装[Microsoft Web 平台安装程序 (v5 或更高版本)](http://www.microsoft.com/web/downloads/platform.aspx)。
 * 使用它来安装 Microsoft Azure Powershell


## <a name="connect-to-azure"></a>连接到 Azure

启动 Azure PowerShell 并[连接到您的订购](../powershell-install-configure.md)︰

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>获取通知

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>添加通知


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>示例 1

电子邮件通知我如果服务器响应的 HTTP 请求，平均 5 分钟以上，低于 1 秒。 我的见解应用程序资源称为 IceCreamWebApp，，Fabrikam 的资源组。 我是订阅的 Azure 的所有者。

GUID 是订阅 ID （未应用程序检测密钥）。

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>示例 2

我有一个应用程序中使用[TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric)报告规格，名为"salesPerHour"。 发送电子邮件至我的同事，如果"salesPerHour"低于 100，平均超过了 24 小时。

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

同样的规则可以用于报告通过使用另一个跟踪调用，例如 TrackEvent 或 trackPageView 的[测量参数](app-insights-api-custom-events-metrics.md#properties)的度量。

## <a name="metric-names"></a>指标名称

指标名称 | 屏幕名称 | 说明
---|---|---
`basicExceptionBrowser.count`|浏览器异常|在浏览器中引发未捕获的异常数。
`basicExceptionServer.count`|服务器异常|由应用程序引发的未处理异常的计数
`clientPerformance.clientProcess.value`|客户端处理时间|直到加载 DOM 时接收文档的最后一个字节之间的时间。 仍在处理异步请求。
`clientPerformance.networkConnection.value`|页面加载网络连接时间| 连接到网络浏览器所用的时间。 如果缓存，则可以是 0。
`clientPerformance.receiveRequest.value`|接收响应时间| 将请求发送到在开始接收响应的浏览器之间的时间。
`clientPerformance.sendRequest.value`|发送请求的时间| 浏览器发送请求所花费的时间。
`clientPerformance.total.value`|浏览器页面加载时间|从用户请求直到加载 DOM、 样式表、 脚本和图像的时间。
`performanceCounter.available_bytes.value`|可用内存|立即可用的进程或系统使用的物理内存。
`performanceCounter.io_data_bytes_per_sec.value`|处理 IO 率|每秒读取和写入文件、 网络和设备的总字节数。
`performanceCounter.number_of_exceps_thrown_per_sec`|异常率|每秒引发的异常。
`performanceCounter.percentage_processor_time.value`|CPU 的进程|所有进程线程使用处理器执行指令到应用程序进程运行时间的百分比。
`performanceCounter.percentage_processor_total.value`|处理器时间|处理器在非空闲线程所花的时间的百分比。
`performanceCounter.process_private_bytes.value`|进程的专用字节|专门到被监视的应用程序进程分配内存。
`performanceCounter.request_execution_time.value`|ASP.NET 的请求执行时间|最近的请求的执行时间。
`performanceCounter.requests_in_application_queue.value`|ASP.NET 执行队列中的请求|应用程序请求队列的长度。
`performanceCounter.requests_per_sec`|ASP.NET 的请求速率|从 ASP.NET 应用程序每秒的所有请求的速率。
`remoteDependencyFailed.durationMetric.count`|依赖项失败|失败对外部资源的服务器应用程序调用的计数。
`request.duration`|服务器的响应时间|从接收 HTTP 请求并完成发送响应的时间。
`request.rate`|请求速率|应用程序每秒对所有请求的速率。
`requestFailed.count`|失败的请求|计数的 HTTP 请求的响应代码 > = 400
`view.count`|网页视图|客户端对 web 页的用户请求的计数。 综合通信过滤掉。
{您自定义指标名称}|{您度量名称}|度量值报告通过[TrackMetric](app-insights-api-custom-events-metrics.md#track-metric)或[测量参数的跟踪调用](app-insights-api-custom-events-metrics.md#properties)中。

指标由不同遥测模块发送︰

度量组 | 收集器模块
---|---
basicExceptionBrowser，<br/>客户端性能，<br/>视图 | [浏览器的 JavaScript](app-insights-javascript.md)
没有任何 | [性能](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [依赖项](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
请求，<br/>requestFailed|[服务器请求](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

您可以[自动执行对警报的响应](../monitoring-and-diagnostics/insights-webhooks-alerts.md)。 当触发警报时，azure 将调用您选择的 web 地址。

## <a name="see-also"></a>请参见


* [脚本来配置应用程序的见解](app-insights-powershell-script-create-resource.md)
* [从模板创建应用程序的见解和 web 测试资源](app-insights-powershell.md)
* [自动完成离合器 Microsoft Azure 诊断与应用程序的见解](app-insights-powershell-azure-diagnostics.md)
* [自动化对警报的响应](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
