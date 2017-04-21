<properties 
    pageTitle="Azure 的 WebJobs 文档资源" 
    description="学习如何使用 Azure WebJobs 和 Azure WebJobs SDK 的推荐的资源。" 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure 的 WebJobs 文档资源

## <a name="overview"></a>概述

有关如何使用 Azure WebJobs 和 Azure WebJobs SDK 文档资源为本主题链接。 Azure 的 WebJobs，可以方便地作为后台进程[的应用程序服务 web 应用程序、 API 应用程序中或移动应用程序](../app-service/app-service-value-prop-what-is.md)的上下文中运行脚本或程序。 您可以上载和运行 cmd，如可执行文件，exe (.NET) 就立刻 ps1，sh，php 上, 一年度，js 和 jar。 这些程序运行作为 WebJobs 的时间表 (cron) 或持续。

[WebJobs SDK](websites-webjobs-resources.md)的目的是简化 WebJob 可以执行，如图像处理、 队列处理常见任务而编写的代码、 RSS 聚合、 文件维护，并发送电子邮件。 WebJobs SDK 中的内置功能使用 Azure 存储和服务总线、 调度任务和处理错误，以及其他许多常见的方案。 此外，其设计是可扩展的并且没有[打开源存储库的扩展](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview)。 [Azure 的作用](../azure-functions/functions-overview.md)（当前预览） 中基于 WebJobs SDK 配合 C# 脚本，Node.js 和其他语言的版本。 

创建、 部署和管理 WebJobs 是无缝与在 Visual Studio 中的集成工具。 您可以从模板创建 WebJobs、 发布和管理 （运行/停止/显示器/调试） 它们。 

Azure 门户中的 WebJobs 仪表板提供了功能强大的管理功能，可以完全控制执行的 WebJobs，包括调用各个函数 WebJobs 中的能力。 仪表板也显示函数的运行时和日志记录输出。 

##<a name="getstarted"></a>与 WebJobs 和 WebJobs SDK 快速入门

* [Azure WebJobs 简介](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs 超，首先应立即使用它们 ！](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) （特洛伊查寻通过博客文章）。
* [Azure WebJobs 功能](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [WebJobs SDK 是什么](websites-dotnet-webjobs-sdk.md)
* [通过 Microsoft 模式和做法的背景作业指导](/documentation/articles/best-practices-background-jobs/)
* [宣布推出 1.1.0 RTM 的 Microsoft Azure WebJobs SDK](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [开始使用 Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [如何使用 WebJobs SDK Azure 队列存储](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [如何使用 WebJobs SDK Azure blob 存储](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [如何使用 WebJobs SDK Azure 表存储](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [如何使用 WebJobs SDK Azure 服务总线](websites-dotnet-webjobs-sdk-service-bus.md)
* [如何将 WebHooks 与 WebJobs SDK，GitHub、 IFTTT 和 HTTP 提供的示例使用](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK 快速参考 （PDF 下载）](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [在 GitHub 的 WebJobs 设置文档](https://github.com/projectkudu/kudu/wiki/Web-jobs)。
* 视频
    * [WebJobs 和 WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [第 9 频道上的 azure WebJobs 视频系列](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [介绍 WebJobs 的 Visual Studio 工具](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs 工具和远程调试](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure WebJobs 更新 Pranav Rastogi-1.1 版本中的可扩展性](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

请参阅以下各节上[部署 WebJobs](#deploy)和[测试和调试 WebJobs](#debug)。

##<a name="deploy"></a>部署 WebJobs

* [如何使用 Visual Studio 部署 Azure WebJobs](websites-dotnet-deploy-webjobs.md)
* [如何部署使用 Azure 门户 WebJobs](web-sites-create-web-jobs.md)
* [启用 Azure WebJobs 的命令行或连续的传递](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Git 将.NET 控制台应用程序部署到 Azure 使用 WebJobs](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [部署到 Azure 的 F# WebJob](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [作为 Azure Webjobs 部署自定义的服务](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* 视频
    * [介绍 WebJobs 的 Visual Studio 工具](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs 工具和远程调试](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>计划 WebJobs

* [添加 Azure WebJob 对话框](websites-dotnet-deploy-webjobs.md#configure)
* [在 Azure 的门户网站中创建计划的 WebJob](web-sites-create-web-jobs.md#CreateScheduled)
* [挂钩到 WebJob 调度程序作业](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [使用 cron 表达式调度 Azure WebJobs](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [时间安排个别 WebJob 功能，使用 WebJobs SDK TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>测试和调试 WebJobs

* [新的开发人员和 Azure WebJobs 在 Visual Studio 中调试功能](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [查看 WebJobs 的仪表板](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [如何编写使用 WebJobs SDK 的日志并查看仪表板中](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [远程调试 WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [该 blob 的编写者？](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [在云环境中承载交互式代码](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [添加跟踪到 Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [监视、 诊断和解决 Microsoft Azure 存储](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* 视频
    * [WebJobs 工具和远程调试](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>缩放比例 WebJobs

* [扩展 Web 应用程序与 Azure 网站](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure 应用程序服务︰ 构建大规模的就绪的企业 Web 应用程序](https://channel9.msdn.com/Events/Build/2014/3-626)。 介绍 web 应用程序与 WebJobs，包括 WebJobs SDK 的缩放比例。
* 视频
    * [横向扩展 WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>WebJobs 的其他资源

* [通过 Magnus Mårtensson azure WebJobs GA 博客张贴内容](http://magnusmartensson.com/azure-webjobs-ga)
* [在 Azure 应用程序服务上运行 Powershell Web 作业](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [完成您 Azure 触发 WebJobs 时得到通知](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [与 WebJobs 的简单 Web 应用程序备份保留策略](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure 的 Web 应用程序和云服务速度慢，第一次请求](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/)。 演示如何使用 WebJobs 来模拟功能仅适用于标准定价层 AlwaysOn 功能。
* [WebJobs 正常关机](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl)。 对于 WebJobs SDK 正常关机，请参阅[宽容关机](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful)。)
* [自动使用 Azure 的 WebJobs 和 AzCopy 的备份](http://markjbrown.com/azure-webjobs-azcopy/)
* 视频
    * [通过 Magnus Mårtensson azure WebJobs 视频](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [第 9 频道上的 azure WebJobs 视频系列](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>WebJobs SDK 的其他资源

* [WebJobs SDK 发行说明](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK 的源代码](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK 扩展的源代码](https://github.com/Azure/azure-webjobs-sdk-extensions)，使用[扩展性模型的详细的指南](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview)。  
* [WebJobs SDK 脚本源代码](https://github.com/Azure/azure-webjobs-sdk-script/)（用于[Azure 函数](../azure-functions/functions-overview.md)）
* [WebJob 将 FREB 文件上载到 Azure 存储使用 WebJobs SDK](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [宿主在 Azure 之外的 Azure webjobs，从 Azure 记录带来了承载 webjob](http://bypassion.dk/?p=510)
* [构建了 Azure WebJobs 的数据导入工具](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Azure 功能概述](../azure-functions/functions-overview.md)
* 视频
    * [第 9 频道上的 azure WebJobs 视频系列](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>WebJob 的示例应用程序

* [在 GitHub 上的 WebJobs 团队所提供的示例应用程序](https://github.com/azure/azure-webjobs-sdk-samples)
* [与 WebJobs 后端使用 WebJobs SDK 的简单 Azure Web 应用程序](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77)。 演示如何使用计划和事件驱动的 WebJobs。 请参阅博客文章[重建使用 Azure WebJobs SDK SiteMonitR](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs)。

##<a name="blogs"></a>博客

* [Azure 的博客](/blog)
* [Amit 苹果博客](http://blog.amitapple.com/)。 WebJobs (不 SDK) 的重点。
* [Magnus Mårtensson 博客](http://magnusmartensson.com/)

##<a name="gethelp"></a>帮助您使用 WebJobs

* [对于 WebJobs StackOverflow](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow WebJobs sdk](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow Azure 的函数](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure 和 ASP.NET 论坛](http://forums.asp.net/1247.aspx)
* [Azure 应用程序服务 Web 应用程序论坛](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure 的 Web 应用程序用户语音站点](https://feedback.azure.com/forums/169385-websites/)
* [使用 twitter](http://twitter.com/)。 使用 hashtag #AzureWebJobs。
* [报告 WebJobs bug 或问题](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

