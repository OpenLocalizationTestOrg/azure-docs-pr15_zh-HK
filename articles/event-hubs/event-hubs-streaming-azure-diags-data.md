<properties
    pageTitle="流式处理 Azure 诊断数据中使用事件集线器的热路径 |Microsoft Azure"
    description="阐释如何配置与事件集线器的端到端，Azure 诊断包括常见方案的指南。"
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>通过使用事件集线器流 Azure 诊断数据热路径中

Azure 诊断程序提供了灵活的方法从云服务的虚拟机 (Vm) 中收集指标和日志，并将结果传输到 Azure 存储。 从开始的 3 月 2016 (SDK 2.9) 时间段内，可以接收到完全自定义的数据源的诊断程序并使用[Azure 事件集线器](https://azure.microsoft.com/services/event-hubs/)将热路径数据传输以秒为单位。

支持的数据类型包括︰

- 事件跟踪 Windows (ETW) 事件
- 性能计数器
- Windows 事件日志
- 应用程序日志
- Azure 诊断基础结构日志

本文介绍了如何采用事件中枢端到端配置 Azure 诊断。 指南还提供了用于下列常见情形︰

- 如何自定义日志和测量数据，得到 sinked 的事件集线器
- 如何更改每个环境中的配置
- 如何查看事件集线器流数据
- 如何解决连接问题  

## <a name="prerequisites"></a>系统必备组件

在 Azure 诊断中接收事件集线器支持云服务、 虚拟机、 虚拟机的扩展集，以及 Visual Studio 在 Azure SDK 2.9 和相应的 Azure 工具启动服务结构。

- Azure 诊断扩展 1.6 （[Azure SDK 的.NET 2.9 或更高版本](https://azure.microsoft.com/downloads/)的目标这默认情况下）
- [Visual Studio 2013年或更高版本](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- 现有的配置的 Azure 诊断程序在应用程序中使用的*.wadcfgx*文件和以下方法之一︰
    - Visual Studio︰[配置诊断 Azure 的云服务和虚拟机](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Windows PowerShell︰[启用在 Azure 云服务使用 PowerShell 的诊断](../cloud-services/cloud-services-diagnostics-powershell.md)
- 每篇文章中，[开始使用事件集线器](./event-hubs-csharp-ephcs-getstarted.md)配置事件集线器命名空间

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>连接到集线器事件接收器的 Azure 诊断

Azure 诊断总是接收器日志和衡量标准，默认情况下，到 Azure 存储帐户。 应用程序可能另外接收到事件集线器通过向*.wadcfgx*文件的**PublicConfig**部分中的**WadCfg**元素中添加新的**接收器**部分。 在 Visual Studio 中， *.wadcfgx*文件存储在以下路径中︰**云服务项目** > **角色** >  **（角色）** > **diagnostics.wadcfgx**文件。

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

在此示例中，事件集线器 URL 设置为事件中心的完全限定命名空间︰ 事件集线器命名空间 +"/"+ 事件中心名称。  

在[Azure 门户](http://go.microsoft.com/fwlink/?LinkID=213885)事件集线器操控板上显示事件集线器 URL。  

可以为任何有效的字符串设置**水池**名称，只要在配置文件一致地使用相同的值。

> [AZURE.NOTE]  可能有其他接收器，如*applicationInsights*在这一节中配置。 Azure 诊断允许如果每个接收器还声明**PrivateConfig**部分中定义一个或多个接收器。  

集线器事件接收器还必须声明和*.wadcfgx*配置文件的**PrivateConfig**部分中定义。

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

`SharedAccessKeyName`值必须匹配的共享访问签名 (SAS) 密钥和**事件集线器**命名空间中定义的策略。 浏览到[Azure 门户](https://manage.windowsazure.com)中的事件集线器仪表板，请单击**配置**选项卡，设置具有*发送*权限的命名策略 (例如，"SendRule")。 **StorageAccount**也是在**PrivateConfig**声明。 没有必要更改此处的值如果他们。 在此示例中，我们将值保留为空，这是下游资产来设置这些值的符号。 例如， *ServiceConfiguration.Cloud.cscfg*的环境配置文件设置相应的环境的名称和密钥。  

> [AZURE.WARNING] 事件集线器 SA 密钥存储在*.wadcfgx*文件中的纯文本。 通常情况下，此项签入到源代码管理或可用作某项资产在生成服务器中，因此应适当地保护它。 我们建议使用 SA 密钥*仅发送*权限，以使恶意用户可以写入事件的中心，但不是欣赏它或对其进行管理。

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>配置 Azure 诊断日志和事件集线器与接收器的标准

如前所述，所有默认和自定义诊断数据，测量数据和日志，即自动 sinked 到 Azure 存储在配置的时间间隔。 事件集线器和任何其他接收器，可以将 sinked 与事件中心的层次结构中指定任何根或叶节点。 这包括 ETW 事件、 性能计数器、 Windows 事件日志和应用程序日志。   

请务必考虑多少数据点应实际转移到事件集线器。 通常情况下，开发人员将低延迟热路径数据必须使用和解释快速传输。 系统监视警报或自动缩放比例规则是示例。 开发人员可能还配置一个备用分析存储区或搜索存储 — 例如，Azure 流分析、 Elasticsearch、 自定义的监视系统或从其他人最喜爱监视系统。

下面是一些示例配置。

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

在以下示例中，接收器应用于父**改**节点层次结构，即所有子**改**将被都发送到事件集线器中。  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

在前面的示例中，该接收器应用到只有三个计数器︰**排队的请求**，**请求被拒绝**，和**处理器时间百分比**。  

下面的示例演示如何开发人员可以限制发送的数据将用于该服务的运行状况的关键指标的量。  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

在此示例中，接收器将应用于日志并筛选仅对错误级别跟踪。

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>部署和更新一个云服务应用程序和诊断程序配置

Visual Studio 提供了要部署的应用程序和事件集线器接收器配置的最简便途径。 若要查看和编辑该文件，在 Visual Studio 中打开*.wadcfgx*文件，编辑，然后将其保存。 该路径是**云服务项目** > **角色** > **（角色）** > **diagnostics.wadcfgx**。  

在这种情况下，所有部署和部署都更新 Visual Studio，Visual Studio Team System，和所有命令或脚本基于 MSBuild 和使用中的对策**/t︰ 发布**目标在打包过程中包含*.wadcfgx* 。 此外，部署和更新将文件部署到 Azure 的虚拟机上使用的适当的 Azure 诊断代理扩展。

在部署的应用程序和 Azure 诊断配置之后，您将立即看到事件中心的仪表板中的活动。 这表明您就可以转到您所选的侦听程序客户端或分析工具中查看的热路径数据。  

在下图中，事件集线器仪表板可以显示的事件集线器正常发送诊断数据后 11 下午的某个时间启动。 这就是当应用程序已部署更新的*.wadcfgx*文件，和接收器的配置是否正确。

![][0]  

> [AZURE.NOTE] 当您到 Azure 诊断配置文件 (.wadcfgx) 进行了更新时，建议，您将更新推送到整个应用程序，以及配置通过使用 Visual Studio 发布或 Windows PowerShell 脚本。  

## <a name="view-hot-path-data"></a>热路径数据视图

如前文所述，有许多用例，侦听和处理事件集线器的数据。

一个简单的方法是创建小型测试控制台应用程序可以侦听事件中心和打印输出流。 您可以将下面的代码中[开始使用事件集线器](./event-hubs-csharp-ephcs-getstarted.md)中的详细说明），在控制台应用程序。  

请注意，该控制台应用程序必须包含[事件处理器主机 Nuget 程序包](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)。  

请记住在**Main**函数中的尖括号中的值替换为您的资源的值。   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>解决事件集线器接收器

- 事件中心不显示传入或传出的事件活动如预期的那样。

    请检查已成功设置事件中心。 *.Wadcfgx*的**PrivateConfig**部分中的所有连接信息必须与在门户中看到所需的资源的值相都匹配。 请确保您在门户中有定义的 SAS 策略 (如示例中的"SendRule") 和*发送*权限被授予。  

- 更新后，事件中心不再显示传入或传出的事件活动。

    首先，请确保事件中心和配置信息是正确的如前面所述。 有时是在部署更新重置**PrivateConfig** 。 建议的解决方法是对项目中的*.wadcfgx*进行的所有更改，然后按一个完整应用程序更新。 如果无法做到这一点，请确保诊断程序更新将包括 SA 密钥完成**PrivateConfig** 。  

- 我已尝试执行这些建议，并且事件中心仍无法正常工作。

    请尝试查找在 Azure 存储表的 Azure 诊断本身包含日志和错误︰ **WADDiagnosticInfrastructureLogsTable**。 一个选项是使用[Azure 存储浏览器](http://www.storageexplorer.com)之类的工具来连接到此存储帐户，查看此表格，并在过去 24 小时中的时间戳添加查询。 该工具可用于导出.csv 文件和应用程序，如 Microsoft Excel 中将其打开。 Excel 可以轻松地搜索电话卡字符串，如**EventHubs**，若要查看错误报告。  

## <a name="next-steps"></a>下一步行动

•[了解事件集线器](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>附录︰ 完成 Azure 诊断程序的配置文件 (.wadcfgx) 示例

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

为了使本示例补充*ServiceConfiguration.Cloud.cscfg*的外观。

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
