<properties
    pageTitle="Visual Studio 模板 Azure 批 |Microsoft Azure"
    description="了解这些 Visual Studio 项目模板可以帮助您实施和批 Azure 上运行您的计算密集型工作负载"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
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

# <a name="visual-studio-project-templates-for-azure-batch"></a>Azure 批次的 Visual Studio 项目模板

在**作业管理器**和批处理**任务处理器 Visual Studio 模板**提供代码来帮助您实现并运行您的计算密集型工作负载最少的工作量的批次。 本文档描述这些模板，并提供了有关如何使用这些指南。

>[AZURE.IMPORTANT] 本文讨论信息仅适用于下列两个模板，并假定您熟悉的批服务和与之相关的关键概念︰ 池，计算节点、 作业和任务、 作业管理器任务、 环境变量和其他相关信息。 [基础知识的 Azure 批](batch-technical-overview.md)、[批次为开发人员提供的功能概述](batch-api-basics.md)，和[学习如何使用.NET 的 Azure 批库](batch-dotnet-get-started.md)中，可以找到详细信息。

## <a name="high-level-overview"></a>概述

作业管理器和任务处理器模板可用于创建有用的两个组件︰

* 作业管理器任务实现可以将作业划分为可以独立并行运行的多个任务的作业拆分器。

* 可用于执行预处理和后处理围绕应用程序命令行任务处理器。

例如，影片呈现的情况下，作业拆分器将变为单个影片作业成百上千个独立将单独处理各个帧的任务。 相应地，任务处理器将调用呈现应用程序和所有相关的流程来呈现每个所需的框架，以及执行任何其他操作 （例如，正在呈现的帧复制到的存储位置）。

>[AZURE.NOTE] 作业管理器和任务处理器模板无关的以便您可以选择使用这两者，或只是其中之一，根据要求计算作业和您的首选项。

下面的关系图中所示，使用这些模板的计算作业将经历三个阶段︰

1. （例如，应用程序、 web 服务等） 的客户端代码提交到 Azure，其作业管理器任务作业管理器程序指定的批服务作业。

2. 批次服务在计算节点上运行作业管理器任务和作业拆分器上启动任务处理器任务，指定的数量，因为许多计算节点的要求，根据参数和作业拆分器代码中的规范。

3. 任务处理器任务独立，并行运行，以处理输入的数据并生成的输出数据。

![显示客户端代码如何与批服务交互图][diagram01]

## <a name="prerequisites"></a>系统必备组件

要使用批次模板，您需要︰

* 一台计算机使用 Visual Studio 2015，或更高版本，已安装在其上。

* 批次模板，它们可从[Visual Studio 库][vs_gallery]为 Visual Studio 扩展。 有两种方法来获取模板︰

  * 安装使用 Visual Studio 中的**扩展和更新**对话框中的模板 (有关详细信息，请参阅[查找和使用 Visual Studio 扩展][vs_find_use_ext])。 在**扩展和更新**对话框中，搜索并下载以下两个扩展名的文件︰

    * Azure 批处理作业管理器作业拆分
    * Azure 批处理任务处理器

  * Visual Studio 中的联机库中下载模板︰ [Microsoft Azure 批项目模板][vs_gallery_templates]

* 如果您打算使用[应用程序数据包](batch-application-packages.md)功能部署作业管理器并到批处理任务处理器计算节点，您需要链接到您的批处理帐户的存储帐户。

## <a name="preparation"></a>准备工作

建议创建一个解决方案，它可以包含您的作业管理器以及任务处理器，因为这可能会使您的作业管理器和任务处理器程序之间共享代码变得更容易。 若要创建此解决方案，请执行以下步骤︰

1. 打开 Visual Studio 2015年并选择**文件** > **New** > **项目**。

2. 在**模板**下展开**其他项目类型**， **Visual Studio 解决方案**，请单击，然后选择**空白解决方案**。

3. 键入一个名称来描述您的应用程序和此解决方案 (例如，"LitwareBatchTaskPrograms") 的目的。

4. 若要创建新的解决方案，请单击**确定**。

## <a name="job-manager-template"></a>作业管理器模板

作业管理器模板可帮助您实现的作业管理器任务，可以执行以下操作︰

* 将作业拆分为多个任务。
* 提交在批处理运行这些任务。

>[AZURE.NOTE] 作业管理器任务有关的详细信息，请参阅[批处理功能概述的开发](batch-api-basics.md#job-manager-task)。

### <a name="create-a-job-manager-using-the-template"></a>创建作业管理器使用的模板

将作业管理器添加到您在前面创建的解决方案，请执行以下步骤︰

1. 在 Visual Studio 2015年打开现有的解决方案。

2. 在解决方案资源管理器中，右击解决方案，再单击**添加** > **新的项目**。

3. 下**C#**，**云**，请单击，然后单击**Azure 批处理作业管理器作业拆分**。

4. 键入一个名称，描述您的应用程序并将该项目标识为作业管理器 (如"LitwareJobManager")。

5. 若要创建项目，请单击**确定**。

6. 最后，生成项目，以强制 Visual Studio 加载所有被引用的 NuGet 程序包，并验证该项目有效，您才能修改它。

### <a name="job-manager-template-files-and-their-purpose"></a>作业管理器模板文件和它们的用途

创建项目时使用作业管理器模板时，它生成代码文件的三个的组︰

* 主程序文件 (Program.cs) 中。 此文件中包含的程序入口点和顶级的异常处理。 通常不需要修改此。

* 该框架目录。 这包含负责完成作业管理器程序 – 解参数，将任务添加到批处理作业等的样板工作的文件。通常不需要修改这些文件。

* 作业分隔文件 (JobSplitter.cs)。 这是您将您的应用程序特定逻辑拆分为多个任务的作业。

当然您可以为支持作业拆分器代码，根据作业分割逻辑的复杂性还需要添加其他文件。

该模板还会生成标准的.NET 项目文件，如.csproj 文件、 app.config、 packages.config 等。

本节的其余部分描述不同的文件和代码结构，并介绍了每个类的用途。

![Visual Studio 解决方案资源管理器中显示作业管理器模板解决方案][solution_explorer01]

**框架文件**

* `Configuration.cs`︰ 封装作业配置数据，如批次帐户详细信息、 链接的存储帐户凭据、 作业和任务信息和作业参数的加载。 它还提供对通过 Configuration.EnvironmentVariable 类的批处理定义的环境变量 （请参阅批处理文档中的任务的环境设置） 的访问。

* `IConfiguration.cs`︰ 以便您能够单元测试时，使用伪造或模拟配置对象您作业拆分器来抽象配置类的实现。

* `JobManager.cs`︰ 协调作业管理器程序的组成部分。 它负责初始化作业拆分器调用作业拆分器和调度作业拆分器返回到任务提交者的任务。

* `JobManagerException.cs`︰ 表示需要作业管理器终止错误。 JobManagerException 用于包装 '预期' 的错误终止的一部分提供特定诊断信息的位置。

* `TaskSubmitter.cs`︰ 此类添加到批处理作业返回作业拆分任务负责。 作业管理器管理类聚合的任务序列分成批次提高效率，但及时添加到该作业，然后调用 TaskSubmitter.SubmitTasks 在后台线程上为每个批次。

**作业分隔**

`JobSplitter.cs`︰ 此类包含特定于应用程序的逻辑分割为多个任务的作业。 框架调用 JobSplitter.Split 方法来获取任务，它将添加到该作业方法将返回这些序列。 这是将在其中插入作业的逻辑类。 实现拆分方法返回的 CloudTask 实例，表示要到该分区作业的任务序列。

**标准的.NET 命令行项目文件**

* `App.config`︰ 标准的.NET 应用程序配置文件。

* `Packages.config`︰ 标准 NuGet 程序包依赖项文件。

* `Program.cs`︰ 包含的程序入口点和顶级的异常处理。

### <a name="implementing-the-job-splitter"></a>实施作业拆分器

打开作业管理器模板项目时，项目会按默认设置打开 JobSplitter.cs 文件。 可以通过使用 split （） 方法显示下面工作负载中实现拆分逻辑的任务︰

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] 在批注部分`Split()`方法是针对于您通过添加逻辑来修改拆分为不同的任务的作业的作业管理器模板代码的唯一部分。 如果您想要修改模板的不同部分，请确保掌握批的工作原理和试验几个[批处理代码示例][github_samples]。

Split （） 实现有权︰

* 作业参数，通过`_parameters`字段。
* CloudJob 表示该作业，通过`_job`字段。
* CloudTask 对象，该对象表示通过的作业管理器任务，`_jobManagerTask`字段。

您`Split()`的实现不需要直接向作业添加任务。 相反，您的代码应返回的 CloudTask 对象序列，它们将被添加到作业自动调用作业拆分框架类。 通常使用 C# 的迭代器 (`yield return`) 功能来实现作业拆分器，因为这将允许开始尽可能快地运行，而不是等待所有任务计算的任务。

**拆分器的作业失败**

如果您的作业拆分器遇到了错误，应该是︰

* 终止序列使用 C#`yield break`语句，在这情况下作业管理器将被视为为成功;或

* 引发的异常，此时，作业管理器将会被视为失败和可能取决于客户端具有如何配置重试）。

在这两种情况下，任何任务已经由作业拆分器返回并添加到批处理作业将可以运行。 如果您不希望出现这种情况，然后您可以︰

* 从作业拆分器返回之前终止作业

* 制定整个任务集合中的返回它之前 (即返回`ICollection<CloudTask>`或`IList<CloudTask>`而不是实现您使用 C# 迭代器的作业拆分器)

* 使用任务相关性进行取决于作业管理器成功完成的所有任务

**作业管理器的重试次数**

如果作业管理器出现故障，它可能由批服务，具体取决于客户端的重试设置重试。 一般情况下，这是安全的因为当框架将任务添加到作业，它将忽略任何已存在的任务。 但是，如果计算任务是昂贵的您可能不希望带来的损失，重新计算任务已添加到该作业;相反，如果重新运行不能保证生成同一个任务 Id 然后忽略重复项行为将不启动。 在这些情况下您应该设计您的作业拆分器来检测工作已完成，并不重复，例如通过执行生成任务开始之前的 CloudJob.ListTasks。

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>退出代码和作业管理器模板中的异常

退出代码和异常提供了一种机制来确定的运行程序，结果，他们可以帮助识别任何问题的程序的执行。 作业管理器模板实现的退出代码和在这一节中所述的例外情况。

使用作业管理器模板实现作业管理器任务可返回三个可能的退出代码︰

| 代码 | 说明 |
|------|-------------|
| 0    | 作业管理器已成功完成。 作业分隔代码运行直到完成，和所有任务被都添加到该作业。 |
| 1    | 作业管理器任务失败，异常为在 '预期' 计划的一部分。 异常被翻译成 JobManagerException 的诊断信息并在可能的情况下，解决故障的建议。 |
| 2    | 作业管理器任务失败，出现异常。 该异常被记录到标准输出，但作业管理器不能添加任何其他的诊断或修复信息。 |

在作业管理器任务失败，某些任务可能仍被添加到服务之前发生错误。 这些任务将运行正常。 有关此代码路径的讨论参见上面的"作业拆分器失败"。

所有返回的异常的信息写入 stdout.txt 和 stderr.txt 文件。 有关详细信息，请参见[错误处理](batch-api-basics.md#error-handling)。

### <a name="client-considerations"></a>客户端考虑事项

调用基于此模板的作业管理器时，本部分将介绍一些客户端的实现要求。 传递参数和环境设置的详细信息，请参阅[如何传递参数和环境变量从客户端代码](#pass-environment-settings)。

**必需的凭据**

要将任务添加到 Azure 批处理作业，作业管理器任务要求您 Azure 批帐户 URL 和密钥。 您必须通过在环境变量名为 YOUR_BATCH_URL 和 YOUR_BATCH_KEY。 您可以设置这些在作业管理器任务环境设置。 例如，在 C# 客户端︰

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**存储凭据**

通常情况下，客户端不需要提供给作业管理器任务的链接的存储帐户凭据，因为 （a） 大多数作业管理器不需要显式访问链接的存储帐户和 （b） 该链接的存储帐户通常提供给所有任务作为公共环境设置的作业。 如果您不提供常见的环境设置中，通过该链接的存储帐户和作业管理器需要链接的存储访问，然后应按以下方式提供链接的存储凭据︰

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**作业管理器任务设置**

客户端应将作业管理器*killJobOnCompletion*标志设置为**false**。

它是通常情况下，客户端将*runExclusive*设置为**false**。

客户端应该使用*resourceFiles*或*applicationPackageReferences*集合作业管理器可执行文件 （和其所需的 Dll） 部署到计算节点。

如果该操作失败，无法将默认情况下，试作业管理器。 这取决于您的作业管理器逻辑，客户端可能需要启用通过*约束*的重试次数/*maxTaskRetryCount*。

**作业设置**

如果作业拆分器发出具有相关性的任务，客户端必须设置作业的 usesTaskDependencies 为 true。

在作业拆分器模型中，很少客户端想要将任务添加到超过作业拆分器所创建的作业。 客户端应该因此通常设置作业的*onAllTasksComplete*到**terminatejob**。

## <a name="task-processor-template"></a>任务处理器模板

任务处理器模板可帮助您实现的任务处理器，可以执行以下操作︰

* 设置每个批处理任务运行所需的信息。
* 运行批处理中的每个任务所需的所有操作。
* 将任务输出保存到永久存储区。

尽管任务处理器不需要运行批处理任务，使用任务处理器的主要优点是，它提供了一个包装在一个位置实现任务执行的所有操作。 例如，如果您需要的每个任务的上下文中运行多个应用程序，或者如果您需要复制数据到永久存储区完成后各项任务。

任务处理者执行的操作可以是简单或复杂，并尽可能多或尽可能少，根据您的工作负载的要求。 此外，通过实施到一个任务处理器的所有任务操作，可以很容易地更新或添加操作基于更改应用程序或工作负载需求。 但是，在某些情况下任务处理器可能不合适的最佳解决方案针对您的实施为这可能增加不必要的复杂性，例如运行时，可以从一个简单的命令行快速启动的作业。

### <a name="create-a-task-processor-using-the-template"></a>创建任务处理器使用的模板

若要将任务处理器添加到您在前面创建的解决方案，请执行以下步骤︰

1. 在 Visual Studio 2015年打开现有的解决方案。

2. 在解决方案资源管理器中，右击解决方案，单击**添加**，然后单击**新建项目**。

3. **C#**，在**云**，请单击，然后单击**Azure 批处理任务处理器**。

4. 键入一个名称，描述您的应用程序并将该项目标识为任务处理器 (例如"LitwareTaskProcessor")。

5. 若要创建项目，请单击**确定**。

6. 最后，生成项目，以强制 Visual Studio 加载所有被引用的 NuGet 程序包，并验证该项目有效，您才能修改它。

### <a name="task-processor-template-files-and-their-purpose"></a>它们的目的和任务处理器模板文件

创建使用任务处理器模板的项目时，它生成代码文件的三个的组︰

* 主程序文件 (Program.cs) 中。 此文件中包含的程序入口点和顶级的异常处理。 通常不需要修改此。

* 该框架目录。 这包含负责完成作业管理器程序 – 解参数，将任务添加到批处理作业等的样板工作的文件。通常不需要修改这些文件。

* 任务处理器文件 (TaskProcessor.cs)。 这是您将您的特定于应用程序逻辑 （通常由拨出到现有的可执行文件） 执行一项任务。 同时这里是前期和后期处理代码，如下载的其他数据或将结果文件上载。

当然您可以为支持任务处理器代码，根据作业分割逻辑的复杂性还需要添加其他文件。

该模板还会生成标准的.NET 项目文件，如.csproj 文件、 app.config、 packages.config 等。

本节的其余部分描述不同的文件和代码结构，并介绍了每个类的用途。

![Visual Studio 解决方案资源管理器中显示任务处理器模板解决方案][solution_explorer02]

**框架文件**

* `Configuration.cs`︰ 封装作业配置数据，如批次帐户详细信息、 链接的存储帐户凭据、 作业和任务信息和作业参数的加载。 它还提供对通过 Configuration.EnvironmentVariable 类的批处理定义的环境变量 （请参阅批处理文档中的任务的环境设置） 的访问。

* `IConfiguration.cs`︰ 以便您能够单元测试时，使用伪造或模拟配置对象您作业拆分器来抽象配置类的实现。

* `TaskProcessorException.cs`︰ 表示需要作业管理器终止错误。 TaskProcessorException 用于包装 '预期' 的错误终止的一部分提供特定诊断信息的位置。

**任务处理器**

* `TaskProcessor.cs`︰ 运行该任务。 框架调用 TaskProcessor.Run 方法。 这是任务的将在其中插入您的特定于应用程序的逻辑的类。 实现对 Run 方法︰
  * 分析和验证任何任务参数
  * 编写用于任何您想要调用的外部程序的命令行
  * 记录任何诊断信息，您可能需要进行调试
  * 启动过程中使用该命令行
  * 等待进程退出
  * 捕获过程来确定它是成功还是失败的退出代码
  * 要保存到永久存储区保存任何输出文件

**标准的.NET 命令行项目文件**

* `App.config`︰ 标准的.NET 应用程序配置文件。
* `Packages.config`︰ 标准 NuGet 程序包依赖项文件。
* `Program.cs`︰ 包含的程序入口点和顶级的异常处理。

## <a name="implementing-the-task-processor"></a>实现任务处理器

打开任务处理器模板项目时，项目会按默认设置打开 TaskProcessor.cs 文件。 可以在任务的运行的逻辑工作负荷使用执行 run （） 方法如下所示︰

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] 执行 run （） 方法中的批注部分是供您修改通过工作负载中添加任务的运行的逻辑的任务处理器模板代码的唯一部分。 如果您想要修改的模板的其他分区，请先熟悉批通过查看批次文档和试用几个批处理代码示例的工作方式。

执行 run （） 方法并负责启动命令行，启动一个或多个进程，等待所有进程完成，保存结果，最后返回退出代码。 执行 run （） 方法将任务实现处理逻辑的地方。 任务处理器框架调用执行 run （） 方法为您;您不需要调用它自己。

执行 run （） 实现有权︰

* 任务参数，通过`_parameters`字段。
* 作业和任务 id，通过`_jobId`和`_taskId`字段。
* 任务配置，通过`_configuration`字段。

**任务失败**

万一发生故障，您可以退出执行 run （） 方法通过引发异常，但这样的最大级别的异常处理程序中控制任务的退出代码。 如果您需要控制的退出代码，以便您可以出于诊断目的，例如区分不同类型的故障，或者因为某些故障模式应终止作业和其他人不应该然后应通过返回非零退出代码退出执行 run （） 方法。 这将成为任务的退出代码。

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>退出代码和该任务处理器模板中的异常

退出代码和异常提供了一种机制来确定的运行程序，结果，他们可以帮助识别任何执行程序的问题。 任务处理器模板实现的退出代码和在这一节中所述的例外情况。

用任务处理器模板实现任务处理器任务可返回三个可能的退出代码︰

| 代码 | 说明 |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | 任务处理器已完成运行。 请注意，这并不意味着，只有您所调用的程序是否成功 — 任务处理器成功调用它，并执行任何后续处理并且无例外。 退出代码的含义取决于所调用的程序 – 退出代码 0 通常意味着程序已经成功完成并退出的任何其他代码意味着程序失败。 |
| 1    | 任务处理器失败，异常为在 '预期' 计划的一部分。 异常被转换为`TaskProcessorException`的诊断信息并在可能的情况下，解决故障的建议。 |
| 2    | 任务处理器失败，出现异常。 该异常被记录到标准输出，但任务处理器不能添加任何其他的诊断或修复信息。 |

>[AZURE.NOTE] 如果调用程序使用退出代码 1 和 2 以指示特定的故障模式，然后使用任务处理器错误退出代码 1 和 2 是不明确的。 通过编辑 Program.cs 文件中的异常情况，可以到各具特色的退出代码更改这些任务处理器错误代码。

所有返回的异常的信息写入 stdout.txt 和 stderr.txt 文件。 有关详细信息，请参阅批处理文档中的错误处理。

### <a name="client-considerations"></a>客户端考虑事项

**存储凭据**

如果任务处理器使用 Azure blob 存储来保持输出，例如使用文件约定帮助程序库中，则需要访问*任何一个*云存储帐户凭据*或*blob 容器 URL 包括共享的访问签名 (SAS)。 该模板包含支持提供凭据通过常见的环境变量。 您的客户端可以通过存储凭据，如下所示︰

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

就可通过 TaskProcessor 类中存储帐户`_configuration.StorageAccount`属性。

如果您愿意使用 SAS 容器 URL，您也可以通过作业常见环境设置，传递这但任务处理器模板当前不包含内置支持。

**存储设置**

建议客户端或作业管理器任务创建任务要求将任务添加到该作业之前任何容器。 这是必填字段使用 SAS 容器 URL，如果这种情况下 URL 未包含创建该容器的权限。 建议即使传递存储帐户凭据，因为它保存无需在容器上调用 CloudBlobContainer.CreateIfNotExistsAsync 的每项任务。

## <a name="pass-parameters-and-environment-variables"></a>传递的参数和环境变量

### <a name="pass-environment-settings"></a>通过环境设置

客户端可以将信息传递给环境设置的窗体中的作业管理器任务。 生成将作为计算作业的一部分运行的任务处理器任务时，然后可以通过作业管理器任务使用此信息。 您可以将环境设置为传递的信息的示例包括︰

* 存储帐户的名称和帐户关键字
* 批次帐户 URL
* 批次帐户密钥

批次服务都有一个简单的机制，若要通过将环境设置传递给作业管理器任务`EnvironmentSettings`在[Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask]。

例如，若要获取`BatchClient`实例为一个批次的帐户，您可以传递来自客户端的环境变量进行编码的 URL 和共享密钥批帐户凭据。 同样，若要访问链接到批次客户的存储帐户，可以将存储帐户名称和存储帐户密钥作为环境变量。

### <a name="pass-parameters-to-the-job-manager-template"></a>将参数传递到作业管理器模板

在许多情况下，最好将每个工作参数传递到作业管理器的任务，来控制拆分过程作业或作业对任务进行配置。 你可以通过上传 JSON 文件命名为 parameters.json，为作业管理器任务的资源文件。 参数会在`JobSplitter._parameters`在作业管理器模板中的字段。

>[AZURE.NOTE] 内置参数处理程序支持仅字符串的字符串字典。 如果您想要作为参数值传递复杂的 JSON 值，您需要将这些字符串作为信息传递和分析其在作业拆分器，或修改框架的`Configuration.GetJobParameters`方法。

### <a name="pass-parameters-to-the-task-processor-template"></a>将参数传递给任务处理器模板

您还可以将参数传递到使用任务处理器模板实现的各项任务。 就像使用作业管理器模板，任务处理器模板查看命名的资源文件

parameters.json，并且如果找到它作为参数字典加载它。 有几种情况下，如何将参数传递给任务处理器︰

* 重复使用 JSON 的作业参数。 这如果非常有效的唯一参数是作业范围内的 （例如，呈现高度和宽度）。 为此，请在作业拆分器中创建的 CloudTask 时添加作业管理器任务 ResourceFiles parameters.json 资源文件对象的引用 (`JobSplitter._jobManagerTask.ResourceFiles`) 到 CloudTask 的 ResourceFiles 集合。

* 生成并特定于任务的 parameters.json 文档上载作业拆分器执行，过程中引用该任务的资源的文件集合中的 blob。 这是有必要的如果不同的任务具有不同的参数。 一个示例可能是 3D 渲染方案其中的帧索引传递给任务作为参数。

>[AZURE.NOTE] 内置参数处理程序支持仅字符串的字符串字典。 如果您想要作为参数值传递复杂的 JSON 值，您需要将这些字符串作为信息传递和分析它们在任务处理器，或修改框架的`Configuration.GetTaskParameters`方法。

## <a name="next-steps"></a>下一步行动

### <a name="persist-job-and-task-output-to-azure-storage"></a>保留作业和任务输出到 Azure 存储

另一个有用的工具，在批处理解决方案的开发是[Azure 批处理文件约定][nuget_package]。 使用此.NET 类库 （目前在预览） 批.NET 应用程序中的轻松地存储和检索任务输出与 Azure 存储。 [保持 Azure 批处理作业和任务输出](batch-task-output.md)包含库和其使用的全面讨论。

### <a name="batch-forum"></a>批次论坛

[Azure 批论坛][forum]在 MSDN 上是很好的讨论批和询问服务有关的问题。 通过有用"粘性"的帖子，在总部和出现时生成批处理解决方案发布您的问题。

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
