<properties
    pageTitle="管理 Azure 云服务使用 Azure 自动化 |Microsoft Azure"
    description="了解如何使用 Azure 自动化服务管理在规模较大的 Azure 的云服务。"
    services="cloud-services, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/20/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-cloud-services-using-azure-automation"></a>管理 Azure 云服务使用 Azure 自动化

本指南将向您介绍 Azure 自动化服务，以及如何使用它来简化对 Azure 的云服务的管理。

## <a name="what-is-azure-automation"></a>什么是 Azure 自动化？

[Azure 自动化](https://azure.microsoft.com/services/automation/)是简化了流程自动化通过云管理 Azure 服务。 使用 Azure 的自动化功能，长时间运行、 手动、 易出错，而且经常重复的任务可以自动执行以提高可靠性、 效率和时间值为您的组织。

Azure 的自动化提供了一个具有高可靠性和高可用性的工作流执行引擎，可以进行扩展以满足您的需求，随着组织的成长。 在 Azure 自动化进程可以是开始时间手动，由第三方系统，或在计划的时间间隔以便任务发生所需的确切时间。

降低操作开销和释放 IT / DevOps 员工将精力集中在工作中添加业务价值通过移动云管理任务由 Azure 自动化中自动运行。


## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Azure 自动化如何帮助管理 Azure 的云服务？

可以通过使用[Azure PowerShell 工具](https://msdn.microsoft.com/library/azure/jj156055.aspx)中可用的 PowerShell cmdlet Azure 自动化中管理 azure 的云服务。 Azure 的自动化，以便您可以执行所有的云服务的管理任务，在服务中，开箱即用，已可用这些云服务 PowerShell cmdlet。 此外可以对 Azure 自动化中使用这些 cmdlet 使用的 cmdlet 的其他 Azure 服务，能够跨 Azure 服务和第三方系统自动化复杂的任务。

Azure 自动化管理 Azure 云服务的使用示例包括︰

- [连续部署的云服务 cscfg 或 cspkg Azure Blob 存储在更新时](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
- [重新启动并行，每次一个升级域的云服务实例](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>下一步行动

既然已经学习了 Azure 自动化以及如何使用它来管理 Azure 的云服务的基本知识，按照这些链接以了解更多有关 Azure 自动化。

- [Azure 自动化概述](../automation/automation-intro.md)
- [我第一个 runbook](../automation/automation-first-runbook-graphical.md)
- [Azure 自动化学习地图](https://azure.microsoft.com/documentation/learning-paths/automation/)
 
