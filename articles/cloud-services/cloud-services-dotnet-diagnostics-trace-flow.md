<properties
    pageTitle="跟踪在云服务应用程序中使用 Azure 诊断程序流 |Microsoft Azure"
    description="将跟踪消息添加到 Azure 应用程序，以帮助调试，测量性能、 监控、 流量分析等。"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>跟踪诊断 Azure 的云服务应用程序的流程

跟踪功能有助于您运行时监视其执行情况的应用程序。 [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx)、 [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)和[System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx)类可以用于记录日志、 文本文件或其他设备以供日后分析中有关错误和应用程序执行的信息。 有关中的跟踪的详细信息，请参阅[跟踪和检测应用程序](https://msdn.microsoft.com/library/zs6s4h68.aspx)。


## <a name="use-trace-statements-and-trace-switches"></a>使用跟踪语句和跟踪开关

通过将[DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx)添加到应用程序配置和应用程序代码中无需调用 System.Diagnostics.Trace 或 System.Diagnostics.Debug 云服务应用程序中实现跟踪。 使用配置文件*app.config*辅助角色和*web.config*的 web 角色。 当您创建新的托管的服务使用 Visual Studio 模板时，Azure 诊断程序将自动添加到项目中，DiagnosticMonitorTraceListener 添加到的角色添加相应的配置文件。

在放置跟踪语句的信息，请参阅[方法︰ 将跟踪语句添加到应用程序代码](https://msdn.microsoft.com/library/zd83saa2.aspx)。

通过在代码中放置[跟踪开关](https://msdn.microsoft.com/library/3at424ac.aspx)，您可以控制是否进行跟踪以及它是如何广泛。 这允许您监控您的应用程序在生产环境中的状态。 这是在使用多台计算机上运行的多个组件的业务应用程序中尤其重要。 有关详细信息，请参阅[如何︰ 配置跟踪开关](https://msdn.microsoft.com/library/t06xyy08.aspx)。

## <a name="configure-the-trace-listener-in-an-azure-application"></a>在 Azure 应用程序中配置跟踪侦听器

跟踪，调试和 TraceSource，需要您设置"侦听器"用于收集和记录发送的消息。 侦听程序收集、 存储和路由跟踪消息。 他们将跟踪输出定向到适当的目标，如日志、 窗口或文本文件。 Azure 诊断使用[DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx)类。

完成以下过程之前，您必须初始化 Azure 诊断显示器。 若要执行此操作，请参阅[启用 Microsoft Azure 中的诊断程序](cloud-services-dotnet-diagnostics.md)。

请注意，是否您使用的 Visual Studio 提供的模板，侦听器配置自动为您添加。


### <a name="add-a-trace-listener"></a>添加跟踪侦听器

1. 打开您的角色的 web.config 或 app.config 文件。
2. 将以下代码添加到文件中。 更改的版本属性若要使用所引用的程序集的版本号。 程序集版本不一定更改每个 Azure SDK 版本，除非它的更新。

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] 请确保您具有对 Microsoft.WindowsAzure.Diagnostics 程序集的项目引用。 更新的版本号上面的 xml 中引用的 Microsoft.WindowsAzure.Diagnostics 程序集的版本匹配。

3. 保存配置文件。

有关监听器的详细信息，请参阅[跟踪侦听器](https://msdn.microsoft.com/library/4y5y10s7.aspx)。

完成添加侦听器的步骤后，您可以向代码添加跟踪语句。


### <a name="to-add-trace-statement-to-your-code"></a>若要将跟踪语句添加到代码中

1. 打开您的应用程序的源文件。 例如，<RoleName>的工作角色或 web 角色的.cs 文件。
2. 添加以下如果尚未添加 using 语句︰
    ```
        using System.Diagnostics;
    ```
3. 添加跟踪语句所需用来获取有关您的应用程序的状态信息。 可以使用多种方法来设置格式的 Trace 语句输出。 有关详细信息，请参阅[方法︰ 将跟踪语句添加到应用程序代码](https://msdn.microsoft.com/library/zd83saa2.aspx)。
4. 保存的源文件。
