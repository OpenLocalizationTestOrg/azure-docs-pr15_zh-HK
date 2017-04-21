<properties 
    pageTitle="监视使用情况和 Windows 桌面应用程序的性能" 
    description="分析使用情况和性能的 Windows 桌面应用程序与 HockeyApp 和应用程序的见解。" 
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
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>监视使用情况和在 Windows 桌面应用程序的性能

*在预览是应用程序的见解。*

[Visual Studio 应用程序见解](app-insights-overview.md)和[HockeyApp](https://hockeyapp.net)允许您监控您部署的应用程序的使用情况和性能。

> [AZURE.IMPORTANT] 我们建议[HockeyApp](https://hockeyapp.net)分发和监视桌面和设备应用程序。 使用 HockeyApp，您可以管理分发、 现场测试和用户反馈，以及监视使用情况和故障报告。 您还可以[导出并查询您遥测与分析](app-insights-hockeyapp-bridge-app.md)。

> 虽然遥测可以从桌面应用程序发送到应用程序的见解，这是主要用于调试和实验目的。


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>从 Windows 应用程序向应用程序的见解遥测

1. 在[Azure 门户](https://portal.azure.com)，[创建应用程序的见解资源](app-insights-create-new-resource.md)。 对于应用程序类型，选择 ASP.NET 应用程序。
2. 采用检测密钥的副本。 找到您刚刚创建的新资源精要下拉列表中。 
3. 在 Visual Studio 中，编辑的 NuGet 程序包的应用程序项目中，并添加 Microsoft.ApplicationInsights.WindowsServer。 (或者选择 Microsoft.ApplicationInsights，如果您只想裸机 API，不含标准遥测集合模块。)
4. 可以在代码中设置检测项︰

    `TelemetryConfiguration.Active.InstrumentationKey = "`*您的密钥*`";` 

    或在 ApplicationInsights.config （如果您安装了标准的遥测包之一）︰
 
    `<InstrumentationKey>`*您的密钥*`</InstrumentationKey>` 

    如果使用 ApplicationInsights.config，请确保在解决方案资源管理器中的属性设置为**生成操作复制到输出目录的内容 = = 副本**。
5. [使用 API](app-insights-api-custom-events-metrics.md)来发送遥测。
6. 运行您的应用程序，并查看在 Azure 门户中创建的资源的遥测数据。

## <a name="telemetry"></a>代码示例

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>下一步行动

* [创建仪表板](app-insights-dashboards.md)
* [诊断搜索](app-insights-diagnostic-search.md)
* [研究指标](app-insights-metrics-explorer.md)
* [编写分析查询](app-insights-analytics.md)
 
