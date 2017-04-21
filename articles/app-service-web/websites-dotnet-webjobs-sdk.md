<properties 
    pageTitle="Azure WebJobs SDK 是什么" 
    description="Azure WebJobs SDK 简介。 介绍了 SDK 是什么，它是有用的并且代码示例的典型方案。" 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Azure WebJobs SDK 是什么

## <a id="overview"></a>概述

本文介绍 WebJobs SDK，回顾它是有用的并概述了如何在代码中使用它的一些常见情况。

[WebJobs](websites-webjobs-resources.md)是 Azure 应用程序服务，使您可以在 web 应用程序、 API 应用程序中或移动应用程序相同的上下文中运行程序或脚本的功能。 [WebJobs SDK](websites-webjobs-resources.md)的目的是简化 WebJob 可以执行，如图像处理、 队列处理常见任务而编写的代码、 RSS 聚合、 文件维护，并发送电子邮件。 WebJobs SDK 中的内置功能使用 Azure 存储和服务总线、 调度任务和处理错误，以及其他许多常见的方案。 此外，它已设计为可扩展。 [WebJobs SDK 是开放源代码](https://github.com/Azure/azure-webjobs-sdk/)，并且没有[打开源存储库的扩展](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview)。

WebJobs SDK 中包含以下组件︰

* **NuGet 程序包**。 NuGet 程序包添加到 Visual Studio 的控制台应用程序项目提供一个框架，您的代码使用由修饰 WebJobs SDK 的属性与方法。
  
* **仪表板**。 WebJobs SDK 的一部分包括在 Azure 应用程序服务，并提供丰富的监控和诊断使用 NuGet 程序包的程序。 您不需要编写代码来使用这些监控和诊断功能。

## <a id="scenarios"></a>方案

以下是一些您可以更轻松地处理与 Azure WebJobs SDK 的典型方案︰

* 图像处理或其他大量占用 CPU 的工作。 一种常见功能是网站的能够上传图像或视频。 通常，您想要操作的内容上载，但并不希望让用户等待时做到这一点。

* 队列处理。 对于 web 前端与后端服务进行通信的常用方法是使用队列。 当该网站需要完成的工作时，它将推送到一个队列上的消息。 后端服务消息从队列中提取并执行工作。 可以将队列用于图像处理︰ 例如，用户上载多个文件后，放在要领取由后端处理的队列消息的文件的名称。 或者，可以使用队列来提高站点的响应速度。 例如，而不是直接写入 SQL 数据库，写入队列，告诉的用户您完成时，，让后端服务句柄高延迟关系数据库工作。 对图像进行处理的队列的示例，请参见[WebJobs SDK 入门教程](websites-dotnet-webjobs-sdk-get-started.md)。

* RSS 聚合。 如果您有网站维护的 RSS 源列表，可以拉所有中源的后台进程的文章中。

* 文件维护，如聚合或清除日志文件。  可能有几个站点或您想要在其上运行分析作业合并单独的时间跨度为正在创建的日志文件。 或者，您可能想要计划任务运行每周清理旧的日志文件。

* 到 Azure 表的入口。 您可能具有存储文件和 blob，并且想要分析它们，并将数据存储在表中。 入口函数可以编写大量行 （在某些情况下的百万） 和 WebJobs SDK 使其可以很容易地实现此功能。 SDK 还提供了实时监视的进度指示器，如写入表中的行数。

* 您想要运行一个后台线程，例如[发送电子邮件](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure)在其他长时间运行任务。 

* 要按照日程安排，例如执行备份操作每晚运行任何任务。

在许多情况下，您可以扩展 web 应用程序可以同时运行多个 WebJobs 的多个虚拟机上运行。 在某些情况下这可能导致相同的数据得到处理多次，但这不是一个问题，当您使用内置的队列、 斑点和服务总线的 WebJobs sdk 的触发器。 SDK 可确保您的函数为每个消息或 blob 只能一次处理。

WebJobs SDK 还便于处理通用错误处理方案。 您可以设置警报以函数将失败，并且您可以设置超时，以便函数将自动取消，如果不在指定的时间限制内完成时发送通知。

## <a id="code"></a>代码示例

用于处理使用 Azure 存储中的典型任务的代码非常简单。 在控制台应用程序的`Main`方法创建`JobHost`坐标对方法的调用，您编写的对象。 WebJobs SDK 框架知道何时调用您的方法和参数值，使用基于您在其中使用的 WebJobs SDK 特性。 该 SDK 提供了指定条件所导致的函数被调用的*触发器*和*联编*程序指定方法参数和获取信息的方式。

例如， [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)属性会导致在队列中，收到一条消息，如果消息格式的字节数组或一个自定义类型 JSON，此消息不自动反序列化时要调用的函数。 [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)属性触发进程，只要在 Azure 存储帐户中创建新的斑点。

下面是一个简单的程序，轮询队列并创建为每个队列消息接收到 blob:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

`JobHost`对象是一组背景功能的容器。 `JobHost`对象监视功能，监视的事件触发，并触发事件发生时执行的函数。 调用`JobHost`方法，以指示您是否希望在当前线程或者是后台线程上运行的容器过程。 在示例中，`RunAndBlock`方法流程连续运行当前线程上。

因为`ProcessQueueMessage`在此示例中的方法具有`QueueTrigger`特性，该触发器的功能是新的队列消息的接收。 `JobHost`对象监视新队列上的消息指定的队列 (如本示例中的"webjobsqueue")，然后找到一个对象，当它调用`ProcessQueueMessage`。 

`QueueTrigger`特性绑定`inputText`队列消息的值的参数。 和`Blob`特性绑定`TextWriter`对 blob 名为"blobname"在名为"容器名称"容器中的对象。  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

该函数使用这些参数将 blob 写入队列消息的值︰

        writer.WriteLine(inputText);

WebJobs SDK 的触发器和联编程序功能大大简化了您必须编写的代码。 低级别的代码需要处理队列，blob 或文件，或启动计划的任务，是由 WebJobs SDK 框架为您完成的。 例如，框架创建队列不存在，打开的队列、 读取队列的消息，删除队列消息时处理完毕、 创建 blob 容器不存在，但，写入 blob，依次类推。

下面的代码示例显示了各种触发器在一个 WebJob: `QueueTrigger`， `FileTrigger`， `WebHookTrigger`，和`ErrorTrigger`。 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>时间安排

`TimerTrigger`特性使您能够对触发器函数按日程安排运行。 您可以通过使用 SDK WebJobs WebJob 的 Azure 或安排各个函数整体计划 WebJob `TimerTrigger`。 下面是一个代码示例。

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

多个代码示例，请参阅在 GitHub.com 上的[TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) azure webjobs sdk 扩展存储库中。

## <a name="extensibility"></a>可扩展性

你不限于内置功能-WebJobs SDK 使您可以编写自定义的触发器和联编程序。  例如，您可以编写触发器用于缓存事件和定期的计划。 [打开源存储库中](https://github.com/Azure/azure-webjobs-sdk-extensions)包含一个[详细介绍 WebJobs SDK 可扩展性的指南](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview)和示例代码，以帮助您开始编写您自己的联编程序和触发器。

## <a id="workerrole"></a>使用外部 WebJobs WebJobs SDK

使用的程序-WebJobs SDK 是一个标准的控制台应用程序，并且可以运行任意位置并不需要运行 WebJob。 您可以在您的开发计算机上本地测试程序，在生产环境中可以运行它在云服务工作者角色或 Windows 服务如果您更喜欢那些环境之一。 

但是，仪表板是仅作为 Azure 应用程序服务 web 应用程序扩展。 如果您想要运行在 WebJob 之外，而且仍使用仪表板，您可以配置 web 应用程序使用相同的存储帐户 WebJobs SDK 仪表板连接字符串引用和 web 应用程序的 WebJobs 仪表板然后将显示有关从其他位置运行的程序的函数执行的数据。 通过使用 URL https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions，可以获得到仪表板。 有关详细信息，请参阅[获取本地开发 WebJobs sdk 的仪表板](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx)，但请注意博客文章显示旧的连接字符串的名称。 

## <a id="nostorage"></a>仪表板的功能

即使您不使用 WebJobs SDK 触发器或活页夹，WebJobs SDK 提供了几个优点︰

* 您可以调用函数的仪表板。
* 您可以重放从仪表板的功能。
* 您可以链接到特定的 WebJob （应用程序日志，使用 Console.Out、 Console.Error、 跟踪等编写的） 或链接到该特定函数的调用生成它们的仪表板中查看日志 (日志写入使用`TextWriter`SDK 将传递给该函数作为参数的对象)。 

有关详细信息，请参阅[如何手动调用一个函数](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual)，以及[如何编写日志](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>下一步行动

有关 WebJobs SDK 的详细信息，请参阅[Azure WebJobs 推荐资源](http://go.microsoft.com/fwlink/?linkid=390226)。

WebJobs SDK 的新增强功能的信息，请参阅[发行说明](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)。
 
