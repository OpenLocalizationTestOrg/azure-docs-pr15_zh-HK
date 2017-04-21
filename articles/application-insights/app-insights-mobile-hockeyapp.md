<properties
    pageTitle="与开发人员分析移动 web 应用程序的性能监视 |Microsoft Azure"
    description="应用程序的性能和使用情况监视的移动应用程序开发人员。 桌面、 web 服务和后端应用程序与 HockeyApp 和应用程序的见解。"
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>HockeyApp 应用程序的见解与移动分析

监视性能和您的试用版测试和部署与[HockeyApp](https://hockeyapp.net/)的移动和桌面应用程序的使用情况。 监视 web 服务和后端应用[Visual Studio 应用程序理解](app-insights-overview.md)与支持。 分析分析在客户端和服务器应用程序中的数据和 Azure 的仪表板中显示的图表与彼此一起。

Microsoft 开发人员分析提供了有关应用程序性能监控的两个解决方案︰

* **HockeyApp 的移动**和桌面应用程序。
 * 分配给所选用户的测试版本。
 * 崩溃分析。
 * 有关使用率分析的自定义遥测。
* **应用程序的 web 站点的见解**和服务，以及后端应用程序。
 * 性能和使用情况指标和警报。
 * 异常报告和诊断跟踪。
 * 包含筛选和相关遥测链接的诊断搜索。

这两种解决方案提供了︰

 * 功能强大的**[分析查询语言](app-insights-analytics.md)**诊断和分析。
 * **[将数据导出](app-insights-export-telemetry.md)**到您自己的存储中。
 * **集成的控制板**显示分析图表和表格。

## <a name="monitor-your-app-components"></a>监视应用程序组件

按照这些页代码中安装 SDK 并开始跟踪您的应用程序中的说明进行操作。

### <a name="web-apps---application-insights"></a>Web 应用程序的应用程序的见解

* [ASP.NET web 应用程序](app-insights-asp-net.md) 
* [Java 的 web 应用程序](app-insights-java-get-started.md)
* [Node.js web 应用程序](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure 的云服务](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>移动应用程序-HockeyApp

* [iOS 应用程序](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X 应用程序](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android 应用程序](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [通用的 Windows 应用程序](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8 和 8.1 应用程序](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Windows Presentation Foundation 应用程序](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

对于 Windows 桌面应用程序，我们建议 HockeyApp。 但是，您还可以[发送遥测从 Windows 应用程序到应用程序的见解](app-insights-windows-desktop.md)。 可能需要执行此操作尝试使用应用程序的见解。


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>分析和 HockeyApp 遥测的导出

您可以调查 HockeyApp 自定义并记录使用分析和连续导出功能的应用程序的见解通过[一座桥梁设置](app-insights-hockeyapp-bridge-app.md)遥测。




