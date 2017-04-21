<properties 
    pageTitle="流式传输日志和控制台" 
    description="流式传输日志和控制台概述" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>流式传输日志和控制台

## <a name="streaming-logs"></a>流式处理日志

**Azure 的门户网站**提供了用于实时查看跟踪事件，从您**的应用程序服务**的应用程序集成流日志查看器。  

设置此功能需要使用几个简单步骤︰

- 在代码中编写跟踪
- 启用**诊断日志**对于您的应用程序的应用程序
- 在**Azure 门户**查看内置**流式传输日志**用户界面的流。

### <a name="how-to-write-traces-in-your-code"></a>如何在代码中编写跟踪 ###

编写跟踪在您的代码非常简单。  在 C# 的本息编写以下代码︰

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Trace 类如何命名空间中的生存之地。

Node.js 应用程序中可以编写此代码，以获得相同的结果︰

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>如何启用和查看流记录
![][BrowseSitesScreenshot]诊断程序将在每个应用程序的基础上启用。 开始浏览到您想要启用此功能的站点。  
  
![][DiagnosticsLogs]从设置菜单向下滚动到**监控**部分，单击**(1) 诊断日志**。 然后**应用程序记录 （文件系统）** **(2) 启用**或**应用程序日志记录 (blob)** **级别**选项允许您更改跟踪，以捕获的严重级别。 如果您只想熟悉的功能，设置**详细**，以确保跟踪语句的所有收集到的级别。

单击**保存**顶部的刀片式服务器，您就可以查看日志。

>[AZURE.NOTE] **严重级别**日志和详细跟踪消耗更多的资源会产生越高。 请确保**严重级别**配置到生产或高流量网站正确的详细级别。 

![][StreamingLogsScreenshot]若要查看**日志流**从 Azure 的门户中，单击**(1) 日志流**还**监视**部分中的设置菜单上。 如果您的应用程序正在写入跟踪语句，然后可以看到它们在**(2) 流式处理登录用户界面**在接近实时。

## <a name="console"></a>控制台
**Azure 的门户**提供了对您的应用程序的控制台访问。 可以浏览应用程序的文件系统和运行 powershell/cmd 脚本。 是由相同的权限集为您运行的应用程序代码执行控制台命令时进行绑定。 访问受保护的目录或者运行需要提升的权限的脚本被阻止。  

![][ConsoleScreenshot]从设置菜单向下的滚动到**开发工具**部分然后单击**控制台 （1）**和**（2） 控制台**用户界面右侧打开。

熟悉**控制台**，请尝试类似的基本命令︰

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
