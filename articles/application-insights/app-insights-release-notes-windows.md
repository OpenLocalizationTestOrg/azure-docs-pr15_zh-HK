<properties 
    pageTitle="发行说明，了解有关 Windows 应用程序的见解" 
    description="Windows 商店 SDK 最新的更新。" 
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
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>发行说明，了解应用程序的见解为 Windows Phone 的 SDK 和存储

应用程序深入 SDK 将遥测有关实时应用程序发送到[应用程序的见解](https://azure.microsoft.com/services/application-insights/)，可以分析其使用情况和性能。


## <a name="version-111"></a>1.1.1 版

### <a name="windows-sdk"></a>Windows SDK

- 使用 Windows Phone 的 Silverlight SDK 时，修复崩溃期间挂起。 更改之后，晚于 ~ 2 秒后对 WindowsAppInitialier.InitializeAsync(...) 的调用发生任何崩溃将保存到磁盘，然后将发送下一次启动该应用程序。 如果在调用后约 2 秒之前发生崩溃，它将被忽略。  
- 对特定版本的核心和 Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3) 中设置的 NuGet 依赖项。   

### <a name="core-sdk"></a>核心 SDK

- 在 github 管理是核心。 [在 github 中](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)找不到的核心 sdk 的未来发行说明

## <a name="version-11"></a>1.1 版

### <a name="core-sdk"></a>核心 SDK

- SDK 现在引入新型遥测```DependencyTelemetry```它包含有关从应用程序的依赖项调用信息
- 新方法```TelemetryClient.TrackDependency```允许从应用程序发送有关依赖项的调用

## <a name="version-100"></a>1.0.0 版

### <a name="windows-app-sdk"></a>Windows 应用程序 SDK

- 新的 Windows 应用程序的初始化工作。 新`WindowsAppInitializer`类的`InitializeAsync()`方法允许引导 SDK 集合的初始化。 此更改允许通过以前的 ApplicationInsights.config 技术更精确的控制和重要的应用程序初始化性能的改进。
- DeveloperMode 将不再自动设置。 若要更改 DeveloperMode 必须在代码中指定的行为。
- NuGet 程序包不再注入 ApplicationInsights.config。 建议您手动添加 NuGet 程序包时使用新的 WindowsAppInitializer。
- 只读取 ApplicationInsights.config `<InstrumentationKey>`，WindowsAppInitializer 设置的首选项被忽略所有其他设置。
- 存储市场会自动收集的 SDK。
- 大量的错误修复、 稳定性和性能增强。

### <a name="core-sdk"></a>核心 SDK

- ApplicationInsights.config 文件不再是 requiered。 和不添加 NuGet 程序包。 可以在代码中完全指定配置。
- NuGet 程序包将不再向解决方案中添加一个目标文件。 这将删除 DeveloperMode 调试生成过程中的自动设置。 DeveloperMode 应在代码中手动设置。

## <a name="version-017"></a>版本为 0.17

### <a name="windows-app-sdk"></a>Windows 应用程序 SDK

- 现在，Windows 应用程序 SDK 支持通用针对 Windows 10 技术预览和 VS 2015 RC 创建的 Windows 应用程序。

### <a name="core-sdk"></a>核心 SDK

- TelemetryClient 默认值来初始化与 InMemoryChannel。
- 新的 API 添加， `TelemetryClient.Flush()`。 此 Flush 方法将触发所有登录到该客户端的遥测即时阻止上载。 这使手动触发之前关闭进程进行上载。
- NuGet 程序包添加.Net 4.5 目标。 此目标中，没有任何外部相关性 （删除 BCL 和 EventSource 依赖项）。

## <a name="version-016"></a>版 0.16 

2015-04-28 预览

- SDK 现在支持 DNX 来启用监视[核心.NET 框架](http://www.dotnetfoundation.org/NETCore5)应用程序的目标平台。
- 实例的```TelemetryClient```不缓存不再检测键。 现在，如果检测项中未设置```TelemetryClient```明确```InstrumentationKey```将返回 null。 它解决了的问题，当您设置```TelemetryConfiguration.Active.InstrumentationKey```后已收集了一些遥测、 遥测模块如依赖项收集器、 web 请求数据集和性能计数器收集器将使用新的检测项。

## <a name="version-015"></a>版 0.15

- 新的属性```Operation.SyntheticSource```现已在```TelemetryContext```。 现在可以将遥测项目标记为"不真实的用户通信流"，并指定如何生成该通信。 示例将此属性设置为可以从您的测试自动化与负载测试流量区分通信量。
- 通道逻辑移到单独的 NuGet 称为 Microsoft.ApplicationInsights.PersistenceChannel。 默认通道现在称为 InMemoryChannel
- 新方法```TelemetryClient.Flush```允许同步刷新缓冲区中的遥测项目

## <a name="version-013"></a>版本为 0.13

没有发行说明，了解可用的旧版本。 
