<properties
   pageTitle="在云中的批次和 HPC 解决方案 |Microsoft Azure"
   description="了解批处理和高性能计算 （HPC 和大计算） 方案以及在 Azure 中的解决方案选项"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="07/27/2016"
   ms.author="danlep"/>

# <a name="batch-and-hpc-solutions-in-the-azure-cloud"></a>在 Azure 的云的批次和 HPC 解决方案

Azure 提供高效、 可扩展的云解决方案︰ 批处理和高性能计算 (HPC)-也称为*大计算*。 在此处了解大计算工作负载和 Azure 的服务，以支持它们，或者直接跳转到本文中稍后介绍的[解决方案应用场景](#scenarios)。 本文主要针对技术决策者、 IT 经理和独立软件供应商，但其他 IT 专业人员和开发人员可以使用它来熟悉这些解决方案。

组织具有大规模计算问题︰ 工程设计和分析、 图像呈现、 复杂建模、 Monte Carlo 模拟、 财务风险的计算，和其他人。 Azure 可以帮助组织解决这些问题的资源、 规模和他们需要的计划。 Azure，组织能够︰

* 创建混合解决方案，扩展内部 HPC 群集卸载到云的峰值工作负荷

* 运行完全用 Azure 的 HPC 群集工具和工作负载

* 使用管理而且可扩展的 Azure 服务，例如，[批处理](https://azure.microsoft.com/documentation/services/batch/)运行计算密集型工作负载而无需部署和管理计算基础结构

虽然超出了本文的范围，Azure 还为开发人员提供并合作，一套完整的功能、 体系结构选择和开发工具，建立大规模的自定义大计算工作流。 和一个不断增长的合作伙伴生态系统已准备就绪，可帮助您使您大计算的工作负载在 Azure 的云的效率高。


## <a name="batch-and-hpc-applications"></a>批处理和 HPC 应用程序

与 web 应用程序和多个业务线应用程序、 批处理和 HPC 应用程序有定义的开头和结尾，它们可以运行的时间表或点播，有时个小时或更长时间。 大多数属于两个主要类别︰*本质上并行*（有时称为"embarrassingly 并行"，因为它们解决的问题同样适用于多台计算机或处理器上并行运行） 和*紧密结合在一起*。 有关这些应用程序类型的详细信息，请参阅下表。 一些 Azure 解决方案方法的一种类型或另一个更好的工作。

>[AZURE.NOTE] 批处理和 HPC 解决方案中应用程序的运行实例通常被称为一项*作业*，并可能获取每个作业划分为*任务*。 并聚集的计算资源的应用程序通常被称为*计算节点*。

类型 | 特征 | 示例
------------- | ----------- | ---------------
**本质上并行**<br/><br/>![本质上并行][parallel] |• 每台计算机独立运行的应用程序逻辑<br/><br/> • 添加计算机允许应用程序扩展，并减少计算时间<br/><br/>• 应用程序包含单独的可执行文件，或被划分为一组服务调用的客户端 （面向服务的体系结构或 SOA 应用程序） |• 金融风险建模<br/><br/>• 图像呈现和图像处理<br/><br/>• 媒体编码和转码<br/><br/>• Monte Carlo 模拟<br/><br/>• 软件测试
**紧密结合在一起**<br/><br/>![紧密结合在一起][coupled] |• 应用程序需要能够交互或交换的中间结果的计算节点<br/><br/>• 计算节点可能会使用消息传递接口 (MPI)，并行计算常用的通信协议进行通信<br/><br/>• 应用程序是对网络延迟和带宽敏感<br/><br/>通过使用诸如无限带宽和远程直接内存访问 (RDMA) 的高速网络技术，可以提高 • 应用程序性能 |• 石油和气体容器建模<br/><br/>• 工程设计和分析，例如计算流体动力学<br/><br/>• 如汽车崩溃和核电站反应的物理模拟<br/><br/>• 天气预测

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>在云中运行批处理和 HPC 应用程序注意事项

您随时可以迁移许多旨在到 Azure，或混合 （跨部署） 环境内部 HPC 群集中运行的应用程序。 但是，可能有一些限制或考虑因素，包括︰


* **云资源的可用性**-这取决于类型的云计算资源使用，您可能无法运行作业时依靠连续机的可用性。 状态处理和进度检查指向是常用的方法，来处理可能的瞬时性故障和更多的需要使用云资源。


* **数据访问**-在企业群集，如 NFS 中通常可用的数据访问技术可能需要特殊的配置，在云中。 或者，您可能需要采用不同的数据访问方法和云的模式。

* **数据移动**-处理大量的数据，策略的应用程序需要将数据移到云存储和计算资源。 您可能需要高速跨内部如[Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)网络。 此外应考虑法律、 法规或者政策限制存储或访问该数据。


* **授权**-运行在云中的许可或其他限制的任何商业应用程序的供应商咨询。 不是所有的供应商提供随增长付费授权。 您可能需要规划许可中为您的解决方案中，云服务器或连接到内部部署许可证服务器。


### <a name="big-compute-or-big-data"></a>大计算或大数据？

大计算和大数据的应用程序之间的分界线并不总是清楚，和一些应用程序可能有这两者的特征。 两者都涉及到通常的计算机群集上运行大规模的计算。 但解决方案方法和支持工具可能会有所不同。

•**大计算**往往涉及应用程序依赖于 CPU 的处理能力和内存，如工程设计模拟、 建模、 财务风险和数字呈现。 一种大计算的解决方案的基础结构可能包括专门执行原始计算和专门的、 高速连接的计算机的网络硬件的多核处理器的计算机。

•**大数据**解决了涉及大量的数据，不能由单个计算机或数据库管理系统的数据分析问题。 例如，大量的 web 日志或其他商业智能数据。 大数据往往更依赖于磁盘容量和比 CPU 的处理能力上的输入/输出性能。 也有专门的大数据工具如 Apache Hadoop 群集和分区数据进行管理。 （Azure HDInsight 和其他 Azure Hadoop 解决方案的信息，请参阅[Hadoop](https://azure.microsoft.com/solutions/hadoop/)。）

## <a name="compute-management-and-job-scheduling"></a>管理和作业级排产计算

运行批处理和 HPC 应用程序通常包括*群集管理器*和*作业计划程序*来帮助管理群集的计算资源并将它们分配给运行的作业的应用程序。 这些功能可能通过单独的工具，或集成的工具或服务。

* **群集管理器**的规定，释放，并管理计算资源 （或计算节点）。 群集管理器可以自动的操作系统映像的安装和计算节点上的应用程序扩展计算资源，根据请求，并监视节点的性能。

* **作业计划程序**-指定的资源 （如处理器或内存） 应用程序的需要，以及在运行时的条件。 作业计划程序维护的作业队列和分配给他们根据分配的优先级或其他特征的资源。

好到 Azure 可以迁移群集和作业级排产基于 Windows 和基于 Linux 的群集的工具。 例如， [Microsoft HPC 包](https://technet.microsoft.com/library/cc514029)，为 Windows 和 Linux HPC 的工作负载，Microsoft 的免费计算群集解决方案提供了几个选项用于在 Azure 中运行。 您还可以构建 Linux 群集运行扭矩和 SLURM 的开放源代码工具。 您还可为 Azure， [TIBCO DataSynapse GridServer](http://www.tibco.com/company/news/releases/2016/tibco-to-accelerate-cloud-adoption-of-banking-and-capital-markets-customers-via-microsoft-collaboration)、 [IBM 平台交响乐](http://www-01.ibm.com/support/docview.wss?uid=isg3T1023592)和[Univa 网格引擎](http://www.univa.com/products/grid-engine)等提供专业的网格解决方案。

以下各节所示，您还可以利用 Azure 服务管理计算资源和计划作业无需 （或除了） 传统的群集管理工具。


## <a name="scenarios"></a>方案

以下是三种常见方案运行大计算工作负载在 Azure 中使用现有的 HPC 群集解决方案、 Azure 服务或两者的组合。 选择每个方案的关键考虑事项列出，但并不详尽。 更多有关您可能使用您的解决方案中可用的 Azure 服务是在文章后面。

  | 方案 | 为什么选择它？
------------- | ----------- | ---------------
**突发的 HPC 群集到 Azure**<br/><br/>[![群集突发][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> 了解更多信息：<br/>•[爆发到 Azure 辅助实例与 HPC 包](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>•[设置混合计算群集与 HPC 包](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>•[爆发到 Azure 批 HPC 包](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/>|• 将与 Azure 混合解决方案中的其他资源组合您的[Microsoft HPC 包](https://technet.microsoft.com/library/cc514029)或其他内部群集。<br/><br/>• 扩展您大计算的工作负载运行平台即服务 (PaaS) 虚拟机实例 （当前 Windows 服务器只）。<br/><br/>• 通过使用可选的 Azure 虚拟网络上部署许可证服务器或数据存储的访问|• 您现有的 HPC 群集并需要更多资源 <br/><br/>• 不想购买和管理其他 HPC 群集的基础结构<br/><br/>• 具有瞬时峰值需求期或特殊项目
**创建完全在 Azure 中的 HPC 群集**<br/><br/>[![在 IaaS 群集][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>了解更多信息：<br/>•[在 Azure 的 HPC 群集解决方案](./big-compute-resources.md)<br/><br/>|• 快速、 一致地部署您的应用程序和群集工具标准或自定义 Windows 或 Linux 基础结构作为服务 (IaaS) 的虚拟机。<br/><br/>• 使用作业级排产您所选择的解决方案来运行各种大计算工作负载。<br/><br/>• 使用其他 Azure 服务包括网络连接和存储，以创建完整的基于云的解决方案。 |• 不想购买和管理其他 Linux 或 Windows HPC 群集基础结构<br/><br/>• 具有瞬时峰值需求期或特殊项目<br/><br/>• 您需要其他群集一段时间，但不想投资于计算机和空间进行部署<br/><br/>• 要减轻计算密集型应用程序，因此它作为完全以云服务运行
**横向扩展到 Azure 并行应用程序**<br/><br/>[![Azure 批][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>了解更多信息：<br/>• [Azure 批处理基础知识](./batch-technical-overview.md)<br/><br/>[学习如何使用.NET 的 Azure 批库](./batch-dotnet-get-started.md)•|• 开发使用[Azure 批](https://azure.microsoft.com/documentation/services/batch/)出各种大计算工作负载运行 Windows 或 Linux 虚拟机池扩展。<br/><br/>• 使用 Azure 平台服务来管理部署和虚拟机、 作业级排产、 灾难恢复、 数据移动、 依赖关系管理和应用程序部署的自动缩放。|• 不想管理计算资源或作业计划程序;相反，您希望将精力集中在运行您的应用程序<br/><br/>您想要减轻计算密集型应用程序，因此它作为服务运行在云中的 •<br/><br/>• 您想要自动缩放您的计算资源进行匹配计算工作负荷


## <a name="azure-services-for-big-compute"></a>针对大计算 azure 服务

下面是有关计算、 数据、 网络和相关的服务可以将合并的大计算解决方案和工作流的详细信息。 Azure 服务的详细指导，请参阅 Azure 服务[文档](https://azure.microsoft.com/documentation/)。 在本文前面[方案](#scenarios)说明使用这些服务的只是一些方法。

>[AZURE.NOTE] Azure 定期引入新的服务，可能适用于您的方案。 如果有疑问，请联系[Azure 合作伙伴](https://pinpoint.microsoft.com/en-US/search?keyword=azure)或电子邮件*bigcompute@microsoft.com*。

### <a name="compute-services"></a>计算服务

Azure 计算服务是一个大计算的解决方案，以及不同情况下不同的计算服务优惠优势的核心。 在基本级别，这些服务提供 Azure 提供了使用 Windows 服务器 Hyper-V 技术的基于虚拟机的计算实例上运行的应用程序的不同的模式。 这些实例可以运行标准和自定义 Linux 和 Windows 操作系统的系统和工具。 Azure 使您选择的[实例大小](../virtual-machines/virtual-machines-windows-sizes.md)具有不同配置的 CPU 内核、 内存、 磁盘容量以及其他特性。 根据您的需要，可以扩展到数千个内核的实例，然后缩小，当您需要更少的资源。

>[AZURE.NOTE] 充分利用 Azure 的计算密集型实例，以提高性能和 HPC 工作负荷包括需要低延迟和高吞吐量的应用程序网络的并行 MPI 应用程序的可伸缩性。 请参阅[关于 H 系列和计算密集型的 A 系列虚拟机](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)。  

服务 | 说明
------------- | -----------
**[虚拟机](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• 提供计算基础结构即服务 (IaaS) 使用 Microsoft Hyper-V 技术<br/><br/>• 使您能够灵活地调配和管理从标准 Windows 服务器或 Linux 映像从[Azure 市场](https://azure.microsoft.com/marketplace/)，或图像和数据磁盘提供的持久的云计算机<br/><br/>• 可以部署和作为[VM 扩展集](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)构建大规模服务从相同的虚拟机，具有自动缩放，以增大或减小容量自动管理<br/><br/>• 运行内部计算群集工具和完全在云中的应用程序<br/><br/>
**[云服务](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• 可以大计算以运行应用程序辅助角色实例，运行版本的 Windows 服务器的虚拟机并完全由 Azure 托管<br/><br/>• 支持可扩展、 可靠具有较低的管理开销，平台即服务 (PaaS) 模型中运行的应用程序<br/><br/>• 可能需要其他工具或开发与内部 HPC 群集解决方案集成
**[批处理](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• 完全托管服务中运行大规模的并行和批处理工作负载<br/><br/>• 提供作业级排产和自动缩放的托管的虚拟机池<br/><br/>• 允许开发人员生成和作为服务运行的应用程序或现有应用程序启用云的<br/>

### <a name="storage-services"></a>存储服务

一个大计算解决方案通常作用于一组输入数据，并生成其结果的数据。 一些大的计算解决方案中使用 Azure 存储服务包括︰

* [Blob、 表和队列存储](https://azure.microsoft.com/documentation/services/storage/)管理大量的非结构化的数据、 NoSQL 数据和消息的工作流和通讯，分别。 例如，您可能使用 blob 存储技术的大型数据集，或输入图像或媒体文件的应用程序进程。 为解决方案中的异步通信，可以使用队列。 请参阅[Microsoft Azure 存储的简介](../storage/storage-introduction.md)。

* [Azure 文件存储](https://azure.microsoft.com/services/storage/files/)的共享公共文件和 Azure 使用标准的 SMB 协议用于一些 HPC 群集解决方案中的数据。

* [数据湖商店](https://azure.microsoft.com/services/data-lake-store/)-提供了云，用于批处理，实时和交互式分析 hyperscale Apache Hadoop 分布式文件系统。

### <a name="data-and-analysis-services"></a>数据和分析服务

一些大的计算方案涉及到大规模的数据流量，或者生成需要进一步处理或分析数据。 Azure 提供一些数据和分析服务，包括︰

* [数据工厂](https://azure.microsoft.com/documentation/services/data-factory/)-生成数据驱动的工作流 （管道） 该联接、 聚合和转换和数据进行内部部署、 基于云的互联网数据存储区。

* [SQL 数据库](https://azure.microsoft.com/documentation/services/sql-database/)-提供托管服务的 Microsoft SQL Server 关系数据库管理系统的主要功能。

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)的部署和规定 Windows 服务器或基于 Linux 的 Apache Hadoop 群集在云中管理、 分析和报告重要数据。

* [机器学习](https://azure.microsoft.com/documentation/services/machine-learning/)-可以帮助您创建、 测试、 操作和管理完全托管服务中预测分析的解决方案。

### <a name="additional-services"></a>其他服务

大计算解决方案可能需要连接到部署的资源，或者在其他环境中其他 Azure 服务。 示例包括︰

* [虚拟网络](https://azure.microsoft.com/documentation/services/virtual-network/)-在 Azure 可以连接 Azure 的资源彼此或内部数据中心中创建逻辑上独立的部分。 大计算应用程序可以跨部署的虚拟网络，与访问内部数据、 Active Directory 服务和许可证服务器

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) -创建 Microsoft 数据中心和基础结构的内部之间或主机托管环境中的专用连接。 ExpressRoute 通过互联网提供了更高的安全性、 更多的可靠性、 更快的速度和低延迟时间比典型的连接。

* [服务总线](https://azure.microsoft.com/documentation/services/service-bus/)提供几种机制为应用程序进行通讯或交换数据，无论它们位于 Azure，在另一个云平台上，或在数据中心。

## <a name="next-steps"></a>下一步行动

* 请参阅[批处理和 HPC 的技术资源](big-compute-resources.md)，以找到要生成解决方案的技术指导。

* 与合作伙伴，包括周期计算和 UberCloud 讨论 Azure 的选项。

* 阅读有关由[塔 Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)、 [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)、 [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)和[d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)的 Azure 大计算解决方案。

* 最新的公告，请参阅[Microsoft HPC 和批处理团队博客](http://blogs.technet.com/b/windowshpc/)和[Azure 的博客](https://azure.microsoft.com/blog/tag/hpc/)。

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
