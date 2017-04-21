<properties
    pageTitle="教程-学习如何使用 Azure 批.NET 库 |Microsoft Azure"
    description="了解 Azure 批以及如何开发与示例方案批服务的基本概念。"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>学习如何使用 Azure 批库.net

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

学习基础知识的[Azure 批][azure_batch]和[批处理.NET] [net_api]库在这篇文章中的我们讨论一个 C# 示例应用程序执行步骤的。 我们要看如何此示例应用程序利用批服务处理并行负载在云中，以及如何与它交互使用[Azure 存储](../storage/storage-introduction.md)临时文件和检索。 您将了解常见的批处理应用程序工作流技术。 您还将作业、 任务、 池，如批次，主要组件的基本理解和计算节点。

![批次解决方案工作流 （基本）][11]<br/>

## <a name="prerequisites"></a>系统必备组件

本文假定您具有 C# 和 Visual Studio 的工作知识。 它还假定您能够满足下面的 Azure 和批处理和存储服务指定的帐户创建要求。

### <a name="accounts"></a>帐户

- **Azure 帐户**︰ 如果还没有[创建免费的 Azure 帐户]的 Azure 订阅[azure_free_account]。
- **批次帐户**︰ 一旦有了 Azure 的订阅，[创建一个批处理 Azure 帐户](batch-account-create-portal.md)。
- **存储帐户**︰ 请参阅[有关 Azure 存储帐户](../storage/storage-create-storage-account.md)中[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)。

> [AZURE.IMPORTANT] 批次当前支持*仅***通用**存储帐户类型，如步骤 5 中所述[关于 Azure 存储帐户](../storage/storage-create-storage-account.md)中[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)。

### <a name="visual-studio"></a>Visual Studio

您必须具有**Visual Studio 2015年**生成的样例项目。 在[概述 Visual Studio 2015年产品]可以找到自由和试用版本的 Visual Studio 的[visual_studio]。

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial*代码示例

[DotNetTutorial] [github_dotnettutorial]示例是[azure 的批次的样本]中发现的许多代码示例之一[github_samples]在 GitHub 上的存储库。 您可以下载示例，通过单击存储库的主页页面上的**下载 ZIP**按钮或单击[azure 批样本 master.zip] [github_samples_zip]直接下载链接。 一旦您解压缩 ZIP 文件的内容，将在以下文件夹中查找解决方案︰

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure 批资源管理器 （可选）

[Azure 批资源管理器][github_batchexplorer]是[azure-批处理示例]中包含免费的实用工具[github_samples]在 GitHub 上的存储库。 尽管并不要求要完成本教程，它可以同时开发和调试您的批处理解决方案。

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial 示例项目概述

*DotNetTutorial*代码示例是一个 Visual Studio 2015年解决方案的两个项目组成︰ **DotNetTutorial**和**TaskApplication**。

- **DotNetTutorial**是客户端应用程序交互的批处理和存储服务上执行并行的工作负荷计算节点 （虚拟机）。 在本地工作站上运行 DotNetTutorial。

- **TaskApplication**是在 Azure 来执行实际的工作中计算节点运行的程序。 在示例中，`TaskApplication.exe`分析从 Azure 存储 （输入文件） 下载文件中的文本。 然后它生成一个文本文件 （输出文件） 包含在输入文件中出现的前三个单词的列表。 它创建的输出文件之后，TaskApplication 将文件上载到 Azure 存储中。 这使得可供下载的客户端应用程序。 在批服务中多个计算节点上并行运行的 TaskApplication。

下图演示通过客户端应用程序、 *DotNetTutorial*，以及应用程序执行的任务， *TaskApplication*执行的主要操作。 此基本工作流是典型的很多都使用批创建的计算解决方案。 虽然它并不演示每个批次服务中可用的功能，几乎每个批次的方案将包括类似的流程。

![工作流批处理示例][8]<br/>

[**第 1 步。**](#step-1-create-storage-containers) 在 Azure Blob 存储创建的**容器**。<br/>
[**第 2 步。**](#step-2-upload-task-application-and-data-files) 将任务应用程序文件和输入的文件上载到容器中。<br/>
[**第 3 步。**](#step-3-create-batch-pool) 创建一个批处理**池**。<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a。** 他们加入了池，池**StartTask**节点下载任务二进制文件 (TaskApplication)。<br/>
[**第 4 步。**](#step-4-create-batch-job) 创建批处理**作业**。<br/>
[**第 5 步。**](#step-5-add-tasks-to-job) 将**任务**添加到该作业中。<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a。** 计划任务在节点上执行。<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b。** 每个任务从 Azure 存储下载其输入的数据，然后开始执行。<br/>
[**第 6 步。**](#step-6-monitor-tasks) 监视任务。<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a。** 完成任务后，他们将其输出数据上载到 Azure 存储。<br/>
[**第 7 步。**](#step-7-download-task-output) 从存储下载任务输出。

正如提到的不是每个批次的解决方案执行这些具体步骤，并可能包括多，但*DotNetTutorial*示例应用程序演示批处理解决方案中找到的通用流程。

## <a name="build-the-dotnettutorial-sample-project"></a>生成的*DotNetTutorial*示例项目

在*DotNetTutorial*项目的成功运行此示例之前，必须指定批处理和存储帐户凭据`Program.cs`文件。 如果您有没有完成此操作，请在 Visual Studio 中打开的解决方案，通过双击`DotNetTutorial.sln`解决方案文件。 或通过在 Visual Studio 内打开它从**文件 > 打开 > 项目/解决方案**菜单。

打开`Program.cs` *DotNetTutorial*项目中。 指定该文件的顶部附近，然后添加您的凭据︰

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] 如上所述，当前必须在 Azure 存储中指定**通用**存储帐户的凭据。 批处理应用程序使用 blob 存储在**通用**存储帐户内。 没有指定选择的*Blob 存储*帐户类型创建存储帐户的凭据。

在[Azure 门户网站]可以找到批处理和存储帐户凭据中的每个服务帐户刀片式服务器[azure_portal]:

![批处理在门户中的凭据][9]
![在门户中的存储凭据][10]<br/>

现在，您已经更新项目使用您的凭据，请右击解决方案资源管理器中的解决方案，然后单击**生成解决方案**。 如果系统提示您时，请确认任何 NuGet 程序包的还原。

> [AZURE.TIP] 如果 NuGet 程序包不会自动还原，或者查看关于故障恢复软件包错误，确保您拥有[NuGet 程序包管理器][nuget_packagemgr]安装。 然后，启用丢失包的下载。 请参阅[启用包还原过程中生成][nuget_restore]启用程序包的下载。

在以下部分中，我们将分解成处理工作负荷在批次服务中，在执行这些步骤的示例应用程序并讨论这些步骤的详细。 我们鼓励您在工作浏览这篇文章的其余部分，因为不是每个示例中的代码行讨论时参考在 Visual Studio 中打开的解决方案。

导航到顶部的`MainAsync`在*DotNetTutorial*项目的方法`Program.cs`文件可从步骤 1 开始。 每个步骤下面，然后大致遵循前进过程中的方法调用`MainAsync`。

## <a name="step-1-create-storage-containers"></a>步骤 1︰ 创建存储容器

![在 Azure 存储中创建容器][1]
<br/>

批次包括与 Azure 存储进行交互的内置支持。 在您的存储帐户的容器将提供在批帐户下运行的任务所需的文件。 容器还提供一个位置来存储任务生成的输出数据。 *DotNetTutorial*客户端应用程序的第一件事是[Azure Blob 存储](../storage/storage-introduction.md)在创建的三个容器︰

- **应用程序**︰ 此容器将存储任务，以及任何其依赖项，如 Dll 来运行该应用程序。
- **输入**︰ 任务将下载要从*输入*容器处理的数据文件。
- **输出**︰ 当任务完成时输入的文件的处理时，他们会将结果上传到*输出*容器。

为了与存储帐户进行交互并创建容器时，我们使用[Azure 存储.NET 客户端库][net_api_storage]。 我们使用[CloudStorageAccount]创建帐户的引用[net_cloudstorageaccount]，并从它创建[CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

我们使用`blobClient`引用整个应用程序并将其作为参数传递给几个方法。 紧跟在后面上面拨给我们的代码块中的示例是`CreateContainerIfNotExistAsync`实际创建容器。

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

一旦创建容器应用程序现在可以上载任务将使用的文件。

> [AZURE.TIP] [如何使用 Blob 存储从.NET](../storage/storage-dotnet-how-to-use-blobs.md)提供了很好地使用 Azure 存储容器和 blob 的概括。 随着您开始使用批处理，它应该是阅读材料列表的顶部附近。

## <a name="step-2-upload-task-application-and-data-files"></a>步骤 2︰ 将任务应用程序和数据文件上载

![将任务应用程序和输入 （数据） 的文件上载到容器][2]
<br/>

在文件上载操作中， *DotNetTutorial*首先定义**应用程序**和**输入**文件路径的集合存在本地计算机上。 然后它将这些文件上载到您在上一步中创建的容器。

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

有两种方法在`Program.cs`的上载过程中涉及到︰

- `UploadFilesToContainerAsync`︰ 此方法返回一个集合的[ResourceFile] [net_resourcefile]对象 （在下面讨论） 和内部调用`UploadFileToContainerAsync`上载*可*参数中传递的每个文件。
- `UploadFileToContainerAsync`︰ 这是该方法实际执行上载文件，并创建[ResourceFile] [net_resourcefile]对象。 之后上传的文件，它获取该文件的共享的访问签名 (SA) 并返回一个 ResourceFile 对象，该对象代表。 共享的访问签名也将在下文。

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [net_resourcefile]提供下载到计算节点运行该任务之前的 Azure 存储中的文件的 URL 与批处理中的任务。 [ResourceFile.BlobSource] [net_resourcefile_blobsource]属性指定文件的完整 URL，因为它在 Azure 存储中存在。 该 URL 还可以包括提供安全访问该文件的共享的访问签名 (SAS)。 大多数任务类型批.NET 中的包括一个*ResourceFiles*属性，包括︰

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

DotNetTutorial 示例应用程序不使用 JobPreparationTask 或 JobReleaseTask 任务类型，但您可以阅读更多有关它们在[Azure 批次运行作业准备工作和完成任务计算节点](batch-job-prep-release.md)。

### <a name="shared-access-signature-sas"></a>共享的访问签名 (SAS)

签名是共享的访问字符串的 — — 当作为 URL 的一部分 — — 提供安全访问容器和 Azure 存储中的 blob。 应用程序使用 blob 和容器共享 DotNetTutorial 访问签名的 Url，并演示如何从存储服务中获取这些共享的访问签名字符串。

- **Blob 共享访问签名**︰ 池的 StartTask 在 DotNetTutorial 中使用 blob 共享访问签名，当它从存储下载的应用程序二进制文件和输入的数据文件 （请参阅下面的步骤 #3）。 `UploadFileToContainerAsync`在 DotNetTutorial 的`Program.cs`包含获取每个 blob 共享的访问签名的代码。 它是通过调用[CloudBlob.GetSharedAccessSignature][net_sas_blob]。

- **容器共享访问签名**︰ 为每个任务完成后它在计算节点上的工作，它将其输出文件上载到 Azure 存储中的*输出*容器。 为此，TaskApplication 使用容器共享的访问签名时上载文件作为路径的一部分提供给容器的写访问权限。 获取容器共享的访问签名以类似的方式为时进行获取 blob 共享访问签名。 在 DotNetTutorial 中，您将找到`GetContainerSasUrl`帮助器方法将调用[CloudBlobContainer.GetSharedAccessSignature] [net_sas_container]若要这样做。 您将读多 TaskApplication 如何使用容器共享中的访问签名"步骤 6︰ 监视器任务。"

> [AZURE.TIP] 签出共享的访问签名两部分的系列[第 1 部分︰ 了解共享的访问签名 (SAS) 模型](../storage/storage-dotnet-shared-access-signature-part-1.md)和[第 2 部分︰ 创建和使用共享的访问签名 (SAS) 的 Blob 存储](../storage/storage-dotnet-shared-access-signature-part-2.md)，以了解更多关于提供安全地访问您的存储帐户中的数据。

## <a name="step-3-create-batch-pool"></a>步骤 3︰ 创建批处理池

![创建一个批处理池][3]
<br/>

一个批次**池**是计算节点 （虚拟机），在其批次执行作业的任务的集合。

它将应用程序和数据文件上载到的存储帐户后， *DotNetTutorial*将通过使用批处理.NET 库开始批服务与它交互。 为此，请[BatchClient] [net_batchclient]第一次创建︰

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

接下来，调用的批处理帐户中创建池的计算节点`CreatePoolAsync`。 `CreatePoolAsync`使用[BatchClient.PoolOperations.CreatePool] [net_pool_create]方法来实际批服务中创建一个池。

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

当您创建一个池与[CreatePool][net_pool_create]，指定几个参数的计算节点、[节点的大小](../cloud-services/cloud-services-sizes-specs.md)和节点的操作系统等。 在*DotNetTutorial*，我们将使用[CloudServiceConfiguration] [net_cloudserviceconfiguration]指定从[云服务](../cloud-services/cloud-services-guestos-update-matrix.md)的 Windows Server 2012 R2。 但是，通过指定[VirtualMachineConfiguration] [net_virtualmachineconfiguration]相反，您可以创建从市场图像创建的节点池包括 Windows 和 Linux 图像 — — 信息，请参阅[设置 Linux 计算 Azure 批池中的节点](batch-linux-nodes.md)。

> [AZURE.IMPORTANT] 收费为批次中的计算资源。 若要最小化成本，可以降低`targetDedicated`为 1 之前运行该示例。

这些物理节点的属性，您还可以指定[StartTask] [net_pool_starttask]的池。 该节点加入池，以及每次重新启动节点，每个节点上执行 StartTask。 StartTask 是在执行任务之前的计算节点上安装应用程序时特别有用。 例如，如果任务通过使用 Python 脚本中处理的数据，您可以使用 StartTask 在计算节点上安装 Python。

在此示例应用程序中，将 StartTask 复制存储从下载的文件 (这由使用[StartTask][net_starttask]。[ResourceFiles] [net_starttask_resourcefiles]属性) 与在该节点上运行的*所有*任务可以都访问该共享目录的 StartTask 工作目录中。 实质上，此操作将复制`TaskApplication.exe`以及其依赖项的每个节点上的共享目录节点连接池，以便在该节点上运行的所有任务都可以访问它。

> [AZURE.TIP] Azure 的批的**应用程序数据包**功能提供了另一种方法来获取计算节点到应用程序池。 有关详细信息，请参阅[与 Azure 批处理应用程序包的应用程序部署](batch-application-packages.md)。

上述代码段中的值得一提也是 StartTask 的*命令行*属性中的两个环境变量的使用︰ `%AZ_BATCH_TASK_WORKING_DIR%` ， `%AZ_BATCH_NODE_SHARED_DIR%`。 在批处理池中每个计算节点会自动获配特定于批处理的几个环境变量。 执行任务的任何进程有权访问这些环境变量。

> [AZURE.TIP] 若要了解更多有关环境批池和有关任务的工作目录中的计算节点可用的变量，请参阅[批处理功能概述的开发](batch-api-basics.md)中的[任务的环境设置](batch-api-basics.md#environment-settings-for-tasks)和[文件和目录](batch-api-basics.md#files-and-directories)部分。

## <a name="step-4-create-batch-job"></a>步骤 4︰ 创建批处理作业

![创建批处理作业][4]<br/>

批处理**作业**是集合的任务，并与池的计算节点相关联。 在关联的池的计算节点上执行作业中的任务。

您可以使用作业不仅用于组织和跟踪相关工作负荷，而且相对于批帐户中的其他作业的作业优先级以及提出某些约束条件 — 例如最大运行时库的作业 （并通过扩展，其任务） 的任务。 但是，在此示例中，该作业是仅与在步骤 3 中创建的池相关联的。 没有附加属性进行配置。

所有批处理作业都都与一个特定的池关联。 这个关联表示哪些节点执行该作业的任务。 通过使用[CloudJob.PoolInformation] ，您指定转换︰[net_job_poolinfo]属性，如以下代码片段中所示。

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

既然已创建一个作业，任务添加要执行的工作。

## <a name="step-5-add-tasks-to-job"></a>步骤 5︰ 将任务添加到作业

![将任务添加到作业][5]<br/>
*（1） 任务添加到该作业、 （2） 任务安排为在节点上，运行和 （3） 任务下载要处理的数据文件*

批次**的任务**是工作的在计算节点执行单独单元。 任务已命令行并运行脚本或可执行文件的命令行中指定的。

实际执行工作，必须将任务添加到作业。 每个[CloudTask] [net_task]配置通过使用命令行属性和[ResourceFiles] [ net_task_resourcefiles] （如池 StartTask） 之前自动执行命令行任务下载到的节点。 在*DotNetTutorial*的示例项目中，每个任务处理只有一个文件。 因此，其 ResourceFiles 集合包含单个元素。

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] 当他们访问环境变量如`%AZ_BATCH_NODE_SHARED_DIR%`或执行应用程序的节点中找不到`PATH`，任务命令行必须以为前缀`cmd /c`。 这明确将执行命令解释程序，指示它在您的命令执行后终止。 这一要求是不必要的如果您的任务在该节点中执行应用程序`PATH`（如*robocopy.exe*或*powershell.exe*） 并不使用任何环境变量。

在`foreach`上面代码段中的循环，您可以看到这样三个命令行参数被传递给*TaskApplication.exe*，构造用于任务的命令行︰

1. **第一个参数**是文件的要处理的路径。 这是它存在于该节点上的文件的本地路径。 ResourceFile 中的对象时`UploadFileToContainerAsync`第一次创建上面，文件名称已用于此属性 （如 ResourceFile 构造函数的一个参数）。 这表明可以与*TaskApplication.exe*相同的目录中找到该文件。

2. **第二个参数**指定 top *N*词应写入的输出文件。 在示例中，这是硬编码这样的前三个单词都被写到输出文件。

3. **第三个参数**是提供到 Azure 存储中的**输出**容器的写访问权限的共享的访问签名 (SAS)。 它将输出文件上载到 Azure 存储时， *TaskApplication.exe*将使用此共享的访问签名 URL。 您可以在此找到代码`UploadFileToContainer`TaskApplication 项目中的方法`Program.cs`文件︰

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>第 6 步︰ 监视任务

![监视任务][6]<br/>
*客户端应用程序 (1) 监视任务的完成和成功状态，和 （2） 任务将结果数据上载到 Azure 存储*

当任务被添加到作业时，它们是自动排队，计划中与此作业关联的池的计算节点上执行。 根据您指定的设置，批次处理所有任务队列、 调度、 重试，和其他任务管理职责为您。 有许多方法监视执行任务。 DotNetTutorial 显示了一个简单的示例，只报告完成和任务失败或成功状态。

在`MonitorTasks`在 DotNetTutorial 的`Program.cs`，有值得讨论的三个批次.NET 概念。 他们下面中出现的顺序列出︰

1. **ODATADetailLevel**︰ 指定[ODATADetailLevel] [net_odatadetaillevel]在列表中操作 （例如获取一份作业的任务） 是确保批处理应用程序的性能非常重要。 [高效查询批处理 Azure 服务](batch-efficient-list-queries.md)向列表中添加阅读如果您打算做任何种类的批处理应用程序内监视的状态。

2. **TaskStateMonitor**: [TaskStateMonitor] [net_taskstatemonitor]监视任务状态提供帮助器实用程序的批处理.NET 应用程序。 在`MonitorTasks`， *DotNetTutorial*将等待所有任务到达[TaskState.Completed] [net_taskstate]在时间限制内。 然后它将终止该作业。

3. **TerminateJobAsync**︰ 终止作业与[JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] （或阻塞 JobOperations.TerminateJob） 将该任务标记为已完成。 一定要这样做，如果批处理解决方案使用[JobReleaseTask][net_jobreltask]。 这是任务的一种特殊，所述[作业准备工作和完成任务](batch-job-prep-release.md)。

`MonitorTasks`方法从*DotNetTutorial*`Program.cs`如下所示︰

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>第 7 步︰ 下载任务输出

![从存储下载任务输出][7]<br/>

现在，作业完成后，可以从 Azure 存储下载任务的输出。 这是通过调用`DownloadBlobsFromContainerAsync`在*DotNetTutorial* `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] 对`DownloadBlobsFromContainerAsync` *DotNetTutorial*应用程序指定应将文件下载到您`%TEMP%`文件夹。 随意修改此输出位置。

## <a name="step-8-delete-containers"></a>步骤 8︰ 删除容器

负责驻留在 Azure 存储中的数据，因为它总是最好删除任何不再需要的批处理作业的 blob。 在 DotNetTutorial 的`Program.cs`，这通过三个帮助器方法调用`DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

该方法本身只是获取到容器，一个引用，然后调用[CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>步骤 9︰ 删除作业和池

在最后一步，提示用户要删除该作业，而且由 DotNetTutorial 应用程序池。 虽然您不负责的作业和任务本身，您**收取计算节点。 因此，我们建议您仅在需要时分配节点。 删除未使用的池，可以是维护过程中的一部分。

BatchClient [JobOperations] [net_joboperations]和[PoolOperations] [net_pooloperations]都有相应的删除方法，称为如果用户确认删除操作︰

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] 请记住，收取计算资源 — — 删除未使用的池将成本降到最低。 还要注意，删除一个池删除此池中的所有计算节点和，在删除该池之后，所有的节点上的数据将无法恢复。

## <a name="run-the-dotnettutorial-sample"></a>运行*DotNetTutorial*示例

当您运行示例应用程序时，控制台输出将类似于以下。 在执行期间，您将体验在暂停`Awaiting task completion, timeout in 00:30:00...`时启动该池的计算节点。 使用[Azure 的门户网站][azure_portal]要监视您的池，计算节点、 作业和任务期间和之后执行。 使用[Azure 的门户网站][ azure_portal] [Azure 存储资源管理器]或[storage_explorers]来查看应用程序创建的存储资源 （容器和 blob）。

典型的执行时间是**大约 5 分钟后**，当您运行该应用程序在其默认配置。

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>下一步行动

随意更改*DotNetTutorial*和*TaskApplication* ，尝试各种不同的计算方案。 例如，请尝试添加执行延迟到*TaskApplication*，如与[Thread.Sleep][net_thread_sleep]，来模拟长时间运行的任务，并监视它们的门户。 尝试添加更多的任务或调整计算节点的数目。 添加逻辑以检查并允许加快执行现有池的使用 (*提示*︰ 检出`ArticleHelpers.cs`在[Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] [批次的 azure 示例]中的项目[github_samples])。

既然您已熟悉一个批次的解决方案的基本工作流程，就应该深入了解到批服务的其他功能。

- 阅读的[批处理功能概述对于开发人员来说](batch-api-basics.md)，这建议为所有新的批处理用户。
- 对其他批次开发文章，**开发深入**[批学习路径]下启动[batch_learning_path]。
- 签出在[TopNWords]中使用批次处理的"前 N 位词"工作负荷的不同实现[github_topnwords]的示例。

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "在 Azure 存储中创建容器"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "将任务应用程序和输入 （数据） 的文件上载到容器"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "创建批处理池"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "创建批处理作业"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "将任务添加到作业"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "监视任务"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "从存储下载任务输出"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "批次解决方案工作流 （完整的关系图）"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "在门户网站中的批次凭据"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "存储在门户网站中的凭据"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "批次解决方案工作流 （小图）"
