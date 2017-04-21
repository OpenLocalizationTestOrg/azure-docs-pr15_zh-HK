<properties
    pageTitle="Azure 词汇表的 Azure 字典 |Microsoft Azure"
    description="使用 Azure 的词汇表来了解云在 Azure 平台上的术语。 此简短的 Azure 字典提供 Azure 的云词汇的定义。"
    keywords="Azure 字典、 云术语，Azure 的词汇、 术语定义、 云条款"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure 术语表︰ 在 Azure 平台上的云术语词典

Microsoft Azure 词汇表是短的平台 Azure 的云术语字典。

## <a name="find-service-definitions-and-other-cloud-terms"></a>查找服务定义和其他云条款

* Azure 服务和其 AWS 定义的对应项请参阅[Microsoft Azure 和 Amazon 的 Web 服务](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/)。

* 一般行业云条款有关[云计算的条款](https://azure.microsoft.com/overview/cloud-computing-dictionary/)。

与上述两个引用的 Azure 词汇表 Azure 的云行业提供端到端分类。  

## <a name="azure-glossary-list"></a>Azure 的术语表列表


### <a name="account"></a>帐户  
工作或学校或用来访问和管理订阅了 Azure 的 Microsoft 个人帐户。  
请参阅[如何 Azure 订阅将与 Azure Active Directory 相关联](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>可用性设置  
一起以提供应用程序冗余和可靠性进行管理的虚拟机的集合。 使用设置的可用性的可确保在计划内或计划外维护事件期间至少一个虚拟机可用。  
另请参阅[管理 Windows 虚拟机的可用性](./virtual-machines/virtual-machines-windows-manage-availability.md)或[管理 Linux 虚拟机的可用性](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Azure 的典型部署模式  
两种[部署模型](resource-manager-deployment-model.md)用来部署在 Azure （新模型是 Azure 资源管理器） 中的资源之一。 一些 Azure 的资源可以部署一个模型或另一个，而其他人可以部署在这两个模型中。 可以与部署指南各个 Azure 资源分类资源的详细信息。


### <a name="cli"></a>Azure 的命令行界面 (CLI)  
可用于管理 Azure 服务窗口、 OSX 和 Linux Pc 从一个[命令行界面](xplat-cli-install.md)。


### <a name="powershell"></a>Azure PowerShell  
一个[命令行界面](powershell-install-configure.md)管理 Azure 服务通过命令行从 Windows 电脑。 可以只通过 PowerShell 或 CLI 管理某些服务或服务功能。 对于每个单独的 Azure 资源详细信息的分类资源的指南可以进行部署。   
请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)


### <a name="arm-model"></a>Azure 的资源管理器部署模型  
两种[部署模型](resource-manager-deployment-model.md)用于部署 Microsoft Azure （另一种是传统的部署模型） 中的资源之一。 一些 Azure 的资源可以部署一个模型或另一个，而其他人可以部署在这两个模型中。 可以与部署指南各个 Azure 资源分类资源的详细信息。


### <a name="fault-domain"></a>容错域  
中可能会失败一次可用性设置虚拟机的集合。 一个示例是一组共享公用电源源和网络交换机机架中机。 在 Azure，可用性组中的虚拟机自动分隔跨多个故障域。  
另请参阅[管理 Windows 虚拟机的可用性](./virtual-machines/virtual-machines-windows-manage-availability.md)或[管理 Linux 虚拟机的可用性](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>地区  
通常包含两个或多个区域的数据派驻一个定义的边界。 边界和内或以外国家的边框可能受税款调整。 每个地区都有至少一个区域。 Geo 的示例包括亚太及日本地区。 也称为*地理位置*。  
请参阅[Azure 的区域](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>geo 复制  
自动复制内容，如 blob、 表和队列对内部的区域进程。  
请参阅[活动的地理-复制 SQL Azure 数据库](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>图像  
一个包含操作系统和应用程序配置，可以用于创建任意数量的虚拟机的文件。 在 Azure 中有两种类型的图像︰ VM 映像和 OS 映像。 虚拟机映像包含操作系统和所有在创建映像时附加到虚拟机的磁盘。 OS 映像包含仅通用的操作系统没有数据磁盘配置。  
请参阅[导航和选择的 Windows 虚拟机映像，使用 PowerShell 或 CLI 的 Azure 中](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>限制  
可以创建的资源或可实现的性能基准测试的数量。 限制是通常与订阅、 服务和产品相关联。  
请参阅[Azure 订阅和服务限制、 配额和约束](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>负载平衡器  
一种分布传入的通信量在网络中的计算机之间的资源。 在 Azure，负载平衡器分配给虚拟机的负载平衡器集内定义的流量。 [负载平衡器](./load-balancer/load-balancer-overview.md)可以面向 internet 或内部。  


### <a name="offer"></a>提供  
定价、 富余吞吐量和相关的条款适用于 Azure 的订阅。  
请参阅[Azure 提供详细信息页面](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>门户网站  
安全的 Web 门户用于部署和管理 Azure 服务。  有两个门户网站︰ [Azure 门户](http://portal.azure.com/)和[经典的门户](http://manage.windowsazure.com/)。 某些服务有两个门户网站，而其他人只是在一个或另一个。 [Azure 门户可用性图表](https://azure.microsoft.com/features/azure-portal/availability/)列出了哪些服务是可用的门户中。  


### <a name="region"></a>地区  
不交叉区域的边框并包含一个或多个数据中心地理区域。 在地区级别公开定价、 区域服务和提供类型。 一个地区与另一地区，可能长达几个几百英里之外，形成一个区域对通常成对出现。 区域对可作为一种机制，用于灾难恢复和高可用性方案中。 也称为一般*位置*。  
请参阅[Azure 的区域](best-practices-availability-paired-regions.md)


### <a name="resource"></a>资源  
该项目，Azure 解决方案的一部分。 每个 Azure 服务使您能够将不同类型的资源，例如数据库或虚拟机的部署。   
请参阅[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>资源组  
容器资源管理器中包含应用程序的相关的资源。 资源组可以包含的所有应用程序的资源或逻辑组合在一起的那些资源。 您可以决定要如何将资源分配给资源组基于什么最有意义的组织。  
请参阅[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>资源管理器模板  
JSON 文件中以声明方式定义一个或多个 Azure 资源以及定义已部署的资源之间的依赖关系。 可以使用模板来部署资源，持续和反复。  
请参阅[创作 Azure 资源管理器模板](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>资源提供程序  
您可以部署和管理通过资源管理器中的资源提供服务。 每个资源提供商提供如何使用部署资源的操作。 资源提供程序可以访问通过 Azure 门户、 Azure PowerShell 和几个编程的 Sdk。  
请参阅[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>角色  
一种方法来控制可以指派给用户、 组和服务的访问。 角色还可以执行的操作，如创建，管理，和自学 Azure 的资源。  
请参阅[RBAC︰ 内置角色](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>服务级别协议 (SLA)  
介绍了 Microsoft 的承诺的正常运行时间和连接协议。 每个 Azure 服务都有一个特定的 SLA。  
请参阅[服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>存储帐户  
让您的存储帐户访问到 Azure Blob、 队列、 表和文件在 Azure 存储服务。 您的存储帐户为 Azure 存储数据对象提供唯一的命名空间。  
请参阅[有关 Azure 存储帐户](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>订阅  
Microsoft 使他们能够获得 Azure 服务与客户的协议。 预订价格和相关的条款受选择订阅优惠。 请参阅[Microsoft 在线订阅协议](https://azure.microsoft.com/support/legal/subscription-agreement/)。  
请参阅[如何 Azure 订阅将与 Azure Active Directory 相关联](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>标记  
使您可以根据您的要求管理或付费的资源分类索引术语。 当您有一个复杂的资源组和资源，并且需要直观显示这些资产中最有意义的方式，您可以使用标记。 例如，可以标记在您的组织中起类似的角色，或者属于同一部门的资源。  
请参阅[使用标签来组织您的 Azure 资源](resource-group-using-tags.md)


### <a name="update-domain"></a>更新域  
在可用性集中在同一时间更新的虚拟机的集合。 同一个更新域中的虚拟机重新启动在一起在计划内维护期间。 Azure 从不重新启动一次的多个更新域。 也称为升级的域。  
另请参阅[管理 Windows 虚拟机的可用性](./virtual-machines/virtual-machines-windows-manage-availability.md)或[管理 Linux 虚拟机的可用性](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>虚拟机  
运行操作系统的物理计算机软件实现。 多个虚拟机可以同时运行在相同的硬件上。 在 Azure，虚拟机可以使用在各种尺寸。  
请参阅[虚拟机文档](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>虚拟机的扩展  
一种实现行为或功能，或者帮助其他程序正常工作，或者为您进行交互的能力提供正在运行的计算机的资源。 例如，可以使用虚拟媒体访问扩展名重新设置或修改远程访问值 Azure 的虚拟机上。  
请参阅[有关虚拟机的扩展和功能 (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md)或[虚拟机的扩展和功能 (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>虚拟网络  
您是独立于所有其他 Azure 承租人的 Azure 资源之间提供连接的网络。 它可以连接到其他 Azure 的虚拟网络，通过[Azure VPN 网关](./vpn-gateway/vpn-gateway-about-vpngateways.md)和内部网络使用[多个选项](./vpn-gateway/vpn-gateway-cross-premises-options.md)。 您可以完全控制的 IP 地址块、 DNS 设置、 安全策略以及在此网络中的路由表。  
请参阅[虚拟网络概述](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**请参见**  
- [开始使用 Azure](https://azure.microsoft.com/get-started/)
- [云资源中心](https://azure.microsoft.com/resources/)  
- [Azure 的业务应用程序](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure 数据中心](https://azure.microsoft.com/overview/business-apps-on-azure/) 





