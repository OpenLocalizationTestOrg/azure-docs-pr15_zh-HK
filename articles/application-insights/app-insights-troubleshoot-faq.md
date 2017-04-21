<properties 
    pageTitle="故障排除和相关应用程序的见解问题" 
    description="某些东西在 Visual Studio 应用程序理解不清楚或不起作用？ 请在此处。" 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>问题-为 ASP.NET 应用程序见解

## <a name="configuration-problems"></a>配置问题

*我有问题设置我︰*

* [.NET 应用程序](app-insights-asp-net-troubleshoot-no-data.md)
* [监视应用程序已在运行](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure 的诊断](app-insights-azure-diagnostics.md)
* [Java 的 web 应用程序](app-insights-java-troubleshoot.md)
* [其他平台](app-insights-platforms.md)

*我从我的服务器获取任何数据*

* [设置防火墙例外](app-insights-ip-addresses.md)
* [ASP.NET 服务器设置](app-insights-monitor-performance-live-website-now.md)
* [设置 Java 服务器](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>可以使用与应用程序见解...？

[请参阅平台][platforms]


## <a name="is-it-free"></a>它是否免费？

* 是的如果您选择[定价层](app-insights-pricing.md)的自由。 您获得大多数功能和数据的慷慨配额。 
* 您必须提供您的信用卡数据注册 Microsoft Azure，但将进行任何费用，除非您使用另一种支付的 Azure 服务，或者显式升级到付费的层。
* 如果您的应用程序发送更多的数据，每月配额比普通层，它将停止记录。 如果发生这种情况，可以选择开始支付，或等到配额将重置在该月的结束。
* 基本的使用情况和会话数据不受配额限制。
* 此外，还有免费 30 天试用版，在此期间您获得免费下载的付费的功能。
* 应用程序的每个资源都有不同的配额，并设置其定价层独立于任何其他人。

#### <a name="what-do-i-get-if-i-pay"></a>如果我付，得什么？

* 较大[的数据每月的配额](https://azure.microsoft.com/pricing/details/application-insights/)。
* 付薪超额继续通过每月的配额收集数据的选项。 如果您的数据需要经过配额，则要承担每 Mb。
* [连续导出](app-insights-export-telemetry.md)。


## <a name="q14"></a>应用程序的见解在项目中修改什么？

详细信息取决于项目的类型。 Web 应用程序︰


+ 将这些文件添加到项目中︰

 + ApplicationInsights.config。 
 + ai.js


+ 安装这些 NuGet 程序包︰

 -  *应用程序理解 API*的核心 API

 -  *对于 Web 应用程序的应用程序的见解 API* -用来从服务器发送遥测

 -  *应用程序理解 API 的 JavaScript 应用程序*-用于从客户端发送遥测

    软件包中包括这些程序集︰

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ 插入到项目︰

 - Web.config

 - packages.config

+ (新项目-如果您[添加到现有项目中的应用程序理解][start]，您需要手动执行此操作。)将代码段插入到客户端和服务器代码来初始化它们与应用程序的见解的资源 id。 例如，在 MVC 应用程序中，代码插入到主页面 Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>如何从旧的 SDK 版本升级？

适合于您的应用程序类型的 sdk，请参阅[发行说明](app-insights-release-notes.md)。 



## <a name="update"></a>如何更改我的项目将数据发送给哪个 Azure 资源？

在解决方案资源管理器中，右击`ApplicationInsights.config`，然后选择**更新应用程序的见解**。 您可以将数据发送到 Azure 中现有的或新的资源。 更新向导会更改中 ApplicationInsights.config，它决定了 SDK 服务器发送数据的位置的检测项。 除非您取消选择"全部更新"，则也会更改 web 页中的显示位置的键。


#### <a name="data"></a>数据保留在门户网站中的多长时间？ 是否安全？

看一看[数据保留和隐私][data]。

## <a name="logging"></a>日志记录

#### <a name="post"></a>如何查看开机自检诊断搜索中的数据？

我们不自动日志发送数据，但您可以使用 TrackTrace 电话︰ 将数据放入消息参数。 尽管不能对其进行筛选，这会有更长的时间大小限制比字符串属性上的限制。 

## <a name="security"></a>安全

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>我的数据是安全的门户？ 保留多长时间？

请参阅[数据保留和隐私][data]。


## <a name="q17"></a>我已启用应用程序的见解中的所有内容吗？

<table border="1">
<tr><th>您应该看到</th><th>如何获取它</th><th>为什么您想要它</th></tr>
<tr><td>可用性图表</td><td><a href="../app-insights-monitor-web-app-availability/">Web 测试</a></td><td>了解您的 web 应用程序已启动</td></tr>
<tr><td>服务器应用程序性能︰ 响应时间...
</td><td><a href="../app-insights-asp-net/">向项目中添加应用程序的见解</a><br/>或 <br/><a href="../app-insights-monitor-performance-live-website-now/">在服务器上安装状态监视器 AI</a> （或编写自己的代码来<a href="../app-insights-api-custom-events-metrics/#track-dependency">跟踪依赖项</a>）</td><td>检测性能问题</td></tr>
<tr><td>依赖项遥测</td><td><a href="../app-insights-monitor-performance-live-website-now/">在服务器上安装 AI 状态监视器</a></td><td>诊断问题数据库或其他外部组件</td></tr>
<tr><td>获取从异常的堆栈跟踪</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">在代码中插入 TrackException 调用</a>（但有些自动报告）</td><td>检测和诊断异常</td></tr>
<tr><td>搜索日志跟踪</td><td><a href="../app-insights-search-diagnostic-logs/">添加日志记录适配器</a></td><td>诊断性能问题的异常</td></tr>
<tr><td>客户端使用基础知识︰ 页面视图、 会话...</td><td><a href="../app-insights-javascript/">在网页中的 JavaScript 初始值设定项</a></td><td>使用情况分析</td></tr>
<tr><td>客户端自定义指标</td><td><a href="../app-insights-api-custom-events-metrics/">在 web 页中调用跟踪</a></td><td>增强的用户体验</td></tr>
<tr><td>服务器的自定义指标</td><td><a href="../app-insights-api-custom-events-metrics/">在服务器代码中调用跟踪</a></td><td>商业智能</td></tr>
</table>


## <a name="automation"></a>自动化

您可以[编写 PowerShell 的脚本](app-insights-powershell.md)来创建和更新应用程序建议的资源。

## <a name="more-answers"></a>更多的答案

* [应用程序的见解论坛](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 