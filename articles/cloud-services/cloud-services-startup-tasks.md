<properties 
pageTitle="在 Azure 的云服务中运行启动任务 |Microsoft Azure" 
description="启动任务帮助准备您为您的应用程序的云服务环境。 这教您启动任务的工作方式以及如何使它们" 
services="cloud-services" 
documentationCenter="" 
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



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>如何配置和运行云服务的启动任务

启动任务可用于角色开始之前执行的操作。 您可能想要执行的操作包括安装组件、 注册 COM 组件、 设置注册表项，或启动一个长时间运行的进程。

>[AZURE.NOTE] 启动任务不可适用于虚拟机，只到云服务站点和辅助角色。

## <a name="how-startup-tasks-work"></a>启动任务的工作方式

启动任务是之前您的角色开始， [ServiceDefinition.csdef]文件中定义通过使用[Task]元素[启动]元素内的所采取的措施。 启动任务经常是批处理文件，但它们也可以是控制台应用程序或批处理文件来启动 PowerShell 脚本。

环境变量将信息传递到启动任务，以及本地存储可用于传递信息启动任务。 例如，环境变量可以指定要安装的程序的路径和文件可写入到本地存储区，然后可以稍后阅读您的角色。

您启动的任务可以由**TEMP**环境变量指定的目录记录信息和错误。 在启动任务， **TEMP**环境变量解析为 c:*\\资源\\温度\\[guid]。 [角色名]\\RoleTemp*目录在云上运行时。

启动任务，也可以执行几次之间重新启动。 例如，回收的角色，每次都将运行启动任务和角色的回收可能不会始终包含重新启动。 启动任务的编写应该以一种允许他们多次运行没有问题。

启动任务必须结束与**错误等级**（或退出代码） 的零启动过程完成。 如果以非零的**错误等级**，启动任务结束，不会启动该角色。


## <a name="role-startup-order"></a>角色启动顺序

下面列出了在 Azure 角色启动过程︰

1. 该实例被标记为**起始**，不接收通信。

2. 根据他们的**taskType**特性执行所有启动任务。
    - 同步，执行**简单**任务一次。
    - **背景**和**前景**启动任务异步并行、 启动任务。  
       
    > [AZURE.WARNING] IIS 配置可能不完全在启动过程中，在启动任务阶段使角色特有的数据可能不可用。 启动任务需要特定于角色的数据应使用[Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)。

3. 角色主机进程已启动，并且在 IIS 中创建网站。

4. 调用[Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)方法。

5. 将该实例标记为**就绪**，通信路由到该实例。

6. 调用[Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)方法。


## <a name="example-of-a-startup-task"></a>启动任务的示例

启动任务定义在[ServiceDefinition.csdef]文件中，在**Task**元素中。 **命令行**属性中指定的名称和参数启动批处理文件或控制台命令， **executionContext**属性中指定的启动任务的权限级别和**taskType**属性指定将如何执行该任务。

在此示例中，环境变量， **MyVersionNumber**，启动任务创建并设置为值"**1.0.0.0**"。

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

在以下示例中， **Startup.cmd**批处理文件写入行"的当前版本 1.0.0.0"与 StartupLog.txt 文件是由 TEMP 环境变量指定的目录中。 `EXIT /B 0`该行能够确保启动任务结尾零**错误级别**。

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] 在 Visual Studio 中，您启动批处理文件**复制到输出目录**属性应设置为**始终复制**以确保启动批处理文件将正确地部署到您的项目在 Azure 上 (**approot\\bin** Web 角色和辅助角色的**approot** )。

## <a name="description-of-task-attributes"></a>任务属性说明

下面介绍**Task**元素在[ServiceDefinition.csdef]文件中的属性︰

**在命令行**中的指定用于启动任务的命令行︰

- 命令，使用可选的命令行参数，该命令开始启动任务。
- 通常这是.cmd 或.bat 的批处理文件的文件名。
- 任务是相对于 AppRoot\\Bin 文件夹的部署。 环境变量不确定的路径和文件名的任务展开。 如果环境扩展是必需的您可以创建小型.cmd 脚本中调用启动任务。
- 可以是一个控制台应用程序或批处理文件来启动[PowerShell 脚本](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)。

**executionContext** -指定启动任务的权限级别。 可以限制或提升的权限级别︰

- **有限**  
启动任务与角色相同的特权运行。 当[运行时]元素的**executionContext**属性也是**有限**的时则使用用户的权限。

- **提升**  
启动任务以管理员权限运行。 这样，启动任务来安装程序、 对 IIS 配置进行更改、 执行注册表更改和其他管理员级别的任务，而无需增加本身的角色的权限级别。  

> [AZURE.NOTE] 启动任务的权限级别不需要为角色本身相同。

**taskType** -指定启动任务执行的方式。

- **简单**  
在[ServiceDefinition.csdef]文件中指定的顺序，一次一个同步地执行任务。 当一个**简单**的启动任务结尾零**错误等级**时，将执行下一个**简单**的启动任务。 如果没有更多的**简单**启动任务要执行，然后将启动本身的角色。   

    > [AZURE.NOTE] 如果以非零的**错误等级**，**简单**的任务结束，该实例将被阻止。 随后**简单**启动任务和角色本身，不会启动。

    要确保您的批处理文件结尾零**错误等级**，执行命令`EXIT /B 0`在批处理文件的末尾。

- **背景**  
与角色的启动并行异步执行任务。

- **前景**  
与角色的启动并行异步执行任务。 **前台**和**后台**任务之间的关键区别是**前台**任务可以防止从回收或关闭，直到已结束任务的角色。 **后台**任务没有此限制。

## <a name="environment-variables"></a>环境变量

环境变量是一种方法来将信息传递给启动任务。 例如，可以将路径放到 blob 包含程序若要安装，您的角色时，会使用的端口号或设置为启动任务的控制功能。

有两种类型的启动任务; 环境变量静态环境变量和环境变量，基于[RoleEnvironment]类的成员。 两者都是在[环境]部分中的[ServiceDefinition.csdef]文件，并同时使用的[变量]的元素和**名称**属性。

静态环境变量使用的**值**特性的[变量]元素。 上面的示例中创建环境变量**MyVersionNumber** "**1.0.0.0**"的静态值。 另一个示例是，创建**StagingOrProduction**环境变量，您可以手动设置到"**暂存**"或"**生产**"的值执行不同的启动操作基于**StagingOrProduction**环境变量的值。

基于 RoleEnvironment 类的成员的环境变量不使用的**值**特性的[变量]元素。 相反， [RoleInstanceValue]的子元素，用相应的**XPath**属性值，用于创建一个环境变量，基于[RoleEnvironment]类中的特定成员。 要访问各种[RoleEnvironment]值的**XPath**属性的值可以找到[这里](cloud-services-role-config-xpath.md)。



例如，若要创建一个环境变量，是"**真正**"在云环境中运行时，运行该实例中的仿真计算程序，并显示"**false**"时，使用下列[变量]和[RoleInstanceValue]元素︰

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>下一步行动
了解如何执行一些与云服务的[启动时的常见任务](cloud-services-startup-tasks-common.md)。

[包](cloud-services-model-and-package.md)云服务。  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[任务]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[启动]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[运行时]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[环境]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[变量]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx