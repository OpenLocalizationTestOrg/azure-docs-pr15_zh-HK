<properties
    pageTitle="任务相关性在 Azure 批 |Microsoft Azure"
    description="创建任务依赖于用于 MapReduce 样式及类似的大数据处理其他任务的成功完成在 Azure 批处理工作负载。"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>在 Azure 批处理任务相关性

如果您想要处理，Azure 批处理任务相关性功能是非常适合︰

- MapReduce 样式在云环境中的工作负荷。
- 其数据处理任务都可以表示为有向非循环图 (DAG) 的作业。
- 其他作业的下游任务取决于上游任务的输出。

批处理任务相关性允许您创建在计算节点上仅在一个或多个其他任务成功完成后执行计划的任务。 例如，可以创建将呈现与单独的并行任务的 3D 影片的每个帧的作业。 最终任务 —"合并任务"-合并呈现帧到整部电影只后的所有框架已成功地都呈现。

您可以创建任务依赖于其他任务在一对一或一对多的关系。 您甚至可以创建区域相关性任务位置取决于一组特定的任务 Id 范围内的任务的成功完成。 可以组合这些三种基本方案，若要创建多对多关系。

## <a name="task-dependencies-with-batch-net"></a>使用批处理.NET 的任务相关性

在本文中，我们将讨论如何使用[批处理.NET]配置任务相关性[net_msdn]库。 我们先向您展示如何[启用任务依赖项](#enable-task-dependencies)作业，然后演示如何[配置具有依赖关系的任务](#create-dependent-tasks)。 最后，我们讨论批支持[依赖项方案](#dependency-scenarios)。

## <a name="enable-task-dependencies"></a>使任务依赖项

批处理应用程序中使用任务相关性，您必须首先告诉批服务作业使用任务相关性。 批次.NET 中启用它在您的[CloudJob] [net_cloudjob]通过设置其[UsesTaskDependencies] [net_usestaskdependencies]属性`true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

在前面的代码段中，"batchClient"是[BatchClient]的实例[net_batchclient]类。

## <a name="create-dependent-tasks"></a>创建相关任务

若要创建任务依赖于一个或多个其他任务的成功完成，您告诉批次，任务"依赖于"其他任务。 在批处理.NET 配置[CloudTask][net_cloudtask]。[取决于][net_dependson] [TaskDependencies]的实例的属性[net_taskdependencies]类︰

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

此代码段将被安排与"雨"和"Sun"的 Id 任务已成功完成后，才在计算节点上运行的"鲜花"的 id 创建的任务。

 > [AZURE.NOTE] 任务将被视为完成时处于**已完成**状态和其**退出代码**为`0`。 在批处理.NET 中，这意味着[CloudTask][net_cloudtask]。[状态][net_taskstate]属性值为`Completed`，CloudTask [TaskExecutionInformation][net_taskexecutioninformation]。[退出代码][net_exitcode]属性值是`0`。

## <a name="dependency-scenarios"></a>依赖项方案

有可以在 Azure 批处理中使用的三种基本任务依赖项方案︰ 一对一，一对多和任务 ID 范围依赖项。 可以组合这些提供的第四个方案中，多为多。

 方案&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 示例 | |
 :-------------------: | ------------------- | -------------------
 [一对一](#one-to-one) | *taskB*取决于*taskA* <p/> *taskB*之前，不能计划执行*taskA*已成功完成 | ![图︰ 一对一的任务相关性][1]
 [一到多](#one-to-many) | *taskC*取决于*taskA*和*taskB* <p/> *taskC* *taskA*和*taskB*已经成功完成之前不会执行计划 | ![图︰ 一到多任务相关性][2]
 [任务 ID 范围](#task-id-range) | *taskD*取决于一系列任务 <p/> *taskD*与 Id *1*到*10*的任务已成功完成之前不会执行计划 | ![图︰ 任务 id 范围依赖项][3]

>[AZURE.TIP] 您可以创建**多对多**关系，例如，在 C、 D、 E 和 F 每个任务依赖于任务 A 和 b。这很有用，例如，在其中您下游任务依赖多个上游任务的输出的并行化预处理方案。

### <a name="one-to-one"></a>一对一

若要创建一个任务成功完成具有相关性的任务，您可以提供单个任务 ID 为[TaskDependencies][net_taskdependencies]。[OnId][net_onid]静态方法时填充[取决于][net_dependson]属性[CloudTask][net_cloudtask]。

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>一到多

若要创建多个任务成功完成具有相关性的任务，提供一套任务 Id 为[TaskDependencies][net_taskdependencies]。[OnIds][net_onids]静态方法时填充[取决于][net_dependson]属性[CloudTask][net_cloudtask]。

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>任务 ID 范围

若要创建一组任务的成功完成其 Id 位于范围内的具有的依赖项的任务，提供第一个和最后一个到[TaskDependencies]范围内任务 Id[net_taskdependencies]。[OnIdRange][net_onidrange]静态方法时填充[取决于][net_dependson]属性[CloudTask][net_cloudtask]。

>[AZURE.IMPORTANT] 当您使用您的依赖性任务 ID 范围时，范围*必须*在该任务 Id 是整数值的字符串表示形式。 此外，在该区域中的每项任务必须成功完成，计划执行相关任务。

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>代码示例

[TaskDependencies] [github_taskdependencies]示例项目是[Azure 批处理代码示例]之一的[github_samples]在 GitHub 上。 此 Visual Studio 2015年解决方案演示如何启用作业任务依赖项、 创建任务依赖于其他任务，并在池的计算节点上执行这些任务。

## <a name="next-steps"></a>下一步行动

### <a name="application-deployment"></a>应用程序部署

批次的[应用程序包](batch-application-packages.md)功能可以轻松地同时部署和版本执行任务的应用程序计算节点。

### <a name="installing-applications-and-staging-data"></a>安装应用程序和数据转移

请查看[安装的应用程序和临时批处理数据计算节点][forum_post]张贴在 Azure 批论坛有关的各种方法来准备您的节点运行的任务的概述。 编写的 Azure 批团队成员之一，这篇文章是计算节点到良好基础知识获取文件 （包括应用程序和任务输入的数据） 的不同方法。

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "图︰ 一对一依赖项"
[2]: ./media/batch-task-dependency/02_one_to_many.png "图︰ 一对多的依赖关系"
[3]: ./media/batch-task-dependency/03_task_id_range.png "图︰ 任务 id 范围依赖项"
