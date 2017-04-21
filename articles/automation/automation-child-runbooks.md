<properties 
   pageTitle="在 Azure 自动化中的子运行手册 |Microsoft Azure"
   description="介绍了 runbook Azure 自动化中从另一个 runbook 和它们之间共享信息的不同方法。"
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>在 Azure 自动化中子运行手册

它是 Azure 自动化来编写可由其他运行手册的离散函数的可重用、 模块化运行手册中的最佳做法。 通常，父 runbook 将调用一个或多个子级运行手册来执行所需的功能。 有两种方法来调用子 runbook，并且每一个都有明显的差别，您应该了解，以便您可以确定这将是最适合您的不同方案。

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>调用使用内联执行子 runbook

若要调用另一个 runbook 从 runbook 内联，runbook 的名称和使用完全相同的活动或 cmdlet 将使用提供的参数值。  相同的自动化帐户中的所有运行手册可供所有其他人使用这种方式。 父 runbook 将等待完成，然后才能移动到下一行中，子 runbook 和直接向父返回任何输出。

在调用 runbook 内联时，运行中的同一个作业作为父 runbook。 将运行子 runbook 作业历史记录中的不会显示。 将与父任何例外和任何子 runbook 的输出流。 这导致更少的作业，并使它们更易于跟踪和解决由于子 runbook 和其流输出的任何引发的任何异常都是与父作业相关联。

Runbook 发布时，必须已经发布，它调用任何子运行手册。 这是因为 Azure 自动化 runbook 编译时生成与任何子运行手册的关联。 如果他们不这样做，父 runbook 看起来正常，发布，但在它启动时，将生成异常。 如果发生这种情况，可以将父 runbook 为了正确引用子运行手册重新发布。 您不需要重新发布父 runbook，如果任何子运行手册被更改，因为该关联将被创建。

调用内嵌子 runbook 的参数可以是任何数据类型，包括复杂的对象，并且没有任何[JSON 序列化](automation-starting-a-runbook.md#runbook-parameters)时开始使用 Azure 管理门户 runbook，或者与开始 AzureRmAutomationRunbook cmdlet。


### <a name="runbook-types"></a>Runbook 类型

哪种类型可以互相调用︰

- [PowerShell runbook](automation-runbook-types.md#powershell-runbooks)和[图形化运行手册](automation-runbook-types.md#graphical-runbooks)可以调用 （两者都是基于 PowerShell） 的每个其他内联。
- [PowerShell 流 runbook](automation-runbook-types.md#powershell-workflow-runbooks)和图形 PowerShell 工作流运行手册可以调用 （两者都是基于 PowerShell 流） 的每个其他内联
- PowerShell 类型以及 PowerShell 流类型不能调用其他内联，并且必须使用开始 AzureRmAutomationRunbook。
    
当 does 发布订单方面︰

- PowerShell 流和图形 PowerShell 工作流运行手册的发布顺序运行手册才重要。


当您调用使用内联执行图形或 PowerShell 流子 runbook 时，您只需使用 runbook 的名称。  当您调用 PowerShell 子 runbook 时，之前必须使用其名称*。\\ * 若要指定脚本位于本地目录中。 

### <a name="example"></a>示例

下面的示例调用接受三个参数、 复杂对象、 整数和布尔值测试子 runbook。 输出的子 runbook 分配给变量。  在这种情况下，子 runbook 是 PowerShell 流 runbook

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

下面是同样的示例，使用 PowerShell runbook 作为子。

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>开始使用 cmdlet 子 runbook

可以使用[启动 AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) cmdlet 来[启动 Windows PowerShell 与 runbook](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell)中所述启动 runbook。 有两种此 cmdlet 的使用模式。  在一个模式中，该 cmdlet 返回作业 id 只要为子 runbook 创建子作业。  在其他模式中，您通过指定启用**-等待**参数，该 cmdlet 将一直等到孩子作业完成，然后将返回子 runbook 输出。

从开始使用 cmdlet 子 runbook 作业将从父 runbook 运行在单独的作业。 这会导致比 runbook 内联调用多个作业，使它们更难以跟踪。 父级可以而无需等待完成每个异步启动多子运行手册。 对于同样的并行执行调用子运行手册内联，父 runbook 将需要使用[并行的关键字](automation-powershell-workflow.md#parallel-processing)。

开始使用 cmdlet 子 runbook 的参数作为哈希表在[Runbook 参数](automation-starting-a-runbook.md#runbook-parameters)中所述。 可只有简单的数据类型。 如果 runbook 有复杂的数据类型的参数，然后它必须调用内联。

### <a name="example"></a>示例

下面的示例子 runbook 以参数初值，然后等待它完成使用开始 AzureRmAutomationRunbook-等待参数。 一旦完成，它的输出是从子 runbook 收集的。

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>调用子 runbook 方法的比较

下表汇总了从另一个 runbook 调用 runbook 的两种方法之间的差异。

| | 现场| Cmdlet|
|:---|:---|:---|
|作业|在与父同一个作业运行子运行手册。|为子 runbook 创建单独的作业。|
|执行|父 runbook 等待子 runbook，完成后才能继续。|父 runbook 将继续启动子 runbook 后立即*或*父 runbook 等待子作业完成。|
|输出|从子 runbook，父 runbook 可以直接获取输出。|父 runbook 必须检索输出从子 runbook 作业*或*父 runbook 可以直接输出从获取子 runbook。|
|参数|子 runbook 参数的值分别指定，并且可以使用任何数据类型。|值为子 runbook 参数必须合并为一个哈希表，并且只能包含简单，数组和对象数据类型，利用 JSON 序列化。|
|自动化的帐户|父 runbook 相同的自动化帐户中只能使用子 runbook。|如果您已连接到它，父 runbook 可以使用子 runbook 从任何自动化帐户来自同一个 Azure 订阅和甚至其他订阅。|
|发布|父 runbook 发布之前，必须先发布子 runbook。|必须启动父 runbook 之前的任何时候发布子 runbook。|

## <a name="next-steps"></a>下一步行动

- [从 runbook 开始在 Azure 自动化](automation-starting-a-runbook.md)
- [Runbook 的输出和 Azure 自动化中的消息](automation-runbook-output-and-messages.md)
