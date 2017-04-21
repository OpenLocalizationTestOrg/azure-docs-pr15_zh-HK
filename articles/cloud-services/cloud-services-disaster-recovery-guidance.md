<properties
    pageTitle="如何在 Azure 服务中断影响 Azure 云服务 |Microsoft Azure"
    description="了解如何影响 Azure 云服务 Azure 服务中断时。"
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>如何在 Azure 服务影响 Azure 云服务的中断

在 Microsoft，我们努力确保我们的服务始终可供您在需要时。 有时超出我们控制范围的因素会影响我们造成计划外的服务中断的方法。

Microsoft 提供的服务级别协议 (SLA) 为其服务作为承诺的正常运行时间和连接。 各个 Azure 服务的 SLA 可以位于[Azure 服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)。

Azure 已经有很多内置的平台功能支持高可用性的应用程序。 有关这些服务的详细信息，请阅读[灾难恢复和 Azure 应用程序的高可用性](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)。

本文介绍了真正的灾难恢复情形中，当整个地区出现因重大自然灾害或广泛分布的服务中断而中断。 这些都是极少出现，但必须是整个区域的停机可能性的准备。 如果整个地区遇到服务中断，您的数据的本地冗余副本会暂时不可用。 如果您已启用了地区复制，Azure 存储 blob 和表的三个附加副本存储在不同的地区。 在完整的区域停机或其中的主要区域是不可恢复的灾难时，Azure 重新映射所有地区复制区域的 DNS 条目。

>[AZURE.NOTE]请注意，您没有对此过程中，任何控制它，才会对数据中心范围内的服务中断。 因此，您还必须依靠其他特定于应用程序的备份策略，以实现最高级别的可用性。 有关详细信息，请参阅有关[数据灾难恢复策略](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR)的部分。 如果您想要能够影响自己的故障切换，您可能需要考虑使用的[读取访问权限地理冗余存储 (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage)，从而在另一个区域中创建数据的只读副本。

为了帮助您处理这些极少数的情况，我们在 Azure 虚拟机应用程序的部署位置的整个地区的服务中断的情况下提供 Azure 的虚拟机 (Vm) 的以下指南。

##<a name="option-1-wait-for-recovery"></a>选项 1︰ 等待恢复
在这种情况下，您没有操作是必需的。 知道，Azure 团队正在坚持不懈地还原服务的可用性。 我们[Azure 服务健康仪表板](https://azure.microsoft.com/status/)上，可以看到当前的服务状态。

>[AZURE.NOTE]如果客户尚未设置 Azure 站点恢复或具有辅助部署在其他地区，这是最好的选择。

对于那些想对其部署的云服务的即时访问客户，提供了下列选项。

>[AZURE.NOTE]请注意，这些选项的某些数据丢失的可能性。     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>选项 2︰ 重新部署到新地区云服务配置

如果您有您的原始代码，您可以简单地只是重新部署应用程序关联的配置，新的云服务中一个新的区域的相关的资源。  

有关如何创建和部署一个云服务应用程序的详细信息，请参阅[如何创建和部署云服务](./cloud-services-how-to-create-deploy-portal.md)。

这取决于您的应用程序的数据源，您可能需要检查您的应用程序的数据源的恢复过程。
  * 对于 Azure 存储数据源，请参阅[Azure 存储复制](../storage/storage-redundancy.md#read-access-geo-redundant-storage)检查可用的选项根据您的应用程序选择复制模型。
  * 对于 SQL 数据库源读取[概述︰ 云业务连续性和数据库灾难恢复与 SQL 数据库](../sql-database/sql-database-business-continuity.md)检查可用的选项根据您应用程序的选择的复制模型。

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>选项 3︰ 使用备份部署通过 Azure 通信管理器
此选项假定您已经设计了注意地区性灾难恢复与您应用程序的解决方案。 如果您已经运行在不同的地区，通过流量管理器通道连接辅助云服务的应用程序部署，可以使用此选项。 在这种情况下，检查辅助部署的运行状况。 如果状态良好，可以重定向流量到它通过 Azure 流量管理器。 利用此策略，您可以利用通信路由方法和故障转移顺序配置的 Azure 流量管理器中。 有关详细信息，请参阅[如何配置通信量管理器设置](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings)。

![Azure 云服务均衡区域使用 Azure 通信管理器](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>下一步行动

若要了解有关如何实现灾难恢复和高可用性策略的详细信息，请参阅[灾难恢复和 Azure 应用程序的高可用性](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)。

制定详细的技术了解云平台的功能，请参阅[Azure 复原技术指导](../resiliency/resiliency-technical-guidance.md)。

如果说明不清除，或者如果您希望 Microsoft 执行这些操作以您的名义请联系[客户支持](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)。
