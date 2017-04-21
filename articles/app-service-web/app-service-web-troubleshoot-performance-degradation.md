<properties
    pageTitle="降低应用程序服务中的 web 应用程序性能 |Microsoft Azure"
    description="这篇文章可以帮助您解决 Azure 应用程序服务中的慢的 web 应用程序性能问题。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="web 应用程序性能，较慢的应用程序，应用程序速度缓慢"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>在 Azure 应用程序服务缓慢的 web 应用程序性能问题的疑难解答

这篇文章可以帮助您解决[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)中的慢的 web 应用程序性能问题。

如果您需要更多的帮助，在这篇文章中的任何一点，您可以联系的 Azure [MSDN Azure 和堆栈溢出论坛](https://azure.microsoft.com/support/forums/)上专家。 或者，您也可以文件 Azure 支持事件。 转到[Azure 支持站点](https://azure.microsoft.com/support/options/)，然后单击**获得支持**。

## <a name="symptom"></a>故障现象

当您浏览 web 应用程序的页面加载速度很慢，有时也是超时。

## <a name="cause"></a>原因

此问题通常被引起应用程序级别的问题，如︰

-   请求需要很长时间
-   使用每个 CPU 高内存的应用程序
-   应用程序崩溃由于出现异常。

## <a name="troubleshooting-steps"></a>故障排除步骤。

疑难解答可以划分为三个不同的任务，按顺序排列︰

1.  [观察和监视应用程序行为](#observe)
2.  [收集数据](#collect)
3.  [缓解问题](#mitigate)

[应用程序服务 Web 应用程序](/services/app-service/web/)提供了各种选项，每一步。

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1.观察和监视应用程序行为

#### <a name="track-service-health"></a>跟踪服务运行状况

Microsoft Azure publicizes 每次出现的服务中断或性能下降时。 在[Azure 门户网站](https://portal.azure.com/)上，您可以跟踪服务的运行状况。 有关详细信息，请参阅[跟踪服务运行状况](../monitoring-and-diagnostics/insights-service-health.md)。

#### <a name="monitor-your-web-app"></a>监视您的 web 应用程序

此选项使您能够找出是否您的应用程序出现的任何问题。 在 web 应用程序的刀片式服务器，请单击**请求和错误**拼贴。 **跃点数**刀片式服务器将显示您可以添加的所有指标。

一些可能需要监视您的 web 应用程序的指标

-   平均内存工作集
-   平均响应时间
-   CPU 时间
-   内存工作集
-   请求

![监视 web 应用程序性能](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

有关详细信息，请参阅︰

-   [在 Azure 应用程序服务的监视 Web 应用程序](web-sites-monitor.md)
-   [接收通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>监视 web 端点状态

如果您正在运行您的 web 应用程序中的**标准**定价层，Web 应用程序允许您监视从 3 地理位置 2 终结点。

终结点监视配置测试响应时间和运行时间的 web Url 的地理分布位置的 web 测试。 测试执行 HTTP GET 操作来确定响应时间和正常运行时间，从每个位置的 web url。 每个已配置的位置运行测试每隔五分钟。

正常运行时间使用 HTTP 响应代码，监视和响应时间以毫秒为单位。 监视测试失败的 HTTP 响应代码是否大于或等于 400 或响应时间超过 30 秒。 终结点会认为如果其监视测试成功从所有指定位置可用。

若要设置它，请参阅[如何︰ 监视 web 端点状态](web-sites-monitor.md#webendpointstatus)。

另请参阅[保持 Azure 网站上加上终结点监视-Stefan Schackow 与](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/)终结点监视视频。

#### <a name="application-performance-monitoring-using-extensions"></a>使用扩展的应用程序性能监控

您可以通过利用_网站扩展_来监视应用程序性能。

每个应用程序服务 web 应用程序提供了允许您利用一组功能强大的工具部署网站扩展为一个可扩展管理终结点。 这些工具涵盖了从源代码编辑器如[Visual Studio 团队服务](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx)如 MySQL 数据库连接到一个 web 应用程序的连接资源的管理工具。

[Azure 应用程序见解](/services/application-insights/)和[新 Relic](/marketplace/partners/newrelic/newrelic/)是两种性能监视站点可用的扩展。 若要使用新的 Relic，您安装的代理在运行时。 若要使用 Azure 应用程序见解，重建与 SDK，您的代码，也可以安装扩展，提供对其他数据访问。 可以使用 SDK 编写代码来监视使用情况和性能的应用程序中更详细地。

若要使用应用程序的见解，请参阅[在 web 应用程序的监视性能](../application-insights/app-insights-web-monitor-performance.md)。

若要使用新的 Relic，请参阅[在 Azure 上新 Relic 应用程序性能管理](../store-new-relic-cloud-services-dotnet-application-performance-management.md)。

<a name="collect" />
### <a name="2-collect-data"></a>2.收集数据

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>启用 web 应用程序的诊断日志记录

Web 应用程序环境提供 web 服务器和 web 应用程序的日志记录信息的诊断的功能。 这些逻辑上可分为 web 服务器诊断程序和应用程序诊断。

##### <a name="web-server-diagnostics"></a>Web 服务器诊断

您可以启用或禁用以下类型的日志︰

-   **详细错误日志记录**的详细的错误消息指示故障 （状态代码 400 或更高版本） 的 HTTP 状态代码。 这可能包含可帮助您确定服务器返回的错误代码的原因的信息。
-   **失败请求跟踪**-失败的请求，其中包括用于处理请求和每个组件中所用的时间的 IIS 组件的跟踪的详细信息。 这很有用，如果您正试图提高 web 应用程序性能或找出什么原因导致一个特定的 HTTP 错误。
-   **Web 服务器日志记录**-使用 W3C 扩展的日志文件格式的 HTTP 事务有关的信息。 在确定总体的 web 应用程序指标，例如处理的请求或来自特定 IP 地址是多少请求的数目时，这非常有用。

##### <a name="application-diagnostics"></a>应用程序诊断

应用程序诊断程序可让您获取生成 web 应用程序的信息。 ASP.NET 应用程序可以使用`System.Diagnostics.Trace`类在应用程序的诊断日志记录的信息。

有关如何配置应用程序以用于日志记录的详细说明，请参阅[启用在 Azure 应用程序服务 web 应用程序的诊断日志记录](web-sites-enable-diagnostic-log.md)。

#### <a name="use-remote-profiling"></a>使用远程分析

在 Azure 应用程序服务中，Web 应用程序和 API 的应用程序，WebJobs 可远程分析。 如果您的进程正在运行速度比预期速度慢或 HTTP 请求的反应会比正常高和进程的 CPU 使用率也很高，可以远程分析您的过程和获取 CPU 采样调用堆栈，以分析流程活动和热的代码路径。

有关详细信息，请参阅[在 Azure 应用程序服务的远程分析支持](/blog/remote-profiling-support-in-azure-app-service)。


#### <a name="use-the-azure-app-service-support-portal"></a>使用 Azure 应用程序服务支持部门门户网站

Web 应用程序为您提供与您的 web 应用程序通过查看 HTTP 日志、 事件日志、 进程转储和更多相关的问题进行故障排除的能力。 您可以访问所有这些信息使用在我们支持部门门户网站**http://&lt;应用程序名称 >.scm.azurewebsites.net/Support**

Azure 应用程序服务支持门户网站可为您提供三个单独的选项卡，以支持常见的故障排除方案的三个步骤︰

1.  注意当前的行为
2.  通过收集诊断信息并运行内置分析器分析
3.  减少

如果现在发生此问题，请单击**分析** > **诊断程序** > **现在诊断**，为您创建诊断会话会收集 HTTP 记录，事件查看器日志，内存转储、 PHP 错误日志和 PHP 处理报表。

一旦收集数据，还对数据进行分析并为您提供的 HTML 报告。

如果您想要下载的数据，默认情况下，它会存储在 D:\home\data\DaaS 文件夹中。

在 Azure 应用程序服务支持门户网站上的详细信息，请参阅[Azure 网站支持网站扩展到新的更新](/blog/new-updates-to-support-site-extension-for-azure-websites)。

#### <a name="use-the-kudu-debug-console"></a>使用 Kudu 调试控制台

Web 应用程序附带了一个调试控制台，可用于调试、 探索、 上传文件，以及用于获取有关您的环境信息的 JSON 终结点。 这被称为_Kudu 控制台_或_SCM 仪表板_的 web 应用程序。

您可以通过转到以下链接访问此仪表板**https://&lt;应用程序名称 >.scm.azurewebsites.net/**。

Kudu 提供的事项包括︰

-   您的应用程序的环境设置
-   日志流
-   诊断转储
-   调试的控制台在其中运行 Powershell cmdlet 和基本的 DOS 命令。


Kudu 的另一个有用的功能是，在万一您的应用程序已引发第一次机会异常，则可以使用 Kudu 和 SysInternals 工具 Procdump 创建内存转储。 这些内存转储进程的快照，通常可以帮助您解决您的 web 应用程序的更复杂的问题。

Kudu 中可用功能的详细信息，请参阅[您应了解有关的 Azure 网站团队服务工具](/blog/windows-azure-websites-online-tools-you-should-know-about/)。

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

 ![重新启动 web 应用程序来解决性能问题](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

您还可以管理您的 web 应用程序使用 Azure Powershell。 有关详细信息，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。
