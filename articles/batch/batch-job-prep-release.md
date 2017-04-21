<properties
    pageTitle="准备和清理批处理作业 |Microsoft Azure"
    description="使用作业级的准备工作，以最小化数据传输到 Azure 批计算节点，然后释放节点清理作业完成的任务。"
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>在 Azure 批处理计算节点上运行作业的准备工作和完成任务

 Azure 批处理作业之前，其任务执行和后期作业维护其任务完成后，通常需要某种形式的安装程序。 您可能需要将常见任务输入的数据下载到您的计算节点，或将任务输出数据上传到 Azure 存储在作业完成后。 **作业准备**和**作业发布**任务可用于执行这些操作。

## <a name="what-are-job-preparation-and-release-tasks"></a>什么是作业准备工作，下达任务？

作业的任务运行之前，在计划运行至少一个任务的所有计算节点上运行作业准备任务。 完成作业后，作业发布任务就执行至少一个任务库中每个节点上运行。 与普通的批处理任务，可指定作业准备或下达任务运行时要调用的命令行。

作业准备和发布任务提供熟悉的批处理任务功能，例如文件下载 ([资源文件][net_job_prep_resourcefiles])，提升执行、 自定义的环境变量，最大的执行持续时间、 重试次数和文件保留时间。

在下面的章节中，您将学习如何使用[JobPreparationTask] [net_job_prep]和[JobReleaseTask] [ net_job_release] [批次.NET]中找到类[api_net]库。

> [AZURE.TIP] 作业准备和发布任务是特别有帮助"共享池"的环境中，池的计算节点作业运行之间持续存在以及使用的很多作业。

## <a name="when-to-use-job-preparation-and-release-tasks"></a>何时使用工作准备和发布任务

作业准备和作业发布任务是非常适合于以下情况︰

**下载公用任务数据**

批处理作业通常会要求一组通用的数据作为输入的作业任务。 例如，在每日风险分析计算，市场数据是特定工作的但通用的作业中的所有任务。 这些市场数据，通常几千兆字节的大小，应下载到每个计算节点仅一次，以便在该节点运行的任何任务可以使用它。 **作业准备任务**用于此数据下载到每个节点，然后作业执行的其他任务。

**删除作业和任务输出**

在"共享池"环境中，其中池的计算节点不是作业之间已停止使用的您可能需要删除作业运行之间的数据。 您可能需要以节省磁盘空间的节点，或满足组织的安全策略。 使用**作业发布任务**删除下载的作业准备工作任务，或在执行任务过程中生成的数据。

**日志保留时间**

您可能想要保留一份日志文件的生成任务，也可能是故障由失败的应用程序的转储文件。 在这种情况下使用**作业发布任务**进行压缩并将此数据上载到[Azure 存储][azure_storage]帐户。

>[AZURE.TIP] 另一种方法来保留日志和其他作业和任务输出数据是使用[Azure 批处理文件规则](batch-task-output.md)库。

## <a name="job-preparation-task"></a>作业准备任务

之前执行的作业任务，批次计划运行任务每个计算节点上执行作业准备任务。 默认情况下，批服务等待作业准备任务都完成后才运行在该节点上执行已计划的任务。 但是，您可以配置该服务不必等待。 如果节点重新启动后，作业准备任务运行，但您也可以禁用此行为。

仅在计划运行任务的节点上执行作业准备任务。 这样可以防止不必要准备任务执行节点未分配任务的情况下。 作业任务的数目小于池中的节点数时会发生该错误。 它还适用于启用[并发任务执行](batch-parallel-node-tasks.md)时，如果任务计数低于总可能的并发任务留一些节点空闲。 通过空闲节点上不运行作业准备任务，数据传输费用上花费更少的钱。

> [AZURE.NOTE] [JobPreparationTask] [net_job_prep_cloudjob]与[CloudPool.StartTask] [ pool_starttask] ，JobPreparationTask 执行每个作业的开始，而 StartTask 执行的计算节点时才首次加入一个池或重新启动。

## <a name="job-release-task"></a>作业发布任务

一旦作业被标记为已完成，执行至少一个任务库中每个节点上执行作业发布任务。 为发出终止请求已完成，您可以标记一个作业。 批服务然后作业状态设置为*终止*，终止任何与此作业关联的活动或正在运行任务并运行作业的发行任务。 作业将为*已完成*状态。

> [AZURE.NOTE] 删除作业还执行作业的发行任务。 但是，如果作业已终止，发布任务不会运行第二次如果以后删除作业。

## <a name="job-prep-and-release-tasks-with-batch-net"></a>作业准备，然后松开与批处理.NET 的任务

若要使用作业准备任务，分配[JobPreparationTask] [net_job_prep]对象与您的作业[CloudJob.JobPreparationTask] [net_job_prep_cloudjob]属性。 同样地，初始化[JobReleaseTask] [ net_job_release] ，并将其分配给作业的[CloudJob.JobReleaseTask] [net_job_prep_cloudjob]属性设置作业的发行任务。

在此代码段中，`myBatchClient`是[BatchClient]的实例[net_batch_client]，和`myPool`是批帐户内的现有池。

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

如前所述，当作业被终止或删除执行发行任务。 终止的作业[JobOperations.TerminateJobAsync][net_job_terminate]。 删除作业与[JobOperations.DeleteJobAsync][net_job_delete]。 您通常终止或删除作业时完成其任务时，或当已达到已定义的超时。

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>在 GitHub 代码示例

要查看作业准备并释放中的操作任务，检出[JobPrepRelease] [job_prep_release_sample]在 GitHub 上的样例项目。 该控制台应用程序将执行以下操作︰

1. 用两个"小"节点创建的池。
2. 与作业准备、 发布和标准的任务创建一个作业。
3. 运行作业准备任务，第一次写入一个节点的"共享"目录中的文本文件的节点 ID。
4. 其任务 ID 写入同一文本文件中的每个节点上运行任务。
5. 一旦完成所有任务 （或超时后），将打印到控制台文件中每个节点的文本内容。
6. 完成作业后，运行作业发布任务从该节点中删除该文件。
6. 打印每个节点执行它们的工作准备和发布任务的退出的代码。
7. 暂停执行允许作业和/或池中删除的确认。

从示例应用程序的输出结果类似于以下︰

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] 由于新的池 （某些节点可供在其他人之前的任务） 中的节点的变量创建和开始时间，可能会看到不同的输出。 具体来说，快速完成任务，因为池的节点之一可能执行的所有作业的任务。 如果发生这种情况，您将注意到该作业准备和不执行任何任务节点不存在发行任务。

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>检查作业准备和发布 Azure 门户中的任务

当您运行示例应用程序时，您可以使用[Azure 的门户网站][portal]若要查看属性的作业，它的任务，或甚至下载共享的文本文件修改的作业任务。

下面的屏幕截图显示在示例应用程序的运行后 Azure 门户中**准备任务刀片式服务器**。 定位到*JobPrepReleaseSampleJob*属性任务均已完成后 （但在删除您的作业和池前） 然后单击**准备任务**或**下达任务**以查看它们的属性。

![在 Azure 门户作业准备属性][1]

## <a name="next-steps"></a>下一步行动

### <a name="application-packages"></a>应用程序包

除了作业准备，您可以使用批处理的[应用程序包](batch-application-packages.md)功能计算节点为执行任务做准备。 此功能是对部署的应用程序不需要运行安装程序，应用程序包含许多 （100 +） 文件或需要严格的版本控制的应用程序特别有用。

### <a name="installing-applications-and-staging-data"></a>安装应用程序和数据转移

此 MSDN 论坛文章提供了多种方法来准备您的节点正在运行的任务的概述︰

[安装应用程序和临时批处理数据计算节点][forum_post]

编写的 Azure 批团队成员之一，它讨论了可用于部署应用程序和数据来计算节点的几种技术。

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

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

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
