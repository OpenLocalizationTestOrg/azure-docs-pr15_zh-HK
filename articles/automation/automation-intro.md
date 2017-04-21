<properties
    pageTitle="什么是 Azure 自动化 |Microsoft Azure"
    description="了解 Azure 自动化提供了什么价值，以便您可以开始创建，使用运行手册和 Azure 自动化 DSC 获得常见问题的答案。"
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="自动化，azure 自动化，azure 自动化示例是什么"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Azure 自动化概述

Microsoft Azure 自动化提供用户自动化云和企业环境中常用的手动、 运行时间较长、 易出错，而且经常重复任务的方法。 它可以节省时间和提高常规管理任务的可靠性并甚至安排他们在固定的时间间隔自动执行。 可以自动执行流程使用运行手册或自动使用所需状态配置的配置管理。 本文简要概述的 Azure 自动化并回答一些常见的问题。 您可以参考有关不同主题的更多详细信息此库中的其他文章。


## <a name="automating-processes-with-runbooks"></a>自动化流程与运行手册

Runbook 是一套在 Azure 自动化执行某些自动化的进程的任务。 它可能是一个简单的过程，例如启动虚拟机，以及创建日志项，或可能有复杂的 runbook 相结合其他较小的运行手册，以跨多个资源或更多个群和内部环境上执行一个复杂的过程。  

例如，您可能具有现有的截断的 SQL 数据库，如果它接近最大值，其中包含多个步骤，如连接到服务器，手动进程连接到数据库、 获取当前数据库的大小、 检查是否超过了阈值然后截断并通知用户。 而不是手动执行每个步骤，您可以创建将作为一个单独的进程中执行所有这些任务 runbook。 将启动 runbook、 提供所需的信息，例如 SQL 服务器名称、 数据库名称和收件人电子邮件，然后坐在过程完成时。 


## <a name="what-can-runbooks-automate"></a>运行手册可以自动化？

在 Azure 自动化运行手册基于 Windows PowerShell 或 Windows PowerShell 工作流，因此它们执行 PowerShell 可以执行的任何操作。 如果应用程序或服务有一个 API，runbook 可以使用它。 如果您有 PowerShell 模块的应用程序，您可以将该模块加载到 Azure 自动化并在您 runbook 中包含这些 cmdlet。 Azure 的自动化运行手册在 Azure 的云环境中运行，并且可以访问任何云资源或可从云的外部资源。 使用[混合 Runbook 工作人员](automation-hybrid-runbook-worker.md)，可在您的本地数据中心管理的本地资源运行运行手册。 


## <a name="getting-runbooks-from-the-community"></a>从社区获取运行手册

[Runbook 库](automation-runbook-gallery.md#runbooks-in-runbook-gallery)包含来自 Microsoft 的运行手册，社区可以使用在您的环境不变，或自行定制它们以自己的目的。 它们也是用作参考以了解如何创建您自己的运行手册。 甚至可以参与到库，您认为其他用户可能会发现有用自己运行手册。 


## <a name="creating-runbooks-with-azure-automation"></a>利用自动化 Azure 创建运行手册 

您可以从头[创建自己的运行手册](automation-creating-importing-runbook.md)或修改运行手册从[Runbook 库](http://msdn.microsoft.com/library/azure/dn781422.aspx)对自己的要求。 有三种不同[runbook 类型](automation-runbook-types.md)，您可以从基于您的需求和 PowerShell 的经验。 然后如果想直接使用 PowerShell 代码，可以使用[PowerShell runbook](automation-runbook-types.md#powershell-runbooks)或编辑脱机或使用[文本编辑器](http://msdn.microsoft.com/library/azure/dn879137.aspx)在 Azure 门户的[PowerShell 流 runbook](automation-runbook-types.md#powershell-workflow-runbooks) 。 如果您更喜欢不公开的基础代码的情况下编辑 runbook，您可以创建[图形 runbook](automation-runbook-types.md#graphical-runbooks) Azure 门户中使用[图形编辑器](automation-graphical-authoring-intro.md)。 

更喜欢观看与阅读？ 看看下面来自 Microsoft Ignite 会话中可能 2015年的视频。 注意︰ 虽然概念和在本视频中所述功能正确，Azure 自动化所处的位置很多由于记录了这段视频，它现在在 Azure 的门户中，具有更广泛的用户界面并支持附加功能。

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>自动化配置管理与所需状态配置 

[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx)是一个管理平台，使您可以管理、 部署和实施物理主机和使用声明性语法 PowerShell 的虚拟机的配置。 您可以定义配置 DSC 拉在中央服务器上的目标计算机可以自动检索和应用。 DSC 提供一套 PowerShell cmdlet，您可以使用配置和资源管理。  

[Azure 自动化 DSC](automation-dsc-overview.md)为提供服务所需的企业环境的 PowerShell DSC 一个基于云的解决方案。  您可以管理 Azure 自动化中的 DSC 资源和应用于从 DSC 拉 Azure 的云服务器检索它们的虚拟或物理机的配置。  它还提供报告通知您重要的事件，如节点未遵从其指派的配置和时新的配置已应用的服务。 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>利用自动化 Azure 创建您自己的 DSC 配置

[DSC 配置](automation-dsc-overview.md#azure-automation-dsc-terms)指定节点所需的状态。  多个节点可以应用相同的配置，以确保它们都保持相同的状态。  可以在您的本地计算机上创建配置使用任何文本编辑器，然后将其导入您可以编译的 Azure 自动化并将其应用到的节点。


## <a name="getting-modules-and-configurations"></a>获取模块和配置 

您可以获得包含在运行手册和 DSC [PowerShell 库](http://www.powershellgallery.com/)配置中可以使用的 cmdlet [PowerShell 模块](automation-runbook-gallery.md#modules-in-powershell-gallery)。 可以启动从 Azure 门户此库，然后直接到 Azure 自动化导入模块也可以下载并手动导入。 您无法安装模块直接从 Azure 的门户网站，但您可以下载这些安装它们，就像任何其他模块。 


## <a name="example-practical-applications-of-azure-automation"></a>示例的 Azure 自动化的实际应用 

以下是什么几种类型的自动化方案与 Azure 自动化的几个示例。 

* 创建和复制 Azure 的不同预订中的虚拟机。 
* 计划文件将从本地计算机复制到 Azure Blob 存储容器。 
* 自动检测到拒绝服务攻击时，如拒绝请求从客户端的安全功能。 
* 确保机器不断配合已配置的安全策略。
* 管理应用程序代码的连续部署跨云和部署基础结构。 
* 构建您的实验室环境中 Azure Active Directory 目录林。 
* 如果数据库已接近最大值，截断 SQL 数据库的表中。 
* 远程更新环境设置 Azure 网站。 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Azure 自动化与其他自动化工具关系？

[服务管理自动化 (SMA)](http://technet.microsoft.com/library/dn469260.aspx)被用于自动处理在私有云的管理任务。 它安装在本地数据中心中的[Microsoft Azure 包](https://www.microsoft.com/en-us/server-cloud/)某一组件。 SMA 和 Azure 自动化使用相同的 runbook 格式根据 Windows PowerShell 和 Windows PowerShell 工作流，但是 SMA 不支持[图形化运行手册](automation-graphical-authoring-intro.md)。  

[System Center 2012 控制器](http://technet.microsoft.com/library/hh237242.aspx)适用于自动化的内部资源。 它使用不同的 runbook 格式比 Azure 自动化和服务管理自动化和图形化的界面，而无需任何脚本创建运行手册。 其运行手册组成专门为控制器编写的集成包中的活动。 


## <a name="where-can-i-get-more-information"></a>从哪里可以获得更多的信息？ 

各种资源都可供您了解更多有关 Azure 自动化和创建您自己的运行手册。 

* **Azure 自动化库**是您是现在。 此库中的项目的配置和管理 Azure 自动化和创作您自己的运行手册提供了完整的文档。 
* [Azure PowerShell cmdlet](http://msdn.microsoft.com/library/jj156055.aspx)提供自动化使用 Windows PowerShell 的 Azure 运营的信息。 运行手册使用这些 cmdlet 使用 Azure 的资源。 
* [管理博客](https://azure.microsoft.com/blog/tag/azure-automation/)从 Microsoft 在 Azure 自动化和其他的管理技术提供的最新信息。 您应该订阅此博客保持最新从 Azure 自动化团队最新的。 
* [自动化论坛](http://go.microsoft.com/fwlink/p/?LinkId=390561)可以发布 Azure 自动化由 Microsoft 和自动化社区来解决有关问题。 
* [Azure 自动化 Cmdlet](https://msdn.microsoft.com/library/mt244122.aspx)提供了用于自动执行管理任务的信息。 它包含 cmdlet 来管理自动化帐户、 资产、 运行手册，DSC。


## <a name="can-i-provide-feedback"></a>我可以提供反馈？ 

**请给我们反馈 ！** 如果您正在寻找一个 Azure 自动化 runbook 解决方案或集成模块，请在脚本中心上发布脚本请求。 如果您有反馈或功能请求的 Azure 自动化，则在[用户语音](http://feedback.windowsazure.com/forums/34192--general-feedback)上发布它们。 谢谢 ！ 


