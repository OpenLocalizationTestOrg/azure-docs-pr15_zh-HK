<properties 
pageTitle="云服务生命周期事件处理 |Microsoft Azure" 
description="了解如何在.NET 中可以使用云服务角色的生命周期方法" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>自定义 Web 或工作人员的角色，在.NET 中的生命周期

当您创建一个辅助角色时，您可以扩展[RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx)类，它提供了您可以重写方法，使您可以对生命周期事件作出响应。 Web 角色对于此类是可选的因此必须使用它来对生命周期事件作出回应。

## <a name="extend-the-roleentrypoint-class"></a>扩展的 RoleEntryPoint 类

[RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx)类包含方法调用的 Azure 时它**启动**、**运行**，或**停止**web 或辅助的角色。 您可以选择重写这些方法以管理角色初始化、 角色关闭序列或角色的执行线程。 

当扩展**RoleEntryPoint**，应该注意的方法中的下列行为︰

-   [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)和[OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)方法返回一个布尔值，这样就可以从这些方法返回**false** 。

     如果您的代码将返回**false**，角色突然终止进程，而无需运行可能有的任何关闭序列。 一般情况下，应避免从**OnStart**方法返回**false** 。
     
-   在**RoleEntryPoint**方法的重载中任何未捕获的异常将被视为未经处理的异常。

     如果生命周期方法之一中发生异常，Azure 将引发[UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx)事件，，然后终止该进程。 您的角色为脱机之后，Azure 将重新启动它。 未经处理的异常发生时，不引发[停止](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx)事件，并不调用**OnStop**方法。

如果您的角色不会启动或正在进行的初始化，忙、 和停止状态之间循环，您的代码可能会引发每次重新启动该角色生命周期事件之一过程中未处理的异常。 在这种情况下，使用[UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx)事件来确定异常的原因并正确地处理。 此外可能从[Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)方法，这将导致重新启动该角色返回您的角色。 有关部署状态的详细信息，请参阅[公共问题的原因角色为回收](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)。

> [AZURE.NOTE] 如果您使用**Microsoft Visual Studio 的 Azure 工具**开发应用程序时，角色项目模板自动扩展**RoleEntryPoint** ， *WebRole.cs*和*WorkerRole.cs*文件中。

## <a name="onstart-method"></a>OnStart 方法

通过 Azure 角色实例联机时称为**OnStart**方法。 OnStart 代码的执行，而角色实例标记为**闲**，没有外部的通信将重定向到它通过负载平衡器。 您可以重写此方法以执行初始化的工作，例如实现事件处理程序和启动[Azure 诊断](cloud-services-how-to-monitor.md)。

如果**OnStart**返回**true**，已成功初始化实例和 Azure 调用**RoleEntryPoint.Run**方法。 如果**OnStart**返回**false**，则角色立即终止而不执行任何已计划的关闭序列。

下面的代码示例演示如何重写**OnStart**方法。 此方法配置和开始诊断显示器，当角色实例启动并设置日志数据传输到存储帐户︰

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop 方法

**OnStop**方法称为角色实例脱机的 Azure 之后以及在进程退出之前。 您可以重写此方法，以调用代码所需的角色实例可以干净地关闭。

> [AZURE.IMPORTANT] 运行**OnStop**方法中的代码已经在有限的时间，完成被调用以外的其他用户启动的关机原因时。 经过此时间后，进程将终止，因此您必须确保在**OnStop**方法可以快速运行该代码或容许未运行直至完成。 **OnStop**方法称为后**停止**事件引发。


## <a name="run-method"></a>运行方法

您可以重写实现角色实例的长时间运行线程的**运行**方法。

重写**方法**不是必需的;默认实现将启动永久休眠的线程。 如果确实重写了**方法**，您的代码应无限期地阻塞。 **Run**方法返回时，如果该角色是自动地回收;换句话说，Azure 引发**停止**事件并调用**OnStop**方法，以便之前角色处于脱机状态，可能会执行关闭序列。


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>实现 web 角色 ASP.NET 生命周期方法

ASP.NET 生命周期方法，以及所提供的**RoleEntryPoint**类，可用于初始化和关闭序列为 web 角色管理。 如果正在移植到 Azure 现有 ASP.NET 应用程序，这可能是出于兼容性目的很有用。 ASP.NET 生命周期方法是从调用**RoleEntryPoint**方法。 **应用程序\_启动** **RoleEntryPoint.OnStart**方法完成后调用方法。 **应用程序\_结束**调用**RoleEntryPoint.OnStop**方法之前调用方法。

## <a name="next-steps"></a>下一步行动
了解如何[创建一个云服务包](cloud-services-model-and-package.md)。