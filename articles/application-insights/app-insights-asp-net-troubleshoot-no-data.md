<properties 
    pageTitle="没有数据的.net 应用程序理解的疑难解答" 
    description="看不到在 Visual Studio 应用程序理解的数据？ 请在此处。" 
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
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>没有数据的.net 应用程序理解的疑难解答

## <a name="some-of-my-telemetry-is-missing"></a>一些我遥测将丢失

*在应用程序的见解，只看到我的应用程序生成的事件的一小部分。*

* 如果始终如一地看到相同的分数，可能是由于自适应[采样](app-insights-sampling.md)。 若要确认这一点，打开搜索 （通过概述刀片式服务器） 并查看请求或其他事件的实例。 在属性部分的底部单击"..."以获取完整的属性的详细信息。 如果请求数 > 1，然后取样是在操作中。 
* 否则，很可能，您所碰到的[数据速率限制](app-insights-pricing.md#limits-summary)定价计划。 每分钟应用这些限制。

## <a name="no-data-from-my-server"></a>从我的服务器的任何数据

*我在我的 web 服务器上安装我的应用程序，现在看不到任何从它的遥测。它从事我开发的计算机的确定。*

* 可能是防火墙问题。 [设置防火墙例外，以便应用程序将数据发送的见解](app-insights-ip-addresses.md)。

*我在我监视现有应用程序的 web 服务器上[安装状态监视器](app-insights-monitor-performance-live-website-now.md)。我没有看到任何结果。*

* 请参阅[疑难解答状态监视器](app-insights-monitor-performance-live-website-now.md#troubleshooting)。 


## <a name="q01"></a>在 Visual Studio 中添加应用程序理解的选项

*当我在 Visual Studio 中，创建一个新项目或右键单击解决方案资源管理器中的现有项目，看不到任何应用程序的见解选项。*

+ 由工具支持并不是所有类型的.NET 项目。 支持 web 和 WCF 项目。 对于其他项目类型，如桌面或服务应用程序，您仍可以[手动添加到您的项目应用程序深入 SDK](app-insights-windows-desktop.md)。
+ 请确保您拥有[3 或更高版本的 Visual Studio 2013年更新](http://go.microsoft.com/fwlink/?LinkId=397827)。 它配备预安装应用程序理解工具。
+ 选择**工具****扩展和更新**并检查**应用程序的见解工具**已安装并启用。 如果是这样，请单击**更新**以查看是否有可用更新。
+ 打开新建项目对话框中，选择 ASP.NET Web 应用程序。 如果您看到的应用程序的见解选项，然后安装这些工具。 如果没有，请尝试卸载然后重新安装应用程序理解工具。


## <a name="q02"></a>未能添加应用程序的见解

*在创建新的 web 项目时或当我尝试到现有项目中添加应用程序的见解，我看到一条错误消息。*

可能的原因是︰

* 通信与应用程序的见解门户失败;或
* 某些问题 Azure 帐户;
* 只具有[读取访问权限的订阅或您已尝试创建新的资源组](app-insights-resources-roles-access-control.md)。

修复︰

+ 请检查您的右 Azure 帐户提供的登录凭据。 
+ 在浏览器中，请检查您有对[Azure 门户](https://portal.azure.com)的访问。 打开设置，查看是否有任何限制。
+ [向现有项目中添加应用程序见解](app-insights-asp-net.md)︰ 在解决方案资源管理器中，右键单击项目，然后选择"添加应用程序的见解"。
+ 如果它仍然没有工作，按照要在门户网站中添加资源，然后向项目中添加 SDK 的[手动过程](app-insights-windows-services.md)。 

## <a name="emptykey"></a>获取错误"检测键不能为空"

看起来好像已安装应用程序的见解或也许记录适配器时出现问题。

在解决方案资源管理器中，右击`ApplicationInsights.config`，然后选择**配置应用程序的见解**。 您将获得一个对话，邀请您登录到 Azure 并且必须要么创建应用程序建议的资源，或重新使用现有。


##<a name="NuGetBuild"></a>"NuGet 程序包缺少"我生成服务器上

*当我在我的开发计算机上进行调试，但 NuGet 错误获得有关生成服务器，一切生成确定。*

请[NuGet 程序包还原](http://docs.nuget.org/Consume/Package-Restore)和[自动包还原](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore)，参阅。

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>丢失的菜单命令从 Visual Studio 中打开应用程序的见解

*当用鼠标右键单击解决方案资源管理器项目时，我看不到任何应用程序理解的命令，或看不到了打开的应用程序理解的命令。*

可能的原因是︰

* 如果您手动创建应用程序理解资源或项目的应用程序理解工具不支持的类型。
* 在您的 Visual Studio 中禁用应用程序理解工具。
* 您的 Visual Studio 会早于 2013年更新 3。

修复︰

* 请确保您的 Visual Studio 版本是 2013年 3 或更高版本的更新。
* 选择**工具****扩展和更新**并检查**应用程序的见解工具**已安装并启用。 如果是这样，请单击**更新**以查看是否有可用更新。
* 右击解决方案资源管理器中的项目。 如果您看到该命令**配置应用程序的见解**，使用它连接到应用程序的见解服务中资源的项目。


否则，您的项目类型不直接支持的应用程序理解工具。 若要查看您遥测，登录到[Azure 的门户](https://portal.azure.com)，在左侧的导航栏中，选择应用程序的见解并选择您的应用程序。

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>拒绝访问从 Visual Studio 中打开应用程序的见解

*打开应用程序见解菜单命令可将我带到 Azure 的门户，但收到访问被拒绝错误。*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Microsoft 登录您上次使用您默认的浏览器上不具有访问[资源时应用程序的见解已被添加到此应用程序中创建的](app-insights-asp-net.md)。 有两个可能的原因︰ 

* 您有多个 Microsoft 帐户-也许工作和 Microsoft 个人帐户？ 登录您上次使用您默认的浏览器上是为一个不同的帐户不具有访问权限[添加到项目的应用程序理解](app-insights-asp-net.md)。 

 * 修复︰ 单击您的名称前右侧的浏览器窗口并注销。 然后使用具有访问权限的帐户登录。 然后在左侧的导航栏上，单击应用程序的见解，并选择您的应用程序。

* 其他人添加到项目中，应用程序的见解，他们忘了让您在其中创建[到该资源组的访问权限](app-insights-resources-roles-access-control.md)。 

 * 修复︰ 如果他们使用组织的帐户，它们可以将您添加到团队;或者他们可以授予个别访问到资源组。



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>资产上找不到从 Visual Studio 中打开应用程序的见解

*打开应用程序见解菜单命令可将我带到 Azure 的门户，但是却资产找不到错误。*

可能的原因是︰

* 您的应用程序的应用程序理解资源已被删除;或
* 检测项时设置或更改 ApplicationInsights.config 中通过直接，而不是更新项目文件对其进行编辑。 

在 ApplicationInsights.config 控件中，以便发送遥测指令插入键。 项目文件中的行控制当您在 Visual Studio 中使用该命令打开的资源。 

修复︰

* 在解决方案资源管理器中右键单击项目并选择应用程序的见解，配置应用程序的见解。 在对话框中，您可以选择将遥测发送到现有的资源，或新建一个。 或者︰
* 直接打开的资源。 登录到[Azure 的门户](https://portal.azure.com)，在左侧的导航栏上，单击应用程序的见解，然后选择您的应用程序。



## <a name="where-do-i-find-my-telemetry"></a>在哪里找到我遥测？

*登录[Microsoft Azure 门户](https://portal.azure.com)，然后我查看 Azure 主仪表板。所以哪里可以找到我的应用程序理解数据？*

* 在左侧的导航栏上，单击应用程序的见解，然后您的应用程序的名称。 如果您那里没有任何项目，您需要[添加或配置应用程序在 web 项目中的见解](app-insights-asp-net.md)。

    您将看到一些摘要图表。 您可以单击浏览它们以查看更多详细信息。

* 在 Visual Studio 中，调试您的应用程序时，请单击应用程序的见解按钮。


## <a name="q03"></a>无服务器数据 （或根本没有数据）

*我运行我的应用程序，然后在 Microsoft Azure 中打开应用程序深入服务，但所有的图表显示了解如何收集...或不配置。* 或者，*只，页面视图模式下，用户数据，但没有服务器数据。*

+ 在 Visual Studio (F5) 在调试模式下运行应用程序。 使用以生成一些遥测的应用。 您可以看到在 Visual Studio 的输出窗口中记录的事件的复选。 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ 在应用程序的见解门户中，打开[诊断搜索](app-insights-diagnostic-search.md)。 数据通常出现在此处第一次。
+ 单击刷新按钮。 刀片式服务器定期刷新本身，但您也可以手动执行它。 刷新间隔的长度较大的时间范围。
+ 检查检测密钥匹配。 在主刀片式服务器为您的应用程序在应用程序的见解门户中，在**精要**下拉列表，查看**检测键**。 然后在 Visual Studio 项目中打开 ApplicationInsights.config，找到`<instrumentationkey>`。 检查两个键相等。 如果不是这样︰
 + 在门户中，单击应用程序的见解，并与正确的键，则查找应用程序资源或
 + 在 Visual Studio 解决方案资源管理器，右键单击项目并选择应用程序的见解，配置。 重置应用程序发送到相应的资源的遥测。
 + 如果找不到匹配项，则检查您正在使用相同的登录凭据在 Visual Studio 中的门户。

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ 在[Microsoft Azure 主仪表板](https://portal.azure.com)中，查看服务运行状况图。 如果有一些警告的迹象，应该等到他们返回到确定然后关闭并重新打开您的应用程序理解应用程序刀片式服务器。
+ 此外检查[我们状态的博客](http://blogs.msdn.com/b/applicationinsights-status/)。
+ 没有为[服务器端 SDK](app-insights-api-custom-events-metrics.md)可能更改中的检测项编写任何代码`TelemetryClient`实例或在`TelemetryContext`？ 也没有编写可能筛选[筛选器或采样配置](app-insights-api-filtering-sampling.md)出太多吗？
+ 如果您编辑 ApplicationInsights.config，请仔细检查[TelemetryInitializers 和 TelemetryProcessors](app-insights-api-filtering-sampling.md)的配置。 错误地命名类型或参数可能导致 SDK 发送任何数据。

## <a name="q04"></a>在页面视图中，浏览器中，使用任何数据

*在页面视图加载时，或在浏览器或使用刀片，我看到服务器的响应时间和服务器请求图表中的数据，但没有数据。*

数据来自于网页中的脚本。 

+ 如果现有的 web 项目，[您必须手动添加脚本](app-insights-javascript.md)中添加应用程序的见解。
+ 请确保 Internet Explorer 在兼容模式下显示您的站点不。
+ 使用浏览器的调试功能 （f12 键在某些浏览器，然后选择网络） 来验证数据发送到`dc.services.visualstudio.com`。

## <a name="no-dependency-or-exception-data"></a>任何依赖项或异常数据

请参阅[依赖项遥测](app-insights-asp-net-dependencies.md)和[异常遥测](app-insights-asp-net-exceptions.md)。

## <a name="no-performance-data"></a>没有性能数据

性能数据 (CPU、 IO 率等) 可用于[Java web 服务](app-insights-java-collectd.md)、 [Windows 桌面应用程序](app-insights-windows-desktop.md)、 [IIS web 应用程序和服务安装状态监视器](app-insights-monitor-performance-live-website-now.md)、 和[Azure 云服务](app-insights-azure.md)。 您可以在设置服务器找到它。

它不适用于 Azure 网站。

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>由于应用程序发布到我的服务器 （服务器） 数据

+ 请检查您实际复制所有 Microsoft。 到服务器，以及 Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll ApplicationInsights Dll
+ 在防火墙中，您可能必须[打开某些 TCP 端口](app-insights-ip-addresses.md#data-access-api)。
+ 如果您必须使用代理发送外出时的企业网络，在 Web.config 中设置[请参见](https://msdn.microsoft.com/library/aa903360.aspx)
+ Windows Server 2008︰ 确保已安装以下更新︰ [KB2468871](https://support.microsoft.com/kb/2468871)， [KB2533523](https://support.microsoft.com/kb/2533523)， [KB2600217](https://support.microsoft.com/kb/2600217)。


## <a name="i-used-to-see-data-but-it-has-stopped"></a>我用来查看数据，但它已停止

* 检查[状态的博客](http://blogs.msdn.com/b/applicationinsights-status/)。
* 您点击您每月的配额的数据点了吗？ 打开设置/配额和价格来看一看。 如果是这样，可以升级您的计划，或支付额外的容量。 请参阅[定价方案](https://azure.microsoft.com/pricing/details/application-insights/)。


## <a name="i-dont-see-all-the-data-im-expecting"></a>我看不到我所需要的所有数据

如果您正在使用的 ASP.NET 版本 2.0.0-beta3 或更高版本应用程序的见解 SDK 应用程序发送大量的数据，[自适应采样](app-insights-sampling.md)功能可能运行和发送只有您遥测的百分比。 

您可以禁用它，但这不建议。 采样被设计以便正确传输相关的遥测，用于诊断目的。 

## <a name="wrong-geographical-data-in-user-telemetry"></a>在用户遥测中地理数据错误

城市、 地区和国家的维度都源自 IP 地址，并不总是准确。

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>"找不到方法"异常在 Azure 云服务运行

未生成.NET 4.6 吗？ 4.6 不自动支持 Azure 云服务的角色中。 在运行您的应用程序之前，[安装在每个角色的 4.6](../cloud-services/cloud-services-dotnet-install-dotnet.md) 。

## <a name="still-not-working"></a>仍无法正常工作。.

* [应用程序的见解论坛](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

