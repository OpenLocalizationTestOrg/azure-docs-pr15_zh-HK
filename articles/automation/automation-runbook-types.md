<properties 
   pageTitle="Azure 自动化 Runbook 类型"
   description="介绍了不同类型的运行手册，您可以使用在 Azure 自动化和确定使用哪种类型时，应考虑到的因素。 "
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
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Azure 自动化 runbook 类型

Azure 自动化支持下表中的运行手册，它简要介绍了四种类型。  以下各节提供了有关何时使用每种因素包括每种类型的详细信息。


| 类型 |  说明 |
|:---|:---|
| [图形](#graphical-runbooks) | 基于 Windows PowerShell 和 Azure 门户中的创建和编辑完全在图形化编辑器。 | 
| [图形化 PowerShell 工作流](#graphical-runbooks) | 基于 Windows PowerShell 工作流创建和编辑完全在 Azure 的门户网站中的图形编辑器。 
| [PowerShell](#powershell-runbooks) | 基于 Windows PowerShell 脚本文本 runbook。
| [PowerShell 工作流](#powershell-workflow-runbooks) | 文本根据 Windows PowerShell 流 runbook。 |


## <a name="graphical-runbooks"></a>图形化运行手册

创建和使用 Azure 的门户网站中的图形编辑器编辑[图形](automation-runbook-types.md#graphical-runbooks)和图形 PowerShell 工作流运行手册。  可以将它们导出到一个文件，然后将其导入另一个自动化帐户，但您不能创建或使用另一个工具对其进行编辑。  图形化运行手册生成 PowerShell 的代码，但您无法直接查看或修改的代码。 图形化运行手册不能转换为一种[文本格式](automation-runbook-types.md)，也可以为图形格式转换文本 runbook。 图形化运行手册可以转换为图形 PowerShell 工作流运行手册中，导入和进行相反的转换过程中。

### <a name="advantages"></a>优势

- 用学到的知识的[PowerShell](automation-powershell-workflow.md)创建运行手册。
- 以直观方式表示管理流程。
- 作为子运行手册创建高级别工作流包括其他运行手册。


### <a name="limitations"></a>限制

- 不能编辑 runbook Azure 门户之外。
- 可能需要包含 PowerShell 的代码来执行复杂的逻辑代码活动。
- 不能查看或直接编辑 PowerShell 代码创建的图形化工作流。 请注意，您可以查看您在代码中的任何活动中创建的代码。


## <a name="powershell-runbooks"></a>PowerShell 运行手册

Windows PowerShell 基于 PowerShell 运行手册。  直接编辑使用文本编辑器在 Azure 的门户网站 runbook 的代码。  您还可以使用到 Azure 自动化任何离线的文本编辑器和[导入 runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) 。

### <a name="advantages"></a>优势

- 实现所有的复杂逻辑，使用 PowerShell 代码没有 PowerShell 流的额外复杂性。 
- 由于它不需要编译运行之前，Runbook 开始比图形或 PowerShell 工作流运行手册。

### <a name="limitations"></a>限制

- 必须熟悉 PowerShell 脚本。
- 不能使用[并行处理](automation-powershell-workflow.md#parallel-processing)中并行执行多个操作。
- 不能使用[检查点](automation-powershell-workflow.md#checkpoints)继续在发生错误时的 runbook。
- PowerShell 工作流运行手册和图形化运行手册只可以作为子运行手册使用它创建新的作业开始 AzureAutomationRunbook cmdlet。

### <a name="known-issues"></a>已知的问题
以下是使用 PowerShell 运行手册的当前已知的问题。

- PowerShell 运行手册不能无法检索加密的[变量资产](automation-variables.md)具有空值。
- PowerShell 运行手册无法检索[变量资产](automation-variables.md)与*~*的名称中。
- 获取进程中继续循环大约 80 次迭代后的 runbook 可能会崩溃。 
- 如果它尝试同时写入到输出流数据量非常大，PowerShell runbook 可能会失败。   您通常可以通过输出仅使用大对象时，您需要将信息变通解决此问题。  例如，而不是输出*Get 过程*类似，可以输出与*获取进程只是必填的字段 |选择 ProcessName，CPU*。

## <a name="powershell-workflow-runbooks"></a>PowerShell 工作流运行手册

PowerShell 工作流运行手册是基于[Windows PowerShell 流](automation-powershell-workflow.md)的文本运行手册。  直接编辑使用文本编辑器在 Azure 的门户网站 runbook 的代码。  您还可以使用到 Azure 自动化任何离线的文本编辑器和[导入 runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) 。

### <a name="advantages"></a>优势

- 实现使用 PowerShell 工作流代码的所有复杂的逻辑。
- 使用[检查点](automation-powershell-workflow.md#checkpoints)继续在发生错误时的 runbook。
- 使用[并行处理](automation-powershell-workflow.md#parallel-processing)中并行执行多个操作。
- 作为子运行手册创建高级别工作流，可以包括其他图形运行手册和 PowerShell 工作流运行手册。


### <a name="limitations"></a>限制

- 作者必须熟悉 PowerShell 工作流。
- Runbook 必须应对其他复杂的 PowerShell 流如[反序列化的对象](automation-powershell-workflow.md#code-changes)。
- Runbook 需要更长的开始比 PowerShell 运行手册，因为它需要在运行之前进行编译。
- PowerShell 运行手册只可以作为子运行手册使用它创建新的作业开始 AzureAutomationRunbook cmdlet。


## <a name="considerations"></a>注意事项

您应考虑到下列其他事项时确定哪种类型用于特定的 runbook。

- 不能将运行手册从图形转换为文字类型或进行相反的转换。
- 有限制使用不同类型的运行手册，作为子 runbook。  有关更多信息，请参见[Azure 自动化中的子运行手册](automation-child-runbooks.md)。

  
## <a name="next-steps"></a>下一步行动

- 要了解更多有关图形 runbook 创作，请参阅[图形创作在 Azure 自动化](automation-graphical-authoring-intro.md)
- 要理解 PowerShell 并且 PowerShell 的区别的运行手册，工作流查看[学习 Windows PowerShell 工作流](automation-powershell-workflow.md)
- 有关如何创建或导入 Runbook 的详细信息，请参阅[创建或导入 Runbook](automation-creating-importing-runbook.md)



