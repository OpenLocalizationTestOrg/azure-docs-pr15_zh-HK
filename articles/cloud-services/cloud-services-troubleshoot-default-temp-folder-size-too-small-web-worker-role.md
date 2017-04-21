<properties
   pageTitle="默认 TEMP 文件夹大小是角色太小 |Microsoft Azure"
   description="云服务角色都有一个有限的空间的临时文件夹。 本文提供了一些建议，如何避免磁盘空间不足。"
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>默认 TEMP 文件夹大小是云服务 web/工作人员角色太小

云服务工作人员或 web 角色的默认临时目录具有最大为 100 MB，这可能会在某些时候满。 本文介绍如何避免临时目录空间不足。

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>为什么运行空间不足？

标准的 Windows 环境变量 TEMP 和 TMP 可供您的应用程序中运行的代码。 TEMP 和 TMP 指向单个目录的最大大小是 100 MB。 存储在此目录中的任何数据的整个生命周期的云服务; 不保持不变如果云服务中的角色实例是回收，在清理目录。

## <a name="suggestion-to-fix-the-problem"></a>若要解决该问题的建议

实现以下替代方法之一︰

- 配置本地存储资源，并直接而不是使用 TEMP 或 TMP 中访问它。 若要从应用程序内运行的代码中访问的本地存储资源，调用[RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx)方法。 

- 配置本地存储资源，并指向 TEMP 和 TMP 目录，使其指向本地存储资源的路径。 这种修改应在[RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)方法中执行。

下面的代码示例演示如何修改为 TEMP 和 TMP 从 OnStart 方法中的目标目录︰


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>下一步行动

阅读博客，介绍了[如何提高 Azure Web 角色 ASP.NET 临时文件夹的大小](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx)。

查看更多的[疑难解答文章](/?tag=top-support-issue&product=cloud-services)的云服务。

若要了解如何通过使用 Azure PaaS 计算机诊断数据解决云服务的角色问题，查看[古柯 Williamson 博客系列](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)。
