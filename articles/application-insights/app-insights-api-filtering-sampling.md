<properties 
    pageTitle="过滤和预处理中应用程序的见解 SDK |Microsoft Azure" 
    description="遥测处理器和 SDK 的遥测初始值设定项进行筛选，或将属性添加到数据遥测发送到应用程序的见解门户之前编写。" 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>过滤和预处理遥测应用程序深入 SDK 中

*在预览是应用程序的见解。*

您可以编写和配置插件以自定义如何捕获和处理，然后将其发送到应用程序的见解服务遥测的应用程序的见解 sdk。 

目前，这些功能是可供 ASP.NET SDK。

* [采样](app-insights-sampling.md)可以遥测的量减少而不会影响您的统计信息。 它总是在一起相关数据点，以便可以在诊断问题时它们之间导航。 在门户中，总计数相乘来弥补采样。
* [遥测处理器与筛选](#filtering)允许您选择或修改 SDK 中的遥测之前将其发送到服务器。 例如，可以减少量遥测从机器人排除请求。 但是，筛选是更基本的方法，以降低流量比采样。 它允许您更好地控制传送的但您必须知道它会影响您的统计信息-例如，如果过滤掉所有成功。
* 从您的应用程序，包括遥测从标准模块发送任何遥测[遥测初始值设定项添加属性](#add-properties)。 例如，可以添加计算出的值;或在门户网站中的数据进行筛选依据的版本号。
* [SDK API](app-insights-api-custom-events-metrics.md)用于发送自定义事件和衡量标准。


在开始之前︰

* 在您的应用程序中安装[应用程序的见解 ASP.NET v2 的 SDK](app-insights-asp-net.md) 。 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>筛选︰ ITelemetryProcessor

这种技术可以包含或排除从遥测流的内容的更直接控制。 可以使用它与采样，结合或分开。

若要筛选遥测，您编写遥测处理器和 SDK 中注册它。 所有遥测都经历您的处理器，并可以选择要从流中删除它或添加属性。 这包括遥测从标准模块如 HTTP 请求收集器，收集器依赖项，以及您已经编写了自己的遥测。 您可以例如，筛选出有关从机器人或成功依赖项调用请求的遥测。 

> [AZURE.WARNING] 筛选来自 SDK 的遥测数据使用处理器可以扭曲在门户中，您看到的统计信息并使其难以按照相关的项目。
> 
> 相反，请考虑使用[采样](app-insights-sampling.md)。

### <a name="create-a-telemetry-processor"></a>创建遥测处理器

1. 验证您的项目中应用程序的见解 SDK 版本 2.0.0 或更高版本。 右键单击您在 Visual Studio 解决方案资源管理器中的项目，然后选择管理 NuGet 程序包。 NuGet 程序包管理器，以检查 Microsoft.ApplicationInsights.Web。

1. 若要创建筛选器，实现 ITelemetryProcessor。 这是另一个可扩展性点遥测模块、 遥测初始值设定项和遥测信道等。 

    请注意遥测处理器构建的处理链。 当您实例化遥测处理器时，您将链接传递到链中的下一个处理器。 当遥测数据点传递到处理方法时，它自己的工作，然后调用链中的下一步的遥测处理器。

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. 插入这在 ApplicationInsights.config 中︰ 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

（这是采样过滤器的初始化位置的相同部分）。

通过提供公共类中的命名的属性，可以将字符串值传递.config 文件中。 

> [AZURE.WARNING] 请注意与类型名称和代码中的类和属性名称.config 文件中的任何属性名称相匹配。 如果.config 文件引用了不存在的类型或属性，SDK 以静默方式可能无法发送任何遥测。

 
**或者，**您可以初始化代码中的筛选器。 在合适的初始化类-例如 AppStart 中 Global.asax.cs-将处理器插入链︰

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

以后创建的 TelemetryClients 将使用您的处理器。

### <a name="example-filters"></a>示例筛选器

#### <a name="synthetic-requests"></a>综合的请求

筛选出 bot 和 web 测试。 虽然标准浏览器使您可以筛选出综合来源，但此选项通过 SDK 在对它们进行筛选减少了通信量。

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>身份验证失败

筛选出与"401"响应的请求。 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>筛选出快速远程依赖项的调用

如果您只想诊断速度很慢的呼叫，筛选出快速。 

> [AZURE.NOTE] 这将会有偏差的统计信息，您在门户网站看到。 依赖关系图表看起来象依赖项调用是所有失败。

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>诊断依赖项问题

[此博客](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/)描述了一个项目，以通过自动将定期 ping 命令发送到依赖项诊断依赖项问题。


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>添加属性︰ ITelemetryInitializer

遥测初始值设定项用于定义全局属性会自动发送带有所有遥测;和重写的标准遥测模块的选定的行为。 

例如，Web 包应用程序见解收集有关 HTTP 请求的遥测。 默认情况下，它可以标记为失败任何请求与响应代码 > = 400。 但是，如果想要 400 视为成功，可提供成功设置了遥测初始值设定项。

如果您提供遥测初始值设定项，则将调用 Track*() 方法的任何调用时。 这包括由标准遥测模块调用的方法。 按照惯例，这些模块未设置已设置了初始值设定项的任何属性。 

**定义您的初始值设定项**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**加载您的初始值设定项**

在 ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*或者，*您可以实例化代码，例如在 Global.aspx.cs 中的初始值设定项︰


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[此示例的详细信息，请参阅。](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>JavaScript 遥测初始值设定项

*JavaScript*

你从门户的初始化代码后立即插入一个遥测初始值设定项︰ 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

可在 telemetryItem 上的非自定义属性的摘要，请参阅[数据模型](app-insights-export-data-model.md#lttelemetrytypegt)。

您可以添加任意多个初始值设定项随意。 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor 和 ITelemetryInitializer

遥测处理器和遥测初始值设定项之间的区别是什么？

* 有在用它们可以干一些重叠︰ 两者都可用于遥测中添加属性。
* TelemetryInitializers 在 TelemetryProcessors 之前始终运行。
* TelemetryProcessors 允许您完全替换或丢弃的遥测项。
* TelemetryProcessors 不处理性能计数器遥测。



## <a name="persistence-channel"></a>持久性通道 

如果您的应用程序运行在 internet 连接并不总是可用的或速度缓慢，请考虑使用持久性的通道，而不默认内存中通道。 

默认内存中通道失去任何尚未发送应用程序关闭时的遥测。 尽管您可以使用`Flush()`尝试发送仍然留在缓冲区中的所有数据，它仍然丢失数据如果没有互联网的连接，或者如果应用程序关闭之前传输已完成。

相比之下，持久性通道缓冲遥测在文件中，然后将它发送到门户。 `Flush()`确保数据存储在文件中。 如果应用程序关闭时不发送任何数据，它仍保留在该文件。 应用程序重新启动时，如果没有互联网连接，将然后，发送数据。 只要是必要的直到连接不可用的文件中收集数据。 

### <a name="to-use-the-persistence-channel"></a>若要使用持久性通道

1. 导入 NuGet 程序包[Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3)。
2. 在您的应用程序，在合适的初始化位置中包含此代码︰
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. 使用`telemetryClient.Flush()`之前的应用程序将关闭，以确保数据发送到门户或保存到文件中。

    请注意，Flush() 同步选项可实现持久性的通道中，但其他渠道的异步。

 
对于设备的方案，其中由应用程序生成的事件的数量是相对较小，该连接通常是不可靠，持久性通道进行了优化。 该频道将事件向磁盘写入到可靠的存储第一次，然后尝试将其发送。 

#### <a name="example"></a>示例

让我们假设您想要监视未经处理的异常。 您订阅的`UnhandledException`事件。 在回调中，您可以包括要刷新的调用以确保遥测保持不变。
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

当应用程序关闭时，您将看到在文件`%LocalAppData%\Microsoft\ApplicationInsights\`，其中包含压缩的事件。 
 
下次启动该应用程序中，通道将拿起此文件，如果可以对应用程序的见解提供遥测。

#### <a name="test-example"></a>测试示例

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


在[github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel)是持久性通道的代码。 


## <a name="reference-docs"></a>参考文档

* [API 概述](app-insights-api-custom-events-metrics.md)

* [ASP.NET 的引用](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK 代码

* [ASP.NET 核心 SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>下一步行动


* [搜索事件和日志][diagnostic]
* [取样](app-insights-sampling.md)
* [故障排除][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
