<properties
    pageTitle="Azure 诊断概述 |Microsoft Azure"
    description="用于调试、 性能测量、 监控、 流量分析在云服务、 虚拟机和服务结构的 Azure 的诊断"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Microsoft Azure 诊断是什么


Azure 的诊断是 Azure 使收集诊断数据上部署的应用程序中的功能。 从多个不同源，可以使用诊断程序扩展。 目前支持是 Azure 云服务 Web 和辅助角色，Azure 运行 Microsoft Windows 和服务结构的虚拟机。 其他 Azure 服务有自己单独的诊断程序。

## <a name="data-you-can-collect"></a>您可以收集的数据

Azure 的诊断可以收集以下类型的数据︰

数据源|说明
---|---
性能计数器 | 操作系统和自定义性能计数器
应用程序日志     | 由您的应用程序写入的跟踪消息
Windows 事件日志   | 信息发送到 Windows 事件日志记录系统
.NET 事件源    | 编写使用.NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)类的事件代码
IIS 日志             | 有关 IIS web 站点的信息
基于 ETW 清单   | 事件跟踪 Windows 事件生成的任何进程
故障转储          | 在应用程序崩溃时的进程的状态信息
自定义错误日志    | 由您的应用程序或服务创建日志
Azure 诊断基础结构日志|诊断程序本身有关的信息

Azure 诊断扩展可以将这些数据传输到 Azure 存储帐户，或将其发送到[应用程序的见解](./application-insights/app-insights-cloudservices.md)等服务。 您可以将数据用于调试和故障排除、 测量性能，监视资源使用情况、 流量分析和容量规划，以及审核。


## <a name="versioning"></a>版本控制
请参阅[Azure 诊断程序版本控制历史记录](azure-diagnostics-versioning-history.md)。

## <a name="next-steps"></a>下一步行动
选择想要收集诊断数据上并使用下列文章开始的服务。 有关特定任务的参考使用 Azure 的常规诊断程序链接。

## <a name="web-apps"></a>Web 应用程序
请注意，Web 应用程序不使用 Azure 诊断。 在[Web 应用程序](./app-service-web/web-sites-enable-diagnostic-log.md)中查找相同的信息

## <a name="cloud-services-using-azure-diagnostics"></a>云服务使用 Azure 诊断程序
- 如果使用 Visual Studio，请参阅开始[使用 Visual Studio，以跟踪一个云服务的应用程序](./vs-azure-tools-debug-cloud-services-virtual-machines.md)。 否则，请参见
- [如何监控云服务使用 Azure 诊断程序](./cloud-services/cloud-services-how-to-monitor.md)
- [设置 Azure 云服务应用程序中的诊断程序](./cloud-services/cloud-services-dotnet-diagnostics.md)

有关更高级主题，请参见

- [云服务应用程序的见解与使用 Azure 的诊断](./application-insights/app-insights-cloudservices.md)
- [跟踪诊断 Azure 的云服务应用程序的流程](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [使用 PowerShell 设置在云服务上的诊断](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>虚拟机使用 Azure 诊断程序
- 如果使用 Visual Studio，请参阅开始[使用 Visual Studio 跟踪 Azure 的虚拟机](./vs-azure-tools-debug-cloud-services-virtual-machines.md)。 否则，请参见
- [设置 Azure 诊断 Azure 虚拟机上](./virtual-machines-dotnet-diagnostics.md)

有关更高级主题，请参见

- [使用 PowerShell 诊断在 Azure 的虚拟机设置](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [使用监视和诊断使用 Azure 资源管理器模板创建 Windows 虚拟机](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>使用 Azure 诊断服务结构
在[监视器服务结构应用程序](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)开始。 许多其他服务结构诊断文章可以在左侧导航树中遇到这篇文章。

## <a name="general-azure-diagnostics-articles"></a>一般 Azure 诊断文章
- [Azure 诊断架构配置](https://msdn.microsoft.com/library/azure/mt634524.aspx)-了解如何更改架构文件以收集和路由诊断数据。 请注意，您还可以使用 Visual Studio 更改架构文件。
- [如何在 Azure 存储中存储数据的 Azure 诊断](./cloud-services/cloud-services-dotnet-diagnostics-storage.md)-知道表和 blob 写入诊断数据的位置的名称。
- 了解到[使用 Azure 诊断过程中的性能计数器](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md)。
- 了解到[工艺路线 Azure 的诊断信息到应用程序的见解](./azure-diagnostics-configure-applicationinsights.md)
- 如果您有使用诊断程序启动或在 Azure 存储表中查找数据时遇到问题，请参阅[疑难解答 Azure 诊断](./azure-diagnostics-troubleshooting.md)
