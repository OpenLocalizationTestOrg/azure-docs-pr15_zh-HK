<properties
    pageTitle="使用多实例任务在 Azure 批运行 MPI 应用程序 |Microsoft Azure"
    description="了解如何执行在 Azure 批处理中使用多实例任务类型的消息传递接口 (MPI) 应用程序。"
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>使用多实例任务在 Azure 批处理运行消息传递接口 (MPI) 的应用程序

多实例任务允许您同时在多个计算节点上运行 Azure 批处理任务。 这些任务使高性能计算方案，如消息传递接口 (MPI) 批次中的应用程序。 在本文中，您将学习如何使用[批处理.NET]的多实例任务执行[api_net]库。

>[AZURE.NOTE] 尽管本文中的示例重点批次.NET 中，MS 的 MPI，并且 Windows 计算节点，这里讨论的多实例任务概念都适用于其他平台和技术 （Python 和 Linux 节点，例如在英特尔 MPI）。

## <a name="multi-instance-task-overview"></a>多实例任务概述

在批处理中，每个任务通常是在单个计算节点-执行提交给作业，多个任务和批服务安排在一个节点上执行每个任务。 但是，通过配置一个任务**多实例设置**，告诉批次，而是创建一个主要任务，然后在多个节点执行的多个子任务。

![多实例任务概述][1]

当您提交一个任务多实例设置为作业时，批处理执行多实例任务唯一的几个步骤︰

1. 批处理服务创建一个**主**和若干**子任务**基于多实例的设置。 （主加上所有子任务） 的任务的总数匹配**实例**（计算节点） 的多实例设置中指定的数量。
1. 批处理计算节点之一指定为**主机**，并安排在主节点上执行的主要任务。 它计划在计算节点分配给该多实例任务，每个节点的一个子任务的剩余部分上执行的子任务。
1. 主站点和所有子任务下载您的多实例设置中指定的任何**通用资源文件**。
1. 公共资源后是否已下载文件，主和子任务执行**协调命令**在多实例设置中指定。 协调命令通常用于为执行任务做好准备的节点。 这可能包括启动后台服务 (如[Microsoft MPI][msmpi_msdn]的`smpd.exe`) 和验证节点就可以处理节点间的消息。
1. 主要的任务上主节点*后*由主站点和所有子任务已成功完成协调命令执行**应用程序命令**。 应用程序命令是命令行的多实例任务本身，并只执行的主要任务。 在[MS MPI][msmpi_msdn]-基于解决方案，这是您执行您的 MPI 应用程序使用`mpiexec.exe`。

> [AZURE.NOTE] 虽然功能上的不同，"多实例任务"不是唯一的任务类型，像[StartTask] [net_starttask]或[JobPreparationTask][net_jobprep]。 多实例任务为只是标准批处理任务 ([CloudTask] [net_task]批次.NET 中) 的多实例设置的配置。 在本文中，我们称它为**多实例任务**。

## <a name="requirements-for-multi-instance-tasks"></a>多实例任务的要求

多实例任务需要**启用的节点间通信**，并**禁用并发任务执行**一个池。 如果您尝试运行多实例任务在一个池被禁用，节点之间的通讯或*maxTasksPerNode*值大于 1，永远不会排定任务 — 它无限期地保持"活动"状态中。 此代码段显示了使用批处理.NET 库池的创建。

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

另外，多实例任务可以执行*仅* **14 12 月 2015年之后创建的池**在节点上。

### <a name="use-a-starttask-to-install-mpi"></a>使用 StartTask 安装 MPI

若要使用多实例任务运行 MPI 应用程序，首先需要在池中的计算节点上安装 MPI 实现 （MS MPI 或英特尔 MPI，例如）。 这是合适的时间使用[StartTask][net_starttask]，其时节点加入一个池，或重新启动时执行。 此代码段创建为[资源文件]指定毫秒 MPI 安装程序包 StartTask[net_resourcefile]。 资源文件下载到该节点后，执行开始任务的命令行。 在这种情况下，命令行执行无人参与的安装的 MS MPI。

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>远程直接内存访问 (RDMA)

当您在批池中的计算节点选择[RDMA 支持大小](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)如 A9 时，MPI 应用程序可以利用 Azure 的高性能、 低延迟的远程直接内存访问 (RDMA) 网络。

寻找的大小指定为"RDMA 支持"中的以下文章︰

* **CloudServiceConfiguration**池

  * [云服务的大小](../cloud-services/cloud-services-sizes-specs.md)(仅限 Windows)

* **VirtualMachineConfiguration**池

  * [Azure 中的虚拟机大小](../virtual-machines/virtual-machines-linux-sizes.md)(Linux)

  * [Azure 中的虚拟机大小](../virtual-machines/virtual-machines-windows-sizes.md)(Windows)

>[AZURE.NOTE] 要利用 RDMA 的[Linux 计算节点](batch-linux-nodes.md)上，必须在节点上使用**英特尔 MPI** 。 CloudServiceConfiguration 和 VirtualMachineConfiguration 池的详细信息，请参阅批处理功能概述的[Pool](batch-api-basics.md#Pool)部分。

## <a name="create-a-multi-instance-task-with-batch-net"></a>使用批处理.NET 创建多实例任务

现在，我们已经介绍的池要求和 MPI 包安装，将让我们创建多实例任务。 在本段中，我们创建标准[CloudTask][net_task]，然后配置其[MultiInstanceSettings] [net_multiinstance_prop]属性。 如前所述，多实例任务不是一种不同的任务类型，但使用多实例设置配置标准批处理任务。

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>主要任务和子任务

当您创建一个任务多实例的设置时，您指定要执行的任务的计算节点数。 当您提交给作业任务时，批服务创建一项**主要**任务，并同时匹配指定的节点的数目足够**多个子任务**。

这些任务指派给*numberOfInstances* -1 0 中的整数 id。 Id 为 0 的任务是主要的任务，和所有其他 id 都是子任务。 例如，如果您创建了以下多实例设置任务，主要任务者没有的标识号为 0，并且子任务将 id 1 至 9。

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>主节点

多实例任务提交时，批处理服务指定为"主"的节点，计算节点之一，并计划在主节点上执行的主要任务。 子任务执行节点分配给该多实例任务的剩余部分计划。

## <a name="coordination-command"></a>协调命令

通过两个主执行**协调命令**和子任务。

阻塞的协调命令调用-不执行批处理应用程序命令直到协调命令已成功返回所有子任务。 协调命令因此应该开始任何所需的后台服务，验证它们可供使用，然后退出。 例如，使用 MS MPI 版本 7 的节点上，启动 SMPD 服务的解决方案为此协调命令然后退出︰

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

请注意，使用`start`在此命令中的协调。 这是必需的因为`smpd.exe`应用程序不会在执行后立即返回。 不使用[启动][cmd_start]命令，此协调命令不会返回，并且因此将阻止从正在运行的应用程序命令。

## <a name="application-command"></a>应用程序命令

一旦执行协调命令完成的主要任务和所有子任务，多实例任务的命令行执行的主要任务*只*。 我们称之为要分开的协调命令的**应用程序命令**。

对于 MS MPI 应用程序，使用应用程序命令执行 MPI 启用应用程序与`mpiexec.exe`。 例如，下面是使用 MS MPI 版本 7 的解决方案应用程序的命令︰

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] 因为 MS-MPI`mpiexec.exe`使用`CCP_NODES`变量的默认值 （请参见[环境变量](#environment-variables)） 示例应用程序命令行上将其排除。

## <a name="environment-variables"></a>环境变量

批量创建多个[环境变量][msdn_env_var]多实例任务分配给多实例任务的计算节点上所特有。 协调与应用程序的命令行可以引用这些环境变量，因为可以脚本和程序执行。

多实例任务被创建批服务使用以下环境变量︰

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

这些和其他批处理计算节点环境变量的完整详细信息，包括其内容和可见性，请参阅[计算节点环境变量][msdn_env_var]。

>[AZURE.TIP] 批 Linux MPI 代码示例包含了可以如何使用这些环境变量的几个示例。 [协调 cmd] [ coord_cmd_example] Bash 脚本下载公共应用程序并输入从 Azure 存储文件、 使主节点上的网络文件系统 (NFS) 共享和配置其他节点作为 NFS 客户机分配给多实例任务。

## <a name="resource-files"></a>资源文件

有两个资源文件，可以考虑多实例任务集︰ 下载*所有*任务的**通用资源文件**(主和子任务)，以及指定的**资源文件**的多实例任务本身的*只有主要*任务下载。

一个任务多实例设置中，您可以指定一个或多个**通用资源文件**。 这些公共资源文件从[Azure 存储](./../storage/storage-introduction.md)下载到每个节点的**任务共享的目录**的主和所有子任务。 可以通过从应用程序和协调命令行访问任务共享的目录`AZ_BATCH_TASK_SHARED_DIR`环境变量。 `AZ_BATCH_TASK_SHARED_DIR`路径是完全相同的多实例任务分配的每个节点上，因此您可以共享主一致性组和所有子任务之间的单一协调命令。 批处理不能"共享"目录中远程访问某种意义上，但可以将其用作装载或共享点上环境变量提示已经提到。

多实例任务本身指定的资源文件将下载到该任务的工作目录， `AZ_BATCH_TASK_WORKING_DIR`，默认情况下。 如上所述，与常见的资源文件，只主要任务下载多实例任务本身为指定的资源文件。

> [AZURE.IMPORTANT] 始终使用环境变量`AZ_BATCH_TASK_SHARED_DIR`和`AZ_BATCH_TASK_WORKING_DIR`来引用这些目录在命令行中。 不要尝试手动构造路径。

## <a name="task-lifetime"></a>任务的生存期

主要任务的生存期控制整个多实例任务的生存期。 当主退出时，所有子任务都将终止。 主的退出代码是任务的退出代码，因此用于确定成功或失败的任务重试的目的。

如果任何子任务失败，退出与非零返回代码，例如，整个多实例任务将失败。 多实例任务终止，然后重试，直到其达到重试限制。

当您删除一个多实例任务时，主站点和所有子任务也删除批服务。 所有子任务的目录，并从计算节点，如同标准任务中删除其文件。

[TaskConstraints] [net_taskconstraints]的多实例任务，例如[MaxTaskRetryCount][net_taskconstraint_maxretry]， [MaxWallClockTime][net_taskconstraint_maxwallclock]， [RetentionTime]和[net_taskconstraint_retention]属性，将生效，但对于标准的任务，并将应用于主站点和所有子任务。 但是，如果您更改[RetentionTime] [net_taskconstraint_retention]将多实例任务添加到该作业，则此更改后的属性仅应用于的主要任务。 所有子任务继续使用原始的[RetentionTime][net_taskconstraint_retention]。

如果最新任务是多实例任务的一部分，计算节点的最近任务列表反映了子任务的 id。

## <a name="obtain-information-about-subtasks"></a>获取有关子任务的信息

若要通过使用批处理.NET 库获取子任务的信息，请调用[CloudTask.ListSubtasks] [net_task_listsubtasks]方法。 此方法返回所有子任务，以及执行任务的计算节点的有关信息。 通过此信息，您可以确定每个子任务的根目录、 池 id、 当前状态、 退出代码，等等。 您可以使用此信息，结合[PoolOperations.GetNodeFile] [poolops_getnodefile]方法来获取子任务的文件。 请注意此方法不返回信息的主要任务 (id 为 0)。

> [AZURE.NOTE] 除非另行说明，否则批.NET 方法的操作上多实例[CloudTask] [net_task]本身*仅*适用于主要的任务。 例如，当您调用[CloudTask.ListNodeFiles] [net_task_listnodefiles]多实例任务的方法，只主要任务的返回的文件。

下面的代码段演示如何获取子任务的信息，以及执行它们的节点从请求文件的内容。

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>代码示例

[MultiInstanceTasks] [github_mpi]在 GitHub 上的代码示例演示如何使用多实例任务运行[MS MPI] [msmpi_msdn]批次的应用程序计算节点。 按照[准备](#preparation)和[执行](#execution)来运行该示例中的步骤。

### <a name="preparation"></a>准备工作

1. 按照[如何编译并运行一个简单的 MS MPI 程序]中的前两个步骤[msmpi_howto]。 这满足后续步骤 prerequesites。
1. 生成*发布*版本的[MPIHelloWorld] [helloworld_proj]示例 MPI 程序。 这是将在计算节点的多实例任务运行的程序。
1. 创建一个 zip 文件中包含`MPIHelloWorld.exe`（哪些您生成步骤 2） 和`MSMpiSetup.exe`(下载步骤 1)。 您将为下一步应用程序包上载此 zip 文件。
1. 使用[Azure 的门户网站][portal]创建名为"MPIHelloWorld"的批处理[应用程序](batch-application-packages.md)并指定 zip 文件创建与前面步骤中作为应用程序包的版本为"1.0"。 有关详细信息，请参阅[上载和管理应用程序](batch-application-packages.md#upload-and-manage-applications)。

>[AZURE.TIP] 生成一个*发布*版本的`MPIHelloWorld.exe`，以便您不必包含任何附加依赖项 (例如，`msvcp140d.dll`或`vcruntime140d.dll`) 在应用程序包中。

### <a name="execution"></a>执行

1. 下载[批处理的 azure 示例][github_samples_zip]从 GitHub。
1. 在 Visual Studio 2015年打开 MultiInstanceTasks**解决方案**。 `MultiInstanceTasks.sln`解决方案文件位于︰

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. 输入批处理和存储帐户凭据在`AccountSettings.settings` **Microsoft.Azure.Batch.Samples.Common**项目中。
1. **生成并运行**MultiInstanceTasks 解决方案执行 MPI 示例应用程序在计算批池中的节点。
1. *可选*︰ 使用[Azure 的门户网站][ portal] [批资源管理器]或[batch_explorer]检查示例池、 作业和任务 （"MultiInstanceSamplePool"、"MultiInstanceSampleJob"、"MultiInstanceSampleTask"） 之前删除的资源。

>[AZURE.TIP] 您可以下载[Visual Studio 社区][visual_studio]免费如果没有 Visual Studio。

从输出`MultiInstanceTasks.exe`是与以下内容类似︰

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>下一步行动

- Microsoft HPC 和 Azure 批团队博客讨论[MPI 的支持对 Azure 批 Linux][blog_mpi_linux]，并包括有关如何使用[OpenFOAM] [openfoam]与批次。 您可以查找[在 GitHub 上的 OpenFOAM 示例]Python 代码示例[github_mpi]。

- 了解如何使用 Azure 批 MPI 解决方案中[创建池的 Linux 计算节点](batch-linux-nodes.md)。

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "多实例概述"
