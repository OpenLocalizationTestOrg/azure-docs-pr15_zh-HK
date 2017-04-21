<properties
    pageTitle="方便了应用程序的安装和管理在 Azure 批 |Microsoft Azure"
    description="使用 Azure 批处理，可以方便地管理多个应用程序和版本安装在批处理应用程序数据包功能计算节点。"
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

# <a name="application-deployment-with-azure-batch-application-packages"></a>与 Azure 批处理应用程序包的应用程序部署

Azure 批处理应用程序数据包功能提供了方便地管理任务的应用程序和其部署到您的池的计算节点。 与应用程序包，您可以上载和管理运行任务，包括其支持文件的应用程序的多个版本。 您可以将自动部署一个或多个计算节点到这些应用程序在您的池。

在本文中，您将学习如何上载和管理应用程序软件包的 Azure 的门户。 然后，您将学习如何使用[批处理.NET]池的计算节点上安装这些[api_net]库。

> [AZURE.NOTE] 这里描述的应用程序的包功能将取代服务的早期版本中的"应用程序批次"功能。

## <a name="application-package-requirements"></a>应用程序封装要求

必须为要使用应用程序软件包的批处理帐户[链接 Azure 存储帐户](#link-a-storage-account)。

本文中讨论的应用程序的包功能兼容时*只*与 10 月 2016年之后创建的批处理池。 不将部署应用程序软件包计算节点在此日期之前创建的池。

此功能在[批处理 REST API]中引入[api_rest]版本 2015年-12-01.2.2 和相应[批次.NET] [api_net]库 3.1.0 版。 我们建议您始终使用最新的 API 版本，使用批处理时。

> [AZURE.IMPORTANT] 目前，只有*CloudServiceConfiguration*池支持应用程序包。 无法使用应用程序包中使用 VirtualMachineConfiguration 图像创建的池。 [虚拟机配置](batch-linux-nodes.md#virtual-machine-configuration)节[提供 Linux 计算在 Azure 批池节点](batch-linux-nodes.md)有关的两个不同的配置详细信息，请参阅。

## <a name="about-applications-and-application-packages"></a>有关应用程序和应用程序包

Azure 批内*应用程序*指的是一套可以自动下载到您的池的计算节点的版本更新二进制文件。 *应用程序包*所引用这些二进制文件的*特定的一组*，表示给定的*版本*的应用程序。

![高级图表的应用程序和应用程序包][1]

### <a name="applications"></a>应用程序

在批处理中的应用程序包含一个或多个应用程序打包应用程序并指定该应用程序的配置选项。 例如，应用程序可以指定在计算节点和是否可以更新或删除其文件包上安装的默认应用程序程序包版本。

### <a name="application-packages"></a>应用程序包

应用程序包是一个.zip 文件，其中包含应用程序二进制文件和支持所需执行的任务通过的文件。 每个应用程序包表示应用程序的特定版本。

您可以指定应用程序池和任务级别的文件包。 当您创建一个池或任务时，可以指定一个或多个这些软件包和版本 （可选）。

* **池应用程序包**部署到池中的*每个*节点。 当节点加入到一个池，并在重新启动或重新映像部署应用程序。

    池中的所有节点都执行工作任务时，池应用程序包是合适的。 在您创建池，并且您可以添加或更新现有池的软件包时，您可以指定一个或多个应用程序包。 如果更新现有池的应用程序包，您必须重新启动以安装新的软件包及其节点。

* 仅对计算节点计划之前运行该任务的命令行运行任务，部署了**任务应用程序包**。 如果指定的应用程序软件包和版本已在节点上，它不重新部署，用于现有的包。

    任务应用程序包可在共享池环境中，其中一个池，运行不同的作业，作业完毕后，不会删除该池。 如果您的工作在此池中是否比节点更少任务，任务应用程序包可以最小化数据传输以来仅对运行任务的节点部署您的应用程序。

    可以从任务应用程序包中受益的其他方案的作业使用特别是大型应用程序，但只有一小部分任务。 例如，预处理阶段或合并任务，其中预处理或合并应用程序是重量级。

> [AZURE.IMPORTANT] 有的应用程序和应用程序包内一个批次的帐户，以及应用程序的最大包大小的数量限制。 有关这些限制的详细信息，请参阅[配额和 Azure 批服务的限制](batch-quota-limit.md)。

### <a name="benefits-of-application-packages"></a>应用程序包的好处

应用程序包可以简化批处理解决方案中的代码，并降低管理任务运行的应用程序所需的系统开销。

池的开始任务不必指定的节点上安装单独的资源文件的长列表。 您不必手动管理您的应用程序文件在 Azure 存储中，或者在您的节点上的多个版本。 而且，您不必担心会生成[SAS 的 Url](../storage/storage-dotnet-shared-access-signature-part-1.md)来提供对您的存储帐户中的文件访问。 在 Azure 存储来存储应用程序软件包和部署它们来计算节点与后台工作批次。

## <a name="upload-and-manage-applications"></a>上载和管理应用程序

您可以使用[Azure 的门户网站][portal]或[批次管理.NET](batch-management-dotnet.md)库来管理您批处理的帐户中的应用程序包。 在下面几节中，我们首先链接存储帐户，然后讨论添加应用程序和程序包以及门户网站中管理它们。

### <a name="link-a-storage-account"></a>链接存储帐户

若要使用应用程序软件包，您必须先链接到批帐户 Azure 存储帐户。 如果尚未为您批处理的帐户配置存储帐户，Azure 门户将显示您单击**应用程序**图块**批帐户**刀片式服务器中的第一次警告。

> [AZURE.IMPORTANT] 批当前支持*仅***通用**存储帐户类型[有关 Azure 存储帐户](../storage/storage-create-storage-account.md)[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)，第 5 步中所述。 当您链接到批帐户，链接*仅***通用**存储帐户的 Azure 存储帐户。

![在 Azure 门户中没有存储帐户配置警告][9]

批服务用于存储和检索的应用程序包关联的存储帐户。 所链接的两个帐户之后，批可以自动部署到计算节点链接的存储帐户中存储的包。 单击**警告**刀片式服务器，**存储帐户设置**，然后单击要链接到您的批处理帐户的存储帐户**存储帐户**刀片式服务器上的**存储帐户**。

![在 Azure 门户中选择存储帐户刀片式服务器][10]

我们建议您创建存储帐户*专门*用于使用批处理帐户，并在此处选择它。 有关如何创建存储帐户的详细信息，请参阅[关于 Azure 存储帐户](../storage/storage-create-storage-account.md)中的"创建存储帐户"。 创建存储帐户后，您可以再将其链接到批帐户使用**存储帐户**刀片式服务器。

> [AZURE.WARNING] 因为批处理使用 Azure 存储存储应用程序包，将[照常收取][storage_pricing]块 blob 数据。 一定要考虑的大小和数量的应用程序的软件包，并定期删除过时的软件包，成本降到最低。

### <a name="view-current-applications"></a>查看当前应用程序

批帐户中查看的应用程序，请查看**批处理帐户**刀片式服务器时单击左侧菜单中的**应用程序**菜单项。

![应用平铺][2]

这将打开刀片式服务器**应用程序**︰

![列出应用程序][3]

刀片式服务器**应用程序**在您的帐户和下面的属性中显示每个应用程序的 ID:

* **包**-与此应用程序相关联的版本数。
* **默认版本**-如果设置应用程序池时不指定版本将安装的版本。 此设置是可选的。
* 允许**允许更新**— 指定是否包更新、 删除和添加的值。 如果该选项设置为**否**，将为应用程序禁用包更新和删除。 可以添加只新应用程序软件包版本。 默认值为**是**。

### <a name="view-application-details"></a>查看应用程序的详细信息

单击**应用程序**刀片式服务器中的应用程序打开刀片式服务器，其中包含该应用程序的详细信息。

![应用程序详细信息][4]

在应用程序的详细信息刀片式服务器，可以为应用程序配置以下设置。

* **允许更新**--指定是否可以更新或删除其应用程序包。 本文中稍后介绍，请参见"更新，或删除一个应用程序包"。
* **默认版本**— 指定一个默认的应用程序程序包部署计算节点。
* **显示名称**--指定"友好"名称的批时显示有关该应用程序，如服务提供您的客户完成批处理的用户界面中，可以使用解决方案。

### <a name="add-a-new-application"></a>添加新应用程序

若要创建新的应用程序，添加一个应用程序包并指定一个新的、 唯一的应用程序 id。 添加新的应用程序 ID 的第一个应用程序包还将创建新的应用程序。

若要打开**新的应用程序**刀片式服务器**应用程序**刀片式服务器上，请单击**添加**。

![在 Azure 门户中的新应用程序刀片式服务器][5]

**新的应用程序**刀片式服务器提供以下字段以指定您的新应用程序和应用程序包中的设置。

**应用程序 id**

此字段指定新的应用程序，这是遵循标准的 Azure 批次 ID 验证规则的 ID:

* 可以包含字母数字字符，包括连字符和下划线的任意组合。
* 不能包含超过 64 个字符。
* 必须是唯一的批处理帐户内。
* 不区分保留和大小写区分。

**版本**

指定要上载应用程序包的版本。 版本字符串是遵循以下验证规则︰

* 可以包含字母数字字符，包括连字符、 下划线和句点的任意组合。
* 不能包含超过 64 个字符。
* 必须是唯一的应用程序中。
* 保留，并且区分大小写的情况。

**应用程序包**

此字段指定.zip 文件，其中包含应用程序二进制文件和执行应用程序所需的支持文件。 单击**选择文件**框中或文件夹图标，浏览到并选择包含您的应用程序文件的.zip 文件。

选择一个文件后，单击**确定**开始到 Azure 存储上载。 在上载操作完成后，将通知您并关闭刀片式服务器。 这取决于您要上载的文件的大小和网络连接的速度，此操作可能需要一些时间。

> [AZURE.WARNING] 在上载操作完成之前不要关闭刀片式服务器**新的应用程序**。 此操作将停止上载过程。

### <a name="add-a-new-application-package"></a>添加新的应用程序包

若要添加现有应用程序的新应用程序软件包版本，刀片式服务器**应用程序**中选择一个应用程序，单击**程序包**，然后单击**添加**以打开**添加包**刀片。

![在 Azure 的门户网站中添加应用程序软件包刀片式服务器][8]

正如您所看到的字段匹配的**新应用程序**刀片式服务器，但该**应用程序 id**框被禁用。 就像新的应用程序，指定**版本**的新软件包，浏览到您**应用程序包**的.zip 文件，然后单击**确定**以将上载程序包。

### <a name="update-or-delete-an-application-package"></a>更新或删除一个应用程序包

若要更新或删除现有的应用程序包，打开详细信息刀片式服务器应用程序中，单击**包**打开**包**刀片，单击想要修改，该应用程序包的行中的**省略号**并选择您想要执行的操作。

![更新或删除包在 Azure 门户][7]

**更新**

当单击**更新**时，将显示*更新包*刀片。 此刀片是类似于*新应用程序包*刀片式服务器，但是启用包选择字段，允许您指定要上载的新 ZIP 文件。

![更新包刀片在 Azure 门户][11]

**删除**

单击**删除**时，系统将要求您确认删除包版本，并批从 Azure 存储中删除包。 如果您删除某个应用程序的默认版本，**默认版本**设置为应用程序删除。

![删除应用程序][12]

## <a name="install-applications-on-compute-nodes"></a>在计算节点上安装应用程序

现在，您已经看到如何管理应用程序软件包使用 Azure 的门户网站，我们可以讨论如何部署它们来计算节点并运行这些批处理任务。

### <a name="install-pool-application-packages"></a>安装应用程序池的软件包

若要在库中的所有计算节点上安装一个应用程序包，指定一个或多个应用程序将包*引用*池。 当该节点加入池，并重新引导或重新映像节点时每个计算节点上安装您指定池的应用程序包。

在批处理.NET 中，指定一个或多个[CloudPool][net_cloudpool]。[ApplicationPackageReferences][net_cloudpool_pkgref]创建一个新池，或为现有的池。 [ApplicationPackageReference] [net_pkgref]类指定的池上安装一个应用程序 ID 和版本计算节点。

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] 如果由于某种原因失败的应用程序软件包部署，批服务标记节点[不可用][net_nodestate]，并将在该节点上执行计划没有任务。 在这种情况下，您应该**重新启动**该节点才能重新启动包部署。 重新启动节点还启用的节点上重新安排的任务。

### <a name="install-task-application-packages"></a>安装任务应用程序包

类似于一个池，您指定应用程序将包*引用*的任务。 当一个任务安排在一个节点上运行时，包下载并提取之前执行任务的命令行。 如果在该节点上已安装指定的软件包和版本，不下载软件包，用于现有的包。

若要安装任务应用程序包，请配置任务的[CloudTask][net_cloudtask]。[ApplicationPackageReferences][net_cloudtask_pkgref]属性︰

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>执行安装的应用程序

下载并解压到指定的目录中所指定的池或任务包`AZ_BATCH_ROOT_DIR`的节点。 批次还会创建一个环境变量，其中包含指定的目录的路径。 任务命令行使用此环境变量引用的节点上的应用程序时。 变量是以下面的格式︰

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`和`version`与您指定的部署的应用程序和软件包版本相对应的值。 例如，如果您指定应为 2.7 版应用程序*搅料机*安装，任务的命令行可以使用此环境变量来访问它的文件︰

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

如果您指定应用程序的默认版本，则可以省略版本后缀。 例如，如果您将设"2.7"*搅料机*的应用程序的默认版本，您的任务可以引用以下环境变量和它们将执行 2.7 版︰

`AZ_BATCH_APP_PACKAGE_BLENDER`

下面的代码段显示了任务命令行示例启动*搅料机*应用程序的默认版本︰

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] 请参阅[批处理功能概述](batch-api-basics.md)有关计算节点环境设置的详细信息中[的任务的环境设置](batch-api-basics.md#environment-settings-for-tasks)。

## <a name="update-a-pools-application-packages"></a>更新池的应用程序包

如果现有池已配置应用程序软件包，您可以指定池的一个新包。 如果您指定新的包引用池，满足以下条件︰

* 连接池的所有新的节点和重新引导或重新映像的任何现有节点将安装新指定的包。
* 计算在池中后更新包引用的节点不会自动安装新的应用程序包。 这些计算节点必须重新引导或最先接收新的包。
* 当部署一个新的包时，创建的环境变量反映出新的应用程序将包引用。

在此示例中，现有池有 2.7 版*搅料机*应用程序配置为其[CloudPool]之一的[net_cloudpool]。[ApplicationPackageReferences][net_cloudpool_pkgref]. 若要更新版本 2.76b 池的节点，请指定新的[ApplicationPackageReference] [net_pkgref]的新版本，并提交更改。

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

既然已经配置的新版本，所有*新*的节点的连接池将具有版本部署到它的 2.76b。 要在*已经*在池中的节点上安装 2.76b，重新引导或重新映射它们。 请注意已重新启动的节点将保留以前部署的包中的文件。

## <a name="list-the-applications-in-a-batch-account"></a>列表中批帐户的应用程序

您可以通过使用[ApplicationOperations]列出的应用程序以及它们批帐户中的包[net_appops]。[ListApplicationSummaries][net_appops_listappsummaries]方法。

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>总结

与应用程序软件包，可以帮助您的客户选择其作业的应用程序并指定要处理作业的批处理启用服务时使用的确切版本。 此外可能会提供为您的客户可以上载和跟踪自己的应用程序在您的服务的能力。

## <a name="next-steps"></a>下一步行动

* [REST API，批][api_rest]还提供了支持，以处理与应用程序包。 有关示例，请参见[applicationPackageReferences] [rest_add_pool_with_packages]元素中[添加到帐户池][rest_add_pool]有关如何指定使用 REST API 来安装软件包的信息。 请参阅[应用程序][rest_applications]有关如何通过使用批处理 REST API 获取应用程序信息的详细信息。

* 了解如何以编程方式[管理 Azure 批帐户和批次管理.NET 的配额](batch-management-dotnet.md)。 [批次管理.NET] [api_net_mgmt]库可以启用批处理应用程序或服务的帐户的创建和删除功能。

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "应用程序打包高级图"
[2]: ./media/batch-application-packages/app_pkg_02.png "在 Azure 的门户应用程序图块"
[3]: ./media/batch-application-packages/app_pkg_03.png "在 Azure 的门户应用程序刀片式服务器"
[4]: ./media/batch-application-packages/app_pkg_04.png "在 Azure 的门户应用程序的详细信息刀片式服务器"
[5]: ./media/batch-application-packages/app_pkg_05.png "在 Azure 门户中的新应用程序刀片式服务器"
[7]: ./media/batch-application-packages/app_pkg_07.png "更新或删除软件包 Azure 门户中的下拉列表"
[8]: ./media/batch-application-packages/app_pkg_08.png "在 Azure 门户中的新应用程序包刀片"
[9]: ./media/batch-application-packages/app_pkg_09.png "没有链接的存储帐户警报"
[10]: ./media/batch-application-packages/app_pkg_10.png "在 Azure 门户中选择存储帐户刀片式服务器"
[11]: ./media/batch-application-packages/app_pkg_11.png "更新包刀片在 Azure 门户"
[12]: ./media/batch-application-packages/app_pkg_12.png "删除包在 Azure 门户的确认对话框"
