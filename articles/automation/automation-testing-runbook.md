<properties 
    pageTitle="在 Azure 自动化测试 runbook |Microsoft Azure"
    description="Runbook 发布了 Azure 自动化之前，可以测试，以确保按预期的方式工作。  本文介绍如何测试 runbook，并查看它的输出。"
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>在 Azure 自动化测试 runbook
Runbook 测试时，执行[的草稿版本](automation-creating-importing-runbook.md#publishing-a-runbook)，它执行的任何操作都已完成。 创建没有作业历史记录，但在测试中显示[输出](automation-runbook-output-and-messages.md#output-stream)和[警告和错误](automation-runbook-output-and-messages.md#message-streams)流输出窗格。 只有[$VerbosePreference 变量](automation-runbook-output-and-messages.md#preference-variables)设置为继续在输出窗格中显示[详细的流](automation-runbook-output-and-messages.md#message-streams)的消息。

即使正在运行的草稿版本，runbook 仍正常执行流，并在环境中执行任何操作对资源。 由于这个原因，只应测试运行在非生产资源手册。

若要测试每种[类型的 runbook](automation-runbook-types.md)的过程都是相同的并在测试文本编辑器和 Azure 的门户网站中的图形编辑器之间没有区别。  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>若要测试 runbook Azure 门户中

您可以使用 Azure 的门户网站中的任何[runbook 类型](automation-runbook-types.md)。

1. 在[文本编辑器](automation-editing-a-runbook.md#Portal)或[图形编辑器](automation-graphical-authoring-intro.md)中打开 runbook 的草稿版本。
2. 单击**测试**按钮以打开测试刀片式服务器。
3. 如果 runbook 参数，它们将被列入左窗格中，您可以提供用于进行测试的值。
4. 如果您想要运行测试[混合 Runbook 工作](automation-hybrid-runbook-worker.md)，然后给**混合工作人员**更改**运行设置**并选择目标组的名称。  否则，保留默认值**Azure**云环境中运行测试。
5. 单击**开始**按钮开始测试。
6. 如果 runbook 是[PowerShell 工作流](automation-runbook-types.md#powershell-workflow-runbooks)或[图形](automation-runbook-types.md#graphical-runbooks)，然后可以停止或暂停该任务而正在经输出窗格下方的按钮。 暂停 runbook，它正在挂起之前完成当前活动。 一旦 runbook 挂起时，可以停止它或重新启动它。
7. 检查在输出窗格中 runbook 的输出。


## <a name="next-steps"></a>下一步行动

- 若要了解如何创建或导入 runbook，请参阅[创建或导入了 Azure 自动化 runbook](automation-creating-importing-runbook.md)
- 有关图形创作的详细信息，请参阅[图形创作在 Azure 自动化](automation-graphical-authoring-intro.md)
- 若要开始使用 PowerShell 工作流运行手册，请参阅[我第一个 PowerShell 流 runbook](automation-first-runbook-textual.md)
- 若要了解更多有关配置 runboks 返回的状态消息和错误，其中包括建议的做法，请参阅[Runbook 输出和 Azure 自动化中的消息](automation-runbook-output-and-messages.md)