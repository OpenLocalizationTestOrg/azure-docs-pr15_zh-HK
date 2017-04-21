<properties
    pageTitle="最大限度使用并行任务的批处理节点使用 |Microsoft Azure"
    description="在 Azure 批池中的每个节点上使用更少的计算节点和运行并发任务提高效率并降低成本"
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
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>最大化并发节点任务的 Azure 批处理计算资源使用情况

通过在 Azure 批池中每个计算节点上同时运行多个任务，您可以最大化池中的节点数较少的资源使用情况。 对于某些工作负载，这可能导致较短的作业时间，降低成本。

在某些情况下受益于将所有节点的资源专用于单个任务，几种情况下受益于允许多个任务可以共享这些资源︰

 - **最小化数据传输**任务时能够共享数据。 在此方案中，您可以大大减少数据传输费用，通过将共享的数据复制到较少的节点，每个节点上并行执行的任务。 这尤其适用于必须地理地区之间传输的数据复制到每个节点。

 - **最大化内存利用率**当任务需要大量的内存，但只有在短期的时间，和在执行过程中的可变时间。 您可以使用具有更大的内存来有效地处理这种峰值的较少，但更大，计算节点。 这些节点必须在每个节点上并行运行的多个任务，但每个任务将利用节点的充足内存在不同的时间。

 - 节点间的通信时所需的池内的**缓解节点个数的限制**。 目前，池为节点间的通讯配置仅限于 50 计算节点。 如果这种池中的每个节点能够并行执行的任务，可以同时执行更多的任务。

 - **复制内部部署计算群集**，如在第一次移动计算环境到 Azure。 如果您当前的内部解决方案执行多个任务，每个计算节点，您可以增加节点任务，以更好地反映该配置最大数量。

## <a name="example-scenario"></a>示例方案

举一个例子来说明执行并行任务的好处，让我们假设您任务的应用程序的 CPU 和内存的要求，[标准\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d)节点已足够。 但是，为了在要求的时间完成作业，需要 1000 个节点。

而不是使用标准\_具有 1 个 CPU 内核的 D1 节点，您可以使用[标准\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d)的有 16 个核心，并且启用并行任务执行节点。 因此， *16 倍更少的节点*可以使用-而不是 1000 节点，需要使用仅 63。 此外，大型应用程序文件或引用数据所需的每个节点，如果作业持续时间和效率是再次改进后的数据复制到只有 16 个节点。

## <a name="enable-parallel-task-execution"></a>启用并行任务执行

在池级配置并行任务执行的计算节点。 成批处理.NET 库，设置[CloudPool.MaxTasksPerComputeNode] [maxtasks_net]属性创建一个池。 如果您使用批处理 REST API，设置[maxTasksPerNode] [rest_addpool]过程池中创建请求主体中的元素。

Azure 的批处理，您可以设置每个节点的最大任务达四倍 （4 倍） 的节点内核的数量。 例如，如果使用池配置的节点然后大小"大"（四个核心）`maxTasksPerNode`可能会设置为 16。 对于每个节点规模的内核数的详细信息，请参阅[为云服务的规模](../cloud-services/cloud-services-sizes-specs.md)。 服务限制的详细信息，请参阅[配额和 Azure 批服务的限制](batch-quota-limit.md)。

> [AZURE.TIP] 一定要考虑到`maxTasksPerNode`时构造[自动缩放公式]的值[enable_autoscaling]池。 例如，公式的计算结果`$RunningTasks`可能会大大受到增加的每个节点的任务。 有关详细信息，请参阅[自动缩放比例计算 Azure 批池中的节点](batch-automatic-scaling.md)。

## <a name="distribution-of-tasks"></a>将任务分发

时在池中，计算节点可以并发执行的任务，很重要，若要指定希望如何分布在池中的节点的任务。

通过使用[CloudPool.TaskSchedulingPolicy] [task_schedule]属性，可以指定任务应均匀地分配在池 （"分配"） 中的所有节点。 也可以指定尽可能多任务应分配给每个节点中之前的任务指派给另一个节点中的池 （"装箱"）。

举例说明此功能的重要方式，应考虑该池的[标准\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) （在上面的示例中） 的节点配置为具有[CloudPool.MaxTasksPerComputeNode] [maxtasks_net]值为 16。 如果[CloudPool.TaskSchedulingPolicy] [ task_schedule] [ComputeNodeFillType]配置[fill_type]的*包*，它将最大限度地使用的每个节点的所有 16 个核心，并且允许[自动缩放池](batch-automatic-scaling.md)删除未使用的节点从池 （而无需分配任何任务节点）。 这将最小化资源使用和节省资金。

## <a name="batch-net-example"></a>批处理的.NET 示例

此[批.NET] [ api_net] API 的代码段演示创建池包含的每个节点的四项任务最多的四个大节点的请求。 它指定任务计划将填充任务之前将任务分配到池中的另一个节点的每个节点的策略。 通过使用批处理.NET API 添加池的详细信息，请参阅[BatchClient.PoolOperations.CreatePool][poolcreate_net]。

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>批处理的剩余部分示例

此[批其余][ api_rest] API 的代码段演示创建池包含的每个节点的四项任务最多的两个大节点的请求。 通过使用 REST API 添加池的详细信息，请参阅[添加到帐户池][rest_addpool]。

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] 您可以设置`maxTasksPerNode`元素和[MaxTasksPerComputeNode] [maxtasks_net]只在池中创建时间的属性。 不能修改后创建一个池。

## <a name="code-sample"></a>代码示例

[ParallelNodeTasks] [parallel_tasks_sample]项目在 GitHub 上的说明如何使用[CloudPool.MaxTasksPerComputeNode] [maxtasks_net]属性。

该 C# 控制台应用程序使用[批处理.NET] [api_net]库，以创建一个具有一个或多个计算节点池。 这些节点来模拟可变负载执行可配置数量的任务。 从应用程序的输出指定哪些节点执行每个任务。 应用程序还提供了摘要的作业参数和持续时间。 摘要部分的两种不同运行示例应用程序的输出如下所示。

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

第一个示例应用程序的执行表明池和默认设置的每个节点，一个任务中的一个节点与作业持续时间是 30 分钟以上。

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

第二个示例演示运行作业工期大大下降。 这是因为池配置中，每个节点，允许并行任务执行中将近四分之一的时间完成该作业的四项任务。

> [AZURE.NOTE] 在上述摘要中的作业工期不包括池创建时间。 每个上述的作业提交到以前创建的池的计算节点在提交时处于*空闲*状态。

## <a name="next-steps"></a>下一步行动

### <a name="batch-explorer-heat-map"></a>批次资源管理器中的热量地图

[Azure 批资源管理器][batch_explorer]，Azure 批处理[示例应用程序]之一[github_samples]，包含提供可视化任务执行的*热量*地图。 当您执行[ParallelTasks] [parallel_tasks_sample]示例应用程序，您可以使用热量地图功能方便直观地显示每个节点上的并行任务的执行。

![批次资源管理器中的热量地图][1]

*显示四个节点，与当前正在执行四项任务的每个节点的池批资源管理器热图*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
