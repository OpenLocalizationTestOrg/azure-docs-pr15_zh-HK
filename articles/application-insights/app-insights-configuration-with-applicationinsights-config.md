<properties 
    pageTitle="使用 ApplicationInsights.config 或.xml 配置应用程序深入 SDK" 
    description="启用或禁用数据收集模块，并添加性能计数器和其他参数" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>使用 ApplicationInsights.config 或.xml 配置应用程序深入 SDK

应用程序的见解.NET SDK 包括大量的 NuGet 程序包。 [核心包](http://www.nuget.org/packages/Microsoft.ApplicationInsights)发送到应用程序理解的遥测提供 API。 [其他程序包](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights)提供自动从您的应用程序和它的上下文跟踪遥测的遥测_模块_和_初始值设定项_。 通过调整配置文件，您可以启用或禁用遥测模块和初始值设定项，并为其中的某些设置参数。

配置文件命名为`ApplicationInsights.config`或`ApplicationInsights.xml`，具体取决于您的应用程序类型。 它会自动添加到您的项目时[安装大多数版本的 SDK][start]。 此外通过[在 IIS 服务器上的状态监视器]添加到 web 应用程序[redfield]，或当您选择 Appplication 见解[Azure 网站或虚拟机的扩展](app-insights-azure-web-apps.md)。

没有等效的文件来控制[web 页中的 SDK][client]。

本文档介绍的部分您看到在配置文件中，他们如何控制组件的 SDK，并的 NuGet 程序包加载这些组件。

## <a name="telemetry-modules-aspnet"></a>遥测模块 (ASP.NET)

每个遥测模块收集特定类型的数据，并使用核心 API 来发送数据。 通过不同的 NuGet 程序包，还到.config 文件中添加所需的行安装了模块。

在配置文件中的每一个模块没有节点。 若要禁用某个模块，请删除节点或对它进行注释。



### <a name="dependency-tracking"></a>依赖项跟踪

[依赖项跟踪](app-insights-dependencies.md)收集有关您的应用程序对数据库和外部服务和数据库调用的遥测。 若要允许 IIS 服务器中使用此模块，您需要[安装状态监视器][redfield]。 在 Azure 的 web 应用程序或虚拟机，[选择应用程序的见解扩展](app-insights-azure-web-apps.md)中使用它。

您还可以编写您自己依赖项跟踪代码使用[TrackDependency API](app-insights-api-custom-events-metrics.md#track-dependency)。


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet 程序包。

### <a name="performance-collector"></a>性能收集器

例如，CPU、 内存和网络[收集的系统性能计数器](app-insights-performance-counters.md)从 IIS 安装加载。 包括您已经设置了您自己的性能计数器，您可以指定要收集的计数器。

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet 程序包。


### <a name="application-insights-diagnostics-telemetry"></a>应用程序的见解诊断遥测

`DiagnosticsTelemetryModule`报告错误见解应用程序检测代码本身中的。 例如，如果代码不能访问性能计数器或`ITelemetryInitializer`将引发异常。 本模块通过跟踪的跟踪遥测显示在[诊断搜索][diagnostic]。 将诊断数据发送到 dc.services.vsallin.net。
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet 程序包。 如果您只安装该软件包，不自动创建的 ApplicationInsights.config 文件。 

### <a name="developer-mode"></a>开发人员模式运行

`DeveloperModeWithDebuggerAttachedTelemetryModule`强制应用程序理解`TelemetryChannel`将立即数据时将调试器附加到应用程序的过程，一次一个遥测项发送。 当您的应用程序跟踪遥测和在应用程序的见解门户网站出现，这减少了时刻之间的时间的量。 它将导致 CPU 和网络带宽很大的开销。

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [应用程序的见解 Windows 服务器](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)NuGet 程序包

### <a name="web-request-tracking"></a>Web 请求跟踪

报告的 HTTP 请求的[响应时间和结果代码](app-insights-asp-net.md)。 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet 程序包

### <a name="exception-tracking"></a>跟踪异常

`ExceptionTrackingTelemetryModule`跟踪您的 web 应用程序中未处理的异常。 请参阅[故障和异常][exceptions]。

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet 程序包


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-跟踪[观察到的任务异常](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx)。
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-跟踪的辅助角色，windows 服务和控制台应用程序未处理的异常。
* [应用程序的见解 Windows 服务器](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)NuGet 程序包。

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Microsoft.ApplicationInsights 包提供的 sdk 的[核心 API](https://msdn.microsoft.com/library/mt420197.aspx) 。 其他遥测模块使用，并且您还可以[使用它来定义您自己的遥测](app-insights-api-custom-events-metrics.md)。

* 在 ApplicationInsights.config 中没有条目。
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet 程序包。 如果您只是安装此 NuGet，生成没有.config 文件。

## <a name="telemetry-channel"></a>遥测信道

遥测信道管理缓冲和遥测到的应用程序理解服务的传输。 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`是默认信道服务。 它在内存中缓冲数据。
* `Microsoft.ApplicationInsights.PersistenceChannel`是用于控制台应用程序的替代方法。 当您的应用程序将关闭，并将其发送应用程序重新启动时，它可以任何未刷新的数据保存到永久存储区。


## <a name="telemetry-initializers-aspnet"></a>遥测初始值设定项 (ASP.NET)

遥测初始值设定项设置上下文属性与遥测的每一项一起发送。 

您可以[编写您自己的初始值设定项](app-insights-api-filtering-sampling.md#add-properties)来设置上下文属性。

标准的初始值设定项是所有通过设置 Web 或 WindowsServer NuGet 程序包︰


* `AccountIdTelemetryInitializer`设置 AccountId 属性。
* `AuthenticatedUserIdTelemetryInitializer`将 AuthenticatedUserId 属性设置为组通过 JavaScript SDK。
* `AzureRoleEnvironmentTelemetryInitializer`更新`RoleName`和`RoleInstance`的属性`Device`遥测流派为从 Azure 的运行时环境中提取信息的上下文。
* `BuildInfoConfigComponentVersionTelemetryInitializer`更新`Version`属性`Component`上下文从提取的值的所有遥测项`BuildInfo.config`由 MS 生成文件生成的。
* `ClientIpHeaderTelemetryInitializer`更新`Ip`属性`Location`遥测的所有项的上下文基于`X-Forwarded-For`请求的 HTTP 标头。
* `DeviceTelemetryInitializer`更新下面的属性`Device`遥测的所有项的上下文。
 - `Type`设置为"PC"
 - `Id`设置为计算机的域名运行 web 应用程序的位置。
 - `OemName`设置为从提取的值`Win32_ComputerSystem.Manufacturer`字段使用 WMI。
 - `Model`设置为从提取的值`Win32_ComputerSystem.Model`字段使用 WMI。
 - `NetworkType`设置从提取的值为`NetworkInterface`。
 - `Language`设置的名称为`CurrentCulture`。
* `DomainNameRoleInstanceTelemetryInitializer`更新`RoleInstance`属性`Device`域名的 web 应用程序已在运行的计算机的所有遥测项目上下文。
* `OperationNameTelemetryInitializer`更新`Name`属性`RequestTelemetry`和`Name`的属性`Operation`遥测的所有项的上下文基于 HTTP 方法以及 ASP.NET MVC 控制器和调用来处理该请求的操作的名称。
* `OperationIdTelemetryInitializer`或`OperationCorrelationTelemetryInitializer`更新`Operation.Id`遥测的所有项的上下文属性跟踪处理与自动生成请求时`RequestTelemetry.Id`。
* `SessionTelemetryInitializer`更新`Id`属性`Session`上下文从提取的值的所有遥测项`ai_session`生成的 ApplicationInsights JavaScript 检测代码运行在用户的浏览器中的 cookie。 
* `SyntheticTelemetryInitializer`或`SyntheticUserAgentTelemetryInitializer`更新`User`， `Session` ，`Operation`时处理请求源的合成，例如，可用性测试，或者搜索引擎机器人跟踪遥测的所有项的上下文属性。 默认情况下，[测量数据资源管理器中](app-insights-metrics-explorer.md)不显示综合遥测。 

    `<Filters>`设置标识属性的请求。
* `UserAgentTelemetryInitializer`更新`UserAgent`属性`User`遥测的所有项的上下文基于`User-Agent`请求的 HTTP 标头。
* `UserTelemetryInitializer`更新`Id`和`AcquisitionDate`的属性`User`上下文从提取的值的所有遥测项`ai_user`cookie 在用户的浏览器中运行的应用程序理解 JavaScript 检测代码生成。
* `WebTestTelemetryInitializer`设置用户 id、 会话 id 和综合源来自[可用性测试](app-insights-monitor-web-app-availability.md)的 HTTP 请求的属性。
`<Filters>`设置标识属性的请求。

## <a name="telemetry-processors-aspnet"></a>遥测处理器 (ASP.NET)

遥测处理器可以筛选和修改遥测的每一项，只是它在发送前从 SDK 到门户网站。

您可以[编写自己的遥测处理器](app-insights-api-filtering-sampling.md#filtering)。


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>（从 2.0.0-beta3) 的自适应采样遥测处理器

默认情况下会启用此选项。 如果您的应用程序发送大量的遥测，此处理器中删除部分内容。

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

参数提供算法试图实现的目标。 SDK 中的每个实例都独立，因此如果您的服务器是群集的多台计算机，将相应地乘以遥测的实际容量。

[了解更多有关采样](app-insights-sampling.md)。



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>（从 2.0.0-beta1) 固定速率采样遥测处理器

另外，还有标准[采样遥测处理器](app-insights-api-filtering-sampling.md#sampling)（从 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>通道参数 (Java)

这些参数影响 Java SDK 应在如何存储和刷新它收集的遥测数据。

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

可以存储在内存中存储的 SDK 的遥测项的数目。 达到此数目后，刷新遥测-即遥测项发送给服务器应用程序的见解。

-   最小值︰ 1
-   最大值︰ 1000年
-   默认值︰ 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

确定如何通常存储在内存中存储的数据应刷新 （发送到应用程序的见解）。

-   最小值︰ 1
-   最大值︰ 300
-   默认值︰ 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

决定的最大大小，以 mb 为单位分配到本地磁盘上的永久存储区。 此存储用于保持遥测项传输到应用程序的见解终结点失败。 当达到存储大小时，新的遥测项目将被放弃。

-   最小值︰ 1
-   最大值︰ 100
-   默认值︰ 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

这将确定您的数据将显示应用程序的见解资源。 通常您创建单独的资源，与使用单独的密钥，为每个应用程序。

如果您想要设置密钥动态-例如如果要将结果发送给不同的资源的应用程序中可以省略参数从配置文件中，并在代码中设置它。

将键设置为 TelemetryClient 的所有实例，包括标准的遥测模块，在 TelemetryConfiguration.Active 中设置项。 在初始化方法中，如 global.aspx.cs ASP.NET 服务中执行此操作︰

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

如果您只想将一组特定的事件发送给不同的资源，可以为特定的 TelemetryClient 设置密钥︰

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[了解更多关于 API][api]。

获取新密钥，[创建新的资源在应用程序的见解门户][new]。

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

