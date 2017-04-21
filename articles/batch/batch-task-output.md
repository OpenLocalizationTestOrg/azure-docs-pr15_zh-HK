<properties
    pageTitle="作业和任务输出 Azure 批处理中的持久性 |Microsoft Azure"
    description="了解如何使用 Azure 存储，因为您的批处理任务和作业的持久存储区输出，并启用在 Azure 门户中查看此持续的输出。"
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>保持 Azure 批处理作业和任务输出

您通常运行在批处理任务产出必须存储和以后检索的作业，该作业，或同时执行客户端应用程序中的其他任务。 该输出可能创建的处理输入的数据文件或日志文件与执行任务。 本文介绍.NET 类库，使用基于规则的技术来保持这种任务输出到 Azure Blob 存储，使它可删除您的池，作业，并计算节点后，仍在。

通过使用本文中的方法，您还可以查看任务输出中**保存输出文件**并**保存日志**在[Azure 的门户网站][portal]。

![保存输出文件并保存在门户中记录选择器][1]

>[AZURE.NOTE] [Azure 批处理文件约定][ nuget_package] .NET 类库在这篇文章中讨论目前在预览中。 此处所述的功能可能会更改在正式供货前。

## <a name="task-output-considerations"></a>任务输出注意事项

设计您的批处理解决方案时，必须考虑几个因素相关的作业和任务输出。

* **计算节点寿命**︰ 计算节点通常是暂时的尤其是在已启用自动缩放的池。 仅当节点存在，并且只能在文件保留时间内设置为该任务时，在一个节点运行的任务的输出不可用。 若要确保保留任务输出，您的任务必须因此其输出将文件上载到持久存储区中，例如，Azure 存储。

* **输出存储**︰ 来持久保存到持久存储任务输出数据，您可以使用[Azure 存储 SDK](../storage/storage-dotnet-how-to-use-blobs.md)任务代码中将任务输出传到 Blob 存储容器。 如果实现容器和文件命名约定时，客户端应用程序或其他任务作业中的可以找到并下载根据约定此输出。

* **输出检索**︰ 如果可检索任务输出直接从计算节点在池中，或从 Azure 存储您的任务将保留其输出。 若要检索任务的输出直接计算节点，您需要的文件的名称和该节点上的输出位置。 如果客户端应用程序必须具有到 Azure 存储通过使用 Azure 存储 SDK 下载中的文件的完整路径和您持续输出到 Azure 存储，下游的任务。

* **查看输出**︰ 当您导航到 Azure 门户中的批处理任务并选择**节点上的文件**时，会出现与该任务相关联的所有文件，你感兴趣的不只是输出文件。 再次，在计算节点上的文件仅在存在的节点，并只在文件保留时间内设置为该任务时。 若要查看您已经保持到 Azure 存储在门户或类似[Azure 存储浏览器]应用程序的任务输出[storage_explorer]，您必须知道它的位置和直接导航到该文件。

## <a name="help-for-persisted-output"></a>帮助保持输出

为了帮助您更轻松地保持作业和任务输出，批团队已定义并实施一套命名约定以及.NET 类库， [Azure 批处理文件约定][nuget_package]库中，您可以在批处理应用程序中使用。 此外，Azure 门户已意识到这些命名约定，以便您可以轻松地找到您已经通过使用库中存储的文件。

## <a name="using-the-file-conventions-library"></a>使用文件约定库

[Azure 批处理文件约定][nuget_package]是批.NET 应用程序可以使用轻松地存储和检索任务输出与 Azure 存储.NET 类库。 它适用于在任务和客户端代码 — 在任务代码中用于保存文件，并列出并检索它们的客户端代码中使用。 任务还可用于检索如上游的任务，这些任务的输出，在[任务相关性](batch-task-dependencies.md)情况下使用库。

规则库负责确保存储容器和任务输出文件命名习俗，以及要上载到正确的位置保持到 Azure 存储时。 当检索输出时，您将可以通过列出或检索的输出通过 ID 和目的，而无需知道文件名或它所在的存储轻松地找到给定的作业或任务的输出。

例如，您可以使用库"列出任务 7 中，所有中间文件"或"获取我的缩略图预览作业*mymovie*，"无需知道文件名称或位置在您的存储帐户内。

### <a name="get-the-library"></a>获取库

您可以获取的库，其中包含新类和扩展[CloudJob] [net_cloudjob]和[CloudTask] [net_cloudtask]类使用新的方法，从[NuGet][nuget_package]。 您可以将其添加到 Visual Studio 项目使用[NuGet 库程序包管理器][nuget_manager]。

>[AZURE.TIP] 您可以找到[源代码][ github_file_conventions] .NET 库 Microsoft Azure SDK 中的 GitHub 上 Azure 批处理文件规则库。

## <a name="requirement-linked-storage-account"></a>要求︰ 链接的存储帐户

若要存储输出到持久存储区使用文件约定库和 Azure 门户中查看它们，批帐户都必须[链接 Azure 存储帐户](batch-application-packages.md#link-a-storage-account)。 如果尚未启动，则使用 Azure 门户存储帐户链接到批帐户︰

**批次帐户**刀片式服务器 >**设置** > **存储帐户** > **存储帐户**（无） > 选择存储帐户，在您的订阅

链接存储帐户有关更多详细浏览，请参见[与 Azure 批处理应用程序包的应用程序部署](batch-application-packages.md)。

## <a name="persist-output"></a>保持输出

有两个与文件约定库保存作业和任务输出时要执行的主要操作︰ 创建存储容器，并将输出保存到容器。

>[AZURE.WARNING] 因为所有的作业和任务输出存储在同一个容器中，[存储节流限制](../storage/storage-performance-checklist.md#blobs)可能会实施如果尝试将文件保留在同一时间大量的任务。

### <a name="create-storage-container"></a>创建存储容器

您的任务开始保持输出到存储区之前，必须创建它们将其输出传至 blob 存储容器。 执行此操作通过调用[CloudJob][net_cloudjob]。[PrepareOutputStorageAsync][net_prepareoutputasync]. 此扩展方法采用[CloudStorageAccount] [net_cloudstorageaccount]对象作为参数，并创建一个容器由 Azure 的门户，并在文章的后面讨论的检索方法命名，其内容是可发现的方式。

通常将此代码放置在客户端应用程序，创建您的池、 作业和任务的应用程序。

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>存储任务输出

现在，您已准备好在 blob 存储容器，任务可以将输出保存到容器使用[TaskOutputStorage] [net_taskoutputstorage]类文件在规则库中找到。

在任务代码中，第一次创建[TaskOutputStorage] [net_taskoutputstorage]对象，然后该任务已完成其工作，当调用[TaskOutputStorage][net_taskoutputstorage]。[SaveAsync][net_saveasync]方法将其输出保存到 Azure 存储。

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

"输出类型"参数对持久的文件进行分类。 有四种预定义的[TaskOutputKind] [net_taskoutputkind]类型:"TaskOutput"、"TaskPreview"、"TaskLog"和"TaskIntermediate"。 如果它们都适用于您的工作流，您还可以定义自定义类型。

这些输出类型允许您指定哪种类型的输出列出给定任务的持续输出以后查询批处理时。 换句话说时列出任务的输出，您可以筛选一种输出类型的列表。 例如，"给我*预览*输出任务*109*的。" 更多有关列表和检索输出将显示在[检索输出](#retrieve-output)在文章的后面。

>[AZURE.TIP] 输出类型还指定在 Azure 门户中特定文件的显示位置︰ *TaskOutput*-分类的文件将出现在"任务输出文件"和*TaskLog*文件将显示在"任务日志"。

### <a name="store-job-outputs"></a>存储区作业输出

除了存储任务输出，您可以存储与整个作业相关联的输出。 例如，在影片呈现作业合并任务中，未能保持完全渲染的影片作为作业输出。 您的工作完成后，客户端应用程序只需列出并检索输出作业，并且不需要查询的单个任务。

通过调用[JobOutputStorage]来存储作业输出[net_joboutputstorage]。[SaveAsync][net_joboutputstorage_saveasync]方法，并指定[JobOutputKind] [net_joboutputkind]和文件名︰

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

如有任务输出的 TaskOutputKind，您可以使用[JobOutputKind] [net_joboutputkind]参数进行分类作业的文件保持不变。 此参数可以为以后的查询 （清单） 的输出特定类型。 JobOutputKind 包括输出和预览类型，并支持创建自定义类型。

### <a name="store-task-logs"></a>日志存储任务

除了任务或作业完成时，将保持持久存储到一个文件，您可能发现有必要来保留期间执行的任务-日志文件更新的文件或`stdout.txt`， `stderr.txt`，例如。 为此目的，Azure 批处理文件规则库提供了[TaskOutputStorage][net_taskoutputstorage]。[SaveTrackedAsync][net_savetrackedasync]方法。 与[SaveTrackedAsync][net_savetrackedasync]，可以跟踪到 （在您指定的时间间隔） 的节点上的文件更新，并持续到 Azure 存储这些更新。

在下面的代码段中，我们使用[SaveTrackedAsync] [net_savetrackedasync]更新`stdout.txt`在 Azure 存储每隔 15 秒执行任务的过程︰

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`是只是占位符通常会执行您的任务的代码。 例如，您可能必须从 Azure 存储下载数据，并在其上执行转换或计算的代码。 此代码段的重要作用证明如何换行这样的代码在`using`块，定期更新文件[SaveTrackedAsync][net_savetrackedasync]。

`Task.Delay`是必需在这结束的`using`块，以确保节点代理有时间刷新的节点 （节点代理为池中的每个节点上运行，并命令控件之间提供接口的节点和批服务程序） 上的 stdout.txt 文件到标准输出的内容。 如果没有这个延迟，就可能错过最后几秒的输出。 这种延迟可能不需要的所有文件。

>[AZURE.NOTE] 启用文件跟踪与 SaveTrackedAsync 时，仅*追加*的跟踪文件将保存到 Azure 存储。 此方法仅用于跟踪非旋转日志文件或其他文件附加到，也就是说，数据只能添加到文件的末尾，它更新时使用。

## <a name="retrieve-output"></a>检索输出

当您使用 Azure 批处理文件规则库的持续的输出检索时，这样做以中心任务和工作的方式。 您可以请求输出给定任务或作业无需知道 blob 存储，或甚至其文件名及其路径。 您可以简单地说，"给我任务*109*的输出文件。"

下面的代码段循环访问所有的作业任务，打印某些信息的输出文件的任务，然后从存储中下载的文件。

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>任务输出和 Azure 门户网站

Azure 门户显示任务输出和日志保存到链接的 Azure 存储帐户，使用命名约定在[Azure 批处理文件约定自述文件]中找到[github_file_conventions_readme]。 您可以实现这些约定自己在您选择的语言，也可以使用文件约定库.NET 应用程序中。

### <a name="enable-portal-display"></a>启用门户显示

若要启用您的门户输出的显示，必须满足以下要求︰

 1. 批次帐户[链接 Azure 存储帐户](#requirement-linked-storage-account)。
 2. 持续输出时，请遵守存储容器和文件预定义的命名约定。 可以在文件约定库[自述文件]中找到这些约定的定义[github_file_conventions_readme]。 如果您使用[Azure 批处理文件约定][nuget_package]库，以保持您的输出，这一要求得到满足。

### <a name="view-outputs-in-the-portal"></a>在门户中查看输出

若要查看任务输出和日志在 Azure 的门户中，导航到任务感兴趣，然后单击**保存输出文件**或**保存日志**的输出。 此图像显示了任务**保存输出文件**id 为"007":

![任务输出在 Azure 门户刀片][2]

## <a name="code-sample"></a>代码示例

[PersistOutputs] [github_persistoutputs]示例项目是[Azure 批处理代码示例]之一的[github_samples]在 GitHub 上。 此 Visual Studio 2015年解决方案演示如何使用 Azure 批处理文件规则库来持久保存到持久存储任务输出。 若要运行该示例，请执行以下步骤︰

1. 在**2015 Visual Studio**中打开该项目。
2. 在 Microsoft.Azure.Batch.Samples.Common 项目中添加**AccountSettings.settings**批次和存储**帐户凭据**。
3. **生成**（但不是运行） 解决方案。 如果出现提示，请还原任何 NuGet 程序包。
4. 使用 Azure 的门户网站**PersistOutputsTask**的上载[应用程序包](batch-application-packages.md)。 包括`PersistOutputsTask.exe`.zip 封装，其依赖程序集设置为"PersistOutputsTask"的应用程序 ID 和应用程序软件包版本为"1.0"。
5. **启动**（运行） **PersistOutputs**项目。

## <a name="next-steps"></a>下一步行动

### <a name="application-deployment"></a>应用程序部署

批次的[应用程序包](batch-application-packages.md)功能可以轻松地同时部署和版本执行任务的应用程序计算节点。

### <a name="installing-applications-and-staging-data"></a>安装应用程序和数据转移

请查看[安装的应用程序和临时批处理数据计算节点][forum_post]在准备您的节点正在运行的任务的各种方法的概述的 Azure 批论坛中发布。 这篇文章编写的 Azure 批团队成员之一，拖到您的计算节点，并且每种方法的一些特殊注意事项是良好基础知识获取文件 （包括应用程序和任务输入的数据） 的不同方法。

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "保存输出文件并保存在门户中记录选择器"
[2]: ./media/batch-task-output/task-output-02.png "任务输出在 Azure 门户刀片"