<properties
    pageTitle="Azure 批服务基础知识 |Microsoft Azure"
    description="了解有关使用 Azure 批服务大规模并行和 HPC 工作负载"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Azure 批处理基础知识

Azure 批处理使您能够有效地在云中运行大规模并行和高性能计算 (HPC) 的应用程序。 它计划计算密集型工作，若要在托管的虚拟机的集合上运行的平台服务，可以自动比例计算资源以满足作业的需要。

使用批次服务中，您定义 Azure 计算资源，执行您的应用程序并行，并在规模较大。 可以按需或按预定时间运行的作业，并且您不需要手动创建、 配置和 HPC 群集，单个虚拟机、 虚拟网络或复杂作业管理和任务调度的基础结构。

## <a name="use-cases-for-batch"></a>批处理的用例

批处理是托管的 Azure 服务所使用的*批次处理*或*批处理计算*— 运行大量的类似任务，才能获得一些所需的结果。 定期处理、 转换和分析大量数据的组织通常使用批处理计算。

批处理可用于 （也称为"embarrassingly 并行"） 的本质上的并行应用程序和工作负载。 本质上并行工作负载轻松地分成多个同时在多台计算机执行工作的任务。

![并行任务][1]<br/>

通常使用这种技术处理的负载的一些示例包括︰

* 财务风险建模
* 气候和 hydrology 的数据分析
* 图像呈现、 分析和处理
* 媒体编码和转码
* 基因序列分析
* 工程应力分析
* 软件测试

批次可以执行结束时，减少步骤并行计算和执行更复杂的 HPC 工作负载，如[消息传递接口 (MPI)](batch-mpi.md)的应用程序。

有关批处理和 Azure 中的其他 HPC 解决方案选项之间的比较，请参阅[批处理和 HPC 解决方案](batch-hpc-solutions.md)。

## <a name="developing-with-batch"></a>使用批次开发

处理并行批处理工作负载通常是以编程方式使用[批处理 Api](#batch-development-apis)的其中之一。 与批处理 Api 中，创建和管理池的计算节点 （虚拟机） 和计划作业和任务，以在这些节点上运行。 客户端应用程序或服务，您创作使用批处理 Api 与批服务进行通信。

可以高效地处理大型工作负载为您的组织也向客户提供服务的前端，以便可以运行这些作业和任务 — 根据需要，或在时间安排--一、 上百或甚至上千个节点上。 此外可以使用批处理作为大流，由工具如[Azure 数据工厂](../data-factory/data-factory-data-processing-using-batch.md)管理的一部分。

> [AZURE.TIP] 时您就可以深入了解批处理的 API，它提供的功能更深入地了解到，检出[批次为开发人员提供的功能概述](batch-api-basics.md)。

### <a name="azure-accounts-youll-need"></a>您将需要的 azure 帐户

当开发批处理解决方案时，您将在 Microsoft Azure 使用以下帐户。

- **Azure 帐户和订阅**-如果没有 Azure 的订阅，您可以激活您[受益的 MSDN 订阅者][msdn_benefits]，或注册一个[免费的 Azure 帐户][free_account]。 在创建帐户时，会自动创建默认订阅。

- **批次帐户**-当您的应用程序与服务进行交互批次，帐户名的帐户和访问键的 URL 用作凭据。 所有批资源池，如都计算节点，作业，并且任务与批帐户相关联。 您可以在 Azure 门户[创建批处理帐户](batch-account-create-portal.md)。

- **存储帐户**-批包括使用[Azure 存储]中的文件的内置支持[azure_storage]。 几乎每个批处理方案使用 Azure 存储 — 临时任务运行的程序和数据的处理，以及它们所生成的输出数据的存储。 若要创建存储帐户，请参阅[关于 Azure 存储帐户](./../storage/storage-create-storage-account.md)。

### <a name="batch-development-apis"></a>批次开发 Api

您的应用程序和服务可以发出直接 REST API 调用，使用一个或多个下面的客户端库或二者兼而有之管理计算资源和运行并行在使用批处理服务的规模较大的工作负荷。

| API    | API 参考 | 下载 | 代码示例 |
| ----------------- | ------------- | -------- | ------------ |
| **批处理的剩余部分** | [MSDN][batch_rest] | N/A | [MSDN][batch_rest] |
| **批次.NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **批次 Python**  | [readthedocs.io][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **批次 Node.js** | [github.io][api_nodejs] | [npm][api_nodejs_npm] | - |
| **批次 Java**（预览） | [github.io][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>批次资源管理

除了客户端 Api，您可以使用以下批处理帐户内管理资源。

- [批次 PowerShell cmdlet][batch_ps]: Azure 批 cmdlet [Azure PowerShell](../powershell-install-configure.md)模块使您能够管理与 PowerShell 批资源。

- [Azure CLI](../xplat-cli-install.md): Azure 命令行界面 (Azure CLI) 是提供与许多 Azure 服务，包括批处理进行交互 shell 命令的跨平台工具集。

- [批次管理.NET](batch-management-dotnet.md)客户端库︰ 还可以通过[NuGet][api_net_mgmt_nuget]，您可以使用批处理管理.NET 客户端库以编程方式管理批次帐户、 配额和应用程序包。 引用在[MSDN]上管理库为[api_net_mgmt]。

### <a name="batch-tools"></a>批处理工具

尽管并不要求构建解决方案使用批次，以下是一些生成和调试您的批处理应用程序和服务时所使用的有价值工具。

 - [Azure 的门户网站][portal]︰ 您可以创建、 监视和删除批处理池、 作业和 Azure 门户的批处理刀片式服务器中的任务。 您可以查看它们的状态信息和运行作业，而即使是其他资源池中的计算节点从下载文件 (下载失败的任务`stderr.txt`进行故障诊断时，例如)。 您还可以下载远程桌面 (RDP) 文件可用于在登录以计算节点。

 - [Azure 批资源管理器][batch_explorer]︰ 批资源管理器提供类似批资源管理功能为 Azure 的入口，但独立的 Windows Presentation Foundation (WPF) 客户端应用程序中。 在[GitHub]上可用批处理.NET 示例应用程序之一[github_samples]，可以生成与 Visual Studio 2015年或以上，并使用它来浏览和管理批处理帐户中的资源，在开发和调试您的批处理解决方案。 查看作业、 池和任务的详细信息，从计算节点下载文件和远程连接到节点与批资源管理器使用您可以下载的远程桌面 (RDP) 文件。

 - [Microsoft Azure 存储浏览器][storage_explorer]︰ 存储浏览器而不是严格 Azure 批处理工具，是另一个重要的工具，已经在开发和调试您的批处理解决方案。

## <a name="scenario-scale-out-a-parallel-workload"></a>方案︰ 扩大规模的并行工作负荷

使用批处理 Api 与批服务进行交互的常见解决方案涉及外扩本质上并行工作，如 3D 场景-池的计算节点上的图像的呈现。 该池的计算节点可以是您"呈现服务器群"，提供几十、 上百或甚至上千的内核为您呈现的作业，例如。

下图显示了通用的批处理工作流，使用客户端应用程序或使用批处理运行并行工作负荷的托管的服务。

![批次解决方案工作流][2]

在常见方案中，您的应用程序或服务流程计算工作负荷在 Azure 批，请执行以下步骤︰

1. 将上载**输入的文件**和**应用程序**将处理这些文件复制到您的 Azure 存储帐户。 输入的文件可以是任何数据都将处理您的应用程序，如财务建模数据或视频文件要转换代码。 应用程序文件可用于处理数据，例如，三维呈现应用程序或媒体转码的任何应用程序。

2. 创建一个批处理**池**计算节点批处理帐户-中的这些节点都是虚拟机将执行您的任务。 可以在 Azure 存储的应用程序的安装节点加入池 （在步骤 1 中您上载应用程序） 时指定[节点大小](./../cloud-services/cloud-services-sizes-specs.md)、 其操作系统和位置等属性。 您还可以配置为[自动缩放](batch-automatic-scaling.md)-池动态调整池中 — 响应任务生成的工作负荷的计算节点的数量。

3. 创建批处理**作业**以池的计算节点上运行工作负荷。 在创建作业时，将其关联与批处理池。

4. 将**任务**添加到该作业中。 将任务添加到作业，批服务自动安排在池中的计算节点上执行的任务。 每个任务使用的应用程序上载处理输入的文件。

    - 4a。 执行任务之前，它可以下载数据 （输入文件），则分配给在计算节点的过程。 如果应用程序没有已安装在该节点上 （请参阅步骤 2），它可以从以下地址下载相反。 完成下载后，在其分配的节点上执行任务。

5. 当任务运行时，您可以查询批处理监视作业，而且其任务的进度。 您的客户端应用程序或服务与服务进行通信批通过 HTTPS，并因为可能监视成千上万的数千台计算节点上运行的任务，一定到[高效地查询批次服务](batch-efficient-list-queries.md)。

6. 为完成任务，他们可以到 Azure 存储上载其结果数据。 您还可以直接从计算节点检索文件。

7. 当您监视检测到您的作业中的任务已经完成时，您的客户端应用程序或服务可以下载进一步处理或计算的输出数据。

请记住，这是只是一种用于批处理，和这种情况下描述只是一些其可用的功能。 例如，您可以在每个计算节点上，执行[并行的多个任务](batch-parallel-node-tasks.md)和[作业准备工作和完成任务](batch-job-prep-release.md)可用于节点准备您的作业，然后事后清理。

## <a name="next-steps"></a>下一步行动

现在，有的批服务概述，就应该深入了解如何使用它来处理您的计算密集型并行工作负载。

- 阅读[为开发人员提供批处理功能概述](batch-api-basics.md)、 为所有人准备使用批处理的基本信息。 本文包含池、 节点、 作业和任务，如批处理服务资源的更多详细的信息和生成批处理应用程序时，可以使用许多 API 功能。

- [学习如何使用.NET 的 Azure 批库](batch-dotnet-get-started.md)若要了解如何使用 C# 和批处理.NET 库来执行简单的工作负载，使用一个通用的批处理工作流。 这篇文章应该是在学习如何使用批处理服务第一站之一。 此外，还有教程的[Python 版本](batch-python-tutorial.md)。

- 下载[代码示例在 GitHub 上的][github_samples]以查看如何 C# 和 Python 可以批处理接口时间安排和过程示例工作负载的要求。

- 检出[批次学习路径][learning_path]以了解可用的资源对您学习使用批处理。

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
