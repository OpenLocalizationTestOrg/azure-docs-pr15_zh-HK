<properties
    pageTitle="解决 502 错误的网关，503 服务不可用错误 |Microsoft Azure"
    description="502 错误的网关和 Azure 应用程序服务中承载 web 应用程序中的 503 服务不可用错误的疑难解答。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 错误的网关，503 服务不可用、 503 错误、 错误 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>"502 错误网关"和"503 服务不可用"Azure 的 web 应用程序中的 HTTP 错误的疑难解答

"502 错误网关"和"503 服务不可用"都是寄宿在[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)web 应用程序中的常见错误。 这篇文章可以帮助您解决这些错误。

如果您需要更多的帮助，在这篇文章中的任何一点，您可以联系的 Azure [MSDN Azure 和堆栈溢出论坛](https://azure.microsoft.com/support/forums/)上专家。 或者，您也可以文件 Azure 支持事件。 转到[Azure 支持站点](https://azure.microsoft.com/support/options/)，然后单击**获得支持**。

## <a name="symptom"></a>故障现象

当您浏览到 web 应用程序时，它将返回 HTTP"502 错误网关"错误或 HTTP"503 服务不可用"错误。

## <a name="cause"></a>原因

此问题通常被引起应用程序级别的问题，如︰

-   请求需要很长时间
-   使用每个 CPU 高内存的应用程序
-   应用程序崩溃由于出现异常。

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>疑难解答步骤来解决"502 错误网关"和"503 服务不可用"错误

疑难解答可以划分为三个不同的任务，按顺序排列︰

1.  [观察和监视应用程序行为](#observe)
2.  [收集数据](#collect)
3.  [缓解问题](#mitigate)

[应用程序服务 Web 应用程序](/services/app-service/web/)提供了各种选项，每一步。

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1.观察和监视应用程序行为

####    <a name="track-service-health"></a>跟踪服务运行状况

Microsoft Azure publicizes 每次出现的服务中断或性能下降时。 在[Azure 门户网站](https://portal.azure.com/)上，您可以跟踪服务的运行状况。 有关详细信息，请参阅[跟踪服务运行状况](../monitoring-and-diagnostics/insights-service-health.md)。

####    <a name="monitor-your-web-app"></a>监视您的 web 应用程序

此选项使您能够找出是否您的应用程序出现的任何问题。 在 web 应用程序的刀片式服务器，请单击**请求和错误**拼贴。 **跃点数**刀片式服务器将显示您可以添加的所有指标。

一些可能需要监视您的 web 应用程序的指标

-   平均内存工作集
-   平均响应时间
-   CPU 时间
-   内存工作集
-   请求

![监视 web 应用程序解决 HTTP 错误 502 错误的网关和 503 服务不可用](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

有关详细信息，请参阅︰

-   [在 Azure 应用程序服务的监视 Web 应用程序](web-sites-monitor.md)
-   [接收通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2.收集数据

####    <a name="use-the-azure-app-service-support-portal"></a>使用 Azure 应用程序服务支持部门门户网站

Web 应用程序为您提供与您的 web 应用程序通过查看 HTTP 日志、 事件日志、 进程转储和更多相关的问题进行故障排除的能力。 您可以访问所有这些信息使用在我们支持部门门户网站**http://&lt;应用程序名称 >.scm.azurewebsites.net/Support**

Azure 应用程序服务支持门户网站可为您提供三个单独的选项卡，以支持常见的故障排除方案的三个步骤︰

1.  注意当前的行为
2.  通过收集诊断信息并运行内置分析器分析
3.  减少

如果现在发生此问题，请单击**分析** > **诊断程序** > **现在诊断**，为您创建诊断会话会收集 HTTP 记录，事件查看器日志，内存转储、 PHP 错误日志和 PHP 处理报表。

一旦收集数据，还对数据进行分析并为您提供的 HTML 报告。

如果您想要下载的数据，默认情况下，它会存储在 D:\home\data\DaaS 文件夹中。

在 Azure 应用程序服务支持门户网站上的详细信息，请参阅[Azure 网站支持网站扩展到新的更新](/blog/new-updates-to-support-site-extension-for-azure-websites)。

####    <a name="use-the-kudu-debug-console"></a>使用 Kudu 调试控制台

Web 应用程序附带了一个调试控制台，可用于调试、 探索、 上传文件，以及用于获取有关您的环境信息的 JSON 终结点。 这被称为_Kudu 控制台_或_SCM 仪表板_的 web 应用程序。

您可以通过转到以下链接访问此仪表板**https://&lt;应用程序名称 >.scm.azurewebsites.net/**。

Kudu 提供的事项包括︰

-   您的应用程序的环境设置
-   日志流
-   诊断转储
-   调试的控制台在其中运行 Powershell cmdlet 和基本的 DOS 命令。


Kudu 的另一个有用的功能是，在万一您的应用程序已引发第一次机会异常，则可以使用 Kudu 和 SysInternals 工具 Procdump 创建内存转储。 这些内存转储进程的快照，通常可以帮助您解决您的 web 应用程序的更复杂的问题。

在 Kudu 中可用功能的详细信息，请参阅[Azure 网站在线工具，您应了解有关](/blog/windows-azure-websites-online-tools-you-should-know-about/)。

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3.缓解问题

####    <a name="scale-the-web-app"></a>扩展该 web 应用程序

Azure 应用程序服务，在更高的性能和吞吐量，可以调整的比例在运行您的应用程序。 Web 应用程序向上扩展包括两个相关的操作︰ 更改到一个更高的定价层、 应用程序服务计划和配置某些设置已切换到更高的定价层后。

扩展的详细信息，请参阅[缩放在 Azure 应用程序服务 web 应用程序](web-sites-scale.md)。

此外，您可以选择多个实例上运行您的应用程序。 这不仅为您提供更多的处理能力，而且还提供了一定的容错能力。 如果该进程在一个实例上断开，另一个实例仍将继续为请求提供服务。

您可以设置手动或自动缩放。

####    <a name="use-autoheal"></a>使用 AutoHeal

AutoHeal 回收应用程序基于您选择 （类似于配置更改、 请求、 基于内存的限制或执行一个请求所需的时间） 设置的辅助进程。 大多数情况下，回收过程是从问题中恢复的最快方法。 虽然您始终可以重新启动该 web 应用程序从 Azure 门户中直接，AutoHeal 将自动为您代劳。 所有您需要做是在为您的 web 应用程序根 web.config 中添加一些必要的触发器。 请注意，这些设置将以相同的方式工作，即使您的应用程序不是一个.Net。

有关详细信息，请参阅[自动修复 Azure 网站](/blog/auto-healing-windows-azure-web-sites/)。


####    <a name="restart-the-web-app"></a>重新启动 web 应用程序

这通常是最简单的方法来恢复从一次性的问题。 在[Azure 门户](https://portal.azure.com/)，在 web 应用程序的刀片，可以停止或重新启动您的应用程序的选项。

 ![重新启动应用程序以解决 HTTP 错误 502 错误的网关和 503 服务不可用](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

您还可以管理您的 web 应用程序使用 Azure Powershell。 有关详细信息，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。
