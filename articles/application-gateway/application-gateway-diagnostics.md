<properties 
   pageTitle="监视应用程序网关访问和性能日志和指标 |Microsoft Azure"
   description="了解如何启用和管理应用程序网关访问和性能日志"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>诊断日志记录和度量应用程序网关

Azure 提供监视资源使用记录和指标的能力

[**日志记录**](#enable-logging-with-powershell)的日志让性能、 访问和其他日志保存或从用于监视目的的资源消耗。

[**指标**](#metrics)的当前应用程序网关具有一个跃点数。 此指标用于衡量应用程序网关的字节 / 秒的吞吐量。

您可以使用 Azure 中不同类型的日志来管理和解决应用程序网关。 某些日志可以通过门户网站，并且可以提取从 Azure blob 存储区，并在不同的工具，如[日志分析](../log-analytics/log-analytics-azure-networking-analytics.md)、 Excel 和 PowerBI 中查看所有日志。 您可以了解更多不同类型的日志从下面的列表︰

- **审核日志︰**可以使用[Azure 审核日志](../monitoring-and-diagnostics/insights-debugging-with-events.md)（以前称为操作日志） 来查看提交到 Azure 的订阅，并且其状态的所有操作。 审核日志默认情况下，将启用，并且可以在 Azure 预览门户中查看。
- **访问日志︰**可以使用此日志可以查看应用程序网关访问模式和分析重要信息包括调用方的 IP，URL 请求、 响应延迟进出返回字节的代码。 访问日志收集每个 300 秒。 该日志包含了应用程序网关的每个实例的一个记录。 实例 Id 属性，可以标识应用程序网关实例。
- **性能日志︰**可以使用此日志以查看应用程序网关实例的性能如何。 此日志捕获基于每个实例包括总请求提供服务，以字节为单位的吞吐量性能信息、 提供服务的请求的总数，失败的请求计数、 正常和不正常的后端实例计数。 性能日志收集每隔 60 秒。
- **防火墙日志︰**可以使用此日志以查看通过使用 web 应用程序防火墙配置应用程序网关检测或防护模式记录的请求。

>[AZURE.WARNING] 日志属性仅适用于资源部署在资源管理器的部署模型。 日志不能用于经典部署模型中的资源。 对于更好地了解两个模型，参考，[了解资源管理器部署和经典的部署](../resource-manager-deployment-model.md)项目。

## <a name="enable-logging-with-powershell"></a>启用日志记录，使用 PowerShell

为每个资源管理器资源自动启用审核日志。 您必须启用访问和性能日志记录开始收集的数据可通过这些日志。 若要启用日志记录，请参阅以下步骤︰ 

1. 请注意您的存储帐户的资源 ID，日志数据的存储位置。 这种形式是︰ /subscriptions/\<subscriptionId\>/resourceGroups/\<资源组名称\>/providers/Microsoft.Storage/storageAccounts/\<存储帐户名称\>。 可以使用在您的订阅任何存储帐户。 您可以使用预览门户找到此信息。

    ![预览门户-应用程序网关诊断](./media/application-gateway-diagnostics/diagnostics1.png)

2. 注意哪些记录到应用程序网关的资源 ID 是启用的。 这种形式是︰ /subscriptions/\<subscriptionId\>/resourceGroups/\<资源组名称\>/providers/Microsoft.Network/applicationGateways/\<应用程序网关名称\>。 您可以使用预览门户找到此信息。

    ![预览门户-应用程序网关诊断](./media/application-gateway-diagnostics/diagnostics2.png)

3. 启用使用以下 powershell cmdlet 的诊断日志记录︰

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] 审核日志都需要一个单独的存储帐户。 存储将用于访问和性能日志记录会导致服务费用。

## <a name="enable-logging-with-azure-portal"></a>启用日志记录与 Azure 门户

### <a name="step-1"></a>第 1 步

导航到所需的资源在 Azure 的门户。 单击**诊断日志**。 如果这是首次配置诊断刀片式服务器类似于下图︰

应用程序网关 3 日志均可用。

- 访问日志
- 性能日志
- 防火墙日志

单击**打开诊断程序**可以开始收集数据。

![诊断程序设置刀片式服务器][1]

### <a name="step-2"></a>第 2 步

在**诊断设置**刀片式服务器，如何诊断日志设置。 在此示例中，日志分析用于存储日志。 在**日志分析功能**来配置您的工作区，请单击**配置**。 事件集线器和存储帐户可以用于保存的诊断日志。

![诊断程序刀片式服务器][2]

### <a name="step-3"></a>第 3 步

选择现有的 OMS 工作区或创建一个新。 此示例使用一个现有。

![oms 的工作区][3]

### <a name="step-4"></a>第 4 步

完成后，确认设置，然后单击**保存**以保存设置。

![确认选择][4]

## <a name="audit-log"></a>审核日志

默认情况下，此日志 （以前称为"操作日志"） 生成通过 Azure。  日志在 Azure 的事件日志存储区中保留 90 天。 通过[查看事件并审核日志](../monitoring-and-diagnostics/insights-debugging-with-events.md)文章阅读中了解有关这些日志的详细信息。

## <a name="access-log"></a>访问日志

如果您已经启用它，才会生成此日志每应用程序网关在前面的步骤中所述的基础。 数据存储在启用日志记录时指定的存储帐户。 每个应用程序网关被登录访问 JSON 格式，如下面的示例所示︰

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>性能日志

在启用了才会生成此日志每应用程序网关在前面的步骤中所述的基础。 数据存储在启用日志记录时指定的存储帐户。 将记录下列数据︰

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>防火墙日志

如果在启用了此日志仅生成每个应用程序网关在前面的步骤中所述。 此日志还要求该 web 应用程序防火墙上使用应用程序网关配置。 数据存储在启用日志记录时指定的存储帐户。 将记录下列数据︰

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>查看和分析审核日志

您可以查看和分析审核日志数据使用以下方法之一︰

- **Azure 的工具︰**通过 Azure PowerShell，Azure 的命令行界面 (CLI)，Azure REST API 或 Azure 预览门户网站的审核日志中检索信息。  对于每个方法的分步指导都在文章中详细[审核操作使用资源管理器中](../resource-group-audit.md)。
- **电源 BI:**如果您尚没有[电源 BI](https://powerbi.microsoft.com/pricing)帐户，您可以免费试用。 使用[Azure 审核日志的内容适用于电源双包](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/)，您可以分析预配置仪表板，您可以使用作为您的数据-，或自定义。

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>查看和分析访问、 性能和防火墙日志

Azure[日志分析](../log-analytics/log-analytics-azure-networking-analytics.md)可收集计数器和事件日志文件从您的 Blob 存储帐户，包括可视化项和功能强大的搜索功能来分析您的日志。

您还可以连接到您的存储帐户并检索访问和性能日志的 JSON 日志项。 一旦您下载的 JSON 文件，可以将它们转换为 CSV 和 Excel、 PowerBI 或任何其他数据的可视化工具中的视图中。

>[AZURE.TIP] 如果您熟悉 Visual Studio 并更改值的常量和变量 C# 中的基本概念，您可以使用可从 Github 的[日志转换器工具](https://github.com/Azure-Samples/networking-dotnet-log-converter)。

## <a name="metrics"></a>指标

度量值是某些 Azure 的资源，您可以在门户中查看性能计数器的功能。 应用程序网关，一个度量标准可以在编写这篇文章的时候是可用。 此统计数据是吞吐量，并可以在门户网站中看到。 导航到应用程序网关并单击**度量值**。  在**可用的指标**部分，以查看值选择的吞吐量。 在下面的图像中，您可以看到可以用于在不同的时间范围内显示的数据的筛选器示例。

若要查看支持的指标的当前列表，请访问[Azure 监视器支持指标](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![公制的视图][5]

## <a name="alert-rules"></a>预警规则

可以根据资源的指标在启动预警规则。 这意味着应用程序网关，警报可以致电 webhook 或电子邮件管理员，如果应用程序网关的吞吐量上方、 下方或在阀值指定的时间段。

下面的示例将引导您完成创建预警规则遭到破坏吞吐量阈值后向管理员发送电子邮件。

### <a name="step-1"></a>第 1 步

单击**添加公制通知**开始。 也可从指标刀片式服务器访问此刀片。

![预警规则刀片式服务器][6]

### <a name="step-2"></a>第 2 步

在**添加规则**刀片式服务器，填写名称，条件，并通知部分后，单击**确定**完成。

4 值、**大于**、**大于或等于**、**小于**，或**小于或等于**允许的**条件**选择器。

领到 6 个小时的期间从 5 分钟的**时间段**选择器，允许。

通过选择**电子邮件所有者、 参与者和读者**电子邮件可以是动态的基于用户有权访问该资源。 否则**其他管理员 email(s)**文本框中可以提供用户的逗号分隔列表。

![添加规则刀片式服务器][7]

如果阈值被破坏，一封电子邮件到达时类似于下图中的一个︰

![破坏阈值的电子邮件][8]

公制警报已创建，并概述的所有预警规则后，将显示警报的列表。

![查看预警规则][9]

若要了解有关通知的详细信息，请访问[接收警报通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

若要了解有关 webhooks 和如何使用警报使用它们的详细信息，请访问[webhook Azure 的公制警报配置](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>下一步行动

- 计数器和事件日志，[日志分析](../log-analytics/log-analytics-azure-networking-analytics.md)与可视化 
- [可视化与电源 BI Azure 审核日志](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx)博客张贴内容。
- [视图分析 Azure 中电源 BI 和更多的审核日志和](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)博客张贴内容。

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png