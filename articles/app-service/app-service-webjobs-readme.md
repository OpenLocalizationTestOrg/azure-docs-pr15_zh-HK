<properties
    pageTitle="WebJobs 在 Azure 应用程序服务"
    description="了解如何构建 WebJobs 以运行后台测试、 存储和服务总线之类的服务交互和创建的计划的任务。"
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>在 Azure 应用程序服务使用 WebJobs

有关如何使用 Azure WebJobs 和 Azure WebJobs SDK 文档资源到此文章链接。 Azure WebJobs 提供对[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)作为后台进程运行脚本或程序的简便方法。 您可以上载和运行 cmd，如可执行文件，exe (.NET) 就立刻 ps1，sh，php 上, 一年度，js 和 jar。 这些程序运行作为 WebJobs 的时间表 (cron) 或持续。

WebJobs SDK，可以容易地使用 Azure 存储。 WebJobs SDK 包含绑定和触发系统，它使用 Microsoft Azure 存储 Blob、 队列和表以及服务总线队列。

创建、 部署和管理 WebJobs 是无缝与在 Visual Studio 中的集成工具。 您可以从模板创建 WebJobs、 发布和管理 （运行/停止/显示器/调试） 它们。

Azure 门户中的 WebJobs 仪表板提供了功能强大的管理功能，可以完全控制执行的 WebJobs，包括调用各个函数 WebJobs 中的能力。 仪表板也显示函数的运行时和日志记录输出。

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
