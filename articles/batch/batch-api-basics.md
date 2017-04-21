<properties
    pageTitle="为开发人员提供的 azure 批处理功能概述 |Microsoft Azure"
    description="了解批处理服务和从开发的角度看其 Api 的功能。"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>开发人员提供的批处理功能概述

在 Azure 批服务的核心组件的概述，我们讨论的主要服务功能和资源的批次开发人员可以使用它来建立大规模的并行计算解决方案。

您正在开发一个分布式计算应用程序或服务发出直接[REST API] [batch_rest_api]调用，或者您正在使用一个[批次的 Sdk](batch-technical-overview.md#batch-development-apis)，您将使用的许多资源和本文中讨论的功能。

> [AZURE.TIP] 更高级别的批服务简介，请参见[Azure 批处理的基础知识](batch-technical-overview.md)。

## <a name="batch-service-workflow"></a>批服务工作流

下列高级工作流是典型的几乎所有应用程序和服务使用批次处理并行工作负载的服务︰

1. 上载**数据文件**，您想要处理到[Azure 存储][azure_storage]帐户。 批包含内置的访问 Azure Blob 存储支持并在任务运行时任务可以将这些文件下载到[计算节点](#compute-node)。

2. 上传您的任务将运行**应用程序文件**。 这些文件可以是二进制文件或脚本以及它们的依赖项，并被执行的作业中的任务。 您的任务可以下载这些文件从您的存储帐户，也可以用于应用程序管理和部署使用的批处理[应用程序数据包](#application-packages)功能。

3. 创建[池](#pool)的计算节点。 在创建池时，您可以指定池、 它们的大小和操作系统的计算节点的数目。 当您在工作中的每个任务运行时，它已分配一个在池中的节点上执行。

4. 创建一个[作业](#job)。 作业管理任务的集合。 您可以关联到一个特定的池在运行该作业的任务将每个作业。

5. 将[任务](#task)添加到该作业中。 每个任务运行的应用程序或脚本上载处理数据文件从您的存储帐户下载的。 为每个任务完成后，它可以将其输出上载到 Azure 存储。

6. 监视作业进度并从 Azure 存储中检索任务输出。

以下各节讨论这些和批处理的启用分布式计算方案的其他资源。

> [AZURE.NOTE] 您需要[批帐户](batch-account-create-portal.md)使用批处理服务。 此外，几乎所有的解决方案使用[Azure 存储][azure_storage]帐户文件存储和检索。 批当前支持仅**通用**存储帐户类型，[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)[有关 Azure 存储帐户](../storage/storage-create-storage-account.md)中的第 5 步中所述。

## <a name="batch-service-resources"></a>批次服务资源

一些下面的资源--客户、 计算节点、 池、 作业和任务-所需的所有解决方案中使用批处理服务。 其他人，作业调度和应用程序包，像是有帮助的但是可选的功能。

- [帐户](#account)
- [计算节点](#compute-node)
- [池](#pool)
- [作业](#job)

  - [作业调度](#scheduled-jobs)

- [任务](#task)

  - [启动任务](#start-task)
  - [作业管理器任务](#job-manager-task)
  - [作业准备和发布任务](#job-preparation-and-release-tasks)
  - [多实例任务 (MPI)](#multi-instance-tasks)
  - [任务相关性](#task-dependencies)

- [应用程序包](#application-packages)

## <a name="account"></a>帐户

批次帐户是批服务中的唯一标识的实体。 所有处理都都批帐户相关联。 在执行批处理服务的操作时，您需要帐户名称和其帐户密钥之一。 您可以[创建批处理 Azure 帐户使用 Azure 的门户](batch-account-create-portal.md)。

## <a name="compute-node"></a>计算节点

计算节点是专用于处理您的应用程序工作负载的一部分 Azure 虚拟机 (VM)。 节点的大小确定 CPU 内核、 内存容量和本地文件系统的大小分配到的节点的数目。 可以通过使用 Azure 云服务或虚拟机市场上的图像创建 Windows 或 Linux 节点的池。 有关这些选项，请参阅下[池](#pool)的详细信息一节。

节点可以运行任何可执行文件或脚本所支持的操作系统环境的节点。 这包括\*.exe， \*.cmd \*.bat 和 PowerShell 脚本的窗口--和二进制文件，外壳，和 Python 脚本用于 Linux。

所有计算节点批处理中的还包括︰

- 标准[文件夹结构](#files-and-directories)和关联的[环境变量](#environment-settings-for-tasks)可用于引用的任务。
- 被配置为控制访问的**防火墙**设置。
- [远程访问](#connecting-to-compute-nodes)的 Windows (远程桌面协议 (RDP)) 和 Linux (安全外壳协议 (SSH)) 节点。

## <a name="pool"></a>池

池是应用程序运行的节点的集合。 该池可以手动创建由您，还是自动批处理服务时指定要完成的工作。 您可以创建和管理资源满足您的应用程序池。 只能由创建它的批处理帐户，可以使用池。 批次帐户可以有多个池。

Azure 批池构建基于核心 Azure 计算平台。 他们提供了大规模分配、 安装应用程序、 数据分发、 运行状况监视和灵活调整的池中 （[缩放](#scaling-compute-resources)） 的计算节点数。

添加到池中的每个节点分配一个唯一的名称和 IP 地址。 从池中删除某个节点后，操作系统的文件进行的任何更改都将丢失，并释放其名称和 IP 地址供将来使用。 当一个节点离开池时，其生命周期内是通过。

在创建池时，您可以指定以下属性︰

- 计算节点**操作系统**和**版本**

    当您池中选择节点的操作系统有两种选择︰**虚拟机配置**和**云服务配置**。

    **虚拟机配置**提供 Linux 和 Windows 映像计算节点从[Azure 虚拟机市场][vm_marketplace]。
    创建池包含虚拟机配置节点时，必须指定不仅大小的节点，但也**虚拟机映像的引用**且批**节点代理 SKU**在节点上安装。 有关指定这些池属性的详细信息，请参阅[设置 Linux 计算 Azure 批池中的节点](batch-linux-nodes.md)。

    **云服务配置**提供 Windows 计算节点*只*。 对于云服务配置池的可用操作系统[Azure 来宾操作系统版本和 SDK 兼容性列表](../cloud-services/cloud-services-guestos-update-matrix.md)中列出。 当您创建一个包含云服务节点的池时，您需要指定仅节点的大小和它的*操作系统家族*。 当您创建池的 Windows 计算节点时，您最常使用云服务。

    - *OS 系列*还确定了与操作系统一起安装的版本的.NET。
    - 如使用在云服务中的辅助角色，您可以指定*操作系统版本*（辅助角色的详细信息，请参阅在[云服务概述](../cloud-services/cloud-services-choose-me.md)[告诉我云服务](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services)部分）。
    - 与工作人员的角色，我们建议您指定为`*`的*OS 版本*，以便自动升级节点，而且没有工作需要适应新发布版本。 用于选择特定的操作系统版本的主用例是为了确保应用程序的兼容性，允许向后兼容性测试，以使更新的版本之前，先执行。 验证之后，可以更新池的*操作系统版本*和新的 OS 映像可以是安装 — 任何正在运行的任务被中断，并且重新排队。

- **节点的大小**

    [云服务的大小](../cloud-services/cloud-services-sizes-specs.md)列出了**云服务配置**计算节点大小。 批次支持各种云服务规模，除`ExtraSmall`。

    **虚拟机配置**计算规模[大小 Azure 中的虚拟机](../virtual-machines/virtual-machines-linux-sizes.md)(Linux) 和[Azure 中的虚拟机大小](../virtual-machines/virtual-machines-windows-sizes.md)(Windows) 中列出的节点。 批次支持除外的所有 Azure VM 大小`STANDARD_A0`以及那些具有高级存储 (`STANDARD_GS`， `STANDARD_DS`，和`STANDARD_DSV2`系列)。

    当选择的计算节点大小，考虑的特点和需求会在节点运行的应用程序。 无论应用程序是多线程，并有助于它占用的内存量等方面确定最合适、 最经济高效的节点大小。 它是通常在选择节点大小，假设一个任务将在一个节点上运行一次。 但是，也可能有多个任务 （和因此多个应用程序实例） 上[运行并行](batch-parallel-node-tasks.md)计算节点执行作业期间。 在这种情况下，是常见的选择以满足增加的需求，对并行任务执行节点较大。 有关详细信息，请参阅[任务调度策略](#task-scheduling-policy)。

    池中的节点都具有相同的大小。 如果想要运行的应用程序具有不同的系统要求和/或加载级别，建议使用单独的池。

- **目标节点数**

    这是您要部署在池中的计算节点的数目。 这称为*目标*因为在某些情况下，您的池可能无法达到所需的节点数。 一个池达到批次帐户-[核心配额](batch-quota-limit.md#batch-account-quotas)或自动调整公式应用到限制的节点 （请参阅下面"的扩展策略"一节） 的最大数量的池是否可能不会到达所需的节点数。

- **扩展策略**

    除了指定静态数量的节点，可以改为编写并将[自动调整公式](#scaling-compute-resources)应用到池。 批次服务定期计算公式，并调整根据各种池、 作业和任务参数，可以指定池内的节点数。

- **任务调度策略**

    [每个节点的最大任务](batch-parallel-node-tasks.md)配置选项决定可以在池中的每个计算节点并行运行的任务的最大数目。

    默认配置是，每次一个任务运行在一个节点上，但情况都是有好处，能够同时在一个节点上执行多个任务。 请参阅[并发节点任务](batch-parallel-node-tasks.md)如何您可以受益于每个节点的多个任务，请参阅本文中的[示例方案](batch-parallel-node-tasks.md#example-scenario)。

    您还可以指定*填充类型*确定批处理任务均匀地分布在池中的所有节点还是包将任务分配给其他节点之前，每个节点的最大任务数。

- 计算节点的**通信状态**

    在大多数情况下，任务将独立工作，并不需要与另一个通信。 但是，有一些应用程序中的任务必须传达，如[MPI 方案](batch-mpi.md)。

    您可以配置池允许 it —**节点之间的通讯**中的节点之间的通信。 当节点间通信的情况下时，在云服务配置池的节点可以相互通信端口大于 1100，和虚拟机配置池不会限制任何端口上的通信。

    请注意，实现节点之间的通讯也会影响群集中的节点的位置，因为部署的限制可能会限制最大池中的节点数。 如果您的应用程序不需要节点之间的通信，批服务可以分配节点有可能大量向池从许多不同的群集和数据中心，使增加的并行处理能力。

- 为计算节点中**启动任务**

    可选*启动任务*执行每个节点上该节点加入到该池，并且每个时间节点是重新启动或重新映像。 开始任务是计算节点准备执行任务，如安装您的任务在计算节点运行的应用程序特别有用。

- **应用程序包**

    您可以指定要部署到池中的计算节点的[应用程序包](#application-packages)。 应用程序包提供简化的部署和任务运行的应用程序的版本控制。 加入该池，每个节点上安装应用程序池指定的包和节点每次重新启动或重新映像。 应用程序包是当前不受支持的 Linux 计算节点上。

- **网络配置**

    您可以指定是否应创建的池的计算节点 Azure[虚拟网络 (VNet)](../virtual-network/virtual-networks-overview.md)的 ID。 要求您的池可以找到在[添加到帐户池]指定 VNet[vnet]批次 REST API 参考中。

> [AZURE.IMPORTANT] 批处理中的所有帐户都具有默认**配额**限制的**核心**，因而计算节点） 数批帐户中。 您可以找到默认配额及说明 （如最大批处理帐户中的内核数）[增加配额](batch-quota-limit.md#increase-a-quota)的[配额和限制的 Azure 批服务](batch-quota-limit.md)中。 如果您发现自己在问"为什么不能我池达到多 X 节点吗？" 此核心配额可能是原因。

## <a name="job"></a>作业

工作是任务的集合。 它管理池中的计算节点上的任务执行计算的方式。

- 该作业指定**池**是用来运行工作。 可以创建一个新的池，每个作业，或将一个池用于很多作业。 您可以创建一个池与作业日程安排，每个作业或作业调度与相关联的所有作业。

- 您可以指定一个可选的**作业优先级**。 并且其优先级高于当前正在进行的作业提交作业后，高优先级作业任务插入到先于优先级较低的作业任务队列中。 不被抢占低优先级作业已在运行中的任务。

- 您可以使用作业**约束**为作业指定某些限制︰

    可以设置**最大 wallclock 次**，这样如果一个作业运行的时间超过指定的最大 wallclock 时间，终止该作业，而且其任务的所有。

    批处理可检测并再重试失败的任务。 您可以指定**的任务重试最大次数**作为约束，包括一项任务*始终*或*从不*重试。 重试任务意味着任务重新排队再次运行。

- 客户端应用程序可以将任务添加到作业，也可以指定一个[作业管理器任务](#job-manager-task)。 作业管理器任务包含在一个池中的计算节点上正在运行的作业管理器任务创建一个作业，需要执行的任务所需的信息。 作业管理器任务处理专门通过批处理-它排队只要作业创建的并且如果该操作失败时重新启动。 作业管理器任务则*需要*实例化该作业之前定义的任务的唯一方法，因为它创建的[作业调度](#scheduled-jobs)的作业。

- 默认情况下，作业处于活动状态时保持作业内的所有任务都都已完成。 您可以更改此行为，以便在作业中的所有任务都都已完成时自动终止作业。 将作业的**onAllTasksComplete**属性设置 ([OnAllTasksComplete] [net_onalltaskscomplete]批次.NET 中) 到*terminatejob*自动终止此作业时其任务的所有处于已完成状态。

    请注意，批服务考虑*没有*任务要完成其任务的所有的作业。 因此，此选项最常用于与[作业管理器任务](#job-manager-task)。 如果您想要使用不带作业管理器作业自动终止，您应该最初将新作业的**onAllTasksComplete**属性设置为*noaction*，然后将其设置为*terminatejob* ，只有在将任务添加到该作业完成后。

### <a name="job-priority"></a>作业优先级

您可以在批处理创建的作业分配优先级。 批次服务使用作业的优先级值以确定作业计划编制中 （这是与[计划的作业](#scheduled-jobs)不要混淆） 的帐户的顺序。 优先级值的范围从-1000年到 1000-1000年的最低优先级和 1000年为最高。 您可以通过使用[更新作业的属性]更新作业的优先级[rest_update_job]操作 (批处理 REST) 或通过修改[CloudJob.Priority] [net_cloudjob_priority]属性 (批处理.NET)。

在相同的帐户更高优先级的作业有安排优先顺序高于低优先级作业。 作业优先级更高的值在一个帐户不具有对优先级较低的值在不同的帐户的其他作业安排优先。

作业级排产跨池无关。 不同的池之间不保证优先级较高的作业计划首先其关联的池是否充足的空闲节点。 在同一池中，具有相同的优先级级别的作业有同等的机会的计划。

### <a name="scheduled-jobs"></a>计划的作业

[作业调度][ rest_job_schedules] ，您可以创建批次服务中的定期作业。 作业调度指定何时运行作业并包含要运行的作业的技术指标。 您可以指定时间安排--多长时间和计划在生效时 — 和频率期间，应创建作业这段时间的持续时间。

## <a name="task"></a>任务

任务是计算的与作业相关联单位。 它在一个节点上运行。 任务分配给一个节点执行，或将排队等待直到节点变得可用。 简单地说，任务运行一个或多个程序或脚本在计算节点来执行您需要完成的工作。

当您创建任务时，您可以指定︰

- 任务**命令行**。 这是在计算节点运行您的应用程序或脚本的命令行。

    请务必注意，命令行不实际运行下一个外壳。 因此，它不能以本机方式利用 shell[环境变量](#environment-settings-for-tasks)扩展功能 (包括`PATH`)。 要充分利用这种功能，您必须调用外壳命令行 — 例如，通过启动`cmd.exe`窗口节点上或`/bin/sh`在 Linux 上︰

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    如果您的任务需要运行应用程序或脚本中的节点不是`PATH`或引用环境变量，请调用显式地在任务命令行 shell。

- **资源文件**，其中包含要处理的数据。 这些文件自动复制到节点从**一般用途的**Azure 存储帐户中的 Blob 存储在执行任务的命令行之前。 有关详细信息，请参阅[启动任务](#start-task)和[文件和目录](#files-and-directories)部分。

- 您的应用程序所需的**环境变量**。 有关详细信息，请参阅[任务的环境设置](#environment-settings-for-tasks)部分。

- 应在其下执行该任务**的约束**。 对于例如，允许任务运行时，应重试失败的任务，最大次数的最长时间和最大值的时间该任务的工作目录中的文件将被保留。

- **应用程序包**部署到计划任务运行的计算节点。 [应用程序包](#application-packages)提供简化的部署和任务运行的应用程序的版本控制。 任务级别应用程序包将其中一个池，运行不同的作业，作业完毕后，不会删除该池的共享池环境中非常有用。 如果您的工作在此池中是否比节点较少的任务，任务应用程序包可以最小化数据传输以来仅对运行任务的节点部署您的应用程序。

您定义的任务除了在节点上，执行计算以下特殊任务还批服务提供︰

- [启动任务](#start-task)
- [作业管理器任务](#job-manager-task)
- [作业准备和发布任务](#job-preparation-and-release-tasks)
- [多实例任务 (MPI)](#multi-instance-tasks)
- [任务相关性](#task-dependencies)

### <a name="start-task"></a>启动任务

通过与池关联的**启动任务**，您可以准备其节点的操作环境。 例如，可以执行像安装任务运行时，应用程序和启动后台进程的动作。 开始任务每次运行节点启动时，只要它仍在池 — 包括当第一个节点添加到池和重新启动或重新映像时。

开始任务的主要优点是信息的，它可以包含所有计算节点配置和安装的应用程序所需的任务执行所需。 因此，提高池中的节点数是简单，只需指定新的目标节点计数 — 批已经具有配置新的节点，以及让他们准备好接受任务所需的信息。

如任何 Azure 批处理任务，可以在[Azure 存储]中指定**资源文件**的列表[azure_storage]，除了执行的**命令行**。 批处理第一次将资源文件复制到该节点从 Azure 存储，然后运行命令行。 对于池开始任务时，文件列表通常包含任务应用程序及其依赖项。

但是，它还可能包括引用数据，可供在计算节点运行的所有任务。 例如，开始任务的命令行可执行`robocopy`操作将从开始任务的[工作目录](#files-and-directories)（这被指定为资源文件并下载到该节点） 的应用程序文件复制到[共享的文件夹](#files-and-directories)，然后运行 MSI 或`setup.exe`。

> [AZURE.IMPORTANT] 批当前支持*唯一*的**通用**存储帐户类型，[创建存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)[有关 Azure 存储帐户](../storage/storage-create-storage-account.md)中的第 5 步中所述。 批处理任务 （包括标准的任务、 启动任务、 作业准备工作和作业发布任务） 必须指定*仅*在**通用**存储帐户驻留的资源文件。

通常所需批次服务等待开始任务完成，然后再考虑该节点可以指派的任务，但您可以配置此。

如果在计算节点上，启动任务失败，然后节点的状态将更新以反映故障，和节点未分配的任何任务。 如果将其资源文件从存储，复制出现问题或由其命令行执行的过程返回非零退出代码，开始任务可能会失败。

如果您添加或更新*现有*池的开始任务，您必须重新启动开始任务要应用到的节点的计算节点。

### <a name="job-manager-task"></a>作业管理器任务

通常使用**作业管理器任务**控制和/或监视作业执行--例如，若要创建和提交作业任务，确定另一些任务来运行，并且在工作完成时确定。 但是，作业管理器任务并不局限于这些活动。 是完全成熟的任务可以执行任何操作所需的作业。 例如，作业管理器任务可能下载的文件指定为一个参数、 分析该文件的内容和提交根据这些内容的其他任务。

作业管理器任务开始之前的所有其他任务。 它提供了以下功能︰

- 它是自动提交作为任务批服务创建作业时。

- 它计划在作业中的其他任务之前执行。

- 其相关联的节点是最后时池规模从池中删除。

- 其终止可与作业中的所有任务的终止。

- 作业管理器任务给出的最高优先级时需要重新启动。 如果空闲节点不可用，请批服务可能终止其他正在运行的任务，在池中，以便腾出空间来运行作业管理器任务之一。

- 一个作业在作业管理器任务没有通过其他作业的任务的优先级。 整个作业，仅作业的优先级被观察到。

### <a name="job-preparation-and-release-tasks"></a>作业准备和发布任务

批次前作业执行安装程序提供作业的准备工作。 下达的工作任务是为后期作业的维护或清理。

- **作业准备任务**︰ 作业准备任务计划运行的任务，在执行任何其他作业任务之前的所有计算节点上运行。 您可以使用复制数据共享的所有任务，而是唯一的作业，例如作业准备任务。
- **作业发布任务**︰ 完成作业后，池中至少一个任务执行的每个节点上运行作业的发行任务。 您可以使用作业发布任务删除作业准备任务，复制的数据或用于压缩并上载诊断日志数据，例如。

同时作业准备工作，下达任务使您得以指定要调用任务时运行的命令行。 它们提供文件下载、 提升的执行、 自定义的环境变量、 最大的执行持续时间、 重试次数和文件保留时间等功能。

作业准备和发布任务的详细信息，请参阅[批处理 Azure 上的运行作业准备和完成任务计算节点](batch-job-prep-release.md)。

### <a name="multi-instance-task"></a>多实例任务

[多实例任务](batch-mpi.md)是配置为同时在多个计算节点上运行的任务。 多实例任务，则可以要求一组一起分配处理单个工作负载 (如消息传递接口 (MPI)) 的计算节点的高性能计算方案。

通过使用批处理.NET 库在批处理中运行 MPI 作业的详细讨论，请检查[使用多实例](batch-mpi.md)任务运行在 Azure 批处理中的消息传递接口 (MPI) 应用程序。

### <a name="task-dependencies"></a>任务相关性

[任务相关性](batch-task-dependencies.md)，顾名思义，允许您指定任务取决于之前执行其他任务的完成。 此功能提供支持的情况下的"下游"的任务使用的"上游"任务-或上游的任务可以执行一些初始化所需的后续任务的输出。 若要使用此功能，您必须首先启用批处理作业任务相关性。 然后，对于每个任务依赖于另一 （或许多其他人），指定此任务所依赖的任务。

与任务相关性，您可以配置方案如下所示︰

* *taskB*取决于*taskA* （*taskB*不会开始执行之前已完成*taskA* ）。
* *taskC*取决于*taskA*和*taskB*。
* *taskD*取决于一系列任务，如任务*1*到*10*，它执行之前。

签出[在 Azure 批处理任务相关性](batch-task-dependencies.md)和[TaskDependencies] [ github_sample_taskdeps] [批次的 azure 示例]中的代码示例[github_samples]GitHub 资料库获取此功能的更多深入的详细信息。

## <a name="environment-settings-for-tasks"></a>任务的环境设置

批服务执行的每个任务都有它在计算节点设置的环境变量的访问权限。 这包括环境变量的批处理服务定义 ([服务定义][msdn_env_vars])，您可以为任务定义的自定义的环境变量。 应用程序和任务执行的脚本在执行过程中有对这些环境变量的访问。

通过填充这些实体的*环境设置*属性，可以在任务或作业级别设置自定义的环境变量。 例如，请参阅[添加到作业任务][rest_add_task]操作 (批处理 REST API) 或[CloudTask.EnvironmentSettings] [net_cloudtask_env]和[CloudJob.CommonEnvironmentSettings] [net_job_env]批次.NET 中的属性。

您的客户端应用程序或服务可以通过[获取有关任务的信息]的获取任务的环境变量，服务定义和自定义，[rest_get_task_info]操作 (批处理 REST) 或通过访问[CloudTask.EnvironmentSettings] [net_cloudtask_env]属性 (批处理.NET)。 在计算节点上执行的进程可以访问这些以及其他环境变量的节点上，例如，通过使用熟悉`%VARIABLE_NAME%`(Windows) 或`$VARIABLE_NAME`(Linux) 语法。

您可以找到所有服务定义环境变量的完整列表中[计算节点环境变量][msdn_env_vars]。

## <a name="files-and-directories"></a>文件和目录

每个任务的*工作目录*下创建零或多个文件和目录。 这个工作目录可以用于存储任务、 处理，数据和执行该处理过程的输出通过运行该程序。 所有文件和目录的任务都负责任务的用户。

批服务公开为*根目录下*的一个节点上的文件系统的一部分。 任务可以通过引用来访问根目录`AZ_BATCH_NODE_ROOT_DIR`环境变量。 有关使用环境变量的详细信息，请参阅[任务的环境设置](#environment-settings-for-tasks)。

根区目录中包含以下目录结构︰

![计算节点目录结构][1]

- **共享**︰ 该目录提供了在一个节点上运行的*所有*任务的读/写访问。 任何在该节点运行的任务可以创建、 读取、 更新和删除此目录中的文件。 任务可以通过引用来访问此目录`AZ_BATCH_NODE_SHARED_DIR`环境变量。

- **启动**︰ 开始任务作为其工作目录使用此目录。 所有下载到该节点的开始任务的文件都存储在这里。 开始任务可以创建、 读取、 更新和删除该目录下的文件。 任务可以通过引用来访问此目录`AZ_BATCH_NODE_STARTUP_DIR`环境变量。

- **任务**︰ 在该节点上运行的每个任务创建一个目录。 它通过引用来访问`AZ_BATCH_TASK_DIR`环境变量。

    在每个任务的目录，批服务创建工作目录 (`wd`) 的唯一路径由`AZ_BATCH_TASK_WORKING_DIR`环境变量。 此目录提供给该任务的读/写访问。 此任务可以创建、 读取、 更新和删除该目录下的文件。 此目录将保留根据为任务指定的*RetentionTime*约束。

    `stdout.txt`和`stderr.txt`︰ 这些文件写入期间执行的任务的任务文件夹。

>[AZURE.IMPORTANT] 从池中删除某个节点后，将删除*所有*存储节点上的文件。

## <a name="application-packages"></a>应用程序包

[应用程序数据包](batch-application-packages.md)功能提供易于管理和部署的应用程序池中的计算节点。 您可以上载和管理任务，包括二进制文件和支持文件来运行应用程序的多个版本。 然后您可以自动部署一个或多个计算节点到这些应用程序在您的池。

您可以指定应用程序池和任务级别的文件包。 当指定池应用程序包时，该应用程序部署到池中的每个节点。 指定任务应用程序包时，将应用程序部署到计划要运行的节点至少一个任务的命令行运行前的工作任务。

批次处理使用 Azure 存储来存储您的应用程序包和部署它们来计算节点，这样可以简化您的代码和管理开销的详细信息。

若要了解有关应用程序打包功能的详细信息，敬请[与 Azure 批处理应用程序包的应用程序部署](batch-application-packages.md)。

>[AZURE.NOTE] 如果您添加到*现有*池的池应用程序包，您必须重新启动要部署到的节点的应用程序包为其计算节点。

## <a name="pool-and-compute-node-lifetime"></a>池和计算节点寿命

设计 Azure 批处理解决方案时，您必须作出设计决策如何以及池的创建，和长短计算这些池内的节点保持可用时。

在光谱的一端，可以创建一个池，以便提交时，每个作业并其任务完成执行后立即删除池。 因为节点仅分配所需，并一旦在闲置状态下关闭时，这可利用率最大化。 虽然这意味着作业必须等待分配的节点，它务必注意一旦节点是单独可用，分配，并开始任务已完成任务计划的执行。 Does 批*不*等待直到在池内的所有节点都都可用之前将任务分配到的节点。 这将确保所有可用节点的最大利用率。

在光谱的另一端，如果有立即启动的作业是最高的优先级，您可以创建一个池提前并使其节点可用之前提交作业时。 在这种情况下，任务可以立即开始，但在等待它们分配节点可能会保持空闲。

组合的方法通常用于处理变量，但正在进行的负载。 可以有多个作业提交到中，但可以扩展池根据作业上移或下移节点数负载 （请参阅下一节中的[缩放比例计算资源](#scaling-compute-resources)）。 您可以执行此操作反应性，基于当前负载，或主动地，如果可以预测负载。

## <a name="scaling-compute-resources"></a>缩放比例计算资源

[自动按比例缩放](batch-automatic-scaling.md)，您可以动态地调整当前工作负载和资源使用情况的计算方案根据池中的计算节点数批服务。 这使您可以降低总体成本运行您的应用程序使用的资源需要并释放那些不需要。

启用自动按比例缩放通过编写[自动缩放公式](batch-automatic-scaling.md#automatic-scaling-formulas)，并将该公式与池相关联。 批次服务使用公式来确定下一个刻度间隔 （可配置的时间间隔） 目标池中的节点数。 当您创建它，或启用缩放池上以后，可以指定池的自动缩放设置。 您还可以更新启用缩放的池上的缩放设置。

举一个例子，可能是某项作业需要提交执行任务的数量非常大。 还可以缩放公式调整根据当前排队的任务的数量和作业中的任务的完成率池中的节点数的池。 批次服务定期计算公式并调整其大小的池，根据工作负荷 （排队任务的多个节点中添加和移除节点没有排队或正在运行的任务） 和其他编辑设置。

扩展公式可以基于以下指标︰

- **时间指标**基于统计数据收集每隔五分钟，在指定的小时数。

- **资源指标**基于 CPU 使用率、 带宽使用情况、 内存使用情况和节点数。

- **任务指标**基于任务状态，如*活动*（排队）、*运行*，或*已完成*。

当自动按比例缩放减少池中的计算节点的数量时，必须考虑如何处理在减小操作时运行的任务。 为此，批提供可以在公式中包含一个*节点解除分配选项*。 例如，您可以指定正在运行的任务是立即停止、 立即停止，然后执行在另一个节点上，一位或允许完成之前从池中删除该节点。

有关自动缩放应用程序的详细信息，请参阅[自动比例计算 Azure 批池中的节点](batch-automatic-scaling.md)。

> [AZURE.TIP] 若要使计算资源利用率最大化，设置节点以零结尾的一项工作，但允许正在运行的任务可以完成的目标数。

## <a name="security-with-certificates"></a>证书安全性

您通常需要使用证书进行加密或解密任务，如项[Azure 存储帐户]的敏感信息时[azure_storage]。 若要支持此功能，您可以在节点上安装证书。 加密的密钥是通过命令行参数传递给任务或嵌入到某个任务的资源中, 和已安装的证书可以用于解密。

使用[添加证书][rest_add_cert]操作 (批处理 REST) 或[CertificateOperations.CreateCertificate] [net_create_cert]方法 (批处理.NET) 将证书添加到批次帐户。 然后，您可以关联到新的或现有池的证书。 当某个证书时与池相关联时，批服务池中的每个节点上安装证书。 该节点启动时，在启动任何 （包括启动任务和作业管理器任务） 的任务之前，批服务安装适当的证书。

如果您将证书添加到*现有*池，您必须重新启动证书要应用到的节点的计算节点。

## <a name="error-handling"></a>错误处理

您可能会发现有必要处理批处理解决方案内的任务和应用程序故障。

### <a name="task-failure-handling"></a>任务失败处理
任务失败分为以下几类︰

- **计划失败**

    如果由于某种原因为任务指定的文件传输失败，为任务设置"计划错误"。

    日程安排错误会发生如果已移动任务的资源文件，存储帐户不再可用，或阻止文件成功复制到节点，遇到了另一个问题。

- **应用程序故障**

    通过任务的命令行指定的过程也可能会失败。 过程就会被视为由该任务执行的过程返回非零退出代码时出现故障 （请参见下一节中的*退出代码的任务*）。

    针对应用程序失败，您可以配置批处理自动重试最多指定次数的任务。

- **约束失败**

    您可以设置用于指定作业或任务， *maxWallClockTime*最大执行时间的约束。 这可用于终止挂起的任务。

    已超出最大的一段时间后，该任务标记为*已完成*，但退出代码设置为`0xC000013A`， *schedulingError*字段标记为`{ category:"ServerError", code="TaskEnded"}`。

### <a name="debugging-application-failures"></a>调试应用程序故障

- `stderr`和`stdout`

    在执行期间，应用程序可能会产生可用于解决问题的诊断输出。 批服务如[文件和目录](#files-and-directories)前面一节中所述，将标准输出和标准错误输出到`stdout.txt`，`stderr.txt`在计算节点上的任务目录中的文件。 可以使用 Azure 门户或一批 Sdk 下载这些文件。 例如，您可以检索这些文件以及其它文件为了排除故障时使用[ComputeNode.GetNodeFile] [net_getfile_node]和[CloudTask.GetNodeFile] [net_getfile_task]批次.NET 库中。

- **任务的退出代码**

    前面已提到，任务被标记为如果执行任务的过程返回非零退出代码由批次服务失败。 在任务执行过程时，批处理填充任务的退出代码属性*返回进程的代码*。 值得注意的是任务的退出代码将**不**由批次服务中，它由进程本身或在其执行该进程的操作系统。

### <a name="accounting-for-task-failures-or-interruptions"></a>在核算的任务失败或中断

有时可能会失败或被中断的任务。 任务应用程序本身可能会失败，可能会重新启动该任务正在其运行的节点，或节点可能会删除从池大小调整操作期间如果该池的解除分配策略设置为立即删除节点，而不必等待要完成的任务。 在所有情况下，任务可以进行自动重新排队通过批处理在另一个节点上执行。

还有可能导致挂起或需要很长时间执行任务的间歇性问题。 您可以设置任务的最大执行时间。 如果超出，则批中断任务应用程序。

### <a name="connecting-to-compute-nodes"></a>计算节点连接

您可以执行其他调试和疑难解答的方法是远程登录到计算节点。 可以使用 Azure 的门户网站下载 Windows 节点远程桌面协议 (RDP) 文件并获得 Linux 节点安全外壳协议 (SSH) 连接信息。 也可以执行此操作通过使用批处理 Api-例如，[批处理].net[net_rdpfile]或[批处理 Python](batch-linux-nodes.md#connect-to-linux-nodes)。

>[AZURE.IMPORTANT] 要连接到的节点通过 RDP 或 SSH，必须首先在该节点上创建用户。 若要执行此操作，您可以使用 Azure 门户，[添加到一个节点的用户帐户][rest_create_user]通过批处理 REST API 调用[ComputeNode.CreateComputeNodeUser] [net_create_user]在批处理.NET 或调用[add_user]方法[py_add_user]批 Python 模块中的方法。

### <a name="troubleshooting-bad-compute-nodes"></a>疑难解答"不利"计算节点

其中一些任务的失败的情况下，批客户端应用程序或服务可以检查的元数据中标识行为错误节点失败的任务。 池中的每个节点具有一个唯一的 ID，以及运行任务的节点包含任务元数据中。 您已经标识了问题中的一个节点后，可以执行以下操作︰

- **重新启动节点**([REST][rest_reboot] | [.NET][net_reboot])

    有时，重新启动节点可以澄清潜在的问题，如停滞或崩溃的进程。 请注意，是否您的池使用开始任务或作业使用作业准备工作任务，执行节点重新启动时。

- **重新映射节点**([REST][rest_reimage] | [.NET][net_reimage])

    这将在该节点上重新安装操作系统。 与重新启动节点，启动任务和作业的准备工作重新运行后已最先该节点。

- **删除库中的节点**([REST][rest_remove] | [.NET][net_remove])

    有时有必要完全从池中删除该节点。

- **禁用在该节点上计划的任务**([REST][rest_offline] | [.NET][net_offline])

    这有效地采用节点"脱机"，使任何进一步的任务指派给它，但允许节点保持运行并在池中。 这使您可以执行进一步调查失败的原因而失败的任务数据 — 然后节点而导致其他任务失败。 例如，您可以禁用任务计划在节点，然后[远程登录](#connecting-to-compute-nodes)来检查该节点的事件日志或执行其他故障排除步骤。 您已经完成了调查后，可以通过启用任务的日程排定然后将节点返回在线状态 ([其余][rest_online] | [.NET][net_online])，或执行一个前面讨论过的其他操作。

> [AZURE.IMPORTANT] 在此部分中，描述每个操作与重新启动，重新映像、 删除和禁用计划任务-现在可以指定执行操作时如何处理任务当前正在运行的节点上。 例如，禁用任务的日程排定在一个节点上使用批处理.NET 客户端库时，您可以指定[DisableComputeNodeSchedulingOption] [net_offline_option]枚举值以指定是否**终止**正在运行的任务， **Requeue**他们在其他节点上安排或允许正在运行的任务完成后才能执行的操作 (**TaskCompletion**)。

## <a name="next-steps"></a>下一步行动

- 通过示例批处理应用程序中[开始使用.NET Azure 批库](batch-dotnet-get-started.md)分步指导。 也是在 Linux 计算节点运行工作负荷教程的[Python 版本](batch-python-tutorial.md)。

- 下载并生成[批次浏览器][github_batchexplorer]批次解决方案在开发时使用的示例项目。 使用批次浏览器，您可以执行以下等等︰
  - 监控和操作池、 作业和批帐户中的任务
  - 下载`stdout.txt`， `stderr.txt`，和从节点中的其他文件
  - 在节点上创建用户和远程登录的 RDP 文件下载

- 了解如何[创建池的 Linux 计算节点](batch-linux-nodes.md)。

- 访问[Azure 批论坛][batch_forum]在 MSDN 上。 论坛是一个好的地方，要提出的问题，是否只学习或在批处理中使用的是专家。

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
