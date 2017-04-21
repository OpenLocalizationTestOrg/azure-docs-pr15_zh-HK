<properties
    pageTitle="在 Azure 批高效列表查询 |Microsoft Azure"
    description="通过请求批资源池、 工作、 任务，如在信息筛选查询提高性能和计算节点。"
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

# <a name="query-the-azure-batch-service-efficiently"></a>高效查询批处理 Azure 服务

在此处，您将学习如何通过减少时查询作业任务，并计算节点与[批处理.NET]服务返回的数据量提高 Azure 批处理应用程序的性能[api_net]库。

几乎所有的批处理应用程序需要执行某些类型的查询批次服务中，往往按固定时间间隔的监视或其他操作。 例如，若要确定是否有任何排队的作业中剩余的任务，您必须在作业中的每项任务上获取数据。 若要确定节点在池中的状态，必须在池中的每个节点上获取数据。 本文介绍了如何以最有效的方式执行此类查询。

## <a name="meet-the-detaillevel"></a>符合 DetailLevel

在生产环境的批处理应用程序，类似作业、 任务和计算节点的实体可以以千为单位进行编号。 请求时对这些资源的信息，可能大量的数据必须"跨线"从批服务到您的应用程序上的每个查询。 通过限制项目数和查询所返回的信息的类型，您可以增加您的查询的速度，因此您的应用程序的性能。

此[批.NET] [ api_net] API 代码段列出的作业，以及*所有*的每个任务的属性与关联的*每*项任务︰

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

您可以通过应用到您的查询的"详细级别"但是，执行效率高得多的列表查询。 这样做通过提供[ODATADetailLevel] [ odata] [JobOperations.ListTasks]对象[net_list_tasks]方法。 此代码段返回 ID、 命令行和已完成任务的计算节点信息属性︰

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

在此示例方案中，如果有成千上万个任务在作业中，从第二个查询的结果通常返回很多比第一个更快。 包括[下面](#efficient-querying-in-batch-net)的有关列表项与批处理.NET API 时使用 ODATADetailLevel 的详细信息。

> [AZURE.IMPORTANT]
> 我们强烈建议，您*总是*提供给您的.NET API 列表调用，以确保最大效率和应用程序性能的 ODATADetailLevel 对象。 通过指定详细信息级别，您可以帮助降低批服务响应时间，提高网络利用率，并减少客户端应用程序的内存使用情况。

## <a name="filter-select-and-expand"></a>筛选，选择，然后展开

[批次.NET] [api_net]和[批其余][ api_rest] Api 提供的功能来减少列表中返回的项目数以及每个返回的信息量。 请通过执行列表查询时指定**筛选器**，**选择**，和**展开的字符串**。

### <a name="filter"></a>筛选器
筛选器字符串是减少返回的项的数目的表达式。 例如，列出作业，只有正在运行的任务列表或只可运行任务的计算节点。

- 筛选器字符串包含一个或多个表达式，表达式的属性名称、 运算符和值组成。 可以指定的属性是特定于每个实体类型的查询时，所支持的每个属性的运算符。
- 可以使用逻辑运算符组合多个表达式`and`， `or`。
- 本示例筛选器字符串列表，仅运行"呈现"任务︰ `(state eq 'running') and startswith(id, 'renderTask')`。

### <a name="select"></a>选择
选择字符串限制为每个项返回的属性值。 您指定的属性名称的列表，这些属性值返回在查询结果中的项目。

- 选择字符串包含以逗号分隔的属性名称的列表。 您可以指定任何您所查询的实体类型的属性。
- 此示例选择字符串指定应为每个任务返回的只有三个属性值︰ `id, state, stateTransitionTime`。

### <a name="expand"></a>展开
扩充字符串可减少所需获得的某些信息的 API 调用的数量。 当使用扩充字符串时，可以获得有关每种物料的详细信息使用一个 API 调用。 而不是第一个获得的实体，然后请求在列表中，每个项目的信息列表使用扩充字符串来获取一个 API 调用中的相同信息。 更少的 API 调用意味着更好的性能。

- 类似于选择字符串，扩充字符串控制是否在查询结果列表中包含某些数据。
- 使用中列出的作业、 作业调度、 任务和池时，才支持扩充字符串。 目前，它只支持统计信息。
- 当需要所有属性也未指定任何选择的字符串时，*必须*展开的字符串用于获取统计信息。 如果选择字符串用于获取属性的一个子集，然后`stats`中选择字符串，可以指定并扩充字符串不需要指定。
- 本示例扩展字符串指定应为列表中的每个项返回的统计信息︰ `stats`。

> [AZURE.NOTE] 构建任何三个查询字符串类型时 （筛选、 选择，并展开），您必须确保属性名称和大小写匹配的 REST API 元素对应。 例如，当使用.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask)类，即使.NET 属性是[CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state)必须指定**状态**而不是**状态**。 请参阅下表中的属性在.NET 和 REST Api 之间的映射。

### <a name="rules-for-filter-select-and-expand-strings"></a>规则的筛选器，选择，然后展开字符串

- 属性名称在筛选器中，选择，并展开字符串应显示与它们在[批其余][ api_rest] API-即使您使用[批处理.NET] [api_net]或其他批次 Sdk 之一。
- 所有属性名称都是区分大小写，但属性值都是区分大小写。
- 日期/时间字符串可以是两种格式，并且前面必须加上`DateTime`。

  - W3C DTF 格式示例︰`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - RFC 1123 格式示例︰`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- 布尔值的字符串是`true`或`false`。
- 如果指定了无效的属性或运算符， `400 (Bad Request)` ，将产生错误。

## <a name="efficient-querying-in-batch-net"></a>高效查询批处理.NET 中

[批次.NET]中[api_net]API、 [ODATADetailLevel] [odata]类用于提供筛选器，选择，和扩展到列表操作的字符串。 ODataDetailLevel 类有三个公用字符串属性，可以指定的构造函数中或直接在对象上设置。 您然后 ODataDetailLevel 对象作为参数传递到不同的列表操作，例如[ListPools][net_list_pools]， [ListJobs][net_list_jobs]， [ListTasks]和[net_list_tasks]。

- [ODATADetailLevel][odata]。[FilterClause][ odata_filter]︰ 限制返回的项目数。
- [ODATADetailLevel][odata]。[SelectClause][ odata_select]︰ 指定哪些属性值将返回，且每一项。
- [ODATADetailLevel][odata]。[ExpandClause][ odata_expand]︰ 对于每个项，而不是单独的调用调用检索单个 API 中的所有项的数据。

下面的代码段使用批处理.NET API 来高效地查询批处理服务统计的一组特定的池。 在这种情况下，批处理用户具有测试和生产池。 测试池 Id 为"test"前缀，前缀与"生产"的生产池 Id。 在段中， *myBatchClient*是[BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient)类的正确初始化的实例。

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] 实例的[ODATADetailLevel] [odata]的配置与选择并展开子句还可以传递给相应的 Get 方法，如[PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx)，来限制返回的数据量。

## <a name="batch-rest-to-net-api-mappings"></a>批其余部分到.NET API 的映射

属性名称在筛选器中，选择，并展开字符串*必须*反映其 REST API 一样，同时在名称和案例。 下表提供了对应的.NET 版本和 REST API 之间的映射。

### <a name="mappings-for-filter-strings"></a>筛选器字符串的映射

- **.NET 列表的方法**︰ 每个.NET API 的方法在本专栏中接受[ODATADetailLevel] [odata]对象作为参数。
- **其余部分列表请求**︰ REST API，每个网页链接到此列中包含一个*筛选器*字符串中指定的属性和操作所允许的表。 在构造[ODATADetailLevel.FilterClause]时，您将使用这些属性名和操作[odata_filter]字符串。

| .NET 的列表方法 | 其余部分列表请求 |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [列表中的帐户的证书][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [列出与任务关联的文件][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [列出的作业准备和作业的作业发布任务的状态][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [列出某一科目中的作业][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [列出在某个节点上的文件][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [列出与作业关联的任务][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [列出某一科目中的作业调度][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [列出与作业调度的作业][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [列出库中的计算节点][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [列出某一科目中的池][rest_list_pools]

### <a name="mappings-for-select-strings"></a>对于选择的字符串映射

- **批次.NET 类型**︰ 批.NET API 类型。
- **REST API 实体**︰ 该列中的每页都包含一个或多个表，其中列出类型的 REST API，属性名称。 在构造*选择*字符串时使用这些属性名称。 在构造[ODATADetailLevel.SelectClause]时，将使用这些相同的属性名称[odata_select]字符串。

| 批处理的.NET 类型 | REST API 实体 |
|---|---|
| [证书][net_cert] | [获取有关证书的信息][rest_get_cert] |
| [CloudJob][net_job] | [获取有关作业的信息][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [获取有关作业日程安排信息][rest_get_schedule] |
| [ComputeNode][net_node] | [获取有关节点的信息][rest_get_node] |
| [CloudPool][net_pool] | [获取有关池的信息][rest_get_pool] |
| [CloudTask][net_task] | [获取有关任务的信息][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>示例︰ 创建一个筛选器字符串

当构造一个筛选器字符串的[ODATADetailLevel.FilterClause][odata_filter]，咨询到查找 REST API 文档页中找到对应于您要从中执行此列表操作之上"的筛选器字符串映射"下的表。 在该页面上的第一个多行表中，您将找到可筛选属性和其支持的运算符。 如果您想要检索其退出代码为零，所有的任务例如，此行[与作业关联的任务列表]上[rest_list_tasks]指定的相应属性字符串和允许的运算符︰

| 属性 | 允许的操作 | 类型 |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

因此，将是列出所有具有非零退出代码的任务的筛选器字符串︰

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>示例︰ 构造选择字符串

若要构造[ODATADetailLevel.SelectClause][odata_select]、 请参阅上述"选择字符串映射"下的表和定位到列出的实体的类型相对应的 REST API 页。 在该页面上的第一个多行表中，您将找到的可选属性和其支持的运算符。 如果您想要检索 ID 和列表中每个任务的命令行，例如，您会发现这些行上[获取有关任务的信息]的相应表中[rest_get_task]:

| 属性 | 类型 | 备注 |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

于是便选择包括 ID 和与每个列出的任务的命令行字符串︰

`id, commandLine`

## <a name="code-samples"></a>代码示例

### <a name="efficient-list-queries-code-sample"></a>有效的列表查询代码示例

签出[EfficientListQueries] [efficient_query_sample]在 GitHub 以查看如何有效列表查询的样例项目可能会影响应用程序的性能。 该 C# 控制台应用程序创建，并将大量的任务添加到作业。 然后，它调用多个[JobOperations.ListTasks] [net_list_tasks]方法并传递[ODATADetailLevel] [odata]这些对象被配置具有不同的属性值来改变要返回的数据量。 它输出类似于以下内容︰

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

运行时间中所示，可以通过限制属性并返回的项的数目大大降低查询响应时间。 [批处理的 azure 示例]中，可以找到这和其他的示例项目[github_samples]在 GitHub 上的存储库。

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics 库和代码示例

除了上面的 EfficientListQueries 代码示例，您可以找到[BatchMetrics] [ batch_metrics] [批次的 azure 示例]中的项目[github_samples]GitHub 资料库。 BatchMetrics 示例项目演示了如何有效地监视使用批处理 API 的 Azure 批处理作业进度。

[BatchMetrics] [batch_metrics]的示例包括一个.NET 类库项目的可以合并到您自己的项目和一个简单的命令行程序进行练习和演示库的使用。

在项目中的示例应用程序演示下列操作︰

1. 选择要下载您需要的属性特定于的属性
2. 要下载自上次查询以来仅更改筛选状态过渡时间

例如，下面的方法在 BatchMetrics 库中显示。 返回指定的 ODATADetailLevel`id`和`state`属性应获得被查询的实体。 它还指定仅指定以来已更改状态的实体`DateTime`应返回参数。

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>下一步行动

### <a name="parallel-node-tasks"></a>节点的并行任务

[最大化 Azure 批处理计算节点并行任务的资源使用状况](batch-parallel-node-tasks.md)是批处理应用程序的性能与相关的另一篇文章。 某些类型的工作负载可以受益于上执行并行任务较大的但更少，计算节点。 签出在项目中的此类方案的详细信息的[示例方案](batch-parallel-node-tasks.md#example-scenario)。

### <a name="batch-forum"></a>批次论坛

[Azure 批论坛][forum]在 MSDN 上是很好的讨论批和询问服务有关的问题。 通过有用"粘性"的帖子，在总部和出现时生成批处理解决方案发布您的问题。


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
